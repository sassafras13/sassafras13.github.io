---
title: Deriving the Boltzmann Distribution
layout: post
---

My recent blog posts have been exploring topics related to modeling binding events at equilibrium in biology and chemistry. So far, I’ve looked at the basics of [classical thermodynamics](https://sassafras13.github.io/thermo-reaction/) (to describe what happens near equilibrium) and how [cooperativity](https://sassafras13.github.io/Cooperativity/) contributes to binding events. One thing I have been missing, however, is a way to connect concepts in thermodynamics like Gibbs free energy and entropy, to models of cooperativity. Today, we are going to begin to bridge this gap by drawing on tools from statistical mechanics, centered around the Boltzmann distribution. I’ll add the keystone to the bridge in a subsequent post on the Law of Mass Action.

I am really excited to write this post because the statistical mechanics ideas that we’re going to use to study biological processes are also used to develop a lot of the theory behind probabilistic graphical models, particularly Markov Random Fields. I think it's really cool to see the same mathematical/physical principles being applied to wildly different fields, and suggests that maybe they’re not so wildly different after all. 

In this post, I’m going to loosely follow the structure of Chapter 6 of Phillips et al.’s Physical Biology of the Cell, 2nd Edition [1], which is an absolutely fantastic textbook, while adding my own commentary and emphasis on certain points. We start by introducing the Boltzmann distribution and why it is useful, then present a detailed derivation of the distribution that incorporates information theory, Lagrangian multipliers and optimization. In the next post, we’ll go on to look at the Law of Mass Action and relate it back to the probability distribution captured by the Boltzmann distribution. 

## The Boltzmann Distribution

In studying physical systems at (or near equilibrium), it is useful to define the probability that we will observe the system in a given state. This probability is described by the **Boltzmann distribution**, which can be written as [1]: 

$$p(E_i) = \frac{1}{Z} e^{\frac{-E_i}{k_BT}}$$

The Boltzmann distribution is a central concept in statistical mechanics that many scientists argue is as important as Newton’s laws of motion [1]. This entire blog post is going to unpack how this probability distribution can be used to make predictions about how systems behave at equilibrium. Let’s start by understanding this equation. 

Each state has an energy associated with it, $$E_i$$, and so we are interested in computing the probability of finding a particular state, $$i$$, with a particular energy. This probability is proportional to the exponential of the energy divided by $$k_B T$$, where $$k_B$$ is the Boltzmann constant and $$T$$ is the temperature. The normalizing constant, $$\frac{1}{Z}$$, serves to ensure that the right hand side evaluates to a value in the range [0, 1] as required by probability theory. In particular, $$Z$$ is known as the **partition function** . Since we know that $$\sum_{i=1}^N p(E_i) = 1$$ from probability theory, we can write the partition function as [1]: 

$$Z = \sum_{i=1}^N e^{\frac{-E_i}{k_B T}}$$

The Boltzmann distribution, as we’ll see, is massively useful for many reasons, but one intuitive reason is that it can be used to compute the expectation of the energy for a system, as follows [1]:

$$E[E] = \sum_{i=1}^N E_i p(E_i)$$

Where we overload notation to say that the expectation of the energy is $$E[E]$$. We can use this expression to write the average energy (i.e. the expectation) as [1]: 

$$\bar{E} = \sum_{i=1}^N E_i p(E_i)$$

$$\bar{E} = \frac{1}{Z} \sum_{i=1}^N E_i e^{\frac{-E_i}{k_B T}}$$

$$\bar{E} = -\frac{1}{Z} \frac{\partial}{\partial \beta} Z$$  

Where $$\beta = \frac{1}{k_B T}$$ and we can see that if we take the derivative of $$e^{-\beta E_i}$$ (which is another way of writing $$Z$$) with respect to $$\beta$$, then we get $$\frac{\partial Z}{\partial \beta} = -E_i e^{-\beta E_i}$$. Also, given that the derivative of a logarithm is $$\frac{d \ln f(x)}{dx} = \frac{1}{f(x)} \frac{df(x)}{dx}$$ then we can further compress this to write [1]: 

$$\bar{E} = - \frac{\partial}{\partial \beta} \ln Z$$

Okay, now that we have a basic handle on the Boltzmann distribution and the average energy derived from the probability distribution, let’s understand where this distribution comes from. 

## Cool Math Concepts We Need to Find the Boltzmann Distribution

In [1], the authors provide 3 different derivations of the Boltzmann distribution, but I’ve chosen to present just my favorite here because it incorporates a little information theory (which also makes its way into a number of machine learning techniques) as well as Lagrangian multipliers and optimization concepts (which are also essential in deep learning). I just think it's so cool to see all of these mathematical tools come to bear on a modeling problem in biology!

This derivation is basically going to find the Boltzmann distribution by guessing what the probability distribution for the system’s energy is, given some limited knowledge of the system, like its average energy. We’ll make this guess using information theory and refine it using constraints and Lagrangian multipliers [1]. In this section, we’ll develop those tools before we use them in the next section.

### Shannon Entropy

First of all, we define the information (or Shannon, named after Claude Shannon) entropy $$S$$ as [1]: 

$$S(p_1, p_2, …, p_N) = - \sum_{i=1}^N p_i \ln p_i$$

Where $$p_i$$ is the probability of the $$i$$-th state. This quantity tells us how much missing information we have (other formulations of the information entropy represent it as $$H(x)$$ [2]). For example, if we are predicting the outcome of a coin toss, the maximum uncertainty (or entropy) is when $$p = 0.5$$, because then either outcome is equally likely. However, if someone told us that the outcome is definitely going to be heads ($$p = 1$$) or tails ($$p = 0$$), then our uncertainty would be 0, since we already know what’s going to happen [2]. 

We are going to use Shannon entropy to select a guess for a probability distribution. Specifically, we are going to choose a probability distribution that _maximizes_ the Shannon entropy. This seems counterintuitive, because it amounts to choosing the probability distribution that ensures you are missing the most amount of information possible! Wouldn’t you want to choose a distribution that minimizes your missing information? Phillips et al. argue that what you are doing by maximizing the Shannon entropy is finding a distribution with the _least bias_ [1]. We’ll see how this works by using this method to find (1) the probability distribution for a fair 6-sided die and then (2) the probability distribution for a biased die. But first, I want to introduce Lagrangian multipliers as another mathematical tool in our toolbox. 

### Lagrange Multipliers
In optimization problems, we often want to incorporate constraints on our function to be optimized. The canonical way of writing this is [3]: 

$$\min_x f(x)$$

$$\text{Subject to: } g_i(x) \leq 0, i = 1, …, m$$

$$\text{                  } h_j(x) = 0, j = 1, …, p$$

Now, solving this problem with these constraints can be trivial if you have a really simple system. For example, if you want to maximize the area of a rectangle, $$A = xy$$, while fixing the perimeter, $$P = 2x + 2y$$, you could solve for $$y$$ using your constraint equation and plug it into the expression for the area, and then just maximize the area with respect to your one remaining parameter, $$x$$ [1]. 

However, this method is unwieldy if you have complicated, nonlinear functions and constraints. This is where Lagrange multipliers come to the rescue. You could recast your expression for the area of the rectangle as a function of both the area and the perimeter [1]: 

$$A_{\mathcal{L}}(x, y) = A(x, y) + \lambda(P - 2x - 2y)$$

Where $$\lambda$$ is the Lagrange multiplier. Now we take the partial derivatives of $$A_{\mathcal{L}}$$ with respect to $$x, y$$ and $$\lambda$$, and set them equal to 0. 

This approach, put more generally, amounts to recasting a constrained minimization problem as a Lagrangian function [4]:

$$\mathcal{L}(x) = f(x) + \lambda g(x)$$

Where $$f(x)$$ is the original function and $$g(x)$$ is the equality constraint from the original problem formulation. The Karuhn-Kush-Tucker conditions generalize this method further and also allow you to incorporate inequality constraints [4]. But the Lagrangian formulation is enough for our purposes today. 

### Finding the Probability Distribution for a Fair 6-Sided Die

Now let’s use the Shannon entropy and Lagrange multipliers to find the unknown probability distribution for a fair, 6-sided die. Obviously you already know that the probability of a fair 6-sided die rolling on any one side is ⅙, so this example is a little trivial. But once we get the hang of how to use our mathematical tools here, we’ll go after something more complicated. Let’s begin by setting up the problem - we have a fair die with $$N = 6$$ possible outcomes. We are going to try to maximize the Shannon entropy, subject to any constraints - and in our case, our only constraint is that the probability distribution we predict must be normalized. Using all of our cool new math tools, we can write this as [1]: 

$$S’ = - \sum_i p_i \ln p_i - \lambda \left( \sum_i p_i - 1 \right)$$

We want to maximize $$S’$$, so we take the derivative with respect to $$p_i$$ and $$\lambda$$ and set both expressions equal to 0 [1]: 

$$\frac{\partial S’}{\partial p_i} = 0 = -\ln p_i - p_i \frac{1}{p_i} - \lambda = - \ln p_i - 1 - \lambda$$  
$$\frac{\partial S’}{\partial \lambda} = 0 = - \left( \sum_i p_i - 1 \right)$$  

Notice that the partial derivative with respect to $$\lambda$$ is just the normalization constraint. If we take the partial derivative with respect to $$p_i$$, then we see that we can solve it as [1]: 

$$p_i = e^{-1 - \lambda}$$

And just by looking at this equation we can see that it does not depend on the state, $$i$$, so all outcomes must be equally likely, which we know has to be true for a fair die [1]. 

Now we need to solve for $$\lambda$$, so we apply the constraint to our expression for $$p_i$$ [1]: 

$$\sum_{i=1}^N e^{-1 - \lambda} = 1$$

$$e^{-1 - \lambda} = \frac{1}{N}$$

And so this implies that the probability for each state is $$p_i = \frac{1}{N}$$, which is what we were expecting to find in this example. 

### Finding the Probability Distribution for a Biased 6-Sided Die
Okay, that was cute, but it wasn’t really that difficult to figure out that the probability of rolling a 6 is $$\frac{1}{6}$$ for a fair die. But now let’s say we’re working with a biased die and we need to work out the probability distribution for it. We have rolled the die many times and calculated the average value as [1]: 

$$\bar{i} = \frac{1}{N} \sum_{n=1}^N i_n$$ 

Where $$i_n$$ is the outcome of the $$i$$-th roll. Normally, for a fair die this value is 3.5, but let’s assume we measure something different, like 4.5. Again, we can write the expectation for this die (which is the same as the average value) as [1]: 

$$\bar{i} = \sum_{i=1}^6 p_i i$$

Now we can again look for a probability distribution by maximizing our Shannon entropy and incorporating our constraints with Lagrange multipliers. Specifically [1]: 

$$S’ = - \sum_{i = 1}^N p_i \ln p_i - \lambda \left( \sum_i p_i - 1 \right) - \mu \left( \sum_i i p_i - \bar{i} \right)$$

Okay, let’s unpack this. The first term is just the Shannon entropy as before. The second term is our normalization constraint. The third term is a second constraint with Lagrange multiplier $$\mu$$ that incorporates our expression for the average value given above. We take the derivative of this expression with respect to the probability to obtain [1]: 

$$\frac{\partial S’}{\partial p_i} = 0 = - \ln p_i - 1 - \lambda - \mu i$$

We can solve for $$p_i$$ to get [1]:

$$p_i = e^{-1 - \lambda} e^{-\mu i}$$

And again we can impose the normalizing condition to get [1]: 

$$ \sum_{i = 1}^6 e^{-1 - \lambda} e^{-\mu i} = 1$$

I can eliminate $$\lambda$$ from this expression to write the probability distribution as [1]: 

$$p_i = \frac{ e^{- \mu i}}{\sum_{i=1}^6 e^{- \mu i}}$$

Now I’m almost there - I have the expression for the probability distribution in terms of one parameter, $$e^{- \mu}$$, which I can replace with $$x$$ for simplicity. This would give me a polynomial expression for $$p_i$$ [1]: 

$$p_i = \frac{x^i}{x + x^2 + x^3 + x^4 + x^5 + x^6}$$ 

And from here I could write an expression for the average value of the biased die in terms of $$x$$ and solve it for the value that I measured experimentally [1]. 

Okay, that was really kind of beautiful and fun, but now we have to relate this approach back to finding the Boltzmann distribution, the problem that got us into this mess in the first place. 

## Deriving the Boltzmann Distribution 
We now have the tools we need to find the Boltzmann distribution by maximizing the Shannon entropy, subject to some constraints. In this case, like the biased die, we have two constraints: that our probability distribution must be normalized, and that we can write the average energy of a system as an expectation. Let’s see this in math [1]: 

$$S’ = - \sum_i p_i \ln p_i - \lambda \left( \sum_i p_i - 1 \right) - \beta \left( \sum_i p_i E_i - \bar{E} \right)$$

As before, when we take the derivative with respect to $$p_i$$ and set it equal to 0 we will get an expression for the probability distribution as $$p_i = e^{-1 - \lambda - \beta E_i}$$. If we impose the normalization constraint then we get [1]: 

$$e^{-1 - \lambda} = \frac{1}{\sum_i e^{- \beta E_i}}$$

This is also our normalization constant, making the partition function $$Z = \sum_i e^{- \beta E_i}$$. Finally, we can write the probability distribution out as [1]: 

$$p_i = \frac{e^{-\beta E_i}}{\sum_i e^{-\beta E_i}}$$

The final step in this derivation is to solve for $$\beta$$. Spoiler alert: $$\beta = \frac{1}{k_B T}$$. But how do we get this? Let’s consider a specific system - an ideal gas, and in particular a single molecule in this system. We want to find the probability distribution for the momentum in the x-direction for this molecule, i.e. $$P(p_x)$$. The energy associated with this particular value of momentum is [1]: 

$$E = \frac{p_x^2}{2m}$$

Which we obtain by recognizing that the kinetic energy is $$\frac{1}{2} m v_x^2$$ and $$p_x = m v_x$$. We can substitute this expression for the energy into the probability distribution we obtained above [1]: 

$$P(p_x) = \frac{e^{- \beta \left( p_x^2 / 2m \right)}}{ \sum_{\text{states}} e^{- \beta \left( p_x^2 / 2m \right)}}$$

Now, in reality the number of possible states isn’t discrete, its continuous, so we can write the denominator as an integral [1]: 

$$\int_{-\infty}^{\infty} e^{- \beta p_x^2 / 2m} d p_x = \sqrt{ \frac{2m\pi}{\beta}}$$

Where did _that_ result come from, you ask? Please see [1] for the full explanation, because I don’t want to interrupt the flow here but I don’t want to leave you hanging, either. But assuming that this is true, we can move on to the next step which is to apply the constraint on the average energy. We have to use a fact from statistical mechanics (without the derivation, I’m afraid) that says that the average energy is always [1]: 

$$\bar{E} = \frac{1}{2} k_B T$$ 

Which we’ll use in a minute. First, let’s rewrite the average energy (or the expectation) [1]: 

$$\bar{E} = \frac{ \int_{-\infty}^{\infty} \frac{p_x^2}{2m} e^{- \beta (p_x^2 / 2m)} dp_x}{\sqrt{2m\pi / \beta}}$$

From here, we can replace $$\alpha = \beta / 2m$$ and solve the integral to obtain, at last [1]: 

$$\beta = \frac{1}{k_B T}$$

If we substitute this back into our expression for the probability distribution, we will see that we obtain the Boltzmann distribution!

## Conclusion
Okay, that was a ton of math but hopefully you enjoyed the whirlwind tour of different useful tours in our quest to derive the Boltzmann distribution. It will have been worth it, because we can use these tools in many other places in this area of biology, as well. In my next post, I’ll apply the Boltzmann distribution to modeling equilibrium using the Law of Mass Action. 

## References
[1] Phillips, R., Kondev, J., Theriot, J., Garcia, H. Physical Biology of the Cell, 2nd Edition. Garland Science, 2013. 

[2] “Entropy (information theory).” Wikipedia. <https://en.wikipedia.org/wiki/Entropy_(information_theory)> Visited 29 May 2022. 

[3] “Optimization problem.” Wikipedia. <https://en.wikipedia.org/wiki/Optimization_problem> Visited 29 May 2022. 

[4] “Lagrange multiplier.” Wikipedia. <https://en.wikipedia.org/wiki/Lagrange_multiplier> Visited 29 May 2022. 
