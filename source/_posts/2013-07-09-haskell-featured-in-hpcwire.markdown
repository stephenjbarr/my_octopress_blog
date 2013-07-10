---
layout: post
title: "Haskell featured in HPCwire"
date: 2013-07-09 19:52
comments: true
categories: haskell, hpc
---

[Lustre Founder Spots Haskell on HPC Horizon](http://www.hpcwire.com/hpcwire/2013-06-24/lustre_founder_spots_haskell_on_hpc_horizon.html). The article can be summarized by the following quote:

{% blockquote Peter Braam http://www.hpcwire.com/hpcwire/2013-06-24/lustre_founder_spots_haskell_on_hpc_horizon.html Haskell on HPC Horizon %}
While the use cases may be small, as big data and more complex models drive further into both research and enterprise settings, the appeal of a functional language that emphasizes correctness and productivity will reveal itself.
{% endblockquote %}

One of the great advantages of Haskell is that the community is very concious of [concurrency and parallelism](http://www.amazon.com/Parallel-Concurrent-Programming-Haskell-Multithreaded/dp/1449335942/ref=sr_1_1?ie=UTF8&qid=1373425179&sr=8-1&keywords=simon+marlow+haskell). By restricting functions from having side-effects, [the compiler](http://www.haskell.org/ghc/) can be made able to understand the exact dependency structure of computations needed for a piece of data. 

I would much rather spend my coding time on the problem at hand, and let other people optimize the compilation and scheduling.


## Links

+ [Parallel Scientific](http://www.parsci.com/)
+ [HPC Wire](http://www.hpcwire.com)
