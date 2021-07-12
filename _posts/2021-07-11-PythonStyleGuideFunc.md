---
title: Google's Python Style Guide Part 1 - Functionality
layout: post
---

I just recently learned that Google published a style guide for how their developers write clean code in Python [1]. I wanted to use a couple of posts to outline some of the things I learned from that style guide. I will write this post to describe some of the functional recommendations given in the style guide, and a follow-up post will detail some of the specific style requirements Google listed. Let’s get started!

## Google’s Python Style Guide - Functional Recommendations

The first half of Google’s style guide focuses on best practices for using different functionalities within Python. I should note that there are more recommendations than I am giving here - I have selected the items that were relevant to aspects of Python that I already use or want to use more frequently. I would highly recommend glancing through the style guide yourself if you want a more complete picture of Google’s recommendations. But for now, here is what I thought was important [1]: 

**Use a code linter.** A code linter is a tool that looks at code and identifies possible errors, bugs or sections that are poorly written and could contain syntax errors [2]. Google recommends using a Python library like pylint to check your code before deploying it.    

**Use import statements for packages and modules but not individual classes or functions.** I think this recommendation helps with namespace management - if you are only importing complete packages/modules, then we will always be able to trace back specific classes or functions to those libraries (i.e. we know that module.class is a class that belongs to “module”). This practice also helps prevent collisions (i.e. having multiple functions with the same name).    

**Import modules by full pathname location.** This is important for helping the code to find modules correctly. Google recommends writing this: 

`from doctor.who import jodie`

Instead of writing this: 

`import jodie`

**Use exceptions carefully.** Usually exceptions are only for breaking out of the flow for specific errors and special cases. Google recommends using built-in exception classes (like KeyError, ValueError, etc. [3]) whenever possible. You should try to avoid using the “Except:” statement on its own because it will catch too many situations that you probably don’t want to have to handle. On a similar note, try to avoid having too much code in a try-except block and make sure to always end with “finally” to make sure that essential actions are always completed (like closing files).   

**Do not use global variables.** Global variables can be variables that have scopes including an entire module or class. Python does not have a specific datatype for constants like other languages, but you can still stylistically create them [4], for example by writing them as _MY_CONSTANT = 13. The underscore at the beginning of the variable name indicates that the variable is internal to the module or class that is using it. 

**It is okay to use comprehensions and generators on simple cases, but avoid using them for more complicated situations.** Comprehensions*1 and generators*2 are really useful because they do not require for loops, and they are elegant and easy to read. They also do not require much memory. However, complicated constructions of comprehensions/generators can make your code more opaque. Generally, Google recommends using comprehensions/generators as long as they fit on one line or the individual components can be separated into individual lines.   

**Use default iterators and operators for data types that support them.** Some data types, like lists and dictionaries, support specific iterator keywords like “in” and “not in.” It’s acceptable to use these iterators because they are simple, readable and efficient, but you want to make sure that you do not change a container when you are iterating over it (since lists and dictionaries are [mutable objects](https://sassafras13.github.io/MutvsImmut/) in Python).   

**Lambda functions are acceptable as one-liners.** Lambda functions define brief functions in an expression, such as [7]: 

`(lambda x: x + 1)(2) = 2 + 1 = 3`

They are convenient but hard to read and debug. They also are not explicitly named, which can be a problem. Google recommends that if your lambda function is longer than 60 to 80 characters, then you should just write a proper function instead. 

**Default argument values can be useful in function definitions.** You can assign default values to specific arguments to a function. You always want to place these parameters last in the list of arguments for a given function. This is a good practice when the normal use case for a function requires default values, but you want to give the user the ability to override those values in special circumstances. One downside to this practice is that the defaults are only evaluated _once_ when the module containing the function is loaded. If the argument’s value is _mutable_, and it gets modified during runtime, then the default value for the function has been modified _for all future uses_ of that function!*3 So the best practice to avoid this issue is to make sure that you do not use mutable objects as default values for function arguments.  

**Use implicit false whenever possible.** All empty values are considered false in a Boolean context, which can really help with improving your code readability. For example, this is how we would write an implicit false: 

`if foo: …`

This is the explicit version, which is not as clean: 

`if foo != [ ]: …`

Not only is the implicit approach cleaner, it is also less error prone. The only exception is **if you are checking integers**, when you want to be explicit, i.e.: 

`if foo == 0: …`

In this case you want to be clear about whether you want to know if the integer variable’s value is zero, or if it is simply empty (in which case you would use “if foo is None”). Also, remember that empty sequences are false - you don’t need to check if they’re empty using “len(sequence)”. 

**Annotate code with type hints.** This is especially good practice for function definitions. It helps with readability and maintainability of your code. It often looks like this: 

```(python3)
def myFunc(a: int) -> list[int]: 
	…
```

That is all for today’s post on functional recommendations in Google’s Python style guide. Next time, I will write more specifically about the stylistic recommendations that Google provides for coding in Python. Thanks for reading! 

## Footnotes

*1 Comprehensions are a tool in Python that let you iterate over certain data types like lists, sets, or generators. They can make your code more elegant, and allow you to generate iterables in one line of code. The syntax for a comprehension looks like this [5]: 
`new_list = [expression for member in iterable]`

*2 Generator functions are useful for iterating over really large datasets. They are called “lazy iterators” because they do not store their internal state in memory. They also use the “yield” statement instead of the “return” statement. This means that they can send a value back to the code that is calling the generator function, but they don’t have to exit after they have returned, as in a regular function. This allows generator functions to remember their state. In this way generators are very memory efficient but allow for iteration similar to comprehensions [6]. 

*3 This happened to a classmate of mine once, and he said it almost ruined a paper submission for him. This is covered in detail in [8]. 

## References

[1] “Google Python Style Guide.” <https://google.github.io/styleguide/pyguide.html> Visited 11 Jul 2021. 

[2] Mallett, E. E. “Code Lint - What is it? What can help?” DCCoder. 20 Aug 2018. <https://dccoder.com/2018/08/20/code-lint/> Visited 28 Jun 2021. 

[3] “Built-in Exceptions.” The Python Standard Library. <https://docs.python.org/3/library/exceptions.html> Visited 11 Jul 2021. 

[4] Hsu, J. “Does Python Have Constants?” Better Programming on Medium. 7 Jan 2020. <https://betterprogramming.pub/does-python-have-constants-3b8249dc8b7b> Visited 11 Jul 2021. 

[5] Timmins, J. “When to Use a List Comprehension in Python.” Real Python.  <https://realpython.com/list-comprehension-python/> Visited 11 Jul 2021. 

[6] Stratis, K. “How to Use Generators and yield in Python.” Real Python. <https://realpython.com/introduction-to-python-generators/> Visited 11 Jul 2021. 

[7] Burgaud, A. “How to Use Python Lambda Functions.” Real Python. <https://realpython.com/python-lambda/> Visited 11 Jul 2021. 

[8] Reitz, K. “Common Gotchas.” The Hitchhiker’s Guide to Python. <https://docs.python-guide.org/writing/gotchas/> Visited 11 Jul 2021. 
