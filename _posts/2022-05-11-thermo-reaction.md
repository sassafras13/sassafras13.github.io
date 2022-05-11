---
title: Thermodynamics and Reaction Kinetics, Revisited 
layout: post
---

It has been almost 10 years since I last took a thermodynamics course and so for recent research investigations, I have gone back to brush up on the basics. This post is mostly going to be a thermodynamics and rate kinetics cheat sheet. At a high level, I wanted to understand thermodynamics and rate kinetics in the context of DNA nanotechnology, because these concepts give important insight into how DNA nanostructures form. The difference between thermodynamics and rate kinetics is that thermodynamics describes what direction a reaction will take, while rate kinetics provides information on how quickly that reaction will occur [1]. We’ll start by discussing thermodynamics and then look at where rate kinetics fill in the gaps. 

## Thermodynamics

I’ll focus my discussion here on classical thermodynamics because that is the branch that is commonly used to analyze DNA nanotechnology, but it is worth noting that there are also other branches such as statistical mechanics and non-equilibrium thermodynamics. Classical thermodynamics refers to the branch that studies processes that are nearly at equilibrium and have bulk properties that we can measure, which are appropriate assumptions for DNA nanotechnology [2]. 

In classical thermodynamics, we have some basic concepts that we apply to physical systems that we study. For example, we typically consider **systems** as the physical material of interest, and their **surroundings** as the environment around the material that is in contact with it. A system will have properties that we can update over time via equations, and we can combine measurements of these properties to define thermodynamic characteristics of the system like internal energy and thermodynamic potentials. We note that **equilibrium** in this context means that there can be small, random exchanges of energy between systems or between components in the system, but there is no net change in energy due to these exchanges. Classical thermodynamics focuses on near-equilibrium situations because these are easier to study and understand than non-equilibrium situations [2]. 

Thermodynamic properties can be either **intensive** or **extensive**. Intensive properties are quantities that have magnitude independent of the system size - for example, density, temperature and refractive are all properties that are independent of the amount of material in the system. Conversely, extensive properties have a magnitude that varies with system size, such as mass, volume or entropy [3]. 

### Laws

At the center of classical thermodynamics are four laws as follows [2]: 

**Zeroth Law:** “If two systems are each in thermal equilibrium with a third, they are also in thermal equilibrium with each other.” This law essentially is a definition of temperature.    

**First Law:** The change in internal energy is equal to the energy gained as heat, minus the thermodynamic work done on the surroundings. In other words [2]: 

$$\Delta U = Q - W$$

Where $$\Delta U$$ is a change in internal energy, $$Q$$ is the heat energy input and $$W$$ is the thermodynamic work that is done. This law indicates that we can transfer energy between systems as either heat, work or mass [2].   

**Second Law:** “Heat does not spontaneously flow from a colder body to a hotter.” This law is defining entropy, because it means that a more ordered system (colder, fewer possible states) cannot become a less ordered system (hotter, more possible states). The entropy in the universe is always increasing, not decreasing. Moreover, entropy is maximized at thermal equilibrium (more on this later) [2]. 

**Third Law:**  This law defines the existence of absolute zero [2]. 

### Conjugate Variables
In thermodynamics, we have many sets of **conjugate** variables, that when multiplied together they can describe the energy transferred from one system to another. One way to think about them is to relate them to a “force” and a “displacement” which, when multiplied together, compute work done. Some examples include [2]: 

* Pressure and volume   
* Temperature and entropy   
* Chemical potential and particle number  

Now let’s look at some specific concepts that we introduced above. 

### Gibbs Free Energy
We frequently use Gibbs free energy in DNA nanotechnology - it is a measure of the maximum work that a closed system can do at constant pressure and temperature. Another way to describe this is to say it is the maximum amount of work that does not cause expansion, i.e. we can exchange heat and work but not mass (because that is the definition of a closed system). We often use it to describe the hybridization reaction between two complementary strands of ssDNA. We can write the Gibbs free energy as [4]: 

$$\Delta G = \Delta H - T \Delta S$$

