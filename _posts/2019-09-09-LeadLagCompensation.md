---
layout: post 
title: Lead Lag Compensators
---

In this post I want to review designing PI/lag and PD/lead and PID/lag-lead controllers/compensators, primarily using root locus plots. All of the information provided here comes from Nise; I have not cited individual sentences because in this post because I have only one source and I am stating explicitly here that all of the information comes from Nise [1]. 

### Introduction   

First some basics - PI, PD and PID controllers are all called controllers and they typically require active amplifiers when implemented in hardware (this can be expensive). In contrast, the equivalent *compensators* are lag, lead and lag-lead compensators, and they can usually be implemented with passive circuits (less expensive). 

We use PI and lag controllers/compensators to improve the steady state response of a system (this can be seen by calculating the steady state error). Conversely, PD and lead controllers/compensators improve the transient response of a system (this can be seen on the root locus plot itself). The combination of the two - PID/lag-lead controllers - can improve both the steady state and transient responses of a system, although sometimes the interplay between the two will yield reduced improvement when compared to implementing pure steady state improvements or pure transient response improvements. In general we can call this group of controllers *dynamic* compensators because they involve the Laplace variable, s, instead of just pure gain.

There are two configurations for compensation: cascade (where the compensator is in the forward path on the “low power” side or left hand side of the plant) and feedback (where the compensator is in the feedback loop. In both cases the compensator will change the open loop poles and zeros, which creates a new root locus that goes through the desired closed loop pole locations (which you are designing for). This is different from pure gain because we are actually adding poles and zeros to the system, not just moving along the root locus created by the open loop poles and zeros. 

PI and PD compensators are called “ideal” compensators because they involve pure integration and pure differentiation, which is very hard to implement in practice. More on this in another blog post. 

### PI and Lag Compensators    

PI/lag compensators improve the steady state error because the zero is larger (i.e farther to the left on the real axis) than the pole so the ratio of the zero to the pole is >1. This ratio is multiplied by the original error coefficient Kp such that Kp is increased, resulting in a reduced steady state error. And when we place the pole and zero close to the origin, they have a minimal effect on the transient response (as can be seen by the fact that the root locus changes very little when they are added). 

Notice that putting the pole and zero too close to the imaginary axis can actually slow the system as it approaches unity. This is because as poles and zeros move farther into the LHP the time-domain response that corresponds to them decays faster because e^-st is a steeper curve as s increases. So while you do not want the poles and zeros to compete with the plants’ existing dominant poles and zeros by moving too far into the LHP, you also don’t want to be sequestered too close to the origin because then it will take forever for your system to reach steady state. 

### PD and Lead Compensators    

When considering the transient response, we are trying to reduce the percent overshoot and the settling time as compared to the uncompensated system. Adding a pure differentiator (an open loop zero) to the system requires an active network and adds unwanted high frequency noise to the system. You know if you have improved the transient response of the system because the dominant second-order poles have moved further into the LHP than the uncompensated system - further to the left along the real axis means shorter settling times. Also they will have smaller peak times because omega_d is larger and according to the peak time equation that means the peak time is reduced. You want to put the zero relatively close to the origin because the farther it goes to the left, the closer the compensated dominant CL poles get to the uncompensated dominant CL poles, and you get less of an improvement in performance. 

### PID and Lead-Lag Compensators    

How do we make steady state error improvements and transient response improvements with the same controller? We use PID or lag-lead compensators. First we improve the transient response, and then we look at the steady state error. (If we do steady state error first, then transient response, this method also works but sometimes improving the transient response can provide additional improvement to the steady state error which means that the steady state error is overdesigned, and the resulting controller becomes unnecessarily expensive.)

When designing with PID, assume one zero and the pole at the origin acts as the integrator; the other zero acts as the differentiator: 

**(1)** Evaluate performance of uncompensated system so you know how much of an improvement you need to make.   
**(2)** Design the PD controller to meet transient response requirements. Choose the zero location and the gain.   
**(3)** Check if you have met the design requirements.   
**(4)** Design the PI controller to improve the steady state response.   
**(5)** Choose the gains.   
**(6)** Check you have met all the design requirements.   

#### References: 
[1] Nise, Norman S. **Control Systems Engineering, 4th Ed.** John Wiley & Sons, Inc. 2004. 

