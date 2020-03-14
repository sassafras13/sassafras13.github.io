---
layout: post
title: Lessons in Writing Code, Part 1
---

Originally I had intended to make this a single blog post but as I was making notes I decided that there was too much information for one article. Instead, I will break up my lessons learned into two categories: general information on how to write good, robust code, and more specific approaches for implementing algorithms (especially those drawn from academic papers) and how to debug them. This post will start with some general lessons on how to write good code. Please note that I am not a trained computer scientist so I welcome your thoughts on the topic if you have different opinions or better ideas!

I read a couple of sources as I was learning this, and they seemed to agree on some overarching themes of what good code looks like. I will start by addressing ideas of **readability**, **modularity**, and **error-handling**, and finish by adding some miscellaneous thoughts. Just to set this discussion in context, I learned these lessons whilst trying to build 3 pieces of code: (1) a dynamics model of a swimming robot, (2) a model of a magnetic field produced by a pair of Maxwell coils and (3) an implementation of the first order solution to the switching time optimization algorithm. The dynamics model I wrote myself, but the model of the magnetic field and the switching time optimization algorithm drew heavily from published works by other researchers so I had a good baseline to work from. All my code was written in MATLAB.

The two sources that I will be citing repeatedly here are from an expert in coding in MATLAB [1] and from the Software Carpentry team [2] which is a fantastic resource for how to code well in many languages. 

## Readability

Code should be readable if you want other people to use it - or if you intend to use it again later after a long hiatus where you will inevitably forget everything important about how the code works. 

### (1) Pick a variable naming scheme and stick to it. 

At the lowest level, make the variable names easy to remember and easy to understand. Be consistent in naming and formatting (it is easier to spot mistakes if they throw off the consistency of the code). Consider adopting a specific capitalization case and using it consistently. The 4 primary options are [3]: 
camelCase
PascalCase
snake_case
kebab-case 

### (2) Keep every piece of code short so the user can remember everything. 


On a higher level, keep the scripts as short as possible, and write functions as often as possible. Then name those functions something obvious so that your reader can read through the high-level approach and understand what’s going on, then dig down into the functions to see the low-level stuff. A note about writing functions: [1] recommends writing functions as their own files if you are going to use them more than once. If you are only going to use a function once, write it as part of the file that calls it. (There is still an advantage to writing that function, because it should make the main code more readable.) 

I wrote separate scripts that just plotted everything, I had scripts that ran simulation cases and optimization cases, and then I had scripts that plotted those outputs. This helped me to create (and recreate) images for the paper, and to unify the formatting of all the images because they were all in the same script. I’m sure there are opportunities for more streamlining there too. But the general idea was that each script served one specific purpose, and the filenames helped me determine which ones I needed to open at a given time. 

### (3) Random tips.

Keep lines readable by using the ellipsis to break up long lines. 

Use comments and headers for all the functions you write, whether they have their own files or they are embedded in another file. In MATLAB, your function documentation will come up when a user hovers over it, which is handy. Software Carpentry [2] recommends writing comments that explain the interfaces (which the user will be interacting with) and also document the reasons why things are done a certain way. DO NOT write comments that explain how you implemented the code - that should be obvious [2]. 

A note on being obvious in your code - there are times when you may get too clever with your code and it will become difficult for others to understand what you’re doing. (An example of this is automating the generation of variables instead of explicitly defining them - that’s generally not recommended.) If you’ve reached that point, you may want to consider keeping the code simple, even if it makes it longer. Code is written for humans, not machines, so while it should be efficient, it should also be readable [1]. 

## Modularity

Making your code modular is extraordinarily important in writing good code. Every line of code you write runs the risk of containing a bug, so the fewer lines of code you write, the fewer lines you have to debug. We make our code as modular as possible so that we can write code by assembling smaller blocks of code that we have already debugged and validated. It also means putting the data you need into convenient structures that you can use throughout one script and across multiple scripts. 

### (1) Use the data structures MATLAB has provided and optimized for. 
Maximize your use of matrices and vectors, they are what MATLAB is optimized for and if you use them well then your MATLAB code will run at speeds comparable to what you would have achieved if you had written in C++ (the underlying language of MATLAB) [1]. I used cell arrays, multidimensional arrays (up to 3) and vectors to store my data and pass from function to function. I would like to think more about which approach was best. I am now playing with tables on a new project and I am beginning to think there are different answers for different applications. 

Something I am particularly confused about was using cell arrays - I think they were largely a cheat I used when I needed to store variable-sized data. I call it a cheat because code is always faster if you pre-allocate memory for each variable; it is bad practice to have varying-size variables. Is there a better way to do this? Again Robbins [1] says to use structures of arrays, not arrays of structures - I should look into whether cell arrays are valuable or not, and how to handle variable size data…

### (2) Do not duplicate data entry. 

I used separate scripts to initialize my variables. Robbins [1] recommends reading in csv files so non-coders can edit them. I’m anticipating everyone who uses my code is going to be MATLAB savvy so I just put it in a separate script so I can reuse values across scripts. It also keeps my main scripts readable (see above).

## Error-Handling

This is something that I am just starting to explore, because most of the projects I have coded have been one-off tasks that changed a lot as I wrote them. I felt that the turbulent condition of my code meant that adding error-handling would be a waste of time. But I found that as my code base matured through the life of the project, the code started to solidify and that would have been a good time to implement error handling techniques. 

### (1) Turn bugs into test cases.

