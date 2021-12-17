---
title: ODE Solvers in MATLAB
layout: post
---

In this post I am going to write about solving ordinary differential equations (ode) in MATLAB. I wanted to explore this area because I use MATLAB’s ODE solvers all the time, and I wanted to capture the details of how they work, when different solvers are appropriate and what parameters are available for tuning. I’m going to stay at a somewhat high level when it comes to the details of different ODE algorithms, so if you want more details on, for example, the steps in the Runge-Kutta method, please see [1]. Let’s begin!

## How ODE Solvers Work
MATLAB offers a [suite](https://www.mathworks.com/help/matlab/math/choose-an-ode-solver.html) of different ODE solvers available for use. They vary in implementation and advantages, but in general they are all solving equations of the form [1]:

$$\frac{dy(t)}{dt} = f(t, y(t))$$

With initial conditions $$y(t_0) = y_0$$. These solvers output a time series of values for $$y(t)$$ by taking small steps, $$h$$, in the direction of the gradient, $$\frac{dy(t)}{dt}$$, starting at the initial conditions, $$y_0$$. We can write this as [1]: 

$$y(t + h) = y(t) + \int_t^{t+h} f(s, y(s)) ds$$

The simplest algorithm for solving this problem numerically is called **Euler’s method**. It uses a fixed step size [1]: 

$$y_{n+1} = y_n + h f(t_n, y_n)$$    
$$t_{n+1} = t_n + h$$

This method is not particularly efficient, and requires very small steps for accuracy. The step size is also fixed, and the choice of step size, at the moment, is rather arbitrary - we don’t have a good sense for what an appropriate size of $$h$$ would be. An important improvement to Euler’s method is including a way to estimate the error in this algorithm - this helps us understand how well our numerical integration is doing and how to change our step size to improve our performance [1]. 

This leads us to the development of **single-step methods** (or **Runge-Kutta methods**), which compute several values for $$f(t, y)$$ in the interval $$\[t_n, t_{n+1}\]$$. We use a linear combination of these intermediate values to compute the value $$y_{n+1}$$ and take a step in that direction. Classical Runge-Kutta methods do not actually include a way to compute the error, either, but MATLAB implements all of its ODE solvers (many of which use Runge-Kutta methods) with an error computation included in the function. This is important to know because the error is used to determine whether to accept the integration step, or reduce the step size and try again. The user can define the threshold above which the error is too high and we reduce the step size [1]. 

If you’re familiar with MATLAB’s ODE solvers, you may already know that there are 2 error parameters that we can set: **relative tolerance** and **absolute tolerance**. The definition of each one is given below [2]:

```RelTol = abs(X - Y) / min(abs(X), abs(Y))```   
```AbsTol = abs(X - Y)```

The key reason why we have 2 parameters is because if either the value of X or Y becomes very small, then the relative tolerance parameter will go to infinity, and so the ODE solver switches to using the absolute tolerance as the cutoff for the error in that situation [2]. These are 2 parameters that you can manipulate when you are trying to optimize your ODE solver for your particular application. 

In fact, MATLAB’s naming convention reflects how it computes the error for each ODE solver. The general format is ```odennxx``` where the ```nn``` digits indicate the orders of the methods used to perform the integration, and the ```xx``` suffix, when used, indicates other special properties of that function. So if we consider our favorite ```ode45```, the naming convention indicates that the function computes the error by comparing the results of a 4th order method with a 5th order method [1]. 

## Stiffness

So far we have seen that all of MATLAB’s ODE solvers are taking steps along a gradient towards the solution of an ordinary differential equation over time. We know that the relative and absolute tolerances are important parameters we can control to limit the maximum error we allow during the integration process. Another important aspect to solving ODEs that we need to think about is how **stiff** our system is. This is a slightly difficult concept to grasp, but let’s try. Moler describes a system as being stiff if “the solution being sought varies slowly, but there are nearby solutions that vary rapidly, so the numerical method must take small steps to obtain satisfactory results” [1]. 

Moler also uses a nice metaphor to explain stiffness from an intuitive standpoint. Trying to solve a stiff system is like following a trail in the dark along the bottom of a narrow valley with very high walls. The naive approach is to wander in a random direction and let the gradient of the valley push you towards the trail. Unfortunately, this is not very efficient as you will likely weave back and forth across the trail and up and down the sides of the valley. This is a stiff problem, and a better way to solve it is to use a flashlight to look ahead of you and see where the trail is and follow it without oscillating up and down the valley walls. The MATLAB equivalent of the flashlight is a subset of ODE solvers that are designed to predict how the solution will continue to evolve and use that information to take steps in the right direction without oscillating via the gradient [1]. 

A good example of the benefit of using a stiff ODE solver is to try to solve a simple model for flame propagation [1]:    
$$\dot{y} = y^2 - y^3$$    
$$y(0) = \delta$$    
$$0 \leq t \leq 2/\delta$$    

For large values of $$\delta$$ (the radius of the ball of flame), this is not a very stiff problem, but as the radius gets smaller, the system does become more stiff. See this for yourself by running the code below. Try varying the value of ```delta``` to see how it makes the problem more stiff, and how ```ode45``` really slows down when it tries to solve the stiff version of the problem. If you then switch over to ```ode23s``` you will see that this solver is much faster and finding the solution [1]. 

```Matlab
delta = 0.01 ; 
%delta = 0.00001; 
F = @(t,y) y^2 - y^3; 
opts = odeset("RelTol",1.e-4); 
ode45(F,[0 2/delta],delta,opts) ; 
%ode23s(F,[0 2/delta],delta,opts);
```

As you can see from this example, knowing whether or not your system is stiff is important because it helps you to select the correct solver for your application. (We call ODE solvers that are designed for stiff systems **implicit solvers** [1].)

## Numerical Precision
So now we know that we can vary the relative and absolute error tolerances, and the type of solver we use. We’ve also talked about how we determine the size of the integration step based on the error computed by the ODE solver. Let’s revisit the step size in a little more detail. 

First of all, you can set a maximum on the initial step size that your solver will use with the ```InitialStep``` option in the [```odeset``` command](https://www.mathworks.com/help/matlab/ref/odeset.html). You will likely find that the smaller the initial step size, the slower your code will run. But you may also want to think about numerical precision as you choose your step size and solve ODEs. Specifically, it is important to note that MATLAB uses floats that have 16 orders of precision, and if you start to venture beyond that point, you will likely see numerical instability in your code. For example, if the difference between two numbers is on the order of 1E-16, MATLAB may not return an accurate result [3]. This is just a reminder that if your code is generating numbers this small, you may want to consider [non-dimensionalizing](https://en.wikipedia.org/wiki/Nondimensionalization) your code to bring all of the values in your solution closer to 1. 

## Closing Thoughts
In this post, we’ve discussed some of the pros and cons of choosing different ODE solvers in MATLAB, and some of the parameters available to us for tuning our code. In closing,  I just wanted to suggest that it may be easiest to simply write code to try a range of different solvers, error tolerances and step sizes and see which combination best suits your particular application. You can evaluate the results based on how smooth they are, how close the solutions are to the correct answer (if you know what it is), and how quickly it takes to get there. I hope this post helps, and happy coding!

## References

[1] Moler, C. “Numerical Computing with MATLAB.” MathWorks, 2004. <https://www.mathworks.com/moler/chapters.html> Visited 17 Dec 2021. 

[2] Jan. “Absolute and relative tolerance definitions.” MATLAB Answers. 22 Jan 2012. <https://www.mathworks.com/matlabcentral/answers/26743-absolute-and-relative-tolerance-definitions> Visited 17 Dec 2021. 

[3] D’Errico, J.  “Unstable derivative approximation when steps get too small.” MATLAB Answers. 2 Jan 2020. <https://www.mathworks.com/matlabcentral/answers/498726-unstable-derivative-approximation-when-steps-get-too-small> Visited 17 Dec 2021. 
