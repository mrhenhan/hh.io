---
title: Unsupervised Skill Discovery in Off-Policy Reinforcement Learning
author: mrhenhan
date: '2020-06-02'
slug: unsupervised-skill-discovery-in-off-policy-reinforcement-learning
categories:
  - current advancements in reinforcement learning
  - unsupervised machine learning
  - research
tags:
  - google brain
  - google research
  - deep reinforcement learning
  - deep learning
  - planning in reinforcement learning
  - dads
subtitle: 'Google AI: DADS - Unsupervised Reinforcement Learning for Skill Discovery'
summary: 'Scientists from Google AI have published exciting research regarding unsupervised skill discovery in deep reinforcement learning. Essentially it will be possible to utilize unsupervised learning methods to learn model dynamics and promising skills in an unsupervised, model-free reinforcement learning enviroment, subsequently enabling to use model-based planning methods in model-free reinforcement learning setups.'
authors: []
lastmod: '2020-06-02T18:56:44+02:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
draft: false

---
AI residents A. Sharma et al. from Google Research and Google Brain published an interesting approach addressing the complicated issue of specifying a well-designed task-specific reward function in an unsupervised manner in order to remedy the problems of manually labeling "goal" states or introducing porbably costly instrumentations in form of sensors.

Usually, an agent in a supervised reinforcement learning environment uses an extrensic reward function specifically designed to address a specific problem area. In contrast, in unsupervised reinforcement learning, an agent utilizes an intrinsic reward function, e.g. procedures mimicing behaviours like curiosity, 'to generate its own trainings signals for acquiring a probably broad set of task-agnostic behaviors' - [Sharma et al. 2020].

Essentially, this would allow for neglegting the effort of desining an extrinsic, task-specific reward function while being generalizable for other tasks as well. Though it can be considered difficult to learn agent-environment interactions without a guiding reward signal, solving this specific problem in an unsupervised fashion could prove extremely rewarding for many domains outside of classic anthropomatics.

Sharma et al. current work is based on two current research papers, [Dynamics-Aware Unsupervised Discovery of Skills](https://arxiv.org/abs/1907.01657) and the more recent [Emergent Real-World Robotic Skills via Unsupervised Off-Policy Reinforcement Learning](https://arxiv.org/abs/2004.12974)

