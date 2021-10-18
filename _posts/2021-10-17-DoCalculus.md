---
title: Pearl's Do-Calculus for Structural Causal Models
layout: post
---

In my last post, I introduced the idea of structural causal models (SCMs), and how you can use them to perform interventions and study counterfactuals. In this post, we are going to build on this idea by studying **Pearl’s augmented SCM** and the associated rules of **do-calculus** that we can use to write the conditional probabilities of an intervention on an SCM. We will end with a discussion about what to do when we encounter **confounding variables** [1]. 

## Pearl’s Augmented SCM

One way to write the conditional probabilities of an SCM that we have intervened on is to study an augmented SCM using a directed acyclic graph (DAG), G+. Before we dive into this, let’s take a quick step back and redefine for ourselves what an ordinary SCM, $$\mathcal{C}$$ with a DAG G looks like. We can say that an ordinary SCM is described by a DAG G and some factors (also called **structural assignments**) [1]: 

$$X_j = f_j(\text{PA}_j, N_j)$$

We say that $$\text{PA}_j$$ are the parents of $$X_j$$ in the DAG G, and the $$N_j$$ terms are independent noise variables. These factors $$f_j$$ are just another way of writing the conditional distribution of $$X_j$$ given $$\text{PA}_j$$. We can also say that $$\text{PA}_j$$ are the causes of $$X_j$$ [1]. 

Now, if we want to intervene on this SCM $$\mathcal{C}$$ with DAG G, we would need to replace at least one of the factors, like so [1]: 

$$X_k = \tilde{f}(\tilde{\text{PA}}_k, \tilde{N}_k)$$

The new SCM, $$\tilde{\mathcal{C}}$$, is called an intervention SCM and the probability distribution corresponding to this SCM is called the **intervention distribution**. To write this new distribution we can say [1]: 

