---
layout: post
title: "More Mixing of R and C++"
date: 2012-08-29 23:09
comments: true
categories: [R, c++, Rcpp]
---

A few of my [previous blog posts](https://www.google.com/search?q=site%3Asteve.planetbarr.com%2Fo&q=rcpp) have mentioned mixing R and C++ using [Rcpp](http://dirk.eddelbuettel.com/code/rcpp.html) and [RInside](http://dirk.eddelbuettel.com/code/rinside.html). The latter package, `RInside`, is for embedding an `R` instance inside of a `C++` program.

One potential use of `RInside` is to use `R`'s optimizers. `R` has implementations of [BFGS](http://en.wikipedia.org/wiki/BFGS_method), [Nelder-Mead](http://en.wikipedia.org/wiki/Nelder%E2%80%93Mead_method), and other optimization algorithms. But, what if

A) the objective function you wanted to optimize is in `C++`, *and*
B) you would like the result of this optimization embedded in a `C++` program.
	
If only the first condition (A) is satisfied, use `Rcpp` and call your `C++` objective function within `R`. But, if conditions (A) and (B) hold, meaning you want to optimize a `C++` function within an existing `C++` application, but use an `R` optimizer to do so, then what you will need to do is:


1. Use `RInside` to embed an `R` instance within `C++`. 
1. Write the `C++` function, having it return a `double`
2. Use `Rcpp::InternalFunction` to expose the `C++` function to the embedded R instance. [Example 9 from RInside](http://dirk.eddelbuettel.com/code/rinside/html/rinside__sample9_8cpp_source.html) shows how to do this.
3. Call `optim` on the exposed function from the embedded `R` instance

The following code demonstrates using `Rcpp::InternalFunction` to expose a few different kinds of functions to `R`
{% gist 3523265 sjb_rinside_ex1.cpp %}

In lines 31-36 we expose an objective function with a minimum at $$x= (3,5.5)$$. The input is an `Rcpp::NumericVector`. This function is called on the embedded `R` instance on line 65, and optimized with `optim` on line 69. 

[The code](https://gist.github.com/3523265) is stored in a [Github Gist](https://gist.github.com/), so you should be able to easily download and (with the included Makefile) compile the code and play with it. Enjoy!