Where $$\Delta G$$ is the change in Gibbs free energy, $$\Delta H$$ is the enthalpy of the system, $$T$$ is the absolute temperature and $$\Delta S$$ is the entropy of the system. We measure the Gibbs free energy in Joules [4]. 

The maximum Gibbs free energy for a process can only be attained if that process is completely reversible. In general, we say that the Gibbs free energy must be less than the non-pressure-volume work (which is typically 0) to be energetically favorable. This means that reactions which have a negative Gibbs free energy are typically energetically favorable, while positive Gibbs free energy values indicate that energy must be added to the reaction for it to occur. So in this way, we can think of the Gibbs free energy as the useful energy available in the system [4]. 

### Enthalpy 

We just encountered enthalpy when we defined the Gibbs free energy, so what is it? Enthalpy describes the energy contained in a chemical system. Strictly speaking, it is defined as the sum of a system’s internal energy and the product of pressure and volume, as shown below, but typically the product of pressure and volume is small relative to the internal energy [5]: 

$$H = U + pV$$

Where $$H$$ is the enthalpy, $$U$$ is the internal energy, $$p$$ is the pressure and $$V$$ is the volume of the system. The enthalpy is only dependent on the final state of the system, not on the path taken to arrive at that final state [5]. 

### Entropy 

We also touched on entropy earlier, but to address it here, it is used to convey whether some processes are irreversible or entirely impossible. It is a measure of the disorder in a system, or, put another way, the number of available states for the system in its current configuration (more states is equivalent to more disorder). Entropy always increases, and thermodynamic entropy is **not** conserved [6]. 

## Rate Kinetics

So now we have seen that thermodynamics has many concepts that define how a reaction occurs and whether or not it will occur. But in the discussion above we never encountered a concept that could provide insight into how quickly a thermodynamically possible reaction would take place. This is where chemical kinetics, or rate kinetics, comes into play [7]. In DNA nanotechnology, we often use fluorescent signals during the annealing process to tell us how quickly a structure is forming, i.e. to extract kinetic rates of a formation process. 

Typically, the rate at which a reaction occurs will depend on one or more of the following [7]: 

* The reactants involved   
* The physical state of the reactants (i.e. solid, liquid, gas)   
* The surface area (if the reactants are solid state)   
* Concentration of reactants   
* Temperature at which the reaction takes place   
* The presence of catalysts   
* Pressure  
* Light absorption   

The reaction rate can be measured several different ways, but one common approach (aside from the approach of measuring fluorescent signals I described above, which is specific to DNA nanotechnology) is to measure how the concentrations of the reactants change over time. We say that **dynamic equilibrium** has been reached when a reversible reaction has equal forward and backward rates, so that, although the reaction is continuing to take place, there is no change in the concentrations of the reactants and the products. The Gibbs free energy can often be used to predict the reaction rates, in fact [7]. 

## Conclusion

Now that I’ve done a whirlwind tour of some of the key concepts of thermodynamics, I’m going to start diving more deeply into topics specific to DNA nanotechnology, beginning with my next post on cooperativity. Stay tuned!

## References:

[1] “Chemical kinetics.” Wikipedia. <https://en.wikipedia.org/wiki/Chemical_kinetics> Visited 10 May 2022. 

[2] “Thermodynamics.” Wikipedia. <https://en.wikipedia.org/wiki/Thermodynamics> Visited 11 May 2022. 

[3] “Intensive and extensive properties.” Wikipedia. <https://en.wikipedia.org/wiki/Intensive_and_extensive_properties> Visited 10 May 2022. 

[4] “Gibbs free energy.” Wikipedia. <https://en.wikipedia.org/wiki/Gibbs_free_energy> Visited 10 May 2022. 

[5] “Enthalpy.” Wikipedia. <https://en.wikipedia.org/wiki/Enthalpy> Visited 11 May 2022. 

[6] “Entropy.” Wikipedia. <https://en.wikipedia.org/wiki/Entropy> Visited 11 May 2022. 

[7] “Chemical kinetics.” Wikipedia. <https://en.wikipedia.org/wiki/Chemical_kinetics> Visited 10 May 2022. 
