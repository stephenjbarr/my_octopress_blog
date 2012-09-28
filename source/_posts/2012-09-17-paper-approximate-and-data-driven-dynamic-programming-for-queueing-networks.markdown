---
layout: post
title: "Paper: Approximate and Data-Driven Dynamic Programming for Queueing Networks"
date: 2012-09-17 18:26
comments: true
categories: paper
published: true
---

## Introduction ##

I just finished reading [*Approximate and Data-Driven Dynamic Programming for Queueing Networks*](http://www.stanford.edu/~bvr/psfiles/adp-queueing.pdf) by [Moallemi](http://moallemi.com/ciamac/), [Kumar](https://www.chicagobooth.edu/faculty/bio.aspx?person_id=26619906048), and [van Roy](http://www.stanford.edu/~bvr/) (MKR).

The paper was interesting from both and theory and applied perspective. The basic idea of the paper was to approach scheduling problems in "complex queueing networks". 

As an example of such a network, the paper uses the example of a crossbar switch. As a kid, I always had an interest in computer networks are part of this includes network topology. In the early 90's, token ring technology had not completely won out over TCP/IP. In a ring-network, the sending computer sends its message out on the network, forwarded along the ring until a computer that is listening for a specific token picks up the message. An alternative to ring networks were to use [TCP/IP](http://en.wikipedia.org/wiki/Internet_protocol_suite) over a **hub**. A computer would send a message on its network port, and the hub it was connected to *broadcasted* the message to all of the other ports. Hubs were later replaced in most cases by **switches**. The advantage of a network switch was that the broadcasting behavior of a hub was replaced by point-to-point routing. Meaning, if machine $$A$$ is talking to machine $$B$$, then machine $$C$$ and $$D$$ will get those packets.

{% img https://s3.amazonaws.com/stevejb_blog_content/NetworkTopology-Ring.png 300 %}

**A crossbar switch** can serve as an example of a complex queueing network. Another way to think of this is an order fulfillment center. The inputs are freight containers full of goods. The outputs are delivery trucks. Inside the fulfillment center, the goal is get the stuff from the freight containers into the delivery trucks. This isn't a perfect analogy, but you should be able to see the parallels.

{% img https://s3.amazonaws.com/stevejb_blog_content/B_Board_Manual_Originated_Calls.jpg 300 %}

<!-- ## Terminology ## -->

<!-- This section has defines some terminology that will useful in the next section -->

<!-- + Optimal Control - *controlling* an intertemporal process, often in the face of uncertainty, such that an objective function is minimized among the set of all possible control strategies -->
<!-- + Markov Decision Process - (from [wikipedia](http://en.wikipedia.org/wiki/Markov_decision_process)) named after Andrey Markov, provide a mathematical framework for modeling decision making in situations where outcomes are partly random and partly under the control of a decision maker -->
<!-- + TD-learning -->
<!-- + Dynamic Programming -->
<!-- + Approximate Dynamic Programming -->
<!-- + Differential Cost Function -->

## Summary ##


### Why Approximate Dynamic Programming? ###
The paper approaches to problem of (near)-optimal control of a complex queueing network. *Optimal control* in this context refers to the scheduling policy of the queueing network. A natural approach to solving these types of problems is [*dynamic programming*](http://en.wikipedia.org/wiki/Dynamic_programming). Dynamic programming often follows the steps of:

1. Create a *value function* representing the current $$+$$ expected future value of each action. This value function exists for every possible time $$t$$ state of the system and the arrival of new stochastic shocks. Often the value function is written in the form of a [Bellman equation](http://en.wikipedia.org/wiki/Bellman_equation).
2. From this value function, create a *policy function* representing the best choice of a $$t+1$$ state for each possible state at time $$t$$

Given that the value function needs to enumerate current states, future states, and stochastic shocks, problems with large state spaces become intractable quickly. Still, dynamic programming is a very useful technique and there are algorithms such as [value iteration](http://johnstachurski.net/lectures/fvi_rpd.html) which can give the optimal policy functions. 

The problem in MKR is not just large, it is provably intractable (MKR ref 24). The problem is that the value function cannot be computed for every state. To get around this, they use the *approximate dynamic programming* (ADP) approach. ADP refers to an entire branch of literature, sometimes called near-optimal control, where the value function is not computed exactly, but rather approximated. The main advantage of ADP is that many problems become tractable. However, there is no generic "best method" of approximating the value function. The introduction to chapter 4 of MKR provides a nice explanation.


{% blockquote MKR %}
The most difficult and crucial aspect of the ADP methodology is the selection of basis functions. One would like to select a broad set of basis functions whose span is likely to be close to the true differential cost function on relevant portions of the state space. For example, in a queueing network under light traffic, one may consider a set of basis functions including indicator functions for all ‘small’ states and functions describing asymptotic behavior for ‘large’ states. This way, the differential cost function can be approximated to a fine level of detail on the ‘small’ states, where the process is most often expected to live. However, this architecture quickly proves to be cumbersome since the number of basis functions will grow exponentially in the dimension of the state space. Further, it may fail to yield good policies under higher levels of load.
{% endblockquote %}

## Results ##
The paper shows that there is an improved performance of the network over the standard heuristic. The improvement is especially great as the traffic load is increased. 

Overall, I really enjoyed reading this paper. It addressed an important problem of complex queueing networks. In doing so, they used ADP, which I believe will become increasingly important as we try to tackle larger and larger problems. They also use an interesting approach to choosing the basis functions and simplifying the problem based on its structure. They end with a section on using a machine learning technique, *Q-learning*, to deal with the situation where the properties of the input arrival process is not known, but there is data available. 

Look for future post with bit more discussion on this interesting paper.


