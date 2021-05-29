---
title: Week of May 24 Paper Reading
layout: post
---

Last week I explored some ideas around motif-centric learning and then began to investigate the importance of equivariance and invariance in deep learning models. This week, I continue to explore why equivariance is important and useful. I also dive into the idea of an attention mechanism and transformers, ideas that I have heard of but never studied. 

## Paper 1: Trajectory Prediction using Equivariant Continuous Convolution by Walters et al. [1]

In this paper, Prof. Rose Yu’s group presents a novel model for predicting the trajectories of cars and pedestrians, called Equivariant Continuous Convolution (ECCO). Instead of using GNNs, they argue that they can leverage the symmetry in the trajectory data and learn to predict the trajectories more efficiently using a continuous convolution model. Specifically, they want their model to capture the fact that some aspects of the trajectory data should be the same regardless of how the data is rotated, like the cars’ velocities, while other aspects should be rotated along with the frame of reference, like the direction in which the car is turning at the intersection. Walters et al. present a very cool idea of using a torus kernel to capture this rotational symmetry in the data where it is present. After testing and comparing their model to the state of the art, Walters et al. conclude that their approach is more sample efficient because it trains faster and requires approximately 10X fewer parameters than other models [1]. 

My big takeaway from this paper is that if you can show that your model is equivariant with respect to the input data, it is more likely to generalize well. This is very useful because models that are able to generalize will provide better predictions in novel conditions, and they will be more sample efficient [1]. This helps me answer my question from last week which is: why is model equivariance useful? 

## Paper 2: Attention Is All You Need by Vaswani et al. [2]

I think this is a pretty significant landmark paper that introduced the Transformer to the deep learning community in 2017. The central argument in this paper is that only the attention mechanism, commonly found in CNNs, RNNs and LSTMs, is necessary to complete sequence transduction modeling activities. The authors argue that the Transformer is a much simpler architecture than its competitors, and thus is much faster and cheaper to train while achieving comparable performance. They demonstrate their model’s efficacy with translation tasks [2].  

I need to get a better handle on the mathematical underpinnings of attention in neural networks, but I think the fundamental idea is that attention is akin to taking the dot product between a query and the key of a key-value pair. This result is then scaled and passed through the softmax function: this becomes the weights applied to the values of the key-value pairs. The transformer still has an encoder and decoder (similar to the architecture of a CNN, for example) and we perform this attention operation over every input to the encoder and over every intermediate layer’s output from the encoder, and so on through the decoder (there is masking in some layers of the decoder to capture the sequential nature of the input data) [2]. 

I think that the overall idea is that by computing these dot products (i.e. projecting a vector onto different bases) we are finding the connections between every input value and every other input value. Ultimately we find the strong connections that help us complete the task at hand. 

 ## References: 

[1] Walters, R., Li, J., & Yu, R. (2020). Trajectory Prediction using Equivariant Continuous Convolution. [http://arxiv.org/abs/2010.11344](http://arxiv.org/abs/2010.11344) 

[2] Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N. Gomez, Łukasz Kaiser, and Illia Polosukhin. 2017. Attention is all you need. In Proceedings of the 31st International Conference on Neural Information Processing Systems (NIPS'17). Curran Associates Inc., Red Hook, NY, USA, 6000–6010.



