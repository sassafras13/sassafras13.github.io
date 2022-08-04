---
title: Dynamic Programming
layout: post
---

I’m getting back into the groove of studying for a coding interview, and I recently came across a question that requires knowledge of **dynamic programming** to solve it. I’ve touched on dynamic programming before, both in the context of [optimal control](https://sassafras13.github.io/Silver3/) and when discussing [message passing algorithms over graphs](https://sassafras13.github.io/MessagePassingAlgos/). This post will review that material and also give a more computer science-focused introduction to the topic. 

## What is Dynamic Programming?

The term dynamic programming*1 is used to describe an approach that breaks problems up into overlapping sub-problems before solving them. In order for this approach to be valid, we must be sure that the **Principle of Optimality** applies. The Principle of Optimality says that if the solution to every sub-problem is optimal, then the overall solution built on those components is _also_ optimal [1, 4]. Basically, this Principle is the thing that ensures breaking the problem into pieces is an acceptable approach. 

One example of a problem that can be solved using dynamic programming is computing entries in the Fibonacci sequence. As shown in Figure 1, if we want to compute the fourth entry in the sequence, we can do this by breaking up the problem into computing the earlier entries following a tree structure. 

![Fig 1]({{ site.baseurl }}/images/2022-08-02-DynamicProgramming-fig1.png "Figure 1"){:width=75%}   
Figure 1 - Source [4]

Dynamic programming can refer to a range of different solutions, and we can broadly categorize them into 2 types of solutions: **top-down** and **bottom-up** solutions [2]. A top-down solution is essentially the same as [recursion](https://sassafras13.github.io/recursion/) - we break the problem into sub-problems recursively and then solve those sub-problems. Conversely, the bottom-up approach solves the smallest sub-problem first and builds those up into a complete solution [2]. Typically, recursion is not time or memory-efficient as compared to bottom-up approaches [3]. We will explore these two approaches in more detail in the next sections. 

## Top-Down Dynamic Programming

The top-down, recursive approach to solving a problem like the Fibonacci problem is shown in Figure 1. Here, we continue to call the ```Fib( )``` function until we reach the base case, then we use the base case solutions to solve the other intermediate function calls. We can see this in the code below [4]:

```python
def callFib(n):

	if n < 2: 
		return n
	return callFib(n-1) + callFib(n-2)

def main():
	callFib(5)
```

As we said above, vanilla top-down dynamic programming is not very efficient. If you look at the recursion tree in Figure 1, we make the same call to ```Fib(1)``` three times, for example. To save time, we can use a technique called **memoization** *2 to store the value of ```Fib(1)``` so that we don’t waste time computing it multiple times [4]. 

## Bottom-Up Dynamic Programming

In bottom-up dynamic programming, we identify the smallest sub-problem and use it as a starting point to solve progressively larger sub-problems until we’ve arrived at the solution that we needed. In the case of solving for the n-th entry in the Fibonacci sequence, we start by solving ```Fib(0)``` and work up to solving ```Fib(n)```. We store every entry in a table along the way, so we often say that we are using **tabulation** to store the solutions to the sub-problem [4]. 

Notice that having the table then makes solving for other entries in the sequence much easier, because we can now just look up the solutions in the table rather than re-computing the solution as we would do with recursion [4]. 

In general, memoization is easier to code than tabulation, because memoization essentially uses a wrapper function to save intermediate values, as follows [4,5]:

```python
def memoizeCallFib(n):
	memo = [-1 for x in range(n+1)] 
	return callFib(memo, n)

def callFib(memo, n):
	if n < 2:
		return n
	
	if memo[n] >= 0:
		return memo[n]

	memo[n] = callFib(memo, n-1) + callFib(memo, n-2)
	return memo[n]

def main():
	memoizeCallFib(5)
```

However, note that the memoization approach requires saving a new entry in an array on every call - if we are using a top-down approach to go through many, many layers of sub-problems, then this can be a huge slow-down in the algorithm. The memo stack will also grow to be extremely large, proportional to the number of sub-problems we solved. At this point, using a tabulation approach may be better because the table will save the sub-problem solutions once, efficiently. In some cases, it will be more complicated to code these table-based methods because we have to define the table architecture and how we’ll find entries in a certain order, but the hard work may pay out in the long run [5]. 

## Footnotes

*1 According to David Silver, the term dynamic programming comes from the following: the word “dynamic” implies that the problem we are solving has some sequential or temporal nature to it, and the term “programming” here is meant in the mathematical sense. That is, we are writing a “program” or a “policy” for solving a problem, not a computer program [1]. 

*2 The value of ```Fib(1)``` is the “memo” that we store in “memoization”. 

## References

[1] Silver, D. “RL Course by David Silver - Lecture 3: Planning by Dynamic Programming.” YouTube. 13 May 2015. <https://www.youtube.com/watch?v=Nd1-UUMVfz4&list=PLqYmG7hTraZDM-OYHWgPebj2MfCFzFObQ&index=3> Visited 08 Aug 2020.

[2] “Dynamic programming.” Wikipedia. https://en.wikipedia.org/wiki/Dynamic_programming Visited 13 July 2022.

[3] “Improving efficiency of recursive functions.” Khan Academy. <https://www.khanacademy.org/computing/computer-science/algorithms/recursive-algorithms/a/improving-efficiency-of-recursive-functions> Visited 11 Jan 2022.

[4] “Grokking Dynamic Programming Patterns for Coding Interviews.” Educative.io. <https://www.educative.io/courses/grokking-dynamic-programming-patterns-for-coding-interviews/m2G1pAq0OO0> Visited 3 Aug 2022. 

[5] Bee. “What is Dynamic Programming with Python Examples.” Skerritt.blog. 31 Dec 2019. <https://skerritt.blog/dynamic-programming/> Visited 3 Aug 2022. 
