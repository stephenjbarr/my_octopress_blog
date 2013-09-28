---
comments: true
date: 2012-05-21 04:00:00
layout: post
slug: my-workflow-for-running-jobs
title: My Workflow for Running Jobs
wordpress_id: 20
categories: workflow
---

In this post, I explain my general setup for running jobs on EC2. Since a picture is worth 1000 words, lets start with a picture and use a few words to fill in the gaps. 

{% img /images/my_workflow.png %}

I start working on [my dev box](http://steve.planetbarr.com/wordpress/?page_id=8), and in one workflow scenario I will have a model that I will want to run across many different parameterizations. For example, I may want to run the model for many different random parameterizations to get an idea of which parameters are driving which behaviors in the data. 

Say my model is an executable called `model`, and it takes command line parameters in the form: 

   
```    
    model --alpha='0.5123' --beta='0.95' --N=1000 
```    

  

where `alpha` and `beta` are the parameters of interest. According to the diagram, the first step is to "Push Tasks" to SQS. 

 

## Making the SQS tasks

  

**Some Python to show the general idea:**

``` python
uuid_g = str(uuid.uuid4()) ### uuid_g (group uuid) gets set outside loop
s3bucket = "my_bucket"     ### s3bucket getrs set outside loop

for i in range(0,N):    
    ## paramstr gets set somewhere here, unique to i
    ## uuid_i is unique to i
    queuemessage = paramstr + "," + s3bucket + "," + uuid_i + "," + uuid_g
    m = Message()
    m.set_body(queuemessage)
    status = q.write(m)
```
  
    

  

In the above code, there are `N` different randomly generated parameterizations, and each one gets its on SQS tasks. At the end of this post, I show a simple way of generating a matrix of random parameterizations. There are a few important things to realize from this: 

  1. The message (called `queuemessage`) is simply a comma separated string of parameter values 
  2. Each task gets its own uuid (called `uuid_i`). Use uuid4. 
  3. Each group of tasks gets its own uuid (called `uuid_g`). Use uuid4.  
  

Full code that does this for one of my projects is available [here](https://bitbucket.org/stevejb/is-solver/src/48c35b61cc18/awslauncher/createISS_compstatic_tasks.py). 

 

 

 

## Given a populated SQS queue, do the tasks in the cloud

  

In this section, I show what I do for running the tasks. Each of my instances on EC2 (denoted CC2 #X) has a bootstrap script that launches a Python client. The client runs the following loop 

  1. Check the SQS queue 
  2. **If** there are tasks in the queue 
    1. Read the message "0.5,0.95,eeba09f3,552a6f8c" 
    2. Construct the string     

```
      ./model --alpha=0.5 --beta=0.95 --output_base='eeba09f3/552a6f8c'
```
    
    

  
    3. Exec the string 
    4. Look for results in `eeba09f3/552a6f8c___results.csv`. Upload this to s3. 
 
  3. Else, there are no tasks in the queue. Shutdown. 
   

`model` takes the parameter `output_base` and all files that `model` generates will be prefixed by this. By making this a combination of `${uuid_g}/${uuid_i}___`, you can simply dump all of the output into an s3 bucket and not worry about collisions. `model` should also create a file called `${uuid_g}/${uuid_i}___params.csv` which records the parameters it was called with, including `alpha`, `beta`, and any other commands. `model` should also make a file called `${uuid_g}/${uuid_i}___success.txt` at the very end of the program, if everything went successfully. The non-existence of this file implies a crash. `${uuid_g}/${uuid_i}___log.txt` will probably be useful as well. I have also found it extremely useful to have a `--verbose` mode to the model which outputs all intermediate values (e.g. `${uuid_g}/${uuid_i}___matrix_A.csv`). The output can go somewhere like `${uuid_g}/${uuid_i}___output.csv`

An example client that does this is located [here](https://bitbucket.org/stevejb/is-solver/src/48c35b61cc18/awslauncher/ISSClient.py), written in Python and using Boto to interface with SQS and S3.  

 

 

## Back on the dev box, getting the results.

  

To actually get the results to my dev box, I use `s3cmd`, but [a modified one that runs in parallel](https://github.com/pcorliss/s3cmd-modification).  

  
    
    s3cmd get --recursive --parallel --workers=24 s3://my_bucket/eeba09f3
    

  

Once I have the results locally, I want to load them into [R](http://www.r-project.org/). The issue is, I have one giant directory `${uuid_g}` which has `N` `output.csv` files and just as many `param.csv` files. The first thing you can do is search for ones that have the `${uuid_g}/${uuid_i}___success.txt` file with grep, and make a list of the `${uuid_g}/${uuid_i}`. In my case, I simply looked for the word "SUCCESS" in the output. 

```
grep -r "SUCCESS" . | perl -ne '$_ =~/(\/.*)___/; print $1 . "\n"' > solved_good.txt
```   
  

should get you most of the way there. From here, it is simply a matter of iterating over each line of `solved_good.txt` and reading in the files. Some `R` code in the appendix should be helpful. 

 

 

## Summary

  

In this post, I have shown a useful way of pushing jobs to AWS and getting the results back. I have also shown a useful way of structuring your core task such that it is amenable to being run in parallel. Remember the following: 

  1. `model` takes as input parameters and a base filename 
  2. base filename is `${uuid_g}/${uuid_i}`
  3. `model` creates at least the following files: 
    1. `${uuid_g}/${uuid_i}___success.txt`
    2. `${uuid_g}/${uuid_i}___output.csv`
    3. `${uuid_g}/${uuid_i}___params.csv`
 
  4. The client which runs on the EC2 machines checks SQS for a task, processes it, dumps the results to s3 
  5. When you want to analyze, get from s3. Aggregate the results. And go for it. 
  

Enjoy and happy computing! 

 

 

## Code Appendix

   

 

### Generate a numpy matrix of random parameterizations, given an upper and lower bound for each parameter

``` python
import numpy as np
from collections import OrderedDict  ## this requires Python 2.7

paramDict = OrderedDict();
paramDict['theta']     = np.array([0.5,0.9])
paramDict['rho']       = np.array([0.15,0.95])
paramDict['sigma_v']   = np.array([0.01,0.6])
paramDict['a']         = np.array([0.001, 0.4])
paramDict['gamma']     = np.array([0.001, 0.05])
paramDict['s']         = np.array([0.0001,0.01])
paramDict['lambda_1']  = np.array([0.01, 0.5])
paramDict['lambda_2']  = np.array([0.0001, 0.01])

########################################
def genRandomRuns(paramDict, Mruns):
  params = paramDict.keys()
  plen = len(params)
  
  runs_matrix = np.empty((Mruns, plen))
  for i in range(Mruns):
    for pp in range(plen):
      ktemp = params[pp]
      kary = paramDict[ktemp]
      if len(kary) == 1:
        runs_matrix[i,pp] = kary[0]
      else:
        lbound = kary[0]
        ubound = kary[1]
        runs_matrix[i,pp] = np.random.uniform(low=lbound, high=ubound)

  return(runs_matrix)
########################################

ptable = genRandomRuns(paramDict, 10000)
```
    

### Read  many output.csv files in R

``` r
## set the root directory
rootpath = "/home/stevejb/mnt/csdata/barr_iss_storage___test/csdtr2"
fname = "solved_good_cs.txt"
dfile.path = paste(rootpath, fname, sep="/")
files = read.table(dfile.path, header=FALSE, as.is=TRUE)
filebases.full = paste(rootpath, files[,1], sep="")

## make a list of distinct uuid_g's and corresponding uuid_i's
guidlist = list()
uuidlist = list()
flbase = files[,1]
ugdf = data.frame()
for(i in 1:length(flbase)) {
  tmp = strsplit(files[i,1],"/")
  guidlist[[i]] = tmp[[1]][2]
  uuidlist[[i]] = tmp[[1]][3]
  ugdf[i,1] =   guidlist[[i]]
  ugdf[i,2] =   uuidlist[[i]]
}
guidlist = unique(unlist(guidlist))
uuidlist = (unlist(uuidlist))
colnames(ugdf) <- c("guid", "uuid") 

cslist = list()

## for each uuid_g, read every uuid_i that corresponds
## (one uuid_g to many uuid_i's)

for(i in 1:length(guidlist)) {

  guid.i = guidlist[i]
  uuids = subset(ugdf, guid==guid.i)$uuid
  datalist = list()
  datalist.ddw = list()

  basefiles   = paste(rootpath, guid.i, uuids, sep="/") 
  paramfiles    = paste(basefiles, "___params.csv", sep="")
  outfiles   = paste(basefiles, "___output.csv", sep="")

  count = 1
  for(ii in 1:length(paramfiles)) {


    OUTEXISTS = file.exists(outfiles[ii])
    PARAMEXISTS = file.exists(paramfiles[ii])

    if( !( OUTEXISTS & PARAMEXISTS)) {
      print("One file is missing")
      next
    }

    outlines =  length(readLines(outfiles[ii]))
    paramlines = length(readLines(paramfiles[ii]))

    if( outlines == 0 |
       paramlines == 0) {
      next
    }


    param.ii = read.csv(paramfiles[ii], header=TRUE)
    out.ii = read.csv(outfiles[ii], header=FALSE)

    ## something ad-hoc to bind the matrices together into
    ## one long named row
    m2df = data.frame(out.ii[,2])
    rownames(m2df) = out.ii[,1]
    rowdf = cbind(param.ii, t(m2df))  
    datalist[[count]] = rowdf

    count = count + 1             
  }

  csdf = do.call("rbind", datalist)
  cslist[[i]] = csdf

}

```
