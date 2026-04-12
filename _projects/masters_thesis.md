---
layout: page
title: Brain-Inspired Spatial Navigation
description: Master's Thesis in Computational Neuroscience
img: assets/img/masters_project/thesis_thumbnail.png
importance: 1
category: work
related_publications: false
---
---
### Summary:
Accurate spatial navigation is essential for both animals and autonomous systems, and the brain’s grid cells in the medial Entorhinal Cortex (mEC) play a key role in this ability. Our project explores how computational models of these cells can enable efficient and accurate navigation in complex environments.

We built on the BION model, a Bayesian framework for spatial localization, and investigated how deep learning could accelerate particle-filter-based inference. Two strategies were tested: 
1. Using neural networks to generate images from spatial poses.
2. Directly estimating observation likelihoods with Energy-Based Models (EBMs) - Neural Likelihoods. 

While both approaches produced high-quality outputs in isolation, integrating them into the particle filter revealed sensitivity to small errors, which limited overall navigation accuracy.

Despite these challenges, our work highlights the potential of EBMs for future research. With improved training and energy landscape refinement, deep-learning-enhanced particle filtering could lead to faster, scalable, and more biologically inspired navigation systems.

---

### Agent and Environment Model:

We ground our experiments in a probabilistic environment defined by the generative model in Figure 1. The agent’s allocentric pose (its true position and heading) evolves according to a known stochastic motion model. At each step, the agent receives observations consisting of its egocentric pose (relative to its starting position) and visual input from the environment.

To study the impact of perceptual uncertainty, we vary the level of visual ambiguity across two settings: (1) a high-contrast scenario with distinctly coloured walls, and (2) a low-contrast scenario where all walls share the same colour, making localisation significantly more ambiguous.

{% include figure.liquid path="assets/img/masters_project/gen_model.png" alt="My figure" caption="Figure 1: Environment Model" class="img-fluid rounded z-depth-1" %}

The agent’s objective is to infer a probability distribution over its true (allocentric) pose, given a sequence of egocentric poses and visual observations. The observer model in Figure 2 formalises this target distribution, defining the posterior the agent seeks to estimate.

{% include figure.liquid path="assets/img/masters_project/observer.png" alt="My figure" caption="Figure 2: Agent Model" class="img-fluid rounded z-depth-1" %}

Previous approaches relied on exhaustive Bayesian filtering or discrete particle filtering, both of which were either computationally expensive or introduced approximation errors. We extend this work by implementing a continuous particle filtering framework, and explore the use of deep learning methods to improve scalability and inference speed.

### Continous Particle Filtering:

We implement a continuous particle filter to approximate the posterior over the agent’s true pose as a set of weighted particles. This representation allows us to capture non-Gaussian transition and observation models, which are common in our setting, particularly under high visual ambiguity. As shown in Figure 3, the filter naturally produces multi-modal posteriors in the low-contrast walls scenario, reflecting the inherent uncertainty in localisation.

{% include figure.liquid path="assets/img/masters_project/particles.png" alt="My figure" caption="Figure 3: Particle Filter Example" class="img-fluid rounded z-depth-1" %}

Continuous particle filtering significantly improved simulation speed compared to full Bayesian filtering, while also increasing accuracy over discrete methods. To push scalability further, we explored deep learning–based amortisation techniques.

While we investigated approaches such as deconvolutional networks and Neural Radiance Fields (NeRFs) to bypass explicit rendering, the most promising results came from directly learning the observation likelihood. As illustrated in Figure 4, we replace the traditional likelihood computation, typically requiring image rendering for many candidate poses at each step, with a neural likelihood model, substantially reducing inference cost.

{% include figure.liquid path="assets/img/masters_project/particle_filter.png" alt="My figure" caption="Figure 4: Particle Filter algorithm with Neural Likelihood" class="img-fluid rounded z-depth-1" %}

### Energy-Based Model for Liklihood Estimation

To model the observation likelihood, we adopt an Energy-Based Modelling (EBM) approach, representing probability distributions as energy functions. This removes the need for explicit normalisation while preserving the relative structure of the distribution.

The network architecture (Figure 5) learns this energy function from image–pose pairs. It consists of an image encoder (CNN) and a pose encoder (MLP), which produce latent embeddings that are combined and passed through an energy head (MLP) to predict the energy of a given pair.

Training is performed using a self-supervised contrastive learning objective. For each batch, the model is given one positive image–pose pair and multiple negative (mismatched) pairs. An InfoNCE loss shapes the energy landscape by lowering the energy of positive pairs and raising it for negatives.

Since the learned energy corresponds to the negative log-likelihood, the model can directly provide likelihood estimates for any image–pose pair at inference time, enabling efficient integration into the particle filtering pipeline.

{% include figure.liquid path="assets/img/masters_project/ebm.png" alt="My figure" caption="Figure 5: Energy-Based Model Architecture" class="img-fluid rounded z-depth-1" %}

### Learned Neural Liklihoods

Below, we present examples of neural likelihoods learned entirely through self-supervised training with our EBM. **Notably, the model captures uncertainty and visual ambiguity directly from data, without any explicit supervision.**

{% include figure.liquid path="assets/img/masters_project/result1.png" alt="My figure" caption="Figure 6: Neural Likelihood example" class="img-fluid rounded z-depth-1" %}

{% include figure.liquid path="assets/img/masters_project/result2.png" alt="My figure" caption="Figure 7:Neural Likelihood example" class="img-fluid rounded z-depth-1" %}

{% include figure.liquid path="assets/img/masters_project/result3.png" alt="My figure" caption="Figure 8: Neural Likelihood example " class="img-fluid rounded z-depth-1" %}

{% include figure.liquid path="assets/img/masters_project/result4.png" alt="My figure" caption="Neural Likelihood example" class="img-fluid rounded z-depth-1" %}