$$P^{\tilde{C}} = P^{do(X_k := \tilde{f}(\tilde{\text{PA}}_k, \tilde{N}_k)}$$

Now that we have understood this, let’s return to the beginning of this section and explain what an augmented DAG G+ would look like. An augmented DAG G+ has all parentless nodes $$I_j$$ pointing to variables $$X_j$$ for all $$j \in [p]$$. We say that $$I_j$$ are members of the union of the domain of $$X_j$$ and all of the non-intervention cases, that is [1]: 

$$I_j \in \{\text{no intervention}\} \cup \mathcal{X}_j$$

If the nodes $$I_j = \{ \text{no intervention} \}$$ then no intervention has occurred on those nodes. Conversely, if $$I_j \in \mathcal{X}_j$$  then that means that we have intervened on $$X_j$$ and set it equal to the value $$I_j$$ [1]. 

Given this information about the role of $$I_j$$, I can now define the augmented SCM $$\mathcal{C}+$$ as containing factors [1]: 

$$X_j = \bar{f}_j (\text{PA}_j(G+), N_j)$$

I can expand this [1]:

$$X_j = f_j (\text{PA}_j, N_j) \mathbb{I}[I_j = \text{no intervention}] + I_j \mathbb{I}[I_j \neq \text{no intervention}]$$

This says that $$X_j$$ is equal to the factors $$f_j$$ that were not intervened on, in addition to the factors $$I_j$$ that were intervened on [1]. 

From this expression we can write the intervention distribution [1]: 

$$P_{Y|I_j = x_j}^{\mathcal{C}+} = P_{Y}^{\mathcal{C} ; do(X_j = x_j)}$$

This says that the probability distribution of Y when there has been an intervention on $$X_j$$ (RHS) is equal to the probability distribution of the augmented SCM $$\mathcal{C}+$$’s distribution for Y conditioned on the interventions $$I_j$$. The statement above also leads to the following [1]: 

$$P^{\mathcal{C} ; do(X = x)}(Y) = P^{\mathcal{C}}(Y | X = x)$$

This is true if Y is d-separated from $$I_X$$ given $$X$$ in the augmented DAG G+. We can derive this for ourselves as follows [1]: 

$$P^{\mathcal{C} ; do(X = x)}(Y) = P^{\mathcal{C}+}(Y | I_X = x)$$

$$P^{\mathcal{C} ; do(X = x)}(Y) = P^{\mathcal{C}+}(Y | I_X = x, X = x)$$

Given that Y is d-separated from $$I_X$$, we can remove that from the conditional probability [1]: 

$$P^{\mathcal{C} ; do(X = x)}(Y) = P^{\mathcal{C}+}(Y | X = x)$$

And we know that the marginal distribution over the non-intervened factors is the same for both the original SCM and the augmented one so [1]: 

$$P^{\mathcal{C} ; do(X = x)}(Y) = P^{\mathcal{C}}(Y | X = x)$$

There is another fact we can derive in a similar style [1]: 

$$P^{\mathcal{C} ; do(X = x)} (Y) = P^{\mathcal{C}}(Y)$$

This is true if Y is d-separated from $$I_X$$ in G+ [1]. 

## Pearl’s Do-Calculus

These 2 rules that we wrote in the previous section can be extended to develop a “calculus” for writing intervention conditionals for SCMs. In this section we will write down all 3 rules explicitly. 

### Insertion/Deletion of Observations

If W is d-separated from Y given Z and X in the graph $$G_Z^+$$ (where $$G_Z^+$$ is the augmented graph G+ with the incoming edges to Z removed) then [1]: 

$$P^{\mathcal{C} ; do(Z=z)}(Y | X, W) = P^{\mathcal{C}; do(Z=z)}(Y|X)$$

### Action/Observation Exchange

If Y is d-separated from $$I_X$$ given X, Z and W in the graph $$G_Z^+$$, then [1]: 

$$P^{\mathcal{C}; do(Z=z, X=x)}(Y|W) = P^{\mathcal{C}; do(Z=z)}(Y|X=x, W)$$

The simplest case of this rule, where $$Z = W = 0$$, is the same as one of the rules we looked at in the previous section, namely [1]: 

$$P^{\mathcal{C}; do(X=x)}(Y) = P^{\mathcal{C}}(Y|X=x)$$

This is true if Y is d-separated from $$I_X$$ given X in the graph G+. 

### Insertion/Deletion of Actions

Finally, we can say that if Y is d-separated from $$I_X$$ given Z and W in the graph $$G_Z^+$$, then [1]:

$$P^{\mathcal{C}; do(Z=z, X=x)}(Y|W) = P^{\mathcal{C}; do(Z=z)}(Y|W)$$

And this relates to our other rule from the previous case if we set $$Z = W = 0$$, that is [1]:

$$P^{\mathcal{C}; do(X=x)}(Y) = P^{\mathcal{C}}(Y)$$ 

This is true if Y is d-separated from $$I_X$$ in G+. 

## Confounding Variables

The mathematical definition of confounding variables says that if we have an SCM $$\mathcal{C}$$ with a directed path from X to Y, then the causal effect from X to Y is confounded if [1]:

$$P^{\mathcal{C}; do(X=x)} (Y) \neq P^{\mathcal{C}}(Y | X=x)$$

Otherwise the causal effect is not confounded. Confounding variables can be used to explain the dependence between X and Y. Let’s consider an example to clarify this idea. 

Let’s say we want to understand the patient’s probability of recovering, R, from a disease given some treatment, T. The treatment, T, is causally related to the recovery, R. Both of these random variables are binary, i.e. [1]:

$$T, R \in \{0, 1\}$$

We want to know the effect that the treatment has on the patient’s recovery, specifically [1]: 

$$P^{\mathcal{C}; do(T=1)} (R = 1) - P^{\mathcal{C}; do(T=0)}(R=1)$$

We find that this difference is **not** equal to the difference between the conditional probabilities of recovery [1]: 

$$\neq P^{\mathcal{C}}(R = 1 | T=1) - P^{\mathcal{C}}(R = 1 | T=0)$$

This indicates that there is a confounding variable present between T and R, which we will call Z. If we look at the original and intervention SCM for this system, we can see that there are some conditionals in the the intervention probability distribution that are equal to the original probability distribution, such as [1]: 

$$P^{\mathcal{C}; do(T=1)}(R=1|Z=z) = P^{\mathcal{C}}(R=1 | T=1, Z=z)$$
$$P^{\mathcal{C}; do(T=1)}(Z=z) = P^{\mathcal{C}}(Z=z)$$

These equalities allow us to rewrite the intervention probability distribution in terms of conditional probabilities that we can observe, that is [1]: 

$$P^{\mathcal{C}; do(T=1)}(R=1) &= \sum_z P^{\mathcal{C}; do(T=1)}(R=1, T=1, Z=z)$$  
$$= sum_z P^{\mathcal{C}; do(T=1)}(R=1 | T=1, Z=z) P^{\mathcal{C}; do(T=1)}(T=1, Z=z)$$  
$$= sum_z P^{\mathcal{C}; do(T=1)}(R=1 | T=1, Z=z)P^{\mathcal{C}; do(T=1)}(Z=z)$$  
$$= \sum_z P^{\mathcal{C}}(R=1 | T=1, Z=z) P^{\mathcal{C}}(Z=z)$$  

All of the probabilities in the last line of the equation above are things that we can observe without intervening on the SCM. In this situation, we can see that Z is a **valid adjustment set** for X and Y because by taking it into account we can completely define the causal relationship for X and Y now [1].

Okay, I think that’s all I want to say today on this topic. Stay tuned for more posts on performing inference with PGMs!

## References
[1] Ravikumar, P. “Causal GMs.” 10-708: Probabilistic Graphical Models. 2021. Class notes.