This is from [2]. Occasionally I found places where my code would fail under certain conditions that I could identify, and I would write in some way of catching that condition and avoiding it. But I should have also recorded that bug and used it to test other pieces of code that were susceptible to the same problem. 

### (2) Add error codes to all your functions. 

This is also from [2]. I found in MATLAB that there are several features to help catch errors. For example, you can check for the right number of arguments passed into a function using nargin. You can also use the sequence “Try-catch-end” - I need to learn to use this better but at least I know it exists [1]. Robbins [1] also suggests using Fopen, eval, fzero to check for function failure before moving on. 

## Miscellaneous Tips

### (1) Folder management
You can add subfolders to the current active directories list with a couple lines of code. So I put all my functions in a subfolder, added it to the list of active directories, and was able to keep the root folder clean while I worked.

### (2) Speed

Do not optimize the code for speed until you know it works [2], but once you do, there are several ways I have found to speed up my code. First: avoid using MATLAB’s symbolics toolbox as much as possible! It is incredibly slow! There was one place where I absolutely had to use it to create the model of the magnetic field and the associated gradient of the field. I could not find a way to generate the correct answer without using symbolics. To minimize the slowing effect that it had on my code, I pre-calculated the magnetic field and the gradient at every point in the workspace for every coil configuration I was considering (I only had 2) and saved the data in 3 dimensional arrays. This was incredibly effective at speeding up my code. 

If you do something like this where you pre-calculate a series of values in space or in time, you can use the interpolation functions in MATLAB to obtain the value at your exact point in space or time. Just be careful about when you allow the interpolation function to also extrapolate. There were times when I allowed the function to extrapolate, and it would throw off my calculation of the reward function because the interp1() function would just return what it thought the field would be outside the workspace, when in fact the field value should have been zero. 

Use MATLAB profile to find bottlenecks - you can use it to dig down until you find the slowest lines of code in your project.

Try out all the different flavors of MATLAB’s ordinary differential equations solvers suite (the ODE suite). You may find that ode45 is not the fastest function to use - in fact, when I tested all the functions in the suite, I found that it was the slowest of them all! If the matrices produced by your ODEs are stiff, often a different function like ode15s will be more efficient in solving them, often by an order of magnitude!

### (3) Take advantage of different data structures in MATLAB to work within a script and between scripts. 

I have just recently started to use tables, and of course there are also arrays, and cells, and other data structures that I’m less familiar with. I mentioned this above but I think there might be some advantages to using particular structures in certain scenarios. I tended to stick to using arrays because that was what I was familiar with, but there might be some value to branching out. 

Another thing I wanted to do was transfer data from one script to another. For example, I would have an array with trajectory data that I had created in one script, and I wanted to plot it in another. I think the most convenient thing to do is save variables (or the entire workspace) as a .mat file and load it in another script. This is convenient if you are just switching between scripts in MATLAB. If I wanted to save data for use by others outside of MATLAB, I would save it in a text or CSV file. I’m not sure if there are better ways to do this. 

### (4) “Commit often and there will be no tears.”

This is solid advice I got from a colleague, and it is very true. Setup a repository AT THE BEGINNING of your project and commit all the time! I want to write a separate article on how to use github (at least the basics) because I have a very rudimentary understanding of how it works. If nothing else, learn how to commit and push changes to your repo. For bonus points, have a master branch and a dev branch so that you can make multiple changes daily to the dev branch and ~monthly changes to the master branch. 

### (5) Have a second (desktop) machine for running code overnight. 

There were points in my project where my code was slow and ran for a really long time. They were sad times but they happened a lot. So I setup a second desktop machine at my desk at work with MATLAB and my github repo. Then I would remotely access the machine, and use it to run scripts overnight, or in parallel with my laptop. The desktop machine is slow - most of the components are from 2009, I believe - but it is great to have something that can run all night in parallel with my laptop. And having remote access was great - I could even check the status of my desktop machine from my cell phone on the bus ride home, and make changes to the code!

Just a note about security with remote access - do not trust it to be secure at all and so do not put personal or valuable data on your second machine!

Using github with this second computer also got tricky, because sometimes I made changes to the code on that machine which I wanted to save. I still do not truly understand how to make edits to a repo from two different machines so I have to learn how to merge. In the meantime - and this is, admittedly, very dangerous - I used: git reset --hard HEAD when I wanted to reset the remote machine’s version. 

### (6) Truly random stuff.

Machine precision is about 1E-9 or so. Keep that in mind. 
You can get creative with using vectors, since that’s what MATLAB likes (find, prod, sum, cumsum, nan, reshape) but also keep in mind your code should still be readable - you can keep a simpler and longer approach commented out nearby so readers can use that if they prefer [1]. 

#### References: 

[1] Robbins, M. “Good Matlab Programming Practices for the Non-Programmer.” <https://www.mathworks.com/matlabcentral/fileexchange/2371-good-matlab-programming-practices> Visited 03/12/2020.

[2] Wilson G, Aruliah DA, Brown CT, Chue Hong NP, Davis M, Guy RT, et al. (2014) Best Practices for Scientific Computing. PLoS Biol 12(1): e1001745. https://doi.org/10.1371/journal.pbio.1001745

[3] Divine, P. “Case Styles: Camel, Pascal, Snake, and Kebab Case.” Medium: BetterProgramming. <https://medium.com/better-programming/string-case-styles-camel-pascal-snake-and-kebab-case-981407998841> Visited 03/12/2020.
