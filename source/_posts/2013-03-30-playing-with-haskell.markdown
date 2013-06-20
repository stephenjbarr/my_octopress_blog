---
layout: post
title: "Playing with Haskell"
date: 2013-03-30 18:45
comments: true
categories: haskell, programming
---
{% img http://s3.amazonaws.com/stevejb_blog_content/Haskell-Logo.svg 300 %}

## Summary ##

I just finished a busy academic quarter and have thus been on Spring break. The majority of my time, I spent with my wife and my son, and it has been a fantastic vacation. I also had the change to dive into Haskell, one of my back-burner projects.

Haskell is an extremely interesting language. It is statically typed and its default evaluation scheme is lazy. That means that you can declare an infinitely long recursively defined sequence, such as the Fibbonacci sequence, and it will not compute the nth value until it is needed. Haskell is a functional programming language. Haskell also has, from its creation, strong support for data parallelism and concurrency. This compares favorably to languages such as C/C++ and R, where the support for programming multiple cores seems to have been added on after the fact, through packages such as OpenMP or R's multicore package. 

## Resources ##
+ Read a [short intro to Haskell](http://www.haskell.org/haskellwiki/Introduction)
+ [Data parallel Haskell](http://www.haskell.org/haskellwiki/GHC/Data_Parallel_Haskell)
+ [Haskell for multicores](http://www.haskell.org/haskellwiki/Haskell_for_multicores)
+ [Watch this talk](http://www.youtube.com/watch?v=hlyQjK1qjw8) by Simon Peyton Jones, creator of Haskell
+ [Watch this talk too](http://www.youtube.com/watch?v=NWSZ4c9yqW8)
	
## The next Haskell Blog Post ##
I am doing some experimentation with the [Accelerate](https://github.com/AccelerateHS/accelerate) library, Haskell code which allows the real-time generation, compilation, and execution of CUDA code on a GPU. The main idea is that vector-based code is written in the Accelerate language (a domain specific language ([DSL](http://en.wikipedia.org/wiki/Domain-specific_language))), and this code can be executed on various backends. One of these backends is [AccelerateCUDA](https://github.com/AccelerateHS/accelerate-cuda), which generates [CUDA](https://developer.nvidia.com/what-cuda) code.

