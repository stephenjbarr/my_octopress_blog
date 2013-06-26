---
layout: post
title: "Haskell: Accelerate CUDA"
date: 2013-06-25 20:47
comments: true
categories: haskell
---

I have been working on a programming project to solve some dynamic programming problems. To do this, I am using [Haskell](http://www.haskell.org/haskellwiki/Haskell) and the [Accelerate](https://github.com/AccelerateHS/accelerate) library. Accelerate allows for the writing of matrix-based programs which then get compiled to specific backends. I am particularly excited about the [CUDA backend](http://www.cs.indiana.edu/~rrnewton/haddock/accelerate-cuda/Data-Array-Accelerate-CUDA.html).

The idea is that a Haskell program can generate a CUDA program *at runtime*, compile it at *runtime*, and use the results to potentially generate more CUDA kernels at runtime. This is very appealing for dynamic programming because we can generate kernels on the fly. The following diagram is taken from [Simon Marlow's slides on Accelerate and CUDA](http://community.haskell.org/~simonmar/slides/cadarache2012/7%20-%20accelerate.pdf).

{% img /images/cuda_accelerate_diagram.png 800 800 %}

I should have some code up in a few days that does something interesting with this.

#### Links

+ [Haskell](http://www.haskell.org/haskellwiki/Haskell)
+ [Accelerate](https://github.com/AccelerateHS/accelerate)
+ [CUDA backend](http://www.cs.indiana.edu/~rrnewton/haddock/accelerate-cuda/Data-Array-Accelerate-CUDA.html)
+ [Simon Marlow's slides on Accelerate and CUDA](http://community.haskell.org/~simonmar/slides/cadarache2012/7%20-%20accelerate.pdf)
