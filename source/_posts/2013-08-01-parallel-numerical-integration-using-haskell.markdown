---
layout: post
title: "Parallel Numerical Integration using Haskell"
date: 2013-08-01 08:52
comments: true
categories: [haskell, hmatrix, gsl, parallel]
---


I have been working on a project so solve a fairly complicated dynamic program, and part of that involves doing a large amount of numerical integrals. Because we want the results as quickly as possible, I have been working to streamline the Matlab reference implementation (which will take months to solve) into a super-fast multi-core Haskell implementation. 

**The code:**
{% gist 6132611 %}

How it works:

I mam using the [hmatrix](http://hackage.haskell.org/package/hmatrix) package which provides access to the [GNU Scientific Library](http://www.gnu.org/software/gsl/) which has some nice numerical integration routines. hmatrix provides an [interface to these routines](http://hackage.haskell.org/packages/archive/hmatrix/0.15.0.0/doc/html/Numeric-GSL-Integration.html).

First, I define a `quad` function which is really just calling `integrateQNG` with a reasonable tolerance.

``` haskell
quad = integrateQNG 1E-6

```

The remaining arguments to `quad` are a function, a lower bound, and an upper bound. E.g.


``` haskell
let integral_result = quad f a b
```

will calculate $$ \int_{a}^{b} f(x) dx$$.

Next, I define a simple function $$f(x) = x^2$$:

``` haskell
myfn :: Double -> Double
myfn x = x^2        

```
The type signature means that this function takes a double precision number as input and produces one as output. I create a function `thefn` of a 2-tuple representing (`lower bound`, `upper bound`). Then, I create a list of lower bounds and a list of upper bounds.

``` haskell 
lowers = [0.01,0.02..] :: [Double]
uppers = [0.1,0.2..] :: [Double]
my_bounds = zip lowers uppers

```

Both lists are infinitely long, and by construction $$\mbox{lowers}_j < \mbox{uppers}_j$$. The `zip` command takes two lists and creates a list of 2-tuples. Since Haskell is lazily evaluated, we can create infinite lists and just use them as we need.


To know what `parMapChunk` is doing, first consider `map`

``` haskell
map :: (a -> b) -> [a] -> [b]
```

This means, `map` takes a function $$f(a) = b$$ and a list of $$a$$'s and produces a list of $$b$$'s. `parMapChunk` has essentially the same functionality, but breaks list ```[a]``` into chunks of size ```n```, sends them to the different CPU cores for evaluation, and gathers the results into list ```[b]```. To learn more about `parMapChunk`, see [monad-par tutorial](http://community.haskell.org/~simonmar/slides/CUFP.pdf).


To compile:

``` bash
ghc --make integrator-v2.hs -rtsopts -threaded
```

and then run with

``` bash
./integrator-v2 +RTS -N20
```

where 20 is the number of CPU cores to dedicate to this run.


**Running: All cores utilized at near 100%**
{% img images/integrator-running.png %}


### Links

+ [monad-par tutorial](http://community.haskell.org/~simonmar/slides/CUFP.pdf)
+ [hmatrix](http://hackage.haskell.org/package/hmatrix)
+ [GNU Scientific Library](http://www.gnu.org/software/gsl/)
+ [Numerical integration routines in hmatrix](http://hackage.haskell.org/packages/archive/hmatrix/0.15.0.0/doc/html/Numeric-GSL-Integration.html)
