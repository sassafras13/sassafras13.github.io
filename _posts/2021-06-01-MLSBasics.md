---
title: Fundamentals from Murray, Li and Sastry
layout: post
---

I have been trying to learn some of the fundamentals of writing equations of motion for multilink robots from the classic text by Murray, Li and Sastry*1. There were some key ideas and mathematical notations that I wanted to record for myself as I continue to work through the text [1]. I also wanted to try out using MathJax to write equations instead of my former method. Many thanks to Ian Goodfellow’s blog post on the topic [2]. 

## Basic Terminology
In MLS, the first thing we are introduced to are some terms for different kinds of motion and different ways to represent those motions mathematically:

* **Rigid motion**: Motion that preserves distances between two points on the moving body.   

We can represent rigid body motion using a mapping, $$g \in \mathbb{R}^3$$ for points and $$g_{*} \in \mathbb{R}^3$$ for vectors. We talk more about _g_ in the Configurations section below.

* **Screw motion**: A combination of rotation and translation that occurs about/along a line.    
 
* **Twist**: A representation of a screw motion that is infinitesimally small.   

A twist can be written as $$\xi = (v, \omega) \in \mathbb{R}^6$$.

* **Wrench**: A representation of a system of forces acting on a rigid body that condenses the forces and torques into 1 force and 1 torque acting along the same line.   

A wrench can be written as $$F = (f, \tau) \in \mathbb{R}^6$$. 

Essentially, the **twist** represents the kinematics of a system, while the **wrench** represents the dynamics. 

## SE(3) and SO(3)
We use different Euclidean and other groups of transformations to describe space [3]. All Euclidean groups contain transformations that preserve the Euclidean distance between two points [3]. The **Euclidean distance** is just the distance between two points as measured by a line that directly connects them [4]. Euclidean groups can contain translations, rotations and reflections. There are two Euclidean groups that we will see a lot in MLS: SE(3) and SO(3). 

The **special Euclidean group, SE(3)** is a group of Euclidean transformations that represent rigid motions. They include translations and rotations, but not reflections [3]. The 3 in SE(3) indicates that this Euclidean group exists in 3D space - similarly, SE(2) refers to a special Euclidean group in 2D space. We can define SE(3) as follows: 

$$SE(3) = \big( A | A = \left[ \matrix{R & r \cr 0 & 1} \right], R \in \mathbb{R}^{3 \times 3}, r \in \mathbb{R}^3, R^TR = RR^T = I, |R| = I \big)$$

Transformations that are part of a special Euclidean group can simultaneously translate and rotate a vector. They are a continuous group so therefore they are differentiable and can be considered a Lie group. 

The other Euclidean group that we will see a lot is the **special orthogonal group, SO(3)**. This is the set of all rotations in 3D space [1]. It is a “special” group because the determinant of all the matrix rotations, det R, is always +1 [1]:

$$SO(3) = \big( R \in \mathbb{R}^{3 \times 3}, RR^T = I, det R = +1 \big)$$

We can say that SE(3) is just the product of $$\mathbb{R}^3$$ with SO(3), that is [1]:

$$SE(3) := \mathbb{R}^3 \times SO(3)$$

## Configurations
Earlier, we introduced the idea that we could write the configuration of a robot given some rigid body transformation, so now let’s formalize that a little. In general, I can write the configuration of a frame of reference, B, with respect to another frame of reference, A, as follows [1]: 

$$g_{ab} = (p_{ab}, R_{ab}) \in SE(3)$$

(I could also say that $$g_{ab} \in SE(3)$$ and specifically $$p_{ab} \in \mathbb{R}^3$$ and $$R_{ab} \in SO(3)$$ [1].)

The configuration $$g_{ab}$$ contains a translation, $$p_{ab}$$ and a rotation, $$R_{ab}$$ which convert the coordinates of a point in the frame B to equivalent coordinates in the frame A. 

There is a matrix form for writing the configuration as well [1]:

$$g_{ab} = \left[ \matrix{R & p \cr 0 & 1} \right]$$

We call $$g$$ a configuration, but this same representation can also be used to write a rigid body transformation [1]. 

## ^ Notation
As we will see in subsequent sections, there is some hatting notation that gets used frequently in MLS which I had not seen before and so would benefit from some additional explanation. Specifically, we frequently see $$\hat{a}$$ is used to represent a vector cross-product operation [1]: 

$$a \times b = \left[ \matrix{a_2 b_3 - a_3 b_2 \cr a_3 b_1 - a_1 b_3 \cr a_1 b_2 - a_2 b_1} \right]$$

And we can capture the operation that **a** performs on **b** as [1]:

$$\hat{a} = \left[ \matrix{0 & -a_3 & a_2 \cr a_3 & 0 & -a_1 \cr -a_2 & a_1 & 0} \right]$$

