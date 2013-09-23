---
layout: post
title: "ORMS Ch1 - Linear Programming"
date: 2012-08-26 11:22
comments: true
categories: linear-programming
---

In a previous post, I mentioned that I was working through the [*Operations Research and Management Science Handbook*](http://www.amazon.com/Operations-Research-Management-Science-Handbook/dp/0849397219/ref=sr_1_1?ie=UTF8&qid=1345966432&sr=8-1&keywords=operations+research+and+management+science+handbook). Chapter 1 is about linear programming.

Let's do a simple example in R, based on Example 1.2 of chapter 1. The LP is defined in the comments. To solve these simple LPs in R, I am using the [linprog package](http://cran.r-project.org/web/packages/linprog/linprog.pdf).

{% codeblock Code to Demonstrate linprog library lang:r %}
library(linprog)

##
## Solve the following LP
##
##
## maximize 15 x1 + 10 x2
##   s.t.   2  x1 +    x2 <= 1500
##             x1 +    x2 <= 1200
##             x1         <= 500
##


cvec <- c(15, 10)
bvec <- c(1500, 1200, 500)
Amat <- matrix( c(2,1,
                  1,1,
                  1,0),
               byrow = TRUE,
               nrow = 3)

solveLP(cvec, bvec, Amat, maximum=TRUE)

{% endcodeblock %}

This will yield the following output
{% codeblock Results of Linear Programming / Linear Optimization lang:r %}

Objective function (Maximum): 13500 

Iterations in phase 1: 0
Iterations in phase 2: 2
Solution
  opt
1 300
2 900

Basic Variables
    opt
1   300
2   900
S 3 200

Constraints
  actual dir bvec free dual dual.reg
1   1500  <= 1500    0    5      300
2   1200  <= 1200    0    5      200
3    300  <=  500  200    0      200

All Variables (including slack variables)
    opt cvec min.c max.c marg marg.reg
1   300   15  10.0    20   NA       NA
2   900   10   7.5    15   NA       NA
S 1   0    0  -Inf     5   -5      300
S 2   0    0  -Inf     5   -5      200
S 3 200    0  -5.0     5    0       NA

{% endcodeblock %}

The solveLP command solves problems of the form 

$$ \min ~ cx ~ \mbox{s.t.} Ax \leq b ~~\mbox{and}~~ x \geq 0 $$

Most of the rest of the chapter is about getting other problems into this form. It was a good chapter and was mostly review. Still, I hadn't thought explicitly about this in a while, so it was nice to review.
