---
layout: post
title: Classical Control Theory Toolbox
---

I just finished my mock qualifying exam this morning and one of the pieces of advice I got from some of the more senior students was to know when to use Bode plots, Nyquist plots and root locus plots. I am going to start a list of when to use each kind of plot, and I may come back to update this post occasionally based on what I learn as I do more problems. 

### Bode Plots
Bode plots give information about the **frequency domain** behavior of a system. They should be used if you are concerned about:    

**(1)** Gain/phase margin     
**(2)** Gain crossover frequency     
**(3)** System bandwidth    
**(4)** DC gain     

### Root Locus
Root locus plots give information about the **time domain** behavior of a system. They should be used if you are concerned about:     

**(1)** Rise time     
**(2)** Settling time          
**(3)** Damping ratio    
**(4)** Overshoot     

### Nyquist Plots
Nyquist plots can give you a view into system stability, especially when you don’t have the root locus plot. They should be used if you are concerned about:     

**(1)** Gain/phase margin     
**(2)** System stability    

### Notes
There are some other tools that you have available which can answer questions that the 3 plots above cannot address. Some of these other tools include: 

#### Final Value Theorem
This helps you calculate the steady state error of a system **if you know the system is stable.** This Theorem should NOT be used if you have not confirmed that your system is stable. 

#### Routh-Hurwitz Criterion
This helps you to check for stability and find gains for which a system is stable, especially if you cannot factor out the characteristic equation for drawing a root locus plot.   
