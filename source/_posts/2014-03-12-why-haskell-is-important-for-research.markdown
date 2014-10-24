---
layout: post
title: "Why Haskell is important for research, part 1 of N"
date: 2014-03-12 00:05:42 -0700
comments: true
categories: [haskell, research, programming]
---

I would like to write a bit about my experience using Haskell for academic research.
I have been working on a project with several UW faculty addressing information asymmetry in inventory models.
Part of this research involved solving a finite horizon dynamic program.
The reference implementation was written in Matlab, and was extremely slow.
So slow, in fact, that it ran for a month and produced no output.

During the tail end of 2013, I had been casually studying Haskell, mostly reading about it on the bus and programming in my mind.
I took the opportunity to use Haskell to solve this problem.
The results were amazing.
What would have taken months to solve in Matlab solved in less than 4 minutes in a program written by an intermediate-level Haskell programmer (and, judging myself as intermediate may be generous).

The implementation and results will be saved for another blog post.
What follows is are the beginnings of my thoughts on why Haskell is the perfect language for academic research.



<!-- # Introduction -->
<!-- The inventory problem is, structurally, a standard finite horizon dynamic program.  -->
<!-- This means that it is amenable to standard solution techniques, such as *backward induction*. -->
<!-- To utilize backward induction, the problem is solved for the final time horizon, denoted $$T$$, and let a given iteration be denoted $$K$$.  -->
<!-- The solution to the $$K = T$$ problem is utilized to solve the $$K = T-1$$ iteration.  -->
<!-- This process is iteratively repeated until $$K=1$$, at which point the optimal policy function can be extracted. -->


## Functional Programming

### What is a function?
Functional programming (FP) is a programming paradigm in which the function itself is a first-class object. 
The term *functional* in FP comes from the idea that within FP, there is a much closer mapping between the mathematical definition of a function and its equivalent in code. 
In mathematics, we define a function $$f$$ to have input $$X$$, and produce output $$Y$$, where the capitalized notation is used to remind ourselves that $$X$$ and $$Y$$ can be numbers, vectors, or arbitrary sets of mathematical objects.
In mathematics, we define a function as in Eq. 1, denoting that output $$Y$$ depends solely on input $$X$$, and the characteristics of the function itself.

Equation 1: $$Y = f(X). $$
Alternatively, we can write $$f: X \rightarrow Y$$, where we read this as "$$f$$ is a mapping from an input in the space of $$X$$ to an output in the space of $$Y$$."
As a simple example, for the Euclidean norm, $$f : \mathbb{R}^2 \rightarrow \mathbb{R}$$, where $$\mathbb{R}^2$$ is the space of inputs and $$\mathbb{R}$$ is the space of outputs.

Two very simple properties emerge from this mapping. 

Property 1.  For $$X = X'$$, $$f(X) = f(X')$$.
Property 2.  $$f$$ is invariant to changes in objects outside the set $$X$$.

These properties may seem like an an obvious exposition, but consider the dissonance between the definition of a function in the context of mathematics, and our use of the word "function" when writing computer code.

{% codeblock lang:c Property 1 Violated %}
double global_variable = 1.0; 

double code_f(double x) {
  double y;
  y = 2.0 * x + global_variable;
  global_variable = global_variable + 1.0; // increment the global variable
  return(y);
}

void main() {
  double y1 = code_f(1.0);             // = 3.0
  double y2 = code_f(1.0);             // = 4.0
}
{% endcodeblock %}

In discussion this code, we would casually refer to `code_f` as a "function", because it takes input and produces output. 
However, in light of Property 1 we see that, due to the global variable, `code_f` is not a function as we have defined it according to Eq. 1. 
Specifically, `global_variable` is a global variable with mutable state. 
The argument set for `code_f` is `x`. 
We see that the first property is violated.
Namely, for \(X = X', f(X) \neq f(X')\).

We can also easily construct, using a global variable, a violation of the second property.

{% codeblock lang:c Property 2 Violated %}
double global_variable = 1.0;  

double code_g(double x) {
  double y;
  y = 2.0 * x + global_variable;
  return(y);
}

void main() {
  double y3 = code_g(1.0);             // = 3.0
  global_variable = 10.0;              // change something outside of X
  double y4 = code_g(1.0);             // = 13.0
}
{% endcodeblock %}

In the previous listing, property two is violated because a change outside of the argument set of `code_g` can change the value of `code_g(1.0)`. 
This, strictly speaking, `code_g` is not a function in the strict mathematical sense.
By contrast, consider the following function for squaring numbers.

{% codeblock lang:c Square %}
double square(double x) {
  return (x*x);
}
{% endcodeblock %}

As `square` does not depend on the global state of any variable, a call to `square` for a given input `x` will always return the same output.
There is a clear mapping between `square` and the mathematical function $$f(x) = x^{2}$$.
To distinguish this from functions which do depend on global state, we can call this a **pure** function.
Functions whose values can change depending on global state are denoted **impure**.

# Functional Programming Concepts
To be continued...

