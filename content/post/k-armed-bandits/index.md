---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "K-Armed Bandits"
subtitle: "How to Learn from Evaluative Feedback in a Nonassociative Setting"
summary: ""
authors: ["mrhenhan"]
tags: ["Multi-armed Bandits", "k-armed bandits", "reinforcement learning", "foundations"]
categories: ["Reinforcement Learning", "Machine Learning"]
date: 2020-07-01T19:45:29+02:00
lastmod: 2020-07-01T19:45:29+02:00
featured: false
draft: true

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
K-armed bandits are a foundational method for solving problems based on evaluative instead of instructive feedback as used in supervised learning. The intention of this blog post is to introduce reinforcement learning in a simplified nonassociative setting in form of a $k$-armed bandit problem before we analyze associative settings, e.g. when the best action depends on a specific situation, as required for solving the 'learning to rank problem' introduced in [Personalize Learning to Rank Results through Reinforcement Learning](https://henrikhain.io/post/reinforcement-learning-for-learning-to-rank) :smile:

Nonetheless, many problems can be formulated within the multi-armed bandit framework, which makes it a useful tool in a machine learning toolbox.

## The $k$-Armed Bandit Problem

Assume you are in a situation where you have to choose amongst k different options repeatedly. The set of options is also called _set of actions_. Each action is associated with a numerical reward from a stationary probability distribution. The primary goal is to maximize the expected total reward for some time period.

{{< figure src="./slot_machines.jpg" title="Row of digital-based slot machines inside McCarran International Airport in Las Vegas - by [Yamaguchi先生](https://en.wikipedia.org/wiki/User:Yamaguchi%E5%85%88%E7%94%9F) - [CC BY-SA 3.0](http://creativecommons.org/licenses/by-sa/3.0/)">}}

The problem formulation is the original form of the $k$-armed bandit problem, where the term and the problem resembles playing at a slot machine or _one-armed bandit_ with $k = 1$. For $k = n$ each action selection corresponds to play one of the machine's $n$ levers, where the rewards are the payoffs for hitting the jackpot. Another area of application would be a homogeneous group of shop visitors where a sales person tries to make the right offer from $k$ offers, with the reward being, that a shop visitor accepts and buys the offer. Here, we are already able to see the versatility of $k$-armed bandits, given a nonassociative setting with stationary reward distributions.

The _value_ of an action is the expected or mean reward, given that the action is selected. Typically, the selected action at timestep $t$ is denoted as $A_t$ and the corresponding reward as $R_t$, subsequently the value of an arbitrary action $a$, denoted as $q_{*}(a)$, is the expected reward given the circumstance $a$ is selected:

$$
q_{*}(a) = \mathbb{E}[R_t | A_t = a]
$$

Because we do not know the value $q_{*}(a)$ of each possible action with certainty, we need to maintain value estimates Q_t(a) with the goal to be close to $q_{*}(a)$.

{{% alert warning %}}
Because we do not know the value $q_{*}(a)$ of each possible action with certainty, we need to maintain value estimates Q_t(a) with the goal to be close to $q_{*}(a)$.
{{% /alert %}}

At each timestep $t$ there is at least one action with the greatest value amongst the maintained action value estimations. Those action are called $greedy$ actions. Selecting greedy actions corresponds to the _exploitation_ of your current knowledge, wheras selecting a non-greedy action corresponds to the _exploration_ of your knowledge by improving non-greedy action value estimates. If the goal is to maximize the reward at the current time step, choosing a greedy action is the right choice. Typically, however, this is a myopic approach to action selection, as _exploration_ may lead to the discovery of actions with higher rewards by improving the action value estimations of non-greedy actions.
