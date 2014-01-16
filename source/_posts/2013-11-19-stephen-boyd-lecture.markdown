---
layout: post
title: "Stephen Boyd Lecture"
date: 2013-11-19 22:44
comments: true
categories: [optimization]
---

I recently attended a lecture by [Stephen P. Boyd](http://www.stanford.edu/~boyd/), an electrical engineering professor at Stanford. He gave a talk to a general audience about optimization, and a more technical talk about convex optimization. Both were interesting. In his technical talk, he spoke about [CVXGEN](http://www.stanford.edu/~boyd/papers/code_gen_impl.html), a software tool which "takes a high level description of a convex optimization problem family, and automatically generates custom C code that compiles into a reliable, high speed solver for the problem family." This is useful for embedded systems where the problems must be solved as quickly as possible.

Code generation is going to be an important part of future software systems. This is because it allows the separation between the problem description and the "bare-metal" implementation. This is a very logical and needed separation. An interesting example of this is the [Accelerate library](http://hackage.haskell.org/package/accelerate-cuda), which can generate high-performance CUDA code for GPUs, but also can generate good CPU code as well.

See Stephen's talk below:

{% youtube YggBqSWBLBk %}
