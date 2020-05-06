---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Categorizing Deep Learning Approaches for Clustering 1"
subtitle: ""
summary: "The following first article part is an attempt to categorize and structure current state of the art (SoA) clustering approaches, making use off of the work of Aljalbout et al. and Min et al. for my current research at TECO Institute from Karlsruhe Institute of Technology regarding missing data imputation in large stock quotation and trade data sets, where clustering would be an obvious first step for retrieving highly correlated quotations or trade movements."
authors: [mrhenhan]
tags: [unsupervised learning, deep learning, clustering, cluster algorithms, loss functions, evaluation methods, finance, stock quotations]
categories: [machine learning]
date: 2020-05-06T00:40:05+02:00
lastmod: 2020-05-06T00:40:05+02:00
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
{{% alert note %}}
[All.In Data](https://www.all-in-data.de/de/kuenstliche-intelligenz/time-series-a-primer/) has republished this article on their blog March the 18th. Thank you :smile:
{{% /alert %}}

# Problem Statement
The goals of cluster analysis are various and basically all related to ‚grouping or segmenting a collection of objects into subsets or „clusters“‚ – (The Elements of Statistical Learning 2nd Ed. 2009), so that there is a stronger relation between the elements of a subset than between elements in different subsets. Those object or data points are usually sets of measurements, which means objects can be described by either those measures or by their relation to other objects. This also means that the notion of similarity (or dissimilarity) describing object proximity is central to clustering.

With rising dimensionality, both the object and the relation description, becomes computationally prohibitive and additional gives birth to another class of problems related to the curse of dimensionality as some calculations have surprising properties in a high dimensional feature space.

The problem of choosing a suitable similarity metric together with the curse of high dimensionality leads to a class of deep clustering algorithms described in the following sections.

# Deep Clustering
Deep learning in the supervised machine learning area excels in extracting hierarchical feature representations thus finding representations useful for the classification of objects. This works exceptional well in the image and other categories with high dimensional data. Deep clustering aims at utilizing those properties for generating representation in a possibly lower dimensional latent space, where clustering performs better than in the original space.

Most current deep clustering approaches are a combination of basically two main components.

1. A deep neural network for input data transformation into a latent space.
2. A clustering component, either separated from the network, e.g. HDBSCAN, or as part of the network itself.

This approach moves the issue of clustering in high dimensional input space to the problem area of representation learning and mapping the input to a latent space.

## Deep Representation Learners
Min et al. state that, though ‚… methods are usually categorized as partition-based, density-based, hierarchical […] it is not suitable to classify methods according to the clustering loss […] instead we should focus on the network architecture used for clustering.‘ (Min et al. 2018), which leads to their approach of categorizing clustering methods according to the currently used neural network architectures for representation learning. Together with Aljalbout et al. Both coarsely discriminate between discriminator and generator networks and list the following networks architectures for representation learning.

1. Multilayer perceptron (MLP) – Typically a simple fully connected feed forward network.
2. Autoencoder (AE)
3. Convolutional neural network (CNN)
4. Deep belief network (DBN)
5. Generative adversarial network (GAN)
6. Variational autoencoder (VAE)

Thus missing out on recurrent neural networks, e.g. long short term memory (LSTM) networks, which could possibly be useful for learning representations of time series where the importance of values further in the past is less important than the last $n$ current values.

Aljalbout et al. mentioned that features creating the latent representation space can be drawn from any combination of layers.

It has to be mentioned that generative approaches allow for the possibility to generate new data drawn from the learned distributions, which could prove quite useful for other domain relevant cases.

## Loss Functions
Central to deep learning in general and deep clustering specifically is the notion of a loss function utilized during training a network. From here on I will use the notation presented in the paper of Min et al., calling them principal and auxiliary loss, though Aljalbout et al. presents similar concepts while naming them differently.

If both loss functions are present, they are usually combined with the help of a lambda weighted linear combination function, like in the following equation, where Lp
represents the principal clustering loss and $L_a$ the auxiliary clustering loss.

Linear combination of two loss functions: $L=\lambda L_p+(1–\lambda)L_a$

Interestingly both papers don’t address losses that utilize correlation based measures needed for identifying highly correlated univariate time series.

### Principal Clustering Loss
The principal loss function models the clustering loss directly. Therefor the model consists of the cluster centroids as well as the cluster assignment of the samples. A useful property of the principal clustering loss is, that if a network is trained using it, it is possible to obtain clusters directly. Typical representatives for principal clustering are

- k-means loss
- cluster assignment hardening loss
- nonparametric maximum margin clustering
- …

### Auxiliary Clustering Loss
The sole purpose of the second kind of loss is to improve the representation learning capabilities
of the used neural network. As it only improves the representation it can not output clusters directly and a network only trained with this kind of loss requires an additional component to obtain clusters. Typical representatives for auxiliary clustering loss are

- locality-preserving loss
- group sparsity loss
- sparse subspace clustering loss
- …

## Evaluation Metrics
Both papers make use of two clustering evaluation metrics, the first being the unsupervised clustering accuracy (ACC) and the second one is the normalized mutual information (NMI). Min et al. claim that these two evaluation metrics are widely used throughout many deep clustering papers without addressing the reason for using specifically those two metric.

To use latter evaluation metrics, the number of clusters is set to the number of ground-truth categories.

### Unsupervised Clustering Accuracy
In the following equation to calculate the unsupervised clustering accuracy, $y_i$ is the ground-truth label, $c_i$ would be the cluster assignment given by the used cluster algorithm, where $m$ is a mapping function that can be calculated using the Hungarian algorithm, which has polynomial complexity $O^4$. The mapping function $m$ ranges over all possible $1-1$ mappings between assignment and ground-truth label.

$ACC = \max_m \frac{\sigma_{i-1}^n 1y_i=m(c_i)}{n}$

### Normalized Mutual Information
In $NMI$, $Y$ denotes the ground-truth labels, whereas $C$ describes the algorithm cluster labels.
$H$ is the informational entropy $log_2$ where $I$ denotes the mutual information metric. The evaluation metric is then given simply by

$NMI(Y,C) = \frac{I(Y,C)}{\frac{1}{2}[H(Y)+H(C)]}$.
    
# Conclusion
This was the first part of the article mini series ‚Clustering with Deep Learning‘. Based on recent work of Aljalbout et al. and Min et al.

So far we covered the basic principles untill we discovered two useful evaluation metrics. The next article will address deep network architectures, beginning with the proposed clustering network from Aljalbout et al. using an autoencoder network together with a k-means clusterer.

Sadly both papers did not cover the automatic retrieval of the number of clusters present, making it necessary to set $k$ upfront.

# Links & References
[Clustering with Deep Learning: Taxonomy and New Methods](https://arxiv.org/abs/1801.07648)

[A Survey of Clustering with Deep Learning: From the Perspective of Network Architecture](https://www.researchgate.net/ptering_with_Deep_Learning_From_the_Perspective_of_Network_Architecture)
