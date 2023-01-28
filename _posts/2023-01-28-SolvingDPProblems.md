---
title: A Framework for Solving Dynamic Programming Problems
layout: post
---

In this post we’re going to talk about strategies for solving dynamic programming problems. I have written about [dynamic programming](https://sassafras13.github.io/DynamicProgramming/) and [recursion](https://sassafras13.github.io/recursion/) before, but now I want to focus specifically on how to frame problems as dynamic programming problems, and develop solutions to them. Let’s get started! 

I’ll begin by outlining a framework for solving any dynamic programming problem presented by Otasevic in [1], and then we’ll go through each step in more detail. The steps are [1]: 

1. Determine if the problem can be solved with dynamic programming  
2. Identify the variables in the problem  
3. Define the recurrence relation   
4. Identify the base case(s)    
5. Choose an iterative or recursive solution    
6. Add memoization    
7. Determine the time complexity   

## 1. Determine if the problem can be solved with dynamic programming

The key characteristic of a problem that can be solved with dynamic programming is that it should be possible to split the problem up into smaller subproblems [1]. And in particular, the Principle of Optimality should apply - that is, if we find the optimal solution to one of the subproblems, it should also be optimal in the case of trying to solve the entire problem. Often problems that ask you to maximize or minimize a quantity, or to count the number of possible arrangements are dynamic programming problems [2]. 

## 2. Identify the variables in the problem

In this step, we need to identify which variables are changing as we work through the subproblems [1]. One way to figure this out is to consider a couple of the subproblems and compare them, and see which variables changed from one subproblem to the next [1]. This is also known as defining the **state** in the problem [2]. This exercise will also help us define the subproblem and it also leads us to our next step…

## 3. Define the recurrence relation

The recurrence relation expresses out the subproblems we found above relate to each other [1]. The relation lays out all the possible next steps you can take from your current state, i.e. all the changes you can make to the variable(s) in your problem given where you are now [1]. Another way to think about the recurrence relation is to see that it is the definition of the **state transition** [2]. You can do this in words, or use some math to explain how each variable could be changed [1]. 

## 4. Identify the base case(s)

A base case is a subproblem that you can’t divide up into any smaller subproblems, and that doesn’t depend on any other subproblems for a solution [1]. It often can look like the solution to when your variables are equal to 0 or 1, for example. A good way to think about the base cases is to ask what the constraints are on the variables you identified in step 2 [1]. For example, if the variable must at minimum be equal to 0, what is the solution to the subproblem in that case? 

## 5. Choose an iterative or recursive solution

Although we did talk about a recurrence relation in step 3, you don’t have to use recursion to solve this problem - you could also iterate over the subproblems in order [1]. Another way to think about the different approaches to solving the problem is to ask if you want to solve it top-down (using a memoization approach to save the solutions to the subproblem) or bottom-up (using a tabular form of saving the solutions) [3]. Often recursive solutions correspond to top-down approaches, while iterative approaches correspond to bottom-up methods [3]. Often there will be a way to solve the problem using either method, and likely in an interview setting either solution will do. However, Otasevic points out that in the real world, it might be possible for a recursive solution to lead to a stack overflow, and so it will be good practice to point out to the interviewer that you are aware that this is a possible danger [1]. 

## 6. Add memoization

We talked about this in my previous blog posts - memoization is a way of saving solutions to subproblems so that if the answer is needed again, you can look it up instead of repeating the computation. Often memoization can really help speed up your algorithm implementation - in my own experience, adding memoization usually makes the difference between failing and passing a Leetcode problem. In Python it can be tempting to just use the ```lru_cache``` decorator to automatically include memoization, but I try to manually include it to teach myself how to implement it on a variety of problems. Otasevic suggests that you memoize anything you are about to return at the end of a function call, and to always check for the memoized solution before performing the computation [1]. 

## 7. Determine the time complexity

Okay, to be honest with you, I am usually very lazy and do not do this when I am Leetcoding, but you probably will want to do it in an actual interview context. Of course it is really important to know the time complexity of your algorithm because that will inform how well it will perform in the real world, and help you to look for other ways to speed up your algorithm. Otasevic recommends computing the time complexity using the following method [1]: 

1. Count the number of states, which usually depends on the number of variables you identified.     
2. Determine the work done in each state   

Now that we have seen a framework for solving dynamic programming problems, let’s apply it to a classic problem, the Knapsack Problem. 

## The Knapsack Problem

The Knapsack Problem is a well-known problem that can be solved using dynamic programming. In the problem, you are given a knapsack that has the capacity to carry up to ```W```units of weight. You need to find the set of ```n``` items, each of which has a value ```v``` and a weight ```w```, that maximizes the total value of the objects in the knapsack, while not exceeding its weight capacity [4]. 

There are many variants of the problem - versions where you can break the objects, or where volume is also an issue, etc. - but the one we will consider here is called the 0-1 variant, where you can either put the entire item in the knapsack or you omit it, but you cannot break it up [4]. Now that we’ve defined the problem, let’s work through our framework. (While I am making my own notes here, I did refer to several solutions presented in [4] to help me fill out the framework.)

### 1. Can the problem be solved with dynamic programming? 

Okay, so I guess I’ve given this one away a little bit, but we can also see that this problem could be broken into subproblems where I have to determine if each object should be added or not. You could also note that it is an optimization problem, which makes it a more likely candidate for a dynamic programming approach. 

### 2. What are the problem variables? 

As given in the problem description, there are 2 variables that we are concerned with here: **weight** and **value**. Any time we add an object, we will know its weight and value, and we will need to decide if adding it will maximize the total value of our knapsack without exceeding its weight capacity. 

### 3. What is the recurrence relation?

Since this is the 0-1 variant, I know that I only have two actions I can take for each object: I can either add it to the knapsack, or discard it. What kind of criteria would I use to make my choice? I want to add the object if it maximizes my total value. 

### 4. What is the base case? 

As we saw in step 2, I have two variables, weight and value. I am keeping track of the total weight and total value of my knapsack. The total weight is allowed to be in the range ```[0, W]``` where ```W``` is the capacity of the knapsack. The total value of my knapsack can range from ```[0, infinity]```. I should also note that I am only allowed to add each object once, so if I have already added all the objects then I can’t add any more. This leads me to my base cases: 

* If my total weight exceeds the knapsack capacity, then I cannot add the object and my additional value is 0.   
* If there are no objects left, then I cannot add any more, and again my additional value is 0.   

### 5. Should I use an iterative or recursive approach? 

Looking back at step 3, I said that I would add an object to the knapsack if it maximized the total value. But I don’t know if adding that object will maximize the total value unless I know that it is part of the optimal set of objects and that every subsequent object I add is also part of this set. But if I frame my solution as a recursive function, then each level in the recursion will only add the object in question if it _does_ maximize the total value optimally. So let’s use a recursive approach here. 

### 6. How to memoize my solution? 

Since my goal is to maximize the total value, I can store the computed total value of every subproblem in order to speed up my algorithm. More specifically, I will want to store the total value of adding each object at every step of the algorithm - this is 2 axes, so I will want a 2D table for storing my total values, where ```table[index][weight]``` stores the value of all the objects in the knapsack at the step ```index``` and for the total weight ```weight``` at that step. 

### 7. What is the time complexity? 

Following the guidance Otasevic gave us, let’s first count the number of subproblems. This is easy in this case, because we are filling out the table of size ```index``` x ```weight```, where ```index``` can have a maximum value equal to the number of objects, ```n```, and the maximum weight is equal to the knapsack’s capacity, ```W```. 

How much work do we have to do at each step? We just do one computation - we select the maximum value computed with or without adding the next object. This is constant time, so in total our algorithm is $\mathcal{O}(n \cdot W)$ time. 

With that, I will close this blog post and keep working on solving dynamic programming problems! I might return in the future with a selection of solutions that are interesting to explore in more detail. Thanks for reading. 

## References 

[1] Otasevic, N. “Follow these steps to solve any Dynamic Programming interview problem.” Free Code Camp, 6 Jun 2018. <https://www.freecodecamp.org/news/follow-these-steps-to-solve-any-dynamic-programming-interview-problem-cc98e508cd0e/> Visited 28 Jan 2023. 

[2] Kumar, N. “How to solve a Dynamic Programming Problem?” GeeksforGeeks, 10 Jan 2023. <https://www.geeksforgeeks.org/solve-dynamic-programming-problem/> Visited 28 Jan 2023. 

[3] Gavis-Hughson, S. “How to Solve Any Dynamic Programming Problem.” Learn to Code with Me, 12 Apr 2020. <https://learntocodewith.me/posts/dynamic-programming/> Visited 28 Jan 2023. 

[4] Thelin, R. “Demystifying the 0-1 knapsack problem: top solutions explained.” Educative, 8 Oct 2020. <https://www.educative.io/blog/0-1-knapsack-problem-dynamic-solution> Visited 28 Jan 2023. 
