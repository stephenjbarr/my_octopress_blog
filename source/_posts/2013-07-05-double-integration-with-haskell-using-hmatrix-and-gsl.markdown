---
layout: post
title: "Double Integration with Haskell using hmatrix and GSL"
date: 2013-07-05 10:10
comments: true
categories: haskell
---

I am currently working on porting some code from Matlab to Haskell to take advantage of Haskell's support for [data parallelism](http://community.haskell.org/~simonmar/par-tutorial.pdf).

To do the integration, I am using the [hmatrix package](http://hackage.haskell.org/package/hmatrix) from [Alberto Ruiz](http://dis.um.es/~alberto/). hmatrix allows for matrices in Haskell, but that is not my primary use of it in this project. Rather, I am using hmatrix's interface to the numerical integration facilities of the [GNU Scientific Library](http://www.gnu.org/software/gsl/) (GSL).


## Single Integration
hmatrix exposes the `integrateQNG` function with the following type

``` haskell

import Numeric.GSL
integrateQNG
  :: Double
     -> (Double -> Double) -> Double -> Double -> (Double, Double)


```
The first argument to `integrateQNG` is the tolerance, since this is numerical integration. The second argument is a function that, given a double as input, returns a double as output. The 3rd and 4th arguments are the lower and upper bounds of integration. Finally, it returns a tuple `(result, approximation_error)`. See [the docs here](http://hackage.haskell.org/packages/archive/hmatrix/0.5.2.2/doc/html/Numeric-GSL-Integration.html).

We can define

``` haskell
quad = integrateQNG 1E-6

```

and, due to Haskell's currying facilities, this we can consider this as a version of `integrateQNG` with the tolerance set to `1E-6`.

Thus, we can define a function as 

``` haskell
f x = x**2.0

```
and then integrate under the curve as ```quad f 0.0 1.0```. This returns the tuple `(0.33333333333333337,3.700743415417189e-15)`, which is accurate, as $$\int_{0}^{1} x^2 = \frac{x^3}{3} |_{0}^{1}$$ which is $$\frac{1}{3} - 0 = \frac{1}{3}$$.


## Double Integration

To do double integration, we can define the function 

``` haskell
quad2 f y1 y2 g1 g2 = quad h y1 y2
  where
    h y = fst $ quad (flip f y) (g1 y) (g2 y)

```
As I am relatively new to Haskell, I still need to think through exactly how to parse this. `quad2` takes a function f and 4 other arguments. 

``` haskell

> :t quad2
quad2
  :: (Double -> Double -> Double)
     -> Double
     -> Double
     -> (Double -> Double)
     -> (Double -> Double)
     -> (Double, Double)
	 
```

+ The first argument is a two-dimensional function over which we will be integrating. 
+ `y1` and `y2` are the bounds of integration for the outer integral, and these are fixed as constants. 
+ `g1` and `g2` are the bounds of the inner integration and these are functions of the variable of the outer integration. 
+ The output is still the tuple `(result, approximation_error)`.

Having `g1` and `g2` allows us to define our regions of integration as functions. For a rectangular region, we can use the functions `const a` and `const b`.

Wikipedia has the following example for [double integration](https://en.wikipedia.org/wiki/Multiple_integral#Double_integral):

$$ \int_7^{10} \int_{11}^{14} x^2 + 4 y ~ dx~ dy$$.

To replicate this in Haskell, put the statement `import Numeric.GSL` and the definitions of `quad` and `quad2` into a module, load this into ghci, and then do the following:

``` haskell

> let f x y = x**2.0 + 4.0 * y
> quad2 f 7 10 (const 11) (const 14)
(1719.0,1.908473379330644e-11)

```

We are using `(const 11)` and `(const 14)` for the inner bounds of integration because, in this case, we just want them to be constants. `const x`  returns a function which returns `x` regardless of the argument. Also, our answer seems to match the Wikipedia answer, which we can verify by looking at the integration itself.

**Special thanks** to Alberto Ruiz for kindly explaining this to me over email. I am relatively new to Haskell and his help was very useful.

## Links

+ [data parallelism](http://community.haskell.org/~simonmar/par-tutorial.pdf)
+ [hmatrix package](http://hackage.haskell.org/package/hmatrix)
+ [Alberto Ruiz](http://dis.um.es/~alberto/)
+ [GNU Scientific Library](http://www.gnu.org/software/gsl/) (GSL)
+ [hmatrix numerical integration docs](http://hackage.haskell.org/packages/archive/hmatrix/0.5.2.2/doc/html/Numeric-GSL-Integration.html)
+ [double integration](https://en.wikipedia.org/wiki/Multiple_integral#Double_integral)
