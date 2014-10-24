---
layout: post
title: "Inventory Problem: Hadley and Whitin 4-23"
date: 2014-03-23 18:36:54 -0700
comments: true
categories: 
---



# Hadley and Whitin 4-23


I have been working on some review problems, will be working on a series of blog posts about solving problems in inventory, optimization, and queuing. 
A classic inventory text book is [Hadley and Whitin, (1963)](http://www.amazon.com/Analysis-Inventory-Systems-International-Quantitative/dp/1258240238/ref%3Dsr_1_1?ie%3DUTF8&qid%3D1395623778&sr%3D8-1&keywords%3Dhadley%2Band%2Bwhitin). 
This blog post is on Hadley and Whitin problem 4-23.

## Problem Statement

An automotive repair shop installs new mufflers on autos.
Past history indicates that the number of units demanded for a certain model of muffler is Poisson distributed with a mean of 1 per day.
The procurement lead time is always either 8 days for 15 days, the probability of 15 days being 0.7.
A muffler costs the shop $6 and it uses an inventory carrying charge of I= 0.20.
The cost of placing an order is estimated to be $1.00.
Requests for muffler changes which occur when the dealer is out of stock are taken elsewhere and the goodwill loss plus lost profits is estimated to be $25.00
If a $$(Q,r)$$ system is used to control the inventory of this muffler, what are $$Q^{*}, r^{*}$$?
What is the average annual cost of uncertainty? 
What assumption was made about the nature of the lead times?
Is it valid?

## Digesting the Problem

When I first read the problem, I try to break it down into the basic parameters and information.
First, we need to know what is a $$(Q,r)$$ policy.
Simply, it is an inventory control policy which says, when inventory drops to $$r$$, place an order of $$Q$$ units. 
These problems are interesting because there are trade offs.
In this case, the trade off is between A:  having too much inventory and overpaying on carrying charges, and B: having too little inventory and overpaying on goodwill costs. 
The optimal $$Q^{*}, r^{*}$$ are the values of the problem which minimize the cost of this trade off.
Also interesting in this problem are the stochastic lead times. 
A higher lead time  would increase $$Q^{*}$$ (to avoid paying goodwill costs), whereas a lower lead time would decrease $$Q^{*}$$.

Parameters:

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="left" />

<col  class="left" />

<col  class="right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="left">Parameter</th>
<th scope="col" class="left">Symbol</th>
<th scope="col" class="right">Value</th>
</tr>
</thead>

<tbody>
<tr>
<td class="left">Demand</td>
<td class="left">$$\lambda $$</td>
<td class="right">Pois, $$= 1$$ per day</td>
</tr>


<tr>
<td class="left">Fixed order cost</td>
<td class="left">$$A$$</td>
<td class="right">1</td>
</tr>


<tr>
<td class="left">Unit cost</td>
<td class="left">$$C$$</td>
<td class="right">6</td>
</tr>


<tr>
<td class="left">Carrying cost</td>
<td class="left">$$I$$</td>
<td class="right">0.20 per unit</td>
</tr>


<tr>
<td class="left">Goodwill/LS</td>
<td class="left">$$L$$</td>
<td class="right">25</td>
</tr>


<tr>
<td class="left">Order Quantity</td>
<td class="left">$$Q$$</td>
<td class="right">&#xa0;</td>
</tr>


<tr>
<td class="left">Reorder threshold</td>
<td class="left">$$r$$</td>
<td class="right">&#xa0;</td>
</tr>


<tr>
<td class="left">Lead time</td>
<td class="left">$$\tau$$</td>
<td class="right">Either 8 or 15</td>
</tr>
</tbody>
</table>

The lead time distribution:

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="left" />

<col  class="right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="left">Time</th>
<th scope="col" class="right">Probability</th>
</tr>
</thead>

<tbody>
<tr>
<td class="left">8 days</td>
<td class="right">0.3</td>
</tr>


<tr>
<td class="left">15 days</td>
<td class="right">0.7</td>
</tr>
</tbody>
</table>

### Limitations of the assumptions

-   In real life, there is an upper bound on the carrying capacity that is based on the physical constraints of the facility.
-   Or, there may be a nonlinear carrying cost function. E.g. once the auto shop's storage space is full, they can rent from a nearby storage unit or from a neighbor, for an increased cost.

## Solution approach

We will take the following approach:

1.  For $$\tau \in [8,15]$$, we will create the function $$K(Q,r;\tau)$$. where $$K(Q,r; \tau)$$ is the average cycle cost for a given $$Q,r$$ and parameter $$\tau$$.
2.  Let the average cycle cost be a weighted average of the $$\tau$$-specific average cycle costs.

In general, assume a lead time distribution $$g(\tau)$$.
Then, then average cost is the probability-weighted $$\tau$$-specific average cycle costs.

\begin{equation}
\label{eq:avg-k-over-tau}
K(Q,r) = \int g(\tau) K(Q,r; \tau) d \tau
\end{equation}

To calculate the average cycle cost for a given $$\tau$$, the approach is

1.  Calculate the expected length of time out of stock per cycle (H&W (4-100))
2.  Calculate the expected number of lost sales per cycle (H&W (4-101))
3.  Calculate the expected unit years of stock held per cycle (H&W (4-105))
4.  Use [1-3] to calculate the average annual cost as a function of $$Q,r; \lambda, \tau$$

### Expected length of stock out period

To derive the out-of-stock probability, 

- Consider the cycle of length $$T$$, which beings when the inventory hits regeneration point $$r$$
- At $$r$$, and order of quantity $$Q$$ is made, which takes $$\tau$$ units of time to ship
- In order to have an out-of-stock event, there must be more than $$r$$ orders in the interval of length $$\tau$$



We can break this down as follows:

- Consider the intervals $$[0,t),[t, t+dt), [t+dt,\tau]$$. If there are $$r-1$$ arrivals in $$[0,t)$$ and 1 arrival in $$[t,t+dt)$$, then all subsequent arrivals will be out-of-stock
- Since the process is Poisson with rate $$\lambda$$, the probability of one arrival in a time of length $$t$$ is $$\lambda t$$.  Thus, probability of one arrival in $$[t,t+dt)$$ is $$\lambda dt$$. This is possible because Poisson processes are memory-less, and also the independent increments property.
- The probability of $$r-1$$ events in time $$t$$ is denoted $$p(r-1; \lambda t)$$, and is just the standard Poisson density, namely
    
    \begin{equation}
    \label{eq:pois-density}
    p[(N(t+ \tau) - N(t)) = k] = \frac{e^{-\lambda \tau} (\lambda \tau)^k}{k!}  \qquad k= 0,1,\ldots,
    \end{equation}
-   Since the increments are independent, we can multiply the probabilities together.
-   Denote the following:
    -   A: $$(r-1)$$ demands in time $$t$$
    -   B: 1 event in $$[t, t + dt)$$
-   Prob(A and B) = $$p (r-1 ; \lambda t) * \lambda dt = \frac{e^{-\lambda t} (\lambda t)^{(r-1)}}{(r-1)!} \lambda dt$$
-   To get the expected length the out of stock period, see that A and B occur, then the time out of stock is approximately $$\tau - t$$. Therefore, the expected value is
    
	$$
    \hat T = \int_{0}^{\tau} (\tau  - t) \frac{e^{-\lambda t} (\lambda t)^{(r-1)}}{(r-1)!} \lambda dt = \int_{0}^{\tau}  \lambda (\tau  - t) \frac{ (\lambda t)^{(r-1)}}{(r-1)!} e^{-\lambda t} dt
	$$

-   To perform this integral, we can use some of the useful identities of HW Appendix 3, are necessary
    -   Eq. (A3.16), $$\int_{0}^{T} p(r; \lambda t ) dt = \frac{1}{\lambda} P(r+1; \lambda T)$$

We are given the exact result as $$\hat T = \tau P(r; \lambda \tau) - \frac{r}{\lambda} P(r + 1; \lambda \tau)$$.
Plugging this into for the values of our problem:

-   For $$\tau = 8$$, $$\hat T_{8}  = 8 * P(r; \tau) - r P(r + 1;   \tau)$$
-   For $$\tau = 8$$, $$\hat T_{15}  = 15 * P(r; \tau) - r P(r + 1;   \tau)$$

### Expected number of lost sales per cycle (h&w (4-101))

to derive the expected number of lost sales per cycle, we can think about the demand during the lead time.
at the start of the lead time, there are exactly $$r$$ items in inventory.
assume there are $$x$$ sales during lead time.
if $$x > r$$, then $$x - r$$ sales are lost.

therefore, to get the expected number of lost sales, 

\begin{equation}
\label{eq:exp-lost-sales}
\sum_{x=r+1}^{\infty} (x-r) p(x; \lambda \tau)
\end{equation}

Using (H&W (A3-10)), we see that this is equal to $$\lambda \tau P(r - 1; \lambda \tau) - r P(\tau; \lambda \tau)$$.

### Expected unit years of stock held per cycle (H&W (4-105))

The expected unit years of stock held per cycle (H&W (4-105)) is calculated in two parts. 
1.  Stock held from order arrival until order point is reached (which is a random amount of time)
    1.  Question - How do we know what the distribution of stock is at this point?
    2.  Answer - The probability of $$w$$ units on hand when the order arrives is based on regeneration point $$r$$ and lead time $$\tau$$, specifically, $$p(r - w)$$
2.  Stock held from reorder time to time when order arrives.

That is, the stock over a period of length $$\tau$$ which begins having $$r$$ units of stock.

For part 1, explain the derivation. 

### The solution

We find that $$(Q^{*}, r^{*})$$ is $$(18,17)$$.

To solve in Haskell, we need to write some code which can do the following:
-   Calculate the average annual cost for a given $$q,r, \Theta$$ where $$\Theta$$ is a particular parameterization of the problem.
-   Set a particular $$\theta_{i}$$ equal to this particular parameterization, that described in the problem statement and summarized in the table.
-   Output the results for plotting

Additionally, it would be interesting if we could explore other parameterizations of the problem, and other distributions of lead times. 
In general, any discrete lead-time distribution is a set of pairs $$\{\tau_{i},p(\tau_{i})\}$$ such that $$\tau_{i} > 0,p(\tau_{i}) > 0 \forall i$$ and  $$\sum_{i} p(\tau_{i}) = 1$$.
This configuration is general enough to include uniform distributions between $$[\tau_{\min},\tau_{\max}]$$, approximations normal distributions, or any other discretized distribution.

To create the solver, I first define a few standard imports:

{% codeblock lang:haskell Imports %}
    {-# LANGUAGE DeriveDataTypeable #-}
    
    module HW4_23 where
    
    import Data.Typeable
    import Data.Generics
    import Math.Factorial
    import Data.List
    import System.IO
    import Data.UUID as U
    import qualified Data.UUID.V4 as U4
    import Control.Monad
    

{% endcodeblock %}

There is nothing special here except to see that I am importing `Data.UUID` in order to generate UUIDs for a particular parameterization.
I have found this to be a useful practice in any kind of simulation.
Namely, make every parameterization attached to a particular uuid, and then let the uuids follow the data around. 
This way, you don't need to play games matching indices or anything like that.
Instead, you can simply dump all of your simulation data in a folder or S3 bucket.

Next, I define an object which represents a parameterization.

{% codeblock lang:haskell Cost Parameters %}
    data CostParams = CostParams {
      _cp_arrial_rate      :: Double,
      _cp_unit_cost        :: Double,
      _cp_fixed_order_cost :: Double,
      _cp_carrying_cost    :: Double,
      _cp_lost_sales_cost  :: Double,
      _cp_lead_time        :: Double
    } deriving (Show, Data, Typeable, Eq, Ord)
    
    
    cost_par_default = CostParams {
      _cp_arrial_rate      = 1,
      _cp_unit_cost        = 6,
      _cp_fixed_order_cost = 1,
      _cp_carrying_cost    = 0.20,
      _cp_lost_sales_cost  = 25,
      _cp_lead_time        = 8
      } 
    

{% endcodeblock %}

The `_cp_` prefix to all of the parameter names because for each data type, a function is created to access it. 
Meaning, for a particular `CostParams`, you can access the unit cost by doing `_cp_unit_cost cp`.
This is a bit cumbersome, and there is an extension called [OverloadedRecordFields](https://ghc.haskell.org/trac/ghc/wiki/Records/OverloadedRecordFields) which will fix this.

Next, we define some Poisson distribution functions.

{% codeblock lang:haskell Poisson functions %}
    pois_pdf :: Double -> Int -> Double
    pois_pdf lambda k = val
      where
        fact_k = (factorial k) :: Double
        val = (exp (- lambda)) * (lambda ^^ k) / fact_k
    
    pois_cdf :: Double -> Int -> Double
    pois_cdf lambda k = sum $ map (pois_pdf lambda) [0..k]
    
    pois_tail_cdf :: Double -> Int -> Double
    pois_tail_cdf lambda k  = 1.0 - (pois_cdf lambda (k-1))
    

{% endcodeblock %}

The average annual cost $$K(q,r; \Theta)$$ is defined as below.
It is important to note that Haskell has partial function application.
Meaning, we can apply `avg_annual_cost` to a `CostParams` and we are left with a function of type `Int -> Int -> Double`, which is equivalent to $$K(q,r)$$ where the parameterization is now implicit in the construction of the function. 
Also notice that `avg_annual_cost` is simply a reordering of the arguments for syntactic convenience.

{% codeblock lang:haskell Average Annual Cost %}
    avg_annual_cost :: Int -> Int -> CostParams -> Double
    avg_annual_cost q r cp = cost
      where
        tau    = (_cp_lead_time cp)
        lambda = (_cp_arrial_rate cp)
        a      = (_cp_fixed_order_cost cp)
        ic     = (_cp_carrying_cost cp)
        mu     = lambda * tau
        pi0    = (_cp_lost_sales_cost cp)
        rd     = fromIntegral r
        qd     = fromIntegral q
        that   = tau * (pois_tail_cdf  mu r) - ( rd / lambda )  * (pois_tail_cdf  mu (r+1))
    
        cmult  = lambda / ( qd + lambda * that)
        term1  = (1/(2 * lambda)) * qd * (qd + 1.0) + ((qd * rd)/lambda) - ((qd * mu)/lambda)
    
        term2  = (((ic * qd)/lambda) + pi0) * ( (mu * (pois_tail_cdf  mu (r-1))) - (rd / lambda) * (pois_tail_cdf  mu r))
    
        cost   = cmult  *  (a +
                            (ic * term1 ) +
                            term2
                               )
    
    avg_annual_cost' cp q r = avg_annual_cost q r cp
    

{% endcodeblock %}

Finally, we define the `weighted_cost_fn`.
This is the code equivalent of Eq. $$K(Q,r) = \int g(\tau) K(Q,r; \tau) d \tau$$.
The input to the function is a `q`, `r`, a list of `[(CostParams,Double)]` where each `CostParam` object has an associated weight.
It is interesting to note at this point that Haskell does not have a guaranteed execution order.
Rather, the functions specify data dependencies, and GHC does what it needs to do. 
This function following steps:

1.  Extract list of `CostParams`
2.  Extract list of weights
3.  Partially apply each `CostParam`, creating a list of functions of type `Int -> Int -> Double`. This list is stored in `fns`
4.  For each fn, apply the function to `q` and `r`, multiply by the associated weight, and add them all together.

{% codeblock lang:haskell Weighted Cost Function %}
    weighted_cost_fn :: Int -> Int -> [(CostParams, Double)] -> Double
    weighted_cost_fn q r cp_wt  = wt_cost
      where
        cost_pars = map fst cp_wt
        weights   = map snd cp_wt
        fns       = map avg_annual_cost' cost_pars
        wt_cost   = sum $ zipWith (\fn wt -> wt * (fn q r)) fns weights
    

{% endcodeblock %}

There are a few helper functions to run and print the results.
You can see them in the full code listing. 
Two that are important are 

{% codeblock lang:haskell  %}
    run_and_stringify :: [(Int,Int)] -> CostParams -> [(Double,Double)] -> String -> [String]
    run_and_print :: [(Int,Int)] -> CostParams -> [(Double,Double)] -> String -> String -> IO ()
    
{% endcodeblock %}

The first `qr_list` is defined using a [list comprehension](http://www.haskell.org/haskellwiki/List_comprehension) to get the list of candidate $$[(q,r)]$$ values.
The lists $$[\tau_{i}], [p(\tau_{i})]$$ are created and `zip` creates $$[(\tau_{i} ,p(\tau_{i}))]$$.
A `uuid` is created, and the simulation us run, with the output saved to `prob4-23_data_v2.csv`.

Next, I vary the weights over linear combinations.
`w0list` is defined by multiplying 0.05 to the infinite list $$[0.0,1.0,2.0,...]$$, and keeping the values $$\leq 1.0$$.
`den_list` is a list of lists representing the weights, constructed such that the weights add to 1. This is $$g(\tau)$$.
`tw_list` is a list of list of tuples. `zip leadtime_val` means take the list $$[8,15]$$ and `zip` it with some other list.
The values are `[[(8.0,0.0),(15.0,1.0)]`, `[(8.0,0.05),(15.0,0.95)]`, ..., `[(8.0,1.0),(15.0,0.0)]]`.
The other lists are each possible density.
A list of `uuid` is created.
Finally, the function `run_and_stringify` is run over every element in `tw_list` where the `zipWith` function pairs each element of this list with a `uuid`.
The output of `run_and_stringify` is `[String]`, so mapping over them creates a list of list of strings. 
Since each string contains the `uuid` as a unique identifier, we can concatenate the list of lists together. `concat` does exactly this.
Results are saved in `p4-23-all2.csv`.


## Analysis

{% img /images/hw4-23-p1_default.png %}
The first plot is of the $$K(q,r)$$ for the default value of theta.
We can not only see $$(Q^{*}, r^{*})$$ is $$(18,17)$$, but how the cost function changes as we move away from the optimal value in any direction.


{% img /images/hw4-23-p2.png %}
Having some fun with the analysis, we can look at linear combinations of the weights.
The number on the top of the subplot is the weight on $$\tau = 8$$.
When this value is $$0.0$$,the problem is reduced to the case with known leadtime $$\tau = 15$$.
The results are intuitive; as the weight on $$\tau = 8$$ increases, the average leadtime goes down and the costs decrease.


{% img /images/hw4-23-q_and_r.png %}
Finally, we can look at how the $$(Q^{*}, r^{*})$$ change as the weights change.
This more easily shows the results in the previous plot.

The R code to create the plots is [at this Gist](https://gist.github.com/stephenjbarr/9748022).
The full Haskell code to run the simulations is [at this Gist](https://gist.github.com/9748031).


# Conclusion

We have found the optimal $$(Q^{*}, r^{*})$$ for the default parameterization.
Using Haskell, we coded $$K(q,r; \Theta)$$ to be evaluated for $$\theta_i \in \Theta$$.
We also allowed for arbitrary discrete distributions.
Plots were helpful in showing how the solution changes with changes in the weights of the various possibilities for the leadtime.
