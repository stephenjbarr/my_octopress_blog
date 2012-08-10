---
comments: true
date: 2012-08-09 06:04:42
layout: post
slug: using-rinside-with-rcppeigen
title: Using RInside with RcppEigen
wordpress_id: 32
---

## 1 Introduction






Greetings and sorry for the long delay in blog posts. Today, I want to discuss combining Eigen and R. [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page) is my favorite C++ library for working with matrices and vectors. The advantages of Eigen are



	
  * It is fast

	
  * It can link against Intel's MKL, making it very fast

	
  * The community is good, particularly the [forums](http://forum.kde.org/viewforum.php?f=74) and the IRC channel, #eigen on [freenode](http://freenode.net/)


However, not everyone uses C++ and Eigen. A large part of the academic community uses [R](http://cran.r-project.org/) because it isreasonably fast, interpreted, and generally gets out of the way and lets the programmer get the job done. One of the downsides of R is that it is not so fast for numerical computing.












## 2 Why would you combine R and Eigen (the 80/20 rule)






In code, one often finds that 80% of the runtime is spent in 20% of the code. This is actually a form of Pareto's [80/20 rule](http://en.wikipedia.org/wiki/Pareto_principle). What this means is that it does not make sense to optimize ALL of the code. Start with the few lines that are taking the most amount of time.

Also, one needs to remember that what you are trying to minimize is not runtime, but the sum of development time and runtime. In most cases, it is faster to develop in `R` than in C++. Additionally, there may be an existing codebase that you want to utilize, and that codebase may be in R. Thus, the developer is often left with one of the following choices



	
  1. Rewrite the existing R code in C++ (using Eigen) because you want to make it faster

	
  2. Rewrite the existing C++ code in R because there is an R module that does _exactly_ what you want, and you don't want to rewrite this in C++

	
  3. Write a modular combination of R and C++ scripts, plus some "glue" scripts to tie them together. For low IO applications (where the data being communicated between the modules is small) this can be reasonable, although you spend a lot of time parsing and messing with "glue" scripts.


With `Rcpp`, `RInside`, and `RCppEigen`, you have two new options



	
  1. Take the existing R code and embed a fast piece of C++/Eigen (`Rcpp`, `RcppEigen`)

	
  2. Take the existing C++ code and embed an R instance (`Rcpp`, `RcppEigen`, `RInside`)


I recently figured out exactly how to do this, and wanted to document it for my own reference and perhaps this will be useful for someone else as well.












## 3 Calling Eigen from R










### 3.1 Prerequisites








	
  1. Install R

	
  2. From within R, do

    
    install.packages(c("Rcpp", "RcppEigen", "inline"))
    library(RcppEigen)
    library(inline)
















### 3.2 Steps






The following example comes from the excellent [RcppEigen vignette](http://cran.r-project.org/web/packages/RcppEigen/vignettes/RcppEigen-intro-nojss.pdf). What we will be doing is making an Eigen verison of the transpose function. It is hard to imagine a case where `t()` is too slow, but the example should work. The C++ code for the transpose is

``` c++ Eigen function to do a transpose   
    using Eigen::Map;
    using Eigen::MatrixXi;
    
    // Map the integer matrix AA from R
    const Map<MatrixXi> A(as<Map<MatrixXi> >(AA));
    
    // evaluate and return the transpose of A
    const MatrixXi At(A.transpose());
    return wrap(At);
```

Go to your R session where you have loaded `RcppEigen` and `inline` and make the above program a string

``` r Load the c++ code into an R string    
    transCpp = 'using Eigen::Map;
    using Eigen::MatrixXi;
    
    // Map the integer matrix AA from R
    const Map<MatrixXi> A(as<Map<MatrixXi> >(AA));
    
    // evaluate and return the transpose of A
    const MatrixXi At(A.transpose());
    return wrap(At);'
```

Next, do the following in your `R` session

``` r   
    ftrans = cxxfunction(signature(AA="matrix"), transCpp, plugin="RcppEigen", verbose=TRUE)
```

the `verbose=TRUE` is not necessary, but is definitely informative. You should get some output that looks like this

``` r    
    ftrans = cxxfunction(signature(AA="matrix"), transCpp,
      plugin="RcppEigen", verbose=TRUE)
    ftrans = cxxfunction(signature(AA="matrix"), transCpp,
    +   plugin="RcppEigen", verbose=TRUE)
     >> setting environment variables: 
    PKG_LIBS =  -L/usr/lib/R/site-library/Rcpp/lib -lRcpp -Wl,-rpath,/usr/lib/R/site-library/Rcpp/lib
    
     >> LinkingTo : RcppEigen, Rcpp
    CLINK_CPPFLAGS =  -I"/usr/local/lib/R/site-library/RcppEigen/include" -I"/usr/lib/R/site-library/Rcpp/include" 
    
     >> Program source :
    
       1 : 
       2 : // includes from the plugin
       3 : #include <RcppEigen.h>
       4 : #include <Rcpp.h>
       5 : 
       6 : 
       7 : #ifndef BEGIN_RCPP
       8 : #define BEGIN_RCPP
       9 : #endif
      10 : 
      11 : #ifndef END_RCPP
      12 : #define END_RCPP
      13 : #endif
      14 : 
      15 : using namespace Rcpp;
      16 : 
      17 : 
      18 : // user includes
      19 : 
      20 : 
      21 : // declarations
      22 : extern "C" {
      23 : SEXP file2c6d59d82c92( SEXP AA) ;
      24 : }
      25 : 
      26 : // definition
      27 : 
      28 : SEXP file2c6d59d82c92( SEXP AA ){
      29 : BEGIN_RCPP
      30 : using Eigen::Map;
      31 : using Eigen::MatrixXi;
      32 : // Map the integer matrix AA from R
      33 : const Map<MatrixXi> A(as<Map<MatrixXi> >(AA));
      34 : // evaluate and return the transpose of A
      35 : const MatrixXi At(A.transpose());
      36 : return wrap(At);
      37 : END_RCPP
      38 : }
      39 : 
      40 : 
    Compilation argument:
     /usr/lib/R/bin/R CMD SHLIB file2c6d59d82c92.cpp 2> file2c6d59d82c92.cpp.err.txt 
    g++ -I/usr/share/R/include   -I"/usr/local/lib/R/site-library/RcppEigen/include" -I"/usr/lib/R/site-library/Rcpp/include"   -fpic  -O3 -pipe  -g -c file2c6d59d82c92.cpp -o file2c6d59d82c92.o
    g++ -shared -o file2c6d59d82c92.so file2c6d59d82c92.o -L/usr/lib/R/site-library/Rcpp/lib -lRcpp -Wl,-rpath,/usr/lib/R/site-library/Rcpp/lib -L/usr/lib/R/lib -lR
    >
```

What happened is that the source shown by "Program source:" was generated automatically by the `cxxfunction`. This generated a simple C++ function with one function that takes a `SEXP` object called `AA` as input and returns a different `SEXP` as output. Within this function,



	
  * line 33 maps to SEXP called `AA` to an Eigen matrix called `A`.

	
  * line 35 takes `A`, and makes a new matrix called `At` from the transpose of `A`

	
  * line 36 uses `Rcpp`'s wrap function take the Eigen matrix and transforms it into a `SEXP` to match the function signature


It is then compiled using the `g++` string that is shown. From `R`, you can then make a matrix and try transposing it. E.g.

``` r    
    A = matrix(1:12, nrow=3)
    ftrans(A)
```












### 3.3 Where to go from here






Make Eigen functions to speed up the slow bits of your R code. Get familiar with what Eigen can do and how fast it is. Definitely read the [RcppEigen vignette](http://cran.r-project.org/web/packages/RcppEigen/vignettes/RcppEigen-intro-nojss.pdf), and also the [Rcpp: Seamless R and C++ Integration](http://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0CF8QFjAA&url=http%3A%2F%2Fcran.r-project.org%2Fweb%2Fpackages%2FRcpp%2Fvignettes%2FRcpp-introduction.pdf&ei=18AiUPGlL8ThiwLkhIGIAg&usg=AFQjCNHU11d_pFYtCs_5gELew4rEYb6CwA&sig2=J9zEEtuSzZyYQnzE5VX89g) paper















## 4 Calling R from a C++ code






You can also find yourself in the situation described above, namely "2. Take the existing C++ code and embed an R instance (`Rcpp`, `RcppEigen`, `RInside`)". This will happen when you have some working C++ code, and want to add some functionality that already exists in R. To do this, you can use [RInsid](http://dirk.eddelbuettel.com/code/rinside.html)e to embed an R instance within a C++ program.

``` c++ Using RInside    
    #include <RcppEigen.h>
    #include <Rcpp.h>
    #include <RInside.h>                            // for the embedded R via RInside
    
    using namespace Rcpp;
    using namespace Eigen;
    using namespace std;
    int main(int argc, char *argv[]) {
    
        RInside R(argc, argv);                      // create an embedded R instance 
        R.parseEval("set.seed(1)");
    
        // Create a matrix in R
        SEXP mytmp = R.parseEval("matrix(rnorm(12),nrow=3)");
        std::cout << "Created a normal matrix in an embedded R instance" << std:: endl;   
    
        // Transform it into an Eigen matrix
        const Map<MatrixXd> mymat(as<Map<MatrixXd> >(mytmp));
        cout << mymat << endl;
    
    }
```

There are a lot of good examples in the `inst/examples/standard/` folder of the [RInside package](http://cran.r-project.org/web/packages/RInside/index.html). Plus, you will also need the Makefile. To compile this, I took a helpful hint from Dirk Eddelbuettel on the [rcpp-devel mailing list](http://lists.r-forge.r-project.org/mailman/listinfo/rcpp-devel), and used the compile string from the call to `cxxfunction` to see what other file needed to be included. In my case, I added `-I/usr/local/lib/R/site-library/RcppEigen/include` to the `CPPFLAGS` line of the Makefile in order to get the makefile to link correctly. Then, simply typing `make` at the command prompt will cause the program to compile and link correctly.

Calling the executable should yield

```    
    stevejb@ursamajor:~/Projects/big_data_sim_mle/simple_smle/R_inside_version$ ./sjb_rinside3 
    Created a normal matrix in an embedded R instance
    -0.626454   1.59528  0.487429 -0.305388
     0.183643  0.329508  0.738325   1.51178
    -0.835629 -0.820468  0.575781  0.389843
```

If you go to an R console and do

``` r    
    > set.seed(1)
    > matrix(rnorm(12),nrow=3)
               [,1]       [,2]      [,3]       [,4]
    [1,] -0.6264538  1.5952808 0.4874291 -0.3053884
    [2,]  0.1836433  0.3295078 0.7383247  1.5117812
    [3,] -0.8356286 -0.8204684 0.5757814  0.3898432
```

you get the same matrix.












## 5 Other References








	
  * [This discussion on Rcpp-devel](http://comments.gmane.org/gmane.comp.lang.r.rcpp/3587)













Date: 2012-08-08




Author: Stephen Jeffrey Barr




Org version 7.8.11 with Emacs version 24


[Validate XHTML 1.0](http://validator.w3.org/check?uri=referer)


