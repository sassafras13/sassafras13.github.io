---
title: The Law of Mass Action
layout: post
---

In my [last post](https://sassafras13.github.io/BoltzmannDistribution/), we learned a bunch of new mathematical tools to derive the Boltzmann distribution, which describes the probability distribution of energies for different states of a system. In this post, we are going to put all that hard work to good use by seeing how we can use the Boltzmann distribution in analyzing systems at equilibrium. Specifically, we are going to learn about the Law of Mass Action and learn to derive equilibrium constraints. The cool thing about this post is that we will learn everything we need to connect the probability distribution to thermodynamics concepts to fully describe a system at equilibrium [1]. Let’s go!

## The Law of Mass Action

In this section, we are going to derive the Law of Mass Action. This law describes how species will behave in dynamic equilibrium. Specifically, it states that the ratio between the concentrations of reactants and products must be constant, and equal to the rate of the reaction, $$K_{eq}$$ [2]. 

Let’s consider a very basic system which has two types of molecules, A and B, which are allowed to bind together to form a new molecule AB. We can write the reaction as [1]: 

$$A + B \rightarrow AB$$

We can represent the number of each species of molecule as $$N_A, N_B$$ and $$N_{AB}$$ respectively. Since this system is at equilibrium, then the change in Gibbs free energy, G, should not change [1]: 

$$dG = \left( \frac{\partial G}{\partial N_A}\right) dN_A + \left( \frac{\partial G}{\partial N_B}\right) dN_B + \left( \frac{\partial G}{\partial N_{AB}}\right) dN_{AB} = 0$$

To make this more concrete, we need to account for the fact that if $$N_A$$ and $$N_B$$ decrease by 1, then $$N_{AB}$$ increases by 1. We can do this by introducing $$v_i$$, which is a stoichiometric coefficient that represents the change in the number of molecules during the reaction. So in this case, $$v_A = v_B = -1$$ and $$v_{AB} = 1$$ [1]. 

The derivatives $$\frac{\partial G}{\partial N_i}$$ are **chemical potentials**, also written as $$\mu_i$$, and represent the free energy cost required to change the number of molecules of species $$i$$ by 1 in solution. So now I can rewrite my equation above as [1]: 

$$dG = 0 \Rightarrow \mu_A dN_A + \mu_B dN_B + \mu_{AB} dN_{AB} = 0$$

And more generally [1]: 

$$dG = \sum_{i =1}^N \mu_i dN_i = 0$$

We can redefine $$dN_i = v_i dN$$ and then write this general summation as follows [1]: 

$$\sum_i v_i \mu_i = 0$$

We can define the chemical potential as follows [1]: 

$$\mu_i = \mu_i^0 + k_B T \ln(\frac{c_i}{c_i^0})$$

Where did this come from? Let’s recall that the chemical potential is essentially a change in Gibbs free energy, which itself [we know](https://sassafras13.github.io/thermo-reaction/) can be written as $$\Delta G = \Delta H - T \Delta S$$. In the same way, a chemical potential is a sum of the nominal chemical potential (or enthalpy) and the temperature multiplied by the entropy, where the entropy is generally expressed as $$k_B \ln W$$ where $$W$$ is the number of states available to the system [1]. The expression $$\frac{c_i}{c_i^0}$$ is just a description of the concentration of the $$i$$-th state normalized by the nominal concentration.

Now we substitute this expression for the chemical potential into our summation [1]: 

$$\sum_i v_i \mu_i^0 = - k_B T \sum_{i=1}^N \ln \left(\frac{c_i}{c_i^0}\right)^{v_i}$$

Now using some logarithmic tricks, I can reshuffle this [1]: 

$$- \frac{1}{k_B T} \sum_{i=1}^N v_i \mu_i^0 = \ln \left[ \prod_{i=1}^N \left( \frac{c_i}{c_i^0} \right)^{v_i} \right]$$

Now I exponentiate both sides to get [1]:

$$\prod_{i=1}^N \left( \frac{c_i}{c_i^0} \right)^{v_i} = e^{-(1 / k_B T) \sum_{i=1}^N v_i \mu_i^0}$$

Finally, if I shuffle $$c_i^0$$ to the right hand side, I get the **Law of Mass Action** [1]: 

$$\prod_{i=1}^N c_i^{v_i} = \left( \prod_{i=1}^N c_{i0}^{v_i} \right) e^{- (1 / k_B T)  \sum_{i=1}^N v_i \mu_i^0}$$

This expression also gives us the equilibrium constant $$K_{eq}$$ [1]: 

$$K_{eq} = \left( \prod_{i=1}^N c_{i0}^{v_i} \right) e^{- \sum_i \mu_{i0}v_i / k_B T}$$

Also note that $$K_{eq} = \frac{1}{K_d}$$ where $$K_d$$ is the dissociation constant. The really cool thing about this law is that we can relate our experimental data to this rate constant and also back to the Gibbs free energy and Boltzmann distribution perspective as well, all via this one expression [1]. For example, we can write the equilibrium constant in terms of the concentrations [1]: 

$$K_{eq} = \frac{c_{AB}}{c_A c_B}$$

## Ligand-Receptor Binding with Law of Mass Action

Now that we have the Law of Mass Action well in hand, let’s use it to examine a couple of example systems at equilibrium. We’ll start with a simple ligand-receptor problem defined pretty much the same as above [1]:

$$L + R \rightarrow LR$$

We can write the dissociation constant as $$K_d = \frac{[L][R]}{[LR]}$$. The probability that the receptor will be bound is equal to [1]: 

$$p_{bound} = \frac{[LR]}{[R] + [LR]}$$

Using our expression for the dissociation constant for this reaction, we can rewrite this probability as [1]: 

$$p_{bound} = \frac{ [L]/K_d}{1 + ([L] / K_d)}$$

We can also draw on information theory again to determine the Shannon entropy of this system as a function of the concentration of [L] and $$K_d$$. Let’s write the Shannon entropy of this system as [1]: 

$$S = - p_{bound} \ln p_{bound} - p_{unbound} \ln p_{unbound}$$

And we can rewrite $$p_{bound} = \frac{l}{1 + l}$$ where $$l = \frac{[L]}{K_d}$$ for simplicity. After simplifying the expression above, we obtain [1]: 

$$S = - \frac{l}{1 + l} \ln l + \ln(1 + l)$$

And when we plot $$S$$ we can see that the information entropy (i.e. the missing information) is maximum when $$[L] = K_d$$ [1]. In the next section, let’s build on this idea but consider two ligands binding to one receptor - this will help us set up the mechanics for discussing cooperativity, which we do in more detail in [this post](https://sassafras13.github.io/Cooperativity/).

## Two Ligands to One Receptor - Cooperativity at Work

Let’s consider a slightly more complex reaction where two ligands bind to one receptor [1]: 

$$L + L + R \rightarrow L_2 R$$

Here the dissociation constant is [1]: 

$$K_d^2 = \frac{[L]^2[R]}{[L_2 R]}$$

And the probability that the receptor will have bound to both ligands is [1]: 

$$p_{bound} = \frac{([L]/K_d) ^2}{1 + ([L]/K_d)^2}$$

Where we could replace 2 with $$n$$ to be more general for $$n$$ ligands. This would give us the Hill function which was one of the first models of cooperativity introduced in biology [1]. We’ll return to this idea in a subsequent post, but before we wrap up this discussion, let’s consider one more example that shows how to link the energy released from a reaction to the concentrations of the reactants and the products. 

## ATP Hydrolysis - Energy and Concentration

When ATP is hydrolyzed, it releases useful energy in a system. The energy released depends on the concentration of ATP and its two products, ADP and inorganic phosphate [1]. Let’s develop some mathematics to capture how this dependency works.

First we recall that the change in free energy during hydrolysis is [1]:

$$dG = \sum_i \mu_i dN_i = \sum_i \mu_i v_i dN$$

And introducing our expression for chemical potentials again, we can rewrite this as [1]: 

$$\Delta G = \sum_i \left[ \mu_{i0} + k_B T \ln \left( \frac{c_i}{c_{i0}} \right) \right] v_i$$

If we use this expression above as the expression for the final free energy available, and compare it to some reference $$\Delta G_{ref}$$, then we can find that the total free energy released is [1]: 

$$\Delta G = \Delta G_{ref} + k_B T \ln \left( \frac{ \prod_{i=1}^N c_i^{v_i}}{\prod_{i=1}^N c_{i,ref}^{v_i}} \right)$$

If we consider $$\Delta G_{ref} = 0$$ because the reference state is equilibrium, then we can simplify this expression, also recognizing that $$\prod_{i=1}^N c_{i,ref}^{v_i} = K_{eq}$$ [1]:

$$\Delta G = k_B T \ln \left( \frac{\prod_{i=1}^N c_i^{v_i}}{K_{eq}} \right)$$

We can use logarithm rules and the stoichiometric ratios to rewrite this equation in its final form to [1]: 

$$\Delta G = k_B T \ln \prod_{i=1}^N c_i^{v_i} - k_B T \ln K_{eq} = - k_B T \ln K_{eq} + k_B T \ln \frac{[ADP][P_i]}{[ATP]}$$

Finally, using the Law of Mass Action, we can relate concentrations in the reaction to the free energy to be released. 

## Conclusion

I hope today’s post helped to clarify how useful the Law of Mass Action, as well as the Boltzmann distribution, can be in studying systems at or near equilibrium. Now that we’ve developed much of the mathematics necessary, I may write one more post going into more detail on some of the models used to capture cooperativity, building on my ideas from [last time](https://sassafras13.github.io/Cooperativity/). 

## References

[1] Phillips, R., Kondev, J., Theriot, J., Garcia, H. Physical Biology of the Cell, 2nd Edition. Garland Science, 2013.

[2] “Law of mass action.” Wikipedia. <https://en.wikipedia.org/wiki/Law_of_mass_action> Visited 29 May 2022. 
