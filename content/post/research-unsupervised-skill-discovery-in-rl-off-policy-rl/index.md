---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Research Unsupervised Skill Discovery in Rl Off Policy Rl"
subtitle: "Google AI: DADS - Unsupervised Reinforcement Learning for Skill Discovery"
summary: "Scientists from Google AI have published exciting research regarding unsupervised skill discovery in deep reinforcement learning. Essentially it will be possible to utilize unsupervised learning methods to learn model dynamics and promising skills in an unsupervised, model-free reinforcement learning enviroment, subsequently enabling to use model-based planning methods in model-free reinforcement learning setups."
authors: ["mrhenhan"]
tags: ["google brain", "google research", "deep reinforcement learning", "deep learning", "model based reinforcement learning", "planning in reinforcement learning", "DADS"]
categories: ["current advancements in reinforcement learning", "unsupervised machine learning", "research"]
date: 2020-06-02T19:43:34+02:00
lastmod: 2020-06-02T19:43:34+02:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---
AI residents A. Sharma et al. from Google Research and Google Brain published an interesting approach addressing the complicated issue of specifying a well-designed task-specific reward function in an unsupervised manner in order to remedy the problems of manually labeling "goal" states or introducing porbably costly instrumentations, e.g. in form of sensors.

Usually, an agent in a supervised reinforcement learning environment uses an extrensic reward function specifically designed to address a specific problem area. In contrast, in unsupervised reinforcement learning, an agent utilizes an intrinsic reward function, e.g. procedures mimicing behaviours like curiosity, 'to generate its own trainings signals for acquiring a probably broad set of task-agnostic behaviors' - [Sharma et al. 2020].

Essentially, this would allow for neglegting the effort of desining an extrinsic, task-specific reward function while being generalizable for other tasks as well. Though it can be considered difficult to learn agent-environment interactions without a guiding reward signal, solving this specific problem in an unsupervised fashion could prove extremely rewarding for many domains outside of classic anthropomatics.

Sharma et al.'s work includes two current research papers, [Dynamics-Aware Unsupervised Discovery of Skills](https://arxiv.org/abs/1907.01657), and the more recent [Emergent Real-World Robotic Skills via Unsupervised Off-Policy Reinforcement Learning](https://arxiv.org/abs/2004.12974), from Google Research and Google Brain.

{{< figure src="./rl_spider_predictable.gif" title="'The behavior on the left is random and unpredictable, while the behavior on the right demonstrates systematic motion with predictable changes in the environment. Our goal is to learn potentially useful behaviors such as those on the right, without engineered reward functions.' - [ai.googleblog.com]( https://ai.googleblog.com/2020/05/dads-unsupervised-reinforcement.html)" lightbox="true" >}}

In their foundational work [Dynamics-Aware Unsupervised Discovery of Skills](https://arxiv.org/abs/1907.01657) they introduce "predictability" as optimization objective for discovering new skills, essentially _allowing to create a dynamics model of the environment which in turn enables the use of planning algorithms_. They prove the practicability of the approach in a simulated robotics setup. Sharma et al. improve the sample efficiency in their follow-up work and prove the practicability of DADS in a real-world scenario using an off-policy variant for training D'Kitty from [ROBEL](https://sites.google.com/view/roboticsbenchmarks/)

DADS is based on the design of an intrinsic reward function which encourages a curiosity like behavior for discovering both a "predictable" and "diverse" skill set. As there is not any reward given by the environment, optimizing skills with respect to diversity allows the agent to discover many potentially useful behaviors.

They utilize a second neural network (_skill-dynamics network_) to actually predict if a skill is associated with a predictable change in the environment. Better prediction performance of the _skill_dynamics network_ is associated with the preditability of environmental state changes and therefor with the predictability of the skill.

{{< figure src="./dads_overview.png" title="'Schematic of the DADS model.' - [ai.googleblog.com]( https://ai.googleblog.com/2020/05/dads-unsupervised-reinforcement.html)" lightbox="true" >}}

Essentially, Sharma et al.'s approach could lead to interesting possibilities in the lesse complex areas of online retail, lead management and customer experience if applied accordingly to specific intrinsic problem domains.

If you are interested in the resources and their current research regarding unsupervised reinforcement learning, have a look at the following resources.

[Google AI Blog Post](https://ai.googleblog.com/2020/05/dads-unsupervised-reinforcement.html)

[Dynamics-Aware Unsupervised Discovery of Skills](https://arxiv.org/abs/1907.01657)

[Emergent Real-World Robotic Skills via Unsupervised Off-Policy Reinforcement Learning](https://arxiv.org/abs/2004.12974)

[Github Repo: DADS](https://github.com/google-research/dads)


Kind regards,

Henrik Hain
