---
title: Tips for Writing Dynamics Models in MATLAB
layout: post
---

This blog post has been a long time in the making. I have been spending many hours this semester writing mathematical models in MATLAB for a research project. During this time, I have encountered a lot of challenges that I had never really come up against in my dynamics and controls courses. I started this project feeling poorly equipped to deal with those challenges, but I have tried to develop a couple tricks and tips that make it easier to write and test different models. This blog post will cover some of those tips. I really hope this post helps some other people because I was very frustrated by this process and it would be great if I could save someone else some aggravation. 

First, let me set the scene so we have some context for what my project entailed, and then I will start listing some specific tips. I wanted to write a model that captured the dynamics of my system from first principles, similar to how we might model canonical systems like a [pendulum](https://ctms.engin.umich.edu/CTMS/index.php?aux=Activities_Pendulum). I wanted to simulate how the model would respond to various control inputs using MATLAB’s ode45 tools. I used analytical tools like looking at stability via the limit cycle and net displacement via [connection curvature functions](https://sassafras13.github.io/CVF/) to further understand my system. I also wanted to test the model with different parameters and to try writing the model using different approaches to check I was correct. 

I am going to break up the tips for working through a process like this into a couple of different steps. First, we’ll talk about how to write the basic dynamics model. Next, we’ll discuss how to test the model we wrote and make sure it’s doing what we expect it to do. Finally, I will end with some tips on how to analyze your model and get the answers you need. 

## Writing a Dynamics Model in MATLAB

In this section I’ll share some tips for how to derive your mathematical model in MATLAB. A lot of this discussion is going to center around LiveEditor scripts, which I absolutely love to use. I would argue that it should become an academic standard that any time you publish a new model in your paper that you include a LiveEditor script with your derivation and your code in your paper supplementary materials!! Begin the campaign!! 

**1. Use LiveEditor scripts.** MATLAB has an equivalent document format to Jupyter notebooks in Python, called LiveEditor scripts. This document format allows you to mix text, LaTeX equations, images and code all in one place, and it is my primary tool for communicating my model derivation as I work on it. I like using it because I can write out the mathematical equation and then on the very next line demonstrate how that equation would be written in code. I also like it because I can save the LiveEditor scripts as PDFs (and possibly as Markdown files too?) and share them with others even if they don’t have a MATLAB license.   

**2. Use the symbolic toolbox.** MATLAB includes a symbolic math toolbox which you can use to write expressions without assigning real values to every single variable. It even allows you to take derivatives, integrate, compute cross products, etc. I write my entire model with the symbolic toolbox and then save the final expressions as MATLAB functions that I can call in regular scripts.   

**3. But don’t get too comfortable with symbolic math.** I am very guilty of letting MATLAB do all my math for me. I mean, why should I have to do all the integration and derivatives if the symbolic toolbox can do it for me? Who do you think I am? Someone who remembers calculus? But the thing is, you **need** to check your work and make sure you trust it. I’m going to touch on this much more later, but when you are modeling a new system for which you do not have a lot of intuition or understanding yet, you should check your math by hand. This also helps you get a better idea of how your model works.   

**4. Make sure to set your symbolic variables to ```real```.** I ran into this problem with a friend of mine when we were both writing code for the same model but only one of us had added the ```real``` flag to the end of our declaration of symbolic variables. If you do not, you may find that your function returns complex numbers in cases when you only wanted the real part. An example of the proper code for this situation would be:   

```Matlab
syms x y z ‘real’ % real command forces these variables to be real-valued
```
**5. Break your LiveEditor scripts into pieces and add numeric prefixes to the filenames to keep them in order.** This might be a problem unique to me because I have an older laptop with a tired CPU, but I do find that my LiveEditor scripts really slow down if there are a lot of lines of text or a lot of computations in a single document. To combat this, I break the document up into multiple LiveEditor scripts and save them with a numeric prefix like ```01_setup.mlx```, ```02_forces.mlx```, etc. I think MATLAB might complain about the numeric start to the filenames, but you can override it and it seems to work fine anyway.    

**6. Declare variables once and run LiveEditor scripts in order.** If you do follow tip #5 above, you will need to run the scripts in order so that all the variables required in a later script have already been created by the earlier scripts. I have found two ways to make sure this happens: the first is just to remember to always run the scripts in order. The second is to add commands to run the earlier scripts at the top of later scripts like this:   

```Matlab
01_setup ; % runs file 01_setup.mlx    
```

**7. Leverage all the useful symbolic toolbox features.** Again, you should always check what the symbolic toolbox does, but you can get it to do some pretty cool things like ```collect``` terms, find the coefficients for a specific variable using ```coeffs```, integrate and differentiate. Also be sure to use ```simplify``` to clean things up if they are really messy.   

**8. You can take time derivatives with the symbolic toolbox.** At this point some academics are probably pretty angry that I am using the symbolic toolbox for everything, but if you have really hairy functions, you are more likely to make mistakes doing the derivatives by hand than if you used the symbolic toolbox, honestly. Please remember to use your best judgment, but here’s a toy example for how I was able to compute time derivatives of functions using symbolic math: 

```Matlab
% create symbolic variables for x (position) and xdot (velocity)
% these variables are not dependent on time
syms x xdot 'real'
 
% define function y(x, t) for which we will compute time derivative
y = x^2 ; 
 
% define the symbolic variables x_t and dx_t 
% these variables are dependent on time
syms t x_t(t) 'real'
dx_t = diff(x_t, t) ; 
 
% replace the value x with x_t to create a time-dependent version of 
% the function y, called y_t
y_t = subs(y, x, x_t) ; 
 
% take the time derivative of y_t to get dy_t, also a function of time
dy_t = diff(y_t, t)
 
% replace x_t and dx_t with the symbolic variables that are
% not functions of time, x and xdot
dy = subs(dy_t, [x_t, dx_t], [x, xdot])
```
Now that we have talked about writing the derivation for your model, let’s assume that you have now got your function(s) for the dynamics of your system and you are ready to start simulating and analyzing your model. 

## Writing Modular Code
A lot of the tips in this section assume that you may have multiple versions of a dynamics model that you want to compare against each other. Following our pendulum example, this might be true if you want to see what happens if you add a torsion spring to your pendulum, or if you distribute the mass along the length of the pendulum instead of writing it as a point mass at the end. In such cases, these tips will really help you keep track of everything. And even if you have only one model that you have written, these tips will hopefully be relevant. 

The big thing I want to emphasize in this section is that writing modular code is going to be essential. As I hope these tips will prove to you, writing each piece of code only once and reusing it via function calls will be critical to making sure you are properly testing your model. Similarly, modular code will allow you to define all your model parameters once and load them infinitely many times with different versions of your model, so you can test how your models perform without worrying that the parameter values have differed from test to test. Let’s see how this works in more detail. 

**9. Write a script that contains all your model parameters and saves them in a .mat file.** Your dynamics model probably requires some parameters to be defined - for our example of a pendulum dynamics model, we would need to define the length and mass of the pendulum. If I am going to compare different models that all need these same parameters, it makes sense that we write them into a separate script that we can call repeatedly. Typically I write a regular script to define all my parameters, and then use the ```save(“myfile.mat”)``` command to save the variables as a .mat file. I can then load this file in a separate script that is going to simulate the dynamics with ```load(“myfile.mat”)```. Those variables are immediately loaded into your workspace and you’re ready to go. 

**10. Use folder structures to your advantage.** One characteristic of good code bases is that all the scripts are in subdirectories that make sense, and the overall repo structure is logical and ordered. For example, in my ```\src\``` directory I like to have subdirectories for ```\utils``` (functions that all my scripts use), ```\parameters``` (parameters of my model that I want to reuse, per tip #9), and ```\model``` (for different models I am trying out). You can do anything you want, but having a structure like this can really help keep things organized. Use ```fileparts``` and ```addpath``` functions in MATLAB to add any subdirectories you need to your current workspace so that the IDE knows to look there for those functions when running your code. 

**11. You can look at intermediate values computed inside your ode45 call to help with debugging.** Let’s say that you are computing the trajectory of your pendulum, but you would like to know the torque acting on the pendulum during the simulation, perhaps to help with debugging or to better understand your system. You can actually write your dynamics function to return this torque as an additional output and rerun the dynamics function after the ode45 call to get these intermediate values. Consider this toy example: 

```Matlab
[t,q] = ode45(@(t, q) dynamics(t, q),Tspan,q0) ; % original call to ode45 
% call your dynamics function again with the output from the line above
[~,torque] = cellfun(@(t,x)  dynamics(t,q.'), num2cell(t), num2cell(q,2),'uni',0);
torque = cell2mat(torque) ; 
```

## Testing Your Model 

The next series of tips is specifically about testing your model. Before I share them, let me get on my soapbox for a minute and talk about why testing your model and requisite functions is so important. In research, we are often developing **new** models for things that we have never studied or built before. This means we are venturing into uncharted territory, and we may not have the intuition to know at a glance if our model is working as it should, or if it has a bug. For example, I know how a simple pendulum should behave but if we are trying to model a 3 link pendulum with torsion springs at all the joints, I cannot really imagine in my head what that system is going to do. Our model is the flashlight we are pointing into the darkness of this unknown design space, and we have to make sure it is working properly. You cannot just write a model, say “yup, thanks to my excellent physics skills I’m confident this works” and then start simulating and analyzing right away. You will want to prove to yourself and others that this model really does do exactly what you expect. This series of tips is going to share a couple of approaches for building a body of evidence that convinces everyone that your model is 100% correct. 

**12. Test functions that perform specific mathematical operations against examples from a textbook.** Part of my modeling activities included implementing a lot of mathematical operations for robot kinematics, and I was using the textbook by [Murray, Li and Sastry](https://sassafras13.github.io/MLSBasics/) as a reference. Every time I implemented a function like computing a matrix exponential, or hatting a twist, or computing a Jacobian, I ran that function on an example that I pulled from their textbook. I did this in a LiveEditor script so I could include a screenshot of the solution directly from the textbook to show that my function output the correct answer. This doesn’t help prove your entire model is correct, but it will make you more confident that your component functions are working properly.   

**13. Design simple tests to check if your model obeys simple laws of physics.** I really have to thank one of my advisors for this tip. If you have a complicated system (like that 3 link pendulum with torsion springs everywhere), think about the simplest possible tests you could run on your model to make sure it did simple things correctly. For example, if you displace one link from its at-rest position, does it fall back to its at-rest position and stop moving? If all the torsion springs had spring constants of zero, do the links just hang along the vertical axis? This may seem trivial, but if your model **can’t** do this, then you know you have problems that you need to address. The other nice thing about using this strategy is that it's easy to know what the model **should** do in such situations - this might not be true for more complicated situations.   

**14. Use experimental data as ground truth.** Of course, you may reach a point in your testing process where you are confident the model performs correctly in simple tests, but you want to see if it does the right thing in more complex situations. If possible, consider building a quick and cheap prototype that you can use to study the system’s response to different inputs. Capture video of your experimental system and use OpenCV to track the motion of all the links to build up a body of ground truth data that you can use to validate your model. (Using OpenCV in Python might sound challenging but there is lots of documentation available online and I found that I was able to go from never having used it to getting useful data in less than a day’s work thanks to the Internet.)   

**15. Build up from a model you already have or trust to the model you want.** If you’re working on a project where someone else has already written a model similar to yours, consider using their model as a starting point and modify it until it has all the features you need. Again, we are exploring the unknown here and someone else’s model is like a stepping stone that helps you start to cross the stream. However, I should also caution that this process may make things more complicated if you need to modify the model in ways that generate very different results. It may become difficult to compare the models and you may create more confusion than if you had started from scratch.   

## Getting the Answers You Need
Okay, so at this point let’s assume that you’ve built a dynamics model for your system and you have rigorously tested it. Now you want to analyze it with cool techniques like plotting the limit cycle or generating constraint curvature functions. Let me share a few final tips on how to conduct the analytical process for great success. 

**16. Write a new script for each analytical process.** This might seem like a no-brainer, but have separate, well-named scripts for each analysis you are going to conduct. Since you already have modular code with parameters that you can load from a .mat file, you can perform different analyses on the same model with the same parameters easily. I also like to write code to save figures from each analytical process in these scripts. Ideally, each script should output a figure that you use in your paper, and other researchers looking at your code base should be able to recreate all your figures from these scripts.   

**17. If you want to run analyses that generate lots of data or figures, consider automating directory creation and file saving to organize the output of this analysis.** To give you a concrete example here, let’s say that we want to see how our pendulum model will perform for different combinations of lengths and masses. In your analysis script, you can create subdirectories and save plots and data files to these subdirectories automatically. You can run the script while you’re out for lunch and then come back and look at all of the resultant plots that have already been organized into subfolders for you.   

**18. Save figures as .fig files so you can continue to edit them later.** I often find that it takes me many iterations before I have a figure that I really like, but if I save my figures as .png files then I cannot edit those files later, and instead I have to re-run all the code used to make that figure before I can edit it. That’s lame and a waste of your precious time, so instead save your figures as .fig files, which is the native MATLAB figure format (or save your figures twice, once as .fig and once as .png if you want!). Now you can always reload these figures to your workspace and edit them with MATLAB’s figure editor.   

**19. Building animations of your dynamic systems is almost always a worthwhile investment of your time.** This might be one of my most important tips: if you want people to really understand what you are doing, building information-rich animations is the best way to get them to quickly grok what you are doing. MATLAB allows you to generate animations (I prefer .gifs but I think you could output .mp4s and other formats if you wanted). MATLAB of course has excellent documentation on different ways to build animations, but to be honest with you, I once copied [this script from MIT](http://web.mit.edu/8.13/matlab/MatlabTraining_IAP_2012/AGV/DemoFiles/ScriptFiles/html/Part3_Animation.html) and I have just continued to modify it for my purposes ever since. (Thanks MIT!)  

## Concluding Thoughts
My vision for this post was to share lots of strategies for developing dynamics models that are not often taught in class. They may not all be helpful, but I had a lot of trouble finding useful advice on the Internet so I hope that this blog post helps fill that gap. If you know of more resources on this topic, please share them because I am always looking to learn more best practices for writing robust models. 

