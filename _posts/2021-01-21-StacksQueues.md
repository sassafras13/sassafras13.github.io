---
title: Obligatory Stacks and Queues Post
layout: post
---

Okay, today we are going to talk about a really fundamental pair of concepts: **stacks** and **queues**. I have written about these before in the [linked lists](https://sassafras13.github.io/LinkedLists/) post but there are more details to their runtimes and when they’re useful that I wanted to cover today. As always, if you’re interested in implementing these abstract data structures in code, feel free to look at [this code](https://github.com/sassafras13/coding-interview/tree/main/stacks-queues) that draws from a tutorial by [1]. 

## Stacks

You can think of a stack as a pile of dinner plates, where the most recent plate added to the stack is the first one that you remove when you’re serving dinner [2]. We call this **Last In, First Out (LIFO)** access [2]. It can be advantageous to store data in such an order in certain situations, such as when you are implementing recursive algorithms and you want to push data onto the stack temporarily, then remove it as you backtrack up through the recursion [2]. 

There are some basic operations that you should be able to perform on a stack, including [2]: 

1. **pop()**: remove the top item from the stack.    
2. **push(item)**: add an item to the top of the stack.    
3. **peek()**: return the top of the stack (without removing it).   
4. **isEmpty()**: return True if and only if the stack is empty.   

For these operations, the pop() and push() methods will run in O(1) time because there is no need to move elements around (as there might be for [dynamically resized lists](https://sassafras13.github.io/DynamicResizeLists/)) [2]. However, a stack will not be very time efficient if you want to access the i-th element in the stack; you would be better off using some form of list or array instead [2]. Stacks are commonly used in language parsing, runtime memory management and depth-first search (there is a post coming on this last algorithm, I promise) [3]. 

If you want to implement a stack in Python, there are several ways to do this, and the easiest way is using a simple list [3]. If you recall from our discussion of [dynamically resized lists](https://sassafras13.github.io/DynamicResizeLists/), the _amortized_ runtime of resizing a list is O(1), so pushing and popping (which might require a resizing operation) will happen in _amortized_ O(1) time, satisfying the requirement for a stack [3]. Even better, a list will not only give O(1) time access to the item at the front of the stack, but they will also give O(1) random access time [3]. However, because the cost of resizing a list can be much greater than O(1) for a single event, the list-based implementation of a stack can be a little less robust, and more volatile, than other implementations using specialized Python modules like collections.deque [3]. 

One interesting thing to keep in mind if you are implementing a stack in Python with a list is which end of the list is considered the “front” [3]. The best practice is to consider the end of the list as the “front,” and items should be pushed and popped from this location [3]. This allows the list (or stack) to grow towards higher indices, and decrease towards lower indices in the list [3]. It also results in much better performance than if the beginning of the list were considered the “front” - in this case, you would need to shift all the items in the list _every time_ you pushed or popped, which would not be time efficient at all [3]. 

## Queues 

In contrast to stacks, queues can be modeled by a line at the movie theater (okay, I know we’re 11 months into a pandemic, so please cast your mind back to when we could go to movie theaters) [2]. The first person in line at the theater is the first person who gets to buy tickets - in other words, **First In, First Out (FIFO)** [2]. Unlike a stack, where we remove items in reverse order to when they were added, in a queue, we _do_ remove items in the _same_ order as they were added [2]. 

Some common operations for a queue are very similar to those for a stack [2]: 

1. **add(item)**: add an item to the end of the queue (also called **enqueue**).   
2. **remove()**: remove the first item in the queue (also called **dequeue**).   
3. **peek()**: return the top item in the queue, without removing it.  
4. **isEmpty()**: return True if and only if the stack is empty.   

Queues can be implemented specifically with _linked_ lists, because this data structure allows for easy access to both the front and back of the list so that both the add(item) and remove() methods run equally efficiently in constant time [2].*1 By contrast, a regular list is a terrible way to implement a queue because adding or removing items from the front of the list would require shifting all the items in the list, as discussed above [3]. In either case, queues are typically _not_ used for random access to their elements [3]. We often use queues in breadth-first search (there is a post coming on this topic), in scheduling, or in implementing a cache. 

Thanks for reading this post on stacks and queues. The material discussed here is fairly straightforward, but I wanted to set the foundation now because, as I hinted in the body of this post, we are going to revisit these data structures when we discuss traversing graphs with depth-first search and breadth-first search. 

## Footnotes: 

*1 Laakmann McDowell reminds her readers that it can be very easy to mess up the process of updating either the first or last nodes in a queue. This is important if you’re preparing for a coding interview. 

## References: 

[1] Kojin. “Data structures in Python, Series 2: Stacks/Queues.” Medium. 25 Nov 2016. <https://medium.com/@kojinoshiba/data-structures-in-python-series-2-stacks-queues-8e2a1703d67b> Visited 21 Jan 2021. 

[2] Laakmann McDowell, G. Cracking the Coding Interview, 6th edition. 2016. CareerCup, LLC.

[3] Bader, D. “Common Python Data Structures (Guide).” Real Python. 26 Aug 2020. <https://realpython.com/python-data-structures/#stacks-lifos> Visited 21 Jan 2021. 
