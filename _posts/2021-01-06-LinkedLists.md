---
title: Linked Lists
layout: post
---

As part of my efforts to learn the basics of computer science (or at least enough for a coding interview), I am trying to build my own linked list implementation in Python. I have been relying heavily on the awesome tutorial by Pedro Pregueiro of Real Python [1], as well as on the overview provided by Gayle Laakman McDowell in her book, Cracking the Coding Interview [2]. In this post, I’m going to give an overview of what linked lists are and why they’re useful, and include some of the code I wrote. 

## What are Linked Lists?

Linked lists are a data structure that represent a sequence of nodes that are not necessarily stored in a contiguous block of memory [1-3]. Each node contains some **data** and a **pointer** to the location in memory of the next node in the sequence, as shown in Figure 1 [1]. The first node is called the **head** and the final node’s pointer is null (or None if you’re being Pythonic) [1]. Linked lists can be either **singly** or **doubly** linked, as demonstrated in Figure 1 [2]. In a singly linked list, each node only points to the next node; in a doubly linked list, each node has pointers to _both_ the previous node and the next node [2]. 

![Fig 1]({{ site.baseurl }}/images/2021-01-06-LinkedLists-fig1.png "Figure 1"){:width=75%}      
Figure 1    

I followed the Real Python tutorial to implement a basic LinkedList class (wrapped around a Node class) in Python 3, and added some additional methods inspired by the challenges given at the end of the tutorial [1]. You can access my code [here](https://github.com/sassafras13/coding-interview/tree/main/linked-lists), and please keep in mind that the code is drawing heavily from the Real Python tutorial [1]. 

## Why are Linked Lists Useful?

It wasn’t obvious to me at first why a linked list would be a useful data structure. At first blush, they don’t seem very powerful - for instance, if you want to access the _k_-th element from within a linked list, you have to iterate through _k_ elements before you find it (i.e. O(n) time) [2]. That’s weak sauce compared to an array or a regular list, where you can access any element in constant time (i.e. O(1) time) [2]. 

There’s no clear advantage to using a linked list from a memory usage perspective either. Linked lists require some additional space to store the pointers in addition to the data [3]. If the data being stored in the linked list is relatively small in comparison to the memory addresses, then linked lists may actually require significantly more memory than a regular list [3]. 

But the advantage to the linked list data structure is that we can easily add or remove items from anywhere in the sequence because we only have to fix the pointers of the adjacent node(s) [3]. Regular lists and arrays are dynamically sized, and so it can take additional time to resize them by restructuring the entire block of memory which contains that list or array (more on this in a future post) [1,3]. But even though the action of adding or removing nodes from a linked list can be done in constant time, the fact that linked lists do not allow for constant time random access means that we will still need to take O(n) time to find the position in the list where we want to add or remove a node [3]. So the only time when it is faster to add or remove nodes to a linked list, as compared to an array or regular list, is when you are adding or removing the head of the list.

When is it helpful to be able to quickly add or remove items from the beginning of a sequence?  When you want to build a queue [1]! Queues follow a first-in, first-out (FIFO) ordering, which means that they form the same way you form a queue to buy movie tickets, as shown in Figure 2 [2]. The first person in line is the first person to buy their tickets, and additional customers line up behind them [2]. So when you need to pull items from a queue, you always pull from the head of a queue, which is easy to do in constant time with a linked list, but requires O(n) time for a regular list [1-2]. 

![Fig 2]({{ site.baseurl }}/images/2021-01-06-LinkedLists-fig2.png "Figure 2"){:width=75%}      
Figure 2 - Source [1]

Okay, I hope you followed me in this post - I’m feeling a little out of my depth as a mechanical engineer. But I will be trying again soon with another post, probably on dynamically sized arrays and lists, or maybe hash tables? We’ll find out!

## References:

[1] Pregueiro, P. “Linked Lists in Python: An Introduction.” Real Python. 01 Apr 2020. <https://realpython.com/linked-lists-python/> Visited 05 Jan 2021. 

[2] Laakmann McDowell, G. Cracking the Coding Interview, 6th edition. 2016. CareerCup, LLC. 

[3] “Linked list.” Wikipedia. <https://en.wikipedia.org/wiki/Linked_list> Visited 05 Jan 2021. 