Notice that $$\hat{a}$$ is a skew-symmetric matrix, i.e. $$\hat{a}^T = -\hat{a}$$ [1].

## “Hatting” and “Unhatting” (or “Vee” and “Wedge” Operators)
Related to the previous idea, we can convert between vectors and matrices in our notation using the “vee” and “wedge” operators and MLS does this frequently throughout the book. (This conversion is also often referred to as “hatting” and “unhatting,” I believe.) 

The “vee” operator extracts a vector from a matrix [1]: 

$$\left[ \matrix{\hat{\omega} & v \cr 0 & 0} \right] ^ {V} = \left[ \matrix{v \cr \omega} \right]$$

And conversely the “wedge” converts a vector to a matrix [1]:

$$\left[ \matrix{v \cr \omega}^{\hat{}} \right] = \left[ \matrix{\hat{\omega} & v \cr 0 & 0} \right]$$

## Matrix Exponentials
In MLS we often want to write the rotation of a point or vector as a function of the direction of rotation, $$\omega$$, and some angle of rotation, $$\theta$$ [1]. To do that, imagine we are going to compute the velocity of a point that is rotating, as follows [1]: 

$$\dot{q}(t) = \omega \times q(t) = \hat{\omega}q(t)$$

This is a linear, time-invariant differential equation, and we can integrate to write [1]:

$$q(t) = e^{\hat{\omega}t} q(0)$$

(If you are familiar with the state transition matrix from control theory, this is a very similar idea.) Now we have $$e^{\hat{\omega}t}$$ which is called a matrix exponential [1]. It allows us to write a function for the rotation, R, as being dependent on $$\omega$$ and $$\theta$$ by $$R(\omega, \theta) = e^{\hat{\omega}\theta}$$. We can write the matrix exponential as an infinite series [1]: 

$$e^{\hat{\omega}t} = I + \hat{\omega}t + \frac{(\hat{\omega}t)^2}{2!} + \frac{(\hat{\omega}t)^3}{3!} + …$$

But generally infinite series are not useful in a computational setting, so we need to find a closed-form representation of the matrix exponential. Moreover, MLS argues that in general it is useful to write a matrix exponential or a skew-symmetric matrix representation as the product of a **unit** skew-symmetric matrix and a real number [1]. So we can define $$\lvert \omega \rvert = 1$$ and a real number $$\theta$$ so that we can rewrite the expression above as: 

$$e^{\hat{\omega}\theta} = I + \theta \hat{\omega} + \frac{\theta^2}{2!} \hat{\omega}^2 + \frac{\theta^3}{3!} \hat{\omega}^3 + …$$

From here, we can write an analytical expression for this version of the matrix exponential using **Rodrigues’ formula** [1]:

$$e^{\hat{\omega}\theta} = I + \hat{\omega}sin \theta + \hat{\omega}^2(1 - cos \theta)$$

## Velocity
We can write the **spatial velocity** of a rigid body as follows [1]:

$$\hat{V}^{s} = \dot{g}g^{-1}$$ 

Where the spatial velocity is defined as the velocity of a rigid body as observed at the origin of the reference frame. We could also define the **body velocity** as [1]:

$$\hat{V}^{b} = g^{-1}\dot{g}$$

Where the body velocity is the velocity of the object as written in the instantaneous body frame. 

## Adjoint Transformations
We use adjoint transformations to transform twists from one coordinate frame to another. We can use them with either velocities or wrenches [1]. In general, we can write an adjoint transformation as [1]:

$$\text{Ad}_g = \left[ \matrix{R & \hat{p}R \cr 0 & R} \right]$$

If I want to apply an adjoint transformation to a wrench, I would write [1]: 

$$F_c = Ad_{g_{bc}}^T F_b$$

## Conclusion

I hope this post helps capture some key facts from Chapter 2 of MLS. I may write additional posts in the future on other topics from the book, or on some of the interesting mathematical tools used here. Thanks for reading!

## Footnotes:
*1 Hardcore nerds call the book “MLS” for short. 

## References: 
[1] Murray, R., Li, Z., Sastry, S. “A Mathematical Introduction to Robotic Manipulation.” CRC Press. 1994. <http://www.cse.lehigh.edu/~trink/Courses/RoboticsII/reading/murray-li-sastry-94-complete.pdf> Visited 01 Jun 2021. 

[2] Goodfellow, Ian. “LaTex in Jekyll.” 7 Nov, 2016. <http://www.iangoodfellow.com/blog/jekyll/markdown/tex/2016/11/07/latex-in-markdown.html> Visited 24 May 2021. 

[3] Wikipedia. “Euclidean group.” <https://en.wikipedia.org/wiki/Euclidean_group> Visited 01 Jun 2021. 

[4] Wikipedia. “Euclidean distance.” <https://en.wikipedia.org/wiki/Euclidean_distance> Visited 01 Jun 2021. 

