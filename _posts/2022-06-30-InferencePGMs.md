---
title: Inference with Probabilistic Graphical Models
layout: post
---

In a [previous blog post](https://sassafras13.github.io/GNN/), I wrote about message passing with probabilistic graphical models (PGMs), which is used to perform inference (i.e. calculating a probability). Today, I wanted to explore some other approaches to inference, with a particular focus on algorithms for **approximate inference**. In exact inference, you need to be able to calculate the probabilities for every possible case in your model, and this can quickly become intractable, which is why we use approximate inference methods that avoid this problem. In this post, I will again revisit some of the basic concepts around inference that motivate the need for approximate inference, then I will discuss some specific approximate inference algorithms. 

## Inference with PGMs

Inference means that we use a known probabilistic graphical model to answer questions about the system that we have modeled [1]. In PGM theory, we can usually categorize inference questions into two types [1]: 

1. **Marginal inference:** We compute the probability of one random variable for all values of other random variables in the model. We do this by summing over the other variables in the model except for the variable of interest: 

$$p(x_1) = \sum_{x_2} … \sum_{x_n} p(x_1, x_2, …, x_n)$$

2. **Maximum a posteriori (MAP) inference:** Here we want to know the most likely values of all of the random variables in the PGM. Instead of summing over the random variables, we compute the argmax: 

$$\text{argmax}_{x_1, …, x_n} p(x_1, …, x_n, y = 1)$$

In this example, $$y$$ is some evidence that we have which helps to fix the values of some of the random variables [1]. This blog post is going to be centered around the MAP inference problem, but before we dive into this, it is worth clarifying that we can answer these inference questions with **exact inference** or **approximate inference**. Exact inference is really only possible for smaller, less complex models because we need to be able to calculate the grand partition function, which is a sum over all the possible states of the model. If there are too many states to enumerate explicitly, then we turn to approximate inference instead. 

## MAP vs. MLE, Revisited

I have a [previous blog post](https://sassafras13.github.io/MLEvsMAP/) where I do a deep dive into how MAP and maximum likelihood estimation (MLE) compare, but I will quickly recap the two ideas here because I want to make clear how MAP uses Bayesian inference while MLE does not*1. Both MAP and MLE are methods for solving the same problem: what probability distribution (described by the parameters of the PGM, in our case) is most likely to give rise to the observed data? In other words, what is the most likely explanation for the data that we are seeing? 

The primary difference between MLE and MAP is that MLE uses _only_ the observed data to answer this question, while MAP allows us to incorporate prior knowledge of the system. We may prefer to use the MAP method when we have a small dataset where it would be difficult to find accurate parameters using the data alone. 

Let’s look at this first by writing down the MLE in math. We use MLE to find the parameters for the probability distribution (the PGM) that maximize some likelihood function - in other words, these parameters for the probability distribution will maximize the likelihood that the distribution returns the observed data [2]. The maximum likelihood estimate is this set of parameters, which we can write as $$\theta = [\theta_1, …, \theta_k]$$. The distribution parameterized by $$\theta$$ is expressed as a parametric family [2]: 

$$\{ f(\cdot ; \theta) | \theta \in \Theta \}$$

Where $$\Theta$$ is the parameter space that we are searching through. The observed data is $$\mathbf{y} = (y_1, …, y_n)$$, and the likelihood function is $$\mathcal{L}_n(\theta ; \mathbf{y}) = f_n(\mathbf{y} ; \theta)$$ [2]. If the random variables are i.i.d., then we can write $$f_n(\mathbf{y} ; \theta)$$ as a product of density function for each random variable, that is [2]: 

$$f_n(\mathbf{y} ; \theta) = \prod_{k=1}^n f_k(y_k ; \theta)$$

Our goal is to find the set of parameters, $$\theta$$, that maximize the likelihood function, i.e. [2]: 

$$\theta^* = \text{argmax}_{\theta \in \Theta} \mathcal{L}_n (\theta ; \mathbf{y})$$

(Note that it can be useful, mathematically, to use the log-likelihood version of this definition, $$\mathcal{l}(\theta ; \mathbf{y}) = \ln \mathcal{L}_n (\theta ; \mathbf{y})$$ [2].) 

Now that we have the MLE in hand, let’s see how the MAP compares. Remember, the MAP is doing the same thing as the MLE but it also includes a prior distribution. Specifically, the MAP assumes that the parameters $$\theta$$ have some prior distribution, $$g$$ [3]. Then we can write the posterior distribution of $$\theta$$ as [3]: 

$$f_n(\theta ; \mathbf{y}) = \frac{f_n( \mathbf{y} | \theta) g(\theta)}{\int_{\Theta} f( \mathbf{y} | \mathcal{v}) g(\mathcal{v}) d \mathcal{v}}$$

The MAP method assumes that $$\theta$$ can be computed as the argmax (i.e. the mode) of the posterior distribution [3]: 

$$\theta^* = \text{argmax}_{\theta} f_n(\theta | \mathbf{y})$$

$$\theta^* = \text{argmax}_{\theta} \frac{f_n( \mathbf{y} | \theta) g(\theta)}{\int_{\Theta} f( \mathbf{y} | \mathcal{v}) g(\mathcal{v}) d \mathcal{v}}$$

Here, we can disregard the denominator (the **marginal likelihood**) in the Bayesian expression for the posterior distribution. This is because it is always positive, and does not depend on $$\theta$$ and so will not affect the value of $$\theta^*$$. This allows us to write the MAP estimate more simply as [3]: 

$$\theta^* = \text{argmax}_{\theta} f_n( \mathbf{y} | \theta) g(\theta)$$

In this form, it is easier to see that the MAP and MLE methods return the same value for $$\theta^*$$ when $$g(\theta)$$ is a uniform distribution. Let me also write the MAP in terms of a UGM just to connect this back to our previous discussion about PGMs [4]. First of all, we can write the joint probability represented by a UGM as [4]: 

$$p(x_1, …, x_n) = \frac{1}{\mathcal{Z}} \prod_{c \in \mathcal{C}} \phi_c (x_c)$$

And the MAP is looking for the values of $$x$$ that maximize the log-likelihood, i.e. [4]: 

$$\max_x \log p(x) = \max_x \sum_c \log \phi_c(x_c) - \log(\mathcal{Z})$$

Okay, so now we have a good understanding of how we can use the MLE and MAP methods for inference, and how the MAP method relies on having a prior distribution over $$\theta$$, which we can obtain from our PGM [3]. Now that we understand MAP inference, we can examine a few different methods for computing the estimate when we must do approximate inference. 

## Approaches to Solving the Approximate Inference Problem

The methods available for solving the approximate inference problem can be broadly divided into two categories: **sampling** and **variational inference**. I will write detailed explanations for some methods within these categories in future posts, but for now let me give an introduction to each of them. 

Sampling methods - which are an older class of methods than variational ones - can be used to do both marginal and MAP inference, as well as computing expectations [5]. The advantage to using sampling algorithms like the Metropolis-Hastings algorithm is that we can compute a probability $$p(x)$$ if we know a function $$f(x)$$ that is _proportional_ to the probability density $$p$$ - since $$f(x)$$ only has to be proportional to $$p(x)$$, we do not need to compute the grand partition function, $$\mathcal{Z}$$, which is intractable [6]. We sample from $$f(x)$$ many times and accept highly probable samples, thereby indicating what the expectation and the MAP for this probability $$p(x)$$ should be [6]. 

Sampling methods have been in use for a long time, but they have some significant downsides which variational methods can overcome. For example, although sampling methods are guaranteed to eventually find a globally optimal solution, it is hard, in practice, to know when they have approached a good solution. Moreover, choosing the sampling technique for some algorithms can have a large effect on the process and is therefore something of an art form [7]. 

Variational inference*2, by contrast, presents the inference problem as an optimization problem. Specifically, if our goal is to compute the MAP for an intractable probability distribution, $$p$$, then our goal is to find a tractactable probability $$q \in \mathcal{Q}$$ that is the most similar distribution to $$p$$. Then we use $$q$$ to perform inference tasks instead of $$p$$. The benefit of this approach is that we have a deep body of knowledge supporting optimization problem solving methods. This brings its own pros and cons to using variational methods. For instance, variational methods are not guaranteed to find the globally optimal solution. However, it is possible to know if they have converged (unlike sampling methods) and they tend to scale better with modern hardware like GPUs [7]. 

I will end this post here, but in my next couple of posts, I will write about sampling and variational inference methods in more detail to better understand how they work. 

## Footnotes
*1 I should point out that this statement is not strictly true. Since MLE can be seen as a special case of MAP, you could say that MLE is a form of Bayesian inference where we assume the priors of the parameters are uniformly distributed [2]. 

*2 Variational inference is called that because it is derived from the calculus of variations, which optimizes functionals (functions of functions) [7]. 

## References

[1] Kuleshov, V. and Ermon, S. “Introduction.” CS228 Probabilistic Graphical Models, 2022. Stanford University. <https://ermongroup.github.io/cs228-notes/preliminaries/introduction/> Visited 30 Jun 2022. 

[2] “Maximum likelihood estimation.” Wikipedia. <https://en.wikipedia.org/wiki/Maximum_likelihood_estimation> Visited 30 Jun 2022. 

[3] “Maximum a posteriori estimation.” Wikipedia. <https://en.wikipedia.org/wiki/Maximum_a_posteriori_estimation> Visited 30 Jun 2022. 

[4] Kuleshov, V. and Ermon, S. “MAP inference.” CS228 Probabilistic Graphical Models, 2022. Stanford University. <https://ermongroup.github.io/cs228-notes/inference/map/> 

[5] Kuleshov, V. and Ermon, S. “Sampling methods.” CS228 Probabilistic Graphical Models, 2022. Stanford University. <https://ermongroup.github.io/cs228-notes/inference/sampling/> Visited 30 Jun 2022. 

[6] “Metropolis-Hastings algorithm.” Wikipedia. <https://en.wikipedia.org/wiki/Metropolis%E2%80%93Hastings_algorithm> Visited 30 Jun 2022. 

[7] Kuleshov, V. and Ermon, S. “Variational inference.” CS228 Probabilistic Graphical Models, 2022. Stanford University. <https://ermongroup.github.io/cs228-notes/inference/variational/> Visited 30 Jun 2022. 

