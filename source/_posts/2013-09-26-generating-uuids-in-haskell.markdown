---
layout: post
title: "Generating UUIDs in Haskell, and a Usage Scenario"
date: 2013-09-26 15:43
comments: true
categories: haskell
---
If you need to generate a list of UUIDs in Haskell, here is one way to do it.

``` haskell

module Main where

import Data.UUID as U
import qualified Data.UUID.V4 as U4
import Control.Monad

main :: IO ()
main = do
  uuids <- replicateM 10 ( do
     y <- U4.nextRandom
     return (U.toString y))

  mapM_ putStrLn uuids


```

You can then run this with ```runhaskell```. 

``` bash

stevejb@ursamajor:~/Projects/haskell_fun$ runhaskell uuid2.hs 
2ddcb1fd-9b61-4805-a1f0-0f3d05716186
6be0f977-43b5-4f01-8783-9609aa07ee4b
3a61864b-6266-4620-8432-8e1ab9ba1d29
507f43e4-4e61-4ccd-b142-f55236210b55
94b02f4f-a8a5-4ea0-b868-345997adbf39
b84ed5da-46ff-41ff-80e4-c2ad1fdb5e0b
76289793-5124-4fee-bb84-e61c08ece7c4
026288b9-65a0-49fb-9717-d810e7846e43
a79b858d-d7c5-4ead-b7c0-3edf105ac16b
ac2aa9b0-c4ee-454c-969d-22ac90ba1eb9

```

These are UUID4's, meaning that they are random, in this case using the OS's source of randomness. I typically use UUID's uniquely identify parameterizations, when I am running the same model with multiple parameterizations. Each file produced by the model is prefixed by the particular uuid. For example, run ```ac2aa9b0-c4ee-454c-969d-22ac90ba1eb9``` may correspond to $$\theta = 3, \alpha = 2.5$$. Files related to this run could be 

+ ```ac2aa9b0-c4ee-454c-969d-22ac90ba1eb9___optimal-policy.mat```
+ ```ac2aa9b0-c4ee-454c-969d-22ac90ba1eb9___simulation.mat```
+ ```ac2aa9b0-c4ee-454c-969d-22ac90ba1eb9___runlog.csv```

The advantage of this approach is that you can dump all of the result files into one folder (such as an S3 bucket) without managing indices or dealing with collisions. For more information, see [my more thorough description of this type of workflow](/blog/2012/05/20/my-workflow-for-running-jobs).
