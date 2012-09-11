---
layout: post
title: "Currently Reading: Optimal Lower Bounds for Anonymous Scheduling Mechanisms"
date: 2012-09-10 11:14
comments: true
categories: papers
---

I am currently reading the paper [*Optimal Lower Bounds for Anonymous Scheduling Mechanisms*](http://web.mit.edu/iashlagi/www/papers/anon-jour.pdf) by [Ashlagi](http://web.mit.edu/iashlagi/www/), [Dobzinski](https://sites.google.com/site/dobzin/), and [Lavi](http://ie.technion.ac.il/~ronlavi/). I am currently investigating the OR/OM literature literature, and I came across this paper from Itai Ashlagi's website.

The paper does a good job selling itself. One way it did so what giving a broader context as to why *anonymous mechanisms* is important. In particular, it gave three reasons:


**One - Algorithmic Perspective:**
{% blockquote %}
The classic algorithms for scheduling on unrelated machines are indeed anonymous. There does not seem to be an algorithmic reason that explains why a specific naming of the machines can help.
{% endblockquote %}


**Two - Mechanism Design Perspective:**
{% blockquote %}
Indeed, it is very easy to come up with non-anonymous mechanisms. However, are non-anonymous mechanisms more powerful than anonymous ones? All state-of-the-art mechanisms for the special cases in the recent literature are anonymous... This might suggest that anonymous mechanisms for this problem are as powerful as non-anonymous mechanisms. In fact, a separation between the power of these two classes will be remarkable.
{% endblockquote %}

**Three - Game-Theoretic Importance:**
{% blockquote %}
Not only are anonymous games interesting from a mechanism-design perspective, they are well-studied also from a wider game-theoretic point of view. In particular, anonymity is a compelling design requirement in many contexts as there is no discrimination between the players, and is therefore commonly studied in game theory as a whole.
{% endblockquote %}

This is nice because it immediately gets the mind going for potential applications of work in this stream of literature. For example, complex websites are often made up of many distributed, loosely couples systems. Things are getting a bit too complex to be all done in one "tight inner loop". Also, a single inner loop is not inherently parallelizable. It is nice that the authors have been able to make their paper sell itself so well.

It is easy to start thinking of websites as being collections of loosely coupled distinct components. In fact, there was a [recent post on the Github blog](https://github.com/blog/1252-how-we-keep-github-fast) about looking at the speed of various components on Github, and it was clear that they were thinking of their website in that sense. I am looking forward to reading and thinking more about this. Stay tuned!
