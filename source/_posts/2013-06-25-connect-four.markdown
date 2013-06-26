---
layout: post
title: "Connect Four, Kids, and Math"
date: 2013-06-25 13:06
comments: true
categories: games, kids, math
---

<!-- # Connect Four and Math for Children -->


## Introduction

My 13-month old son Jeffrey and I were recently at a cafe and I saw a copy of [Connect Four](http://en.wikipedia.org/wiki/Connect_Four). Although Jeffrey is not old enough to understand the rules or strategy, we were able to thoroughly enjoy the game in our own way. I was thinking about the game, and I realized that you could teach quite a few mathematical concepts using this game.

## Matrix Concepts

Connect Four is played on a grid with 6 rows and 7 columns. Each cell can be empty, contain a <font color="red">red</font> token, or contain a black token. Right away, there are some important concepts here:

+ Cell: a particular value in the grid. Cells are *indexed* by the combination of a row number and a column number.
+ Row: a horizontal collection of cells.
+ Column: a vertical collection of cells.
+ Coordinates: for a particular cell, the row number and column number where that cell is located.
+ Vector: either a row or column.
+ Matrix: a two-dimensional grid of cells.

Once we have the concepts of *row*  and *column*, we can talk about concepts such as *full* or *empty*. We can also think about things such as *capacities*. A column has a capacity for 6 tokens, and a row has capacity for 7 tokens. If there a column which has 4 of the 6 slots filled, that column as a *remaining capacity* of two tokens. This teaches the equivalence of the concepts of subtraction and remaining capacity.


## Probability Concepts

+ Random variable: a value that you do not know until you play the game. For example, for the cell in row 3, column 5, in a given game what will it be? 
+ State space: The collection of values that a cell can take. For the purpose of Connect Four, the state space is the set { empty, <font color="red">red</font>, black }, because those are the only values a cell can take.


These are very important concepts that any player of Connect Four would easily grasp, and yet are very important mathematical ideas. Children naturally experiment, and experiments lead to random outcomes. We can call this outcome a *random variable*. For any experiment, even before conducting the experiment, we can think about the possible values of this experiment. 

### Real Experiments and State Spaces

+ Counting a handful of beans will yield a *natural number*. A natural number is any number that can occur as a result of counting. We can equivalently say that the result of counting is a *random variable* and that the state space of this variable is the *set of natural numbers*.
+ Taking a temperature will yield a *real number*. Any measured value that can take on decimal values is a *real number*. When we walk outside and read the temperature, we can say that today's temperature is a random variable and the state space of this random variable is the *set of real numbers*.
+ In Connect Four, before we play a game we can pick a particular cell. We can write down the coordinates of the cell and make some guesses as to what the value of cell will be. Thus, the value of this cell is a random variable and the state space of this cell is the set { empty, <font color="red">red</font>, black }.


## Abstract Representations

Assume that we are in the middle of a very intense Connect Four game. We are in a cafe, but the cafe is closing and we wish to continue the game at home. We need to pack up the game so that we do not lose the tokens on the train ride home. In order to do this, we need to save the state of the game. How can we do this?

One thing we can do is draw a grid on a piece of paper. We only have a green pen with us, so we cannot use colors to differentiate the tokens. We could write the words "empty", "<font color="red">red</font>", and black" in each cell. This would quickly become very cumbersome. To make this faster, we can define the set of equivalences:

+ 0 = empty
+ 1 = <font color="red">red</font>
+ 2 = black

What we have defined is a two-way equivalence between two sets. In math, this is called a *bijection*. The idea that this teaches is that there are multiple representations of the same idea. One way to represent the Connect Four game is with the physical game pieces. Another way is with a matrix on paper with the numbers {0,1,2}. In fact, if you were on a train or airplane and did not have a physical Connect Four game with you, you just as easily and *equivalently* play on paper, provided you chose the right moves (ones that did not violate gravity). The concept of equivalent abstract representations is extremely powerful and leads to a limitless set of possible insights about how math and ideas work.


## Action Spaces

In the last section, we discussed the idea of playing Connect Four on paper. In the physical game of Connect Four, when you insert a token, gravity pulls it into place. On paper Connect Four, you could write values in cells that are gravitationally impossible. Assume that player A is using the <font color="red">red</font> tokens, and player B is using the black tokens. When it is player 1's turn, there is a finite set of moves that are possible in Connect Four. To determine this set of moves, we can use the following process.


```

action_space = empty list;

For each column:
   If the column is full:
	   Go to next column
   
   Otherwise:
       Start at top cell.
       While cell is empty and not the bottom cell:
	      Go downwards
	   When the above condition no longer holds, add cell to action_space list.


```

Following the steps above will yield all of the possible moves that any player can make. The set of possible moves is called the *action space*. We can see that for Connect Four, the set of moves that can be made depends on the state of the game. The action space is *state dependent*. 

The process of systematically enumerating every choice available is an important part of the decision making process. Children can get flustered when there are too many choices, and having the words and ideas for being able to list each possible decision will aid their desire to be rational.


## Algorithms 

Another important idea is the idea of an *algorithm*. It is a very intelligent-sounding word for a child. An algorithm is a list of steps that solves a problem. Having an algorithm is very empowering, because it means that, for a given certain problem, a solution is attainable, regardless of the details of that problem. Developing an algorithm to a problem feels extremely satisfying because, rather than solving an *instance* of a problem, you can solve **any possible problem**.

## Strategy and Policy Functions

Games often have strategies. A *strategy* is essentially the same as an algorithm, in that it is a general approach to solving the problem. More specific than a strategy is that of a *policy function*. To talk about what that is, we need to talk about functions in general. A function is the transformation of an input into an output. A good example of a function is "+1". You can tell a young kid to give you the "next" number, then when you say "1", they say "2". You say "2", they say "3", etc. The idea is that, for an input, there is a certain output.

A policy function says, "for any configuration of the board, what is your move?" Once you have the hang of the game down, it would be interesting to think about and actually write down what the strategy is. Then, try writing it down and playing strictly by that strategy. One strategy could be simply "attempt to fill the columns, left to right". A slightly better strategy would be "attempt to block if possible, otherwise fill from left to right".

One you and your child each have a written strategy, a fun exercise would be to play according strictly according to the strategy. You can play 10 games mechanically, and see which strategy wins. Is there one strategy that wins a majority of the time? Is there a strategy that wins every single time. 

+ Dominant strategy: A strategy that wins more often than loses.
+ *Strictly* dominant strategy: A strategy that wins every single time.


## Future ideas 

There are many many more concepts that can be represented with Connect Four. Linear algebra, clustering, and more! All of the concepts in this article should be familiar and accessible to any player of Connect Four, and the goal was to demonstrate that these concepts have well-developed mathematical underpinnings. My highly-intelligent three-year-old nephew will be receiving a copy of Connect Four soon, and I am looking forward to seeing what he thinks about this.

