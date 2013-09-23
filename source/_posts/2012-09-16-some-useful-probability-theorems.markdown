---
layout: post
title: "Some Useful Probability Theorems"
date: 2012-09-16 18:35
comments: true
categories: [math, stochastic-processes]
---

I have been doing a review of stochastic processes. The other night, I was doing some review problems in [Ross' Stochastic Processes](http://www.amazon.com/Stochastic-Processes-Sheldon-M-Ross/dp/0471120626/ref=sr_1_1?ie=UTF8&qid=1347845730&sr=8-1&keywords=ross+stochastic+processes). Chapter 1 of the book reviews some preliminaries, and it was fun doing some problems and getting a reminder of some probability essentials.

**Problem 1.2** was phrased as follows: If $$X$$ is a continuous random variable having distribution $$F$$, show that (a) $$F(X)$$ is uniformly distributed over $$(0,1)$$; (b) if $$U$$ is a uniform $$(0,1)$$ random variable, then $$F^{-1}(U)$$ has distribution $$F$$, where $$F^{-1}(x)$$ is that value of $$y$$ such that $$F(y) = x$$.

This problem is clever in that it is asking for a proof of the [probability integral transform](http://en.wikipedia.org/wiki/Probability_integral_transform). It is exactly this theorem which is what makes [inverse transform sampling](http://en.wikipedia.org/wiki/Inverse_transform_sampling) work. Inverse transform sampling implies that, if you can generate a random variable $$x \sim U(0,1)$$ and you have access to the inverse CDF of some other distribution, you sample from that other distribution using the inverse transform. A proof of the probability integral transform is in the inverse transform sampling wikipedia article under [Proof of Correctness](http://en.wikipedia.org/wiki/Inverse_transform_sampling#Proof_of_correctness).


**Problem 1.3** Asked for a proof of the [Poisson Limit Theorem](http://en.wikipedia.org/wiki/Poisson_limit_theorem). It is this theorem that proves that in the limit (under regularity conditions) the a process that is binomial with parameters $$(n, p_n), n \geq 1$$ approaches a Poisson process if $$n  p_n \rightarrow \lambda$$.

**Problem 1.4** asked for the mean and variance a binomial random variable. Key to this is simply understanding the definition of mean and variance in terms of expectations, and then using the binomial theorem with some clever substitutions. [Solution here](http://www.math.ubc.ca/~feldman/m302/binomial.pdf). The binomial theorem states:
$$(a+b)^m = \sum_{y=0}^m \frac{m!}{y!(m-y)!} a^y b^m$$.


These problems were useful in that they asked for proofs of fundamental concepts in stochastic processes, but did not intimidate the reader by saying that these problems were "complicated". The three facts that are used,

+ the probability integral transform, 
+ the poisson limit theorem, and
+ the binomial theorem

will be useful to have ready more involved proofs.
