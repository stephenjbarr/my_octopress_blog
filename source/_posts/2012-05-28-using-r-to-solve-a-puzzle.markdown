---
comments: true
date: 2012-05-28 15:02:12
layout: post
slug: using-r-to-solve-a-puzzle
title: Using R to Solve a Puzzle
wordpress_id: 28
---

I recently saw a puzzle that had asked to find a 5 digit number that met the following criteria: 

  1. The product of the first two digits is 24 
  2. The forth digit is half of the second digit 
  3. The sum of the last two digits is the same as the sum of the first and third digits 
  4. The sum of all digits is 26 
  5. Not all the digits are unique 
  

I wanted to see if I could use `R` to solve this. It turns out, it is pretty easy. The key insight is to think of the numbers 1 to 99999 as a 5x99999 matrix. Numbers less than 10,000 need to be padded with zeros in order to fit. 

The code is as follows: 

   
``` r    
    x = 1:99999
    xbig = strsplit(formatC(x, width=5, flag=<span style="color: #2aa198;">"0"</span>), <span style="color: #2aa198;">""</span>)
    xdf = do.call(rbind, xbig)
    xnum = matrix(as.numeric(xdf), ncol=5)
    
    cond1 = (xnum[,1] + xnum[,3] == 24)
    cond2 = (xnum[,4] == (xnum[,2]/2))
    cond3 = ( (xnum[,4] + xnum[,5]) == (xnum[,1] + xnum[,3]))
    cond4 = (rowSums(xnum) == 26)
    
    c14 = (cond1 & cond2 & cond3 & cond4)
    xnum[c14,]
    
    candidates = xnum[c14,]
    cond5 = apply(candidates, 1, <span style="color: #859900;">function</span>(x) length(unique(x)) < 5)
    candidates[cond5,] 
```    
    

  

I made a bunch of intermediate objects so it is easy to inspect what each line is doing. The `formatC` command does the 0-padding for each element of the vector `x`. `strsplit` gives a list of character vectors where the 1st vector looks like `c("0","0","0","0","1")`. I then convert it back into a numerical matrix `xnum`, and from there it is easy to apply the conditions. 

The application of conditions 1-4 lead to 3 candidates. Applying condition 5 eliminates the ones where all digits are unique. 

I do not want to give away the answer, as that would be against the policy of the puzzle. Give the code a try! 
