---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Interpretable Discrete Representation Learning on Time Series"
subtitle: "SOM-VAE, an interpretable probabilistic deep variational autoencoder based approach for representation learning"
summary: ""
authors: ["mrhenhan"]
tags: ["time series", "paper review","representation learning", "clustering", "unsupervised learning", "markov model"]
categories: ["time series", "representation learning", "machine learning"]
date: 2020-06-21T16:05:57+02:00
lastmod: 2020-06-21T16:05:57+02:00
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
Effective and efficient time series representation learning poses an important topic for a vast array of applications like, e.g. clustering. Many currently used approaches share the property of being difficult to interpret though. In many areas it is important that intermediate learned representations are easy to interpret for efficient downstream processing. Fortuin et al. proposed a novel variational autoencoder based time series representation learning framework, making use of discrete dimensionality reduction and deep generative modeling, called [SOM-VAE](https://arxiv.org/abs/1806.02199). Their approach tries to overcome non-differentiability in discrete representation learning by introducing a gradient-based version of the self organizing map ([[SOM - Kohonen1990]](https://ieeexplore.ieee.org/document/58325)) that is more performant while simultanuously allowing for probabilistic interpretation by making use of an internal Markov model in the representation space.

Foruin et al. evaluate SOM-VAE using the static [(Fashion-)MNIST data](https://github.com/zalandoresearch/fashion-mnist) from [Zalando research](https://research.zalando.com/), a [chaotic Lorenz attractor system](https://en.wikipedia.org/wiki/Lorenz_system) with two macro states, and medical time series data from the [eICU dataset](https://eicu-crd.mit.edu/).

## How Does it Work?

Essentially, Foruin et al. used a generative deep learning network (VAE) and extended it to be able to capture temporal smoothness in a discrete representation space by combining it with a self-organizing map (SOM) which introduces topological neighborhood relationships. As the classical SOM formulation has no notion of time, they extend SOM with a probabilistic transition Markov model in such a way, that single time point representations are enriched with information from adjacent time points in the series.

{{< figure src="./overview-som-vae.png" title="Schematic overview of the SOM-VAE model architecture. Image: [SOM-VAE: Interpretable Discrete Representation Learning On Time Series](https://arxiv.org/abs/1806.02199)">}}

The schematic overview depicts time series from the data space [green] being encoded by a neural network [black] time-point wise into the latent space. With the help of a self-organizing map [red] data from the data manifold is being approximated. The discrete representation is achieved by mapping every data point $z_e$ from the latent space to its closest node $z_q$ in the self-organizing map. Subsequently, they train a Markov transition model [blue] for predicting the next discrete representation $z_q^{t+1}$ using the current representation $z_q^t$. Eventually, learned discrete representations can be decoded with the help of another neural network. 

Foruin et al.'s tensorflow-based SOM-VAE implementation is available at GitHub.

SRC-Code: [SOM-VAE Repository](https://github.com/ratschlab/SOM-VAE)


Kind regards,

Henrik Hain
