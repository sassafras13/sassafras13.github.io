---
title: Mutable and Immutable Objects in Python
layout: post
---

In my post on [hash tables](https://sassafras13.github.io/HashTables/), I encountered the concept of immutable and mutable data for the first time, and promised to write a post about it in the future. The future is here, and so we’re going to talk briefly about what mutable and immutable data types are in the context of Python. Many thanks to Lorraine Li’s excellent post on the topic which guides this blog post [1].

In our discussion of hash tables, we said that only immutable data structures could be hashed. We glossed over this topic by saying that immutable data is data whose value cannot be changed once it has been created. But today we’re going to go into more detail. 

First let’s explain that in Python, different data types and structures can all be abstracted as objects [1]. Python is an object-oriented language, where objects have properties (i.e. **attributes**) and some functionalities (i.e. **methods**) associated with them [2]. In Python, you can use the methods via the dot syntax [2]. For example, you can use the append method for a list object to add an item to a list like this: 

**list.append(2)

But the surprising thing (at least to me) is that not only are more complex data structures like lists represented as objects - even basic data types like float or int have methods and attributes associated with them [2]. So the assertion that “everything in Python is an object” is actually true, it's not hyperbole [2]. 

Python distinguishes between objects whose value can change - **mutable objects** - and objects whose values cannot change - **immutable objects** [1]. Let’s clarify what we mean by “value” in this context. Every object in Python has a unique ID, a type and a value [2]. The ID is used by Python to retrieve the object from memory when we need it, and it never changes. Similarly, the type of the object never changes - it controls what operations the object can be involved in and what values can be assigned to it [1]. 

But the _value_ of an object in Python can either be changed or it cannot, depending on whether the object is mutable or immutable [1]. For example, an int object is immutable, but a list is mutable - let’s see how that plays out in an example below [1]. 

If we make an immutable int object called “year” and change the value of “year”, what happens? Did we change the value of “year”? Actually, we didn’t, as we can see when we check the ID of “year” before and after changing the value [1]. When we changed the value of “year”, we actually just made the name point to a new location in memory storing the new value (that’s why the ID changed) [1]. 

![Fig 1]({{ site.baseurl }}/images/2021-01-16-MutvsImmut-fig1.png "Figure 1"){:width=25%}       
Figure 1 

But if we make a mutable list object and remove an element from the list, what happens to the ID of the list? As we can see below, the ID of the list stays the same even when we change the value [1]. That’s what makes the list mutable, while the int is immutable. 

![Fig 2]({{ site.baseurl }}/images/2021-01-16-MutvsImmut-fig2.png "Figure 2"){:width=25%}       
Figure 2

Let’s get the breakdown of mutable and immutable data types in Python. 

Mutable data types [1]: 
* Lists    
* Byte arrays (the mutable version of a byte object)    
* Sets (Python has set and frozenset, they are both unordered collections of immutable objects, but only set is mutable)    
* Dictionaries    

Immutable data types [1]:
* Numeric data types (i.e. bool, floats, ints, etc.)
* Strings    
* Bytes    
* Frozen sets (immutable version of a set)        
* Tuples (a sequence of arbitrary Python objects)    

Okay, that was just a quick post, but hopefully it helped clarify some points on mutability and objects in Python. 

## References: 

[1] Li, L. “Immutable vs Mutable Data Types in Python.” Towards Data Science on Medium. 17 Oct 2019. <https://towardsdatascience.com/immutable-vs-mutable-data-types-in-python-e8a9a6fcfbdc> Visited 16 Jan 2021. 

[2] VanderPlas, J. “Basic Python Semantics: Variables and Objects.” A Whirlwind Tour of Python. <https://jakevdp.github.io/WhirlwindTourOfPython/03-semantics-variables.html#:~:text=Python%20is%20an%20object%2Doriented,Python%20everything%20is%20an%20object.&text=In%20Python%20everything%20is%20an%20object%2C%20which%20means%20every%20entity,accessed%20via%20the%20dot%20syntax.> Visited 16 Jan 2021. 
