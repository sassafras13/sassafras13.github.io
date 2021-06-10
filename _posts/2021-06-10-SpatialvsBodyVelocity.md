---
title: Spatial Velocity vs. Body Velocity
layout: post
---

As I have been learning the mathematics behind robot kinematics, I have been struggling to understand the difference between a spatial velocity and a body velocity. In this post, I am going to try to write a good definition of each of these velocity types. 

## Basic Definitions 

According to Murray, Li and Sastry (MLS), the definitions are as follows [1]: 

* **Spatial velocity**:- mathematically defined as $$\hat{V}^s = \dot{R}R^T = \dot{g} g^{-1}$$. MLS says that the spatial velocity of a rigid motion is the instantaneous velocity of the body as viewed in the spatial frame. It is the “velocity of a (possibly imaginary) point on the rigid body which is traveling through the origin of the spatial frame at time _t_” [1]. So if you are standing at the origin of a spatial frame and you measure the velocity of a point attached to the rigid body and going through the origin where you are standing, that is the spatial velocity [1]. 

* **Body velocity**:- mathematically defined as $$\hat{Va}^b = R^T \dot{R} = g^{-1} \dot{g}$$. MLS says that the body velocity is more “straightforward” to understand; it is the “velocity of the origin of the body coordinate frame relative to the spatial frame, as viewed in the current body frame” [1].  MLS points out that the body velocity is not the velocity of the body relative to the body frame, because that is always zero [1]!

## Body Velocity

These definitions were a little confusing to me at first. Let’s take a step back and think about this with some more intuition. Let’s start with the body velocity, because I can relate that easily to the experience of riding in a car. If you imagine that you are riding in a car at a constant velocity, then the only way you know that you are moving is if you compare your frame of reference (the **body frame**, B) with some fixed frame (the **spatial frame**, A). The body velocity is your instantaneous velocity as measured with respect to this fixed frame, A, but observed from your perspective inside the car’s body frame, B. So when your car’s speedometer tells you that you are moving at 60 MPH, that means that you are moving at 60 MPH with respect to the spatial frame, A, as observed from within the body frame, B [2]. 

We can define your body velocity in this instance as follows. Let’s assume that your position in the car is a point $$q$$ and so your body velocity is written as [2]:

$$v_{q_b} = g_{ab}^{-1} v_{q_a}$$    

Where $$g_{ab}$$ is the transformation that converts a point in the body frame, B, to the spatial frame, A, and $$v_{q_a}$$ is the velocity of the point $$q$$ in the spatial frame, A. Another way to write $$v_{q_a}$$ is $$v_{q_a} = \dot{g}_{ab} q_{b}$$ *1 so we can substitute that in: 

$$v_{q_b} = g_{ab}^{-1} \dot{g}_{ab} q_{b}$$     

Now I can write my body velocity as a function of **some point $$q_b$$ in the body frame that has velocity with respect to the spatial frame, A, as viewed in the body frame, B** [2]. 

$$v_{q_b} = \hat{V}_{ab}^b q_{b}$$   

In other words: 

$$\hat{V}_{ab}^b = g_{ab}^{-1} \dot{g}_{ab}$$    

## Spatial Velocity

The difference between the spatial velocity and the body velocity is that, instead of observing your velocity in the car in the body frame, B, you are observing it from the origin of the spatial frame, A. That is, the **spatial velocity measures the velocity of a point fixed to the body frame, B, with respect to the spatial frame, A, and observed from the spatial frame, A** [2]. I can write the spatial velocity as follows [2]: 

$$v_{q_a} = \dot{g}_{ab} q_b$$   

I can rewrite $$q_b$$ to be in terms of $$q_a$$ as follows [2]:

$$q_b = g_{ab}^{-1} q_a$$   

And therefore I can write the spatial velocity as [2]:

$$v_{q_a} = \dot{g}_{ab} g_{ab}^{-1} q_a$$

And so we see that [2]: 

$$\hat{V}_{ab}^s = \dot{g}_{ab} g_{ab}^{-1}$$

I hope this helps a little bit with getting to grips with the differences between a spatial velocity and a body velocity. I think that it all comes down to where you are observing the velocity from. 

## Footnotes:

*1 This is by the definition of the [rigid motion mapping](https://sassafras13.github.io/MLSBasics/), that is [2]:

$$q_{a} = g_{ab} q_b$$    

And the derivative is [2]: 

$$\dot{q}_a = \dot{g}_{ab} q_b$$    

## References:

[1] Murray, R., Li, Z., Sastry, S. “A Mathematical Introduction to Robotic Manipulation.” CRC Press. 1994. 

[2] Sastry, S. “EE106A Discussion 7: Velocities and Adjoints.” EE C106A: Introduction to Robotics, Fall 2019 course notes. UC Berkeley. <https://ucb-ee106.github.io/ee106a-fa19/assets/discussions/D7___Velocities_and_Adjoints.pdf> Visited 10 Jun 2021. 


