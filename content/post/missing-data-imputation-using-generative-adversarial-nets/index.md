---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Missing Data Imputation Using Generative Adversarial Nets"
subtitle: ""
summary: ""
authors: ["mrhenhan"]
tags: ["time series", "missing data", "missing data imputation", "machine learning", "neural network", "generative adversarial network", "deep learning"]
categories: ["time series", "machine learning", "missing data"]
date: 2020-06-22T19:07:31+02:00
lastmod: 2020-06-22T19:07:31+02:00
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
Missing data, especially missing data points in time series, are a pervasive issue for many applications relying on precise and complete data sets. For example in the financial sector, missing tick data can lead to deviating forecasts and thus to wrong decisions with high losses. So it is not surprising that neural networks and in particular deep generative adversarial networks are utilized for missing data imputation with the goal of generating high precision imputation data of high reliability.

To be able to follow up nicely with this post one has to know, that missing data comes in three distinct categories.

1. Data may be missing completely at random (MCAR)
2. Data may be missing at random (MAR)
3. Data may be missing not at random (MNAR)

Data missing completely at random (MCAR) is if the missingness happens entirely random, which means that missingness does not depend on any of the variables. Data missing at random is if the missingness depends only on the _observed_ variables, whereas data missing not a random (MNAR) is, when the mechanism why the data is missing does not correspond to the former two mechanisms MCAR and MAR, meaning that missingness depends _both_ on the _observed_ and _unobserved_ variables.

Furthermore, it is helpful to know that imputation methods can be categorized as either discriminative or generative. Some representatives from the discriminative imputation category are

- [MICE](https://www.jstatsoft.org/article/view/v045i03)
- [MissForest](https://arxiv.org/abs/1105.0828)
- [Matrix Completion](http://www.jmlr.org/papers/v16/hastie15a.html)

and some generative imputation mechanisms would be

- [Methods based on expectation maximization (EM)](https://dl.acm.org/doi/10.1007/s00521-009-0295-6)
- [Denoising Autoencoders (DAE)](https://dl.acm.org/doi/10.1145/1390156.1390294)
- [Methods based on generative adversarial networks (GAN) and DAE](https://www.cc.gatech.edu/~hays/7476/projects/Avery_Wenchen/)

In this post we are reviewing a more recent novel generative approach from 2018 by Yoon et al. They use a customized generative adversarial network for missing data imputation called [GAIN](https://arxiv.org/abs/1806.02920) which outperforms MICE, MissForest, Matrix, Auto-encode, and EM by a rather large margin on serveral different datasets regarding area under the curve (AUROC), mean bias and mean square error (MSE) performance metrics. So, how does it work?

## How does it work?
