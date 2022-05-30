---
title: More Cooperativity Math!
layout: post
---

We have already examined cooperativity and several models that capture this phenomenon in a [previous post](https://sassafras13.github.io/Cooperativity/), but today I would like to dive into the mathematics of these models in a little more detail. I think that the way these models are developed is a useful inspiration for representing multivalent systems. In this post, we’ll first introduce another handy probability distribution, the Gibbs distribution, and then we’ll use it to derive a simple model of ligand binding with no cooperativity, and compare this to the MWC, Pauling and Adair models that incorporate cooperativity. Let’s get to it!

## The Gibbs Distribution

Previously, we derived an expression for the [Boltzmann distribution](https://sassafras13.github.io/BoltzmannDistribution/), which conveyed the probability of observing a system in a particular state. The Boltzmann distribution can be written as [1]:

$$p(E_i) = \frac{1}{Z} e^{\frac{-E_i}{k_B T}}$$

The Gibbs distribution is different from the Boltzmann distribution because it includes information about how the system is coupled to a larger reservoir that contains particles and energy, whereas the Boltzmann distribution only assumed that the system was in contact with an energy reservoir. Adding a particle reservoir to the paradigm allows us to study ligand binding more realistically because it represents how the number of ligands available in the environment (both the system and the reservoir) affects the reaction [1]. 

Let me just state that the Gibbs distribution can be written as follows, so you can see the differences between this equation and the Boltzmann distribution [1]:

$$p(E_s^{(i)}, N_s^{(i)}) = \frac{e^{-\beta(E_s^{(i)} - \mu N_s^{(i)})}}{\mathcal{Z}}$$

We will define everything in this expression in a moment, but for now let’s just observe that this expression includes additional information about the number of particles $$N_s$$ and their chemical potential, $$\mu$$. 

Now that we’ve seen how the Gibbs distribution is different from the Boltzmann distribution, let’s step through the derivation. Imagine that we are modeling a system as an open container of air in a warm room. The container is our system, and the room is our reservoir. The total number of particles in this environment is the sum of the particles in the system (s) and the reservoir (r), i.e. $$N_{tot} = N_s + N_r$$. Similarly, the total energy is $$E_{tot} = E_s + E_r$$ [1]. 

The key idea here is that we want to compute the probability of finding a given state of the system, which is expressed in terms of energy and number of particles. This probability is proportional to the number of states that the reservoir can take given that the system state is fixed [1]: 

$$p(E_s^{(1)}, N_s^{(1)}) \propto W_r(E_{tot} - E_s^{(1)}, N_{tot} - N_s^{(1)})$$

This might seem a little odd, but remember that if we’ve fixed the state of the system, then the only thing that can vary is the state of the reservoir. I can use the definition of entropy, $$S = k_B \ln W$$ or $$W = e^{S/k_B}$$, to rewrite this expression as [1]:

$$W_r(E_{tot} - E_s^{(1)}, N_{tot} - N_s^{(1)}) \propto e^{S_r(E_{tot} - E_s^{(1)}, N_{tot} - N_s^{(1)})/k_B}$$

I can use the Taylor expansion to rewrite this entropy as [1]: 

$$S_r(E_{tot} - E_s, N_{tot} - N_s) \approx S_r(E_{tot}, N_{tot}) - \frac{\partial S_r}{\partial E} E_s - \frac{\partial S_r}{\partial N}N_s$$

We can rewrite the derivatives in terms of thermodynamic identities, i.e. $$(\partial S / \partial E)_{V,N} = 1/T$$ and $$(\partial S / \partial N)_{E,V} = - \mu/T$$, then we can recast the probability of a given system state as [1]: 

$$p(E_s^{(1)}, N_s^{(1)}) \propto e^{-(E_s^{(1)} - \mu N_s^{(1)}) / k_B T}$$

From here we can write the equality that is the Gibbs distribution by including the **grand partition function**, $$\mathcal{Z}$$ [1]: 

$$p(E_s^{(i)}, N_s^{(i)}) = \frac{e^{-\beta(E_s^{(i)} - \mu N_s^{(i)})}}{\mathcal{Z}}$$

Where $$\beta = \frac{1}{k_B T}$$ and the grand partition function is the sum over all the states [1]: 

$$\mathcal{Z} = \sum_i e^{- \beta(E_s^{(i)} - N_s^{(i)} \mu)}$$

As we saw [previously](https://sassafras13.github.io/BoltzmannDistribution/), it is possible to compute the average quantity (in this case the average number of particles) using a number of clever mathematical tricks and the grand partition function [1]: 

$$\bar{N} = \frac{1}{\beta} \frac{\partial}{\partial \mu} \ln \mathcal{Z}$$

Now that we have the Gibbs distribution in hand, let’s use it to solve a simple problem of one ligand binding to one receptor, before moving on to more complex models that take cooperativity into account. 

## Simple Ligand Binding Problem with Gibbs Distribution

First, let’s introduce an important parameter that we’ll use repeatedly throughout this post: $$\sigma$$. We’ll use $$\sigma$$ as a binary variable that represents that either a binding event has occurred ($$\sigma = 1$$) or not ($$\sigma = 0$$). We’ll also define the binding energy as $$\epsilon_b < 0$$ (negative to indicate that energy is released during the reaction) [1]. 

In this case, we can write the grand partition function as the sum of the two states of the system, the ligand bound to the receptor, and unbound [1]:

$$\mathcal{Z} = \sum_{\sigma = 0}^1 e^{-\beta (\epsilon_b \sigma - \mu \sigma)} = 1 + e^{-\beta(\epsilon_b - \mu)}$$

The average number of ligands bound can be solved for using the fact given above, $$\bar{N} = \frac{1}{\beta} \frac{\partial}{\partial \mu} \ln \mathcal{Z}$$, or simple probability theory [1]: 

$$\bar{N} = \frac{ e^{- \beta(\epsilon_b - \mu)}}{1 + e^{- \beta(\epsilon_b - \mu)}}$$

I can also replace $$\mu$$ with the full expression for chemical potential, $$\mu = \mu_0 + k_B T \ln(c/c_0)$$ and obtain [1]:

$$\bar{N} = \frac{(c/c_0) e^{-\beta \Delta \epsilon}}{1 + (c/c_0) e^{-\beta \Delta \epsilon}}$$

Where $$\Delta \epsilon = \epsilon_b - \mu_0$$, and it represents the energy difference between the ligand in solution and the bound ligand [1]. Now that we have worked through this simple, we have all the ideas we need to develop some more detailed models, starting with a simplified dimeric binding model that takes cooperativity into account. 

## Dimeric Binding Model Including Cooperativity

Let’s assume that we have a receptor with two binding sites that can each, separately, bind or unbind with a ligand. The energy of this system is [1]: 

$$E = \epsilon(\sigma_1 + \sigma_2) + J\sigma_1 \sigma_2$$

Where $$\epsilon$$ is the energy of one binding event, $$\sigma_i$$ describes the binding state of each of the two sites, and $$J$$ is a measure of the cooperativity and indicates that when both sites are bound to ligands, the energy released is _different_ from the sum of the energy of the two individual binding sites [1]. 

We can compute the grand partition function from summing over the four possible states of the system (both unoccupied, one of the two sites occupied, both sites occupied) as follows [1]: 

$$\mathcal{Z} = 1 + e^{-\beta(\epsilon - \mu)} + e^{-\beta(\epsilon - \mu)} + e^{-\beta(2\epsilon + J - 2\mu)}$$

And as before, we can write the average number of binding sites occupied (i.e. average occupancy) as [1]: 

$$\bar{N} = \frac{2 e^{-\beta(\epsilon - \mu)} + 2 e^{-\beta(2 \epsilon + J - 2\mu)}}{1 + e^{-\beta(\epsilon - \mu)} + e^{-\beta(\epsilon - \mu)} + e^{-\beta(2\epsilon + J - 2\mu)}}$$

This relatively simple example captures the cooperativity in the extra term $$J \sigma_1 \sigma_2$$, and shows sigmoidal behavior as the partial pressure of the ligand increases. But there are, as we’ve seen, more complex representations of this cooperativity. One such example is the Monod-Wyman-Changeux (MWC) model that we have seen before, and which we will revisit now with our awesome mathematical tools.

## The MWC Model of Cooperativity with Awesome Math

As we discussed [previously](https://sassafras13.github.io/Cooperativity/), the MWC model introduces the idea that the receptor can be in either a tense (T) or relaxed (R) state, where the T state is favored over the R state when no ligands are bound to the receptor. There is a cost, $$\epsilon$$, associated with switching to the R state from the T state. Similarly, we have binding energies associated with each state, $$\epsilon_T$$ and $$\epsilon_R$$. Each state also has $$\sigma_i$$ to represent whether each of the two binding sites is occupied, and another variable $$\sigma_m$$ to represent whether the receptor is in the T ($$\sigma_m = 0$$) or R states ($$\sigma_m = 1$$). Given all of this, the energy of the system can be written as [1]:

$$E = (1 - \sigma_m) \epsilon_T \sum_{i=1}^2 \sigma_i + \sigma_m \left( \epsilon + \epsilon_R \sum_{i=1}^2 \sigma_i \right)$$

To compute the grand partition function, we need to sum up over 8 unique cases (4 for each state, T or R). The T terms are in red and the R terms are in blue below [1]: 

$$\mathcal{Z} = \textcolor{red}{1 + 2 e^{-\beta(\epsilon_T - \mu)} + e^{-\beta(2 \epsilon_T - 2 \mu)}} + \textcolor{blue}{e^{-\beta \epsilon} ( 1 + 2 e^{-\beta(\epsilon_R - \mu)} + e^{-\beta(2 \epsilon_R - 2\mu)}) }$$

From this, the average occupancy is [1]: 

$$\bar{N} = \frac{2}{\mathcal{Z}} [x + x^2 + e^{-\beta \epsilon} ( y + y^2)]$$

Where $$x = (c/c_0)e^{-\beta(\epsilon_T - \mu_0)}$$ and $$y = (c\c_0) e^{-\beta(\epsilon_R - \mu_0)}$$. As we discussed before, although this model does not explicitly capture cooperativity, the emergent behavior of the system transitioning between T and R states will result in cooperative-like behavior. In the next section, we consider a model for a receptor with four binding sites but no cooperativity, and then add the cooperativity to build the Pauling model. 

## Four Binding Sites Without Cooperativity, then the Pauling Model

In this section we establish the mathematics for the occupancy of a receptor with 4 binding sites. First, we can write the energy as [1]: 

$$E = \epsilon \sum_{\alpha = 1}^4 \sigma_{\alpha}$$

And since there is no cooperativity, each binding event is completely independent of the others, and we can sum up all 16 possible states as [1]:

$$\mathcal{Z} = \sum_{\sigma_1 = 0}^1  \sum_{\sigma_2 = 0}^1  \sum_{\sigma_3 = 0}^1  \sum_{\sigma_4 = 0}^1 e^{-\beta(\epsilon - \mu) \sum_{\alpha=1}^4 \sigma_{\alpha}}$$

These terms can be separated out and then combined (since they’re all based on the same binding energy) into the simpler expression [1]:

$$\mathcal{Z} = (1 + e^{-\beta(\epsilon - \mu)})^4$$

And the occupancy is [1]:

$$\bar{N} = \frac{4 e^{-\beta(\epsilon - \mu)}}{1 + e^{-\beta(\epsilon - \mu)}}$$

Now let’s modify this model to include cooperativity, thereby developing the Pauling model. In this case, we are going to add a term that adds some contribution $$J$$ if two binding sites are both occupied. Specifically [1]:

$$E = \epsilon \sum_{\alpha = 1}^4 \sigma_{\alpha} + \frac{J}{2} \sum_{(\alpha, \gamma)}’ \sigma_{\alpha} \sigma_{\gamma}$$

Here, $$\alpha$$ and $$\gamma$$ are indices of two binding sites and we add energy $$J$$ if they are both occupied. Notice that the $$\sum’$$ indicates that we should not include this term if $$\alpha = \gamma$$ and we divide $$J$$ in half to account for the double contributions of $$\sigma_1 \sigma_2$$ and $$\sigma_2 \sigma_1$$ [1]. 

Again, this energy can be used to compute the grand partition function [1]:

$$\mathcal{Z} = \sum_{\sigma_1 = 0}^1  \sum_{\sigma_2 = 0}^1  \sum_{\sigma_3 = 0}^1  \sum_{\sigma_4 = 0}^1 e^{-\beta(\epsilon - \mu) \sum_{\alpha = 1}^4 \sigma_{\alpha} - \beta(J/2) \sum_{\alpha, \gamma}’ \sigma_{\alpha} \sigma_{\gamma}}$$

And this can be evaluated as a sum of 5 terms that represent 0 to 4 binding sites occupied, and 16 total states [1]: 

$$\mathcal{Z} = 1 + 4 e^{-\beta (\epsilon - \mu)} +  6 e^{-2\beta (\epsilon - \mu)-\beta J} +  4 e^{-3\beta (\epsilon - \mu)-3\beta J} +  e^{-4\beta (\epsilon - \mu)-6\beta J} $$

And then the occupancy becomes [1]: 

$$\bar{N} = \frac{4x + 12 x^2 j + 12x^3 j^3 + 4x^4 j^6}{1 + 4x + 6x^2 j + 4x^3 j^3 + x^4 j^6}$$

Where $$x = (c/c_0) e^{-\beta(\epsilon - \mu_0)}$$ and $$j = e^{-\beta J}$$ [1]. This model requires fitting only two free parameters, but the cost is that it assumes all interactions are paired, and does not take into account interactions among 3 or more binding sites. The final model we consider, the Adair model, will include these as well. 

## The Adair Model for Cooperativity

The Adair model uses the same overarching framework as the Pauling model, but includes interactions between 3 binding sites (captured by parameter $$K$$) and 4 sites ($$L$$). The math will get a little more involved to account for them, but the patterns are the same. Let’s start with the energy for the model [1]: 

$$E = \epsilon \sum_{\alpha = 1}^4 \sigma_{\alpha} + \frac{J}{2} \sum_{\alpha, \gamma}’ \sigma_{\alpha} \sigma_{\gamma} + \frac{K}{3!} \sum_{\alpha, \beta, \gamma}’ \sigma_{\alpha} \sigma_{\beta} \sigma_{\gamma} + \frac{L}{4!} \sum_{\alpha, \beta, \gamma, \delta} \sigma_{\alpha} \sigma_{\beta} \sigma_{\gamma} \sigma_{\delta}$$

The grand partition function this time is a little intimidating but we can simplify it, as shown below in two steps [1]: 

$$\mathcal{Z} = \sum_{\sigma_1 = 0}^1 \sum_{\sigma_2 = 0}^1 \sum_{\sigma_3 = 0}^1 \sum_{\sigma_4 = 0}^1 \exp \left[-\beta(\epsilon - \mu) \sum_{\alpha = 1}^4 \sigma_{\alpha} - \frac{J}{2} \sum_{\alpha, \beta}’ \sigma_{\alpha} \sigma_{\beta} - \frac{K}{3!} \sum_{\alpha, \beta, \gamma}’ \sigma_{\alpha} \sigma_{\beta} \sigma_{\gamma} - \frac{L}{4!} \sum_{\alpha, \beta, \gamma, \delta}’ \sigma_{\alpha} \sigma_{\beta} \sigma_{\gamma} \sigma_{\delta} \right]$$

$$\mathcal{Z} = 1 + 4e^{-\beta(\epsilon - \mu)} + 6e^{-2\beta(\epsilon - \mu) - \beta J} + 4 e^{-3\beta(\epsilon - \mu) - 3 \beta J - \beta K} + e^{-4 \beta(\epsilon - \mu) - 6 \beta J - 4 \beta K - \beta L}$$

And finally, the occupancy is [1]: 

$$\bar{N} = \frac{4x + 12x^2 j + 12 x^3 j^3 k + 4 x^4 j^6 k^4 l}{1 + 4x + 6x^2j + 4x^3 j^3 k + x^4 j^6 k^4 l}$$

Where $$k = e^{-\beta K}$$ and $$l = e^{-\beta L}$$. 

## Conclusion

That wraps up our detailed, math-intensive, deep dive into models of cooperativity here. I think my most useful takeaway is to see how we can incorporate cooperativity with extra terms in our energy expressions. I think this method also creates interesting opportunities for simplifying larger situations where there are enough binding sites to make this manual approach a little too cumbersome… Thanks for reading!

## References
[1] Phillips, R., Kondev, J., Theriot, J., Garcia, H. Physical Biology of the Cell, 2nd Edition. Garland Science, 2013.
