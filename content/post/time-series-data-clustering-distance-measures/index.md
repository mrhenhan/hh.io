---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Time Series Data Clustering Distance Measures"
subtitle: "Typical approaches and distance metrics for unsupervised time series clustering"
summary: "As ubiquitous as time series are, it is often of interest to identify clusters of similar time series in order to gain better insight into the structure of the available data. However, unsupervised learning from time series data has its own stumbling blocks. For this reason, the following article presents some helpful time series specific distance metrics and basic procedures to work successfully with time series data."
authors: ["mrhenhan"]
tags: ["time series", "data clustering", "distance metrics", "distance measures", "unsupervised", "machine learning"]
categories: ["machine learning", "unsupervised"]
date: 2020-06-20T12:45:56+02:00
lastmod: 2020-06-20T12:45:56+02:00
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
Time series data is nearly everywhere and actually most data can be modeled in form of a time series. Typical representatives from the category time series data are stock quotation or tick data, medical data, e.g. EEG readings, or even data from machine state monitoring in form of logs.

Often it is of great interest to identify time series of similar progression or behaviour, but this can prove to be an extremely challenging task. This is due to the fact that it is difficult to define the notion of _similarity_ on time series data that may be differently scaled or translated in both the contextual and behavioral attributes. For this reason, the concept of similarity, especially in the context of time series, is an important core subject of research.

Time series allow for diverse formulations of similarity metric and clustering method, depending on the kind of time series representation, and intend of clustering. As we have already noticed at this point, there are several 'moving' parts with an influence on the final clustering performance, thus a successful approach is usually highly application specific. Basically there are two different approaches to cluster time series data, depending on the question whether we are interested in similar trends or shape of the time series. Finally, this leads to a differentiation into two main categories. _Correlation-based online clustering_ and _shape-based offline clustering_ as stated by Charu C. Aggarwal and Chandan K. Reddy in their book [_Data Clustering - Algorithms and Applications_](http://charuaggarwal.net/clusterbook.pdf).

The remainder of this article follows the argumentation and structure from the book mentioned before.

## Correlation-based Online Clustering

Forecasting and online correlation-based clustering methods are closely interwined. Mostly they cluster streams based on a correlation metric over a past time series window with horizon $h$. This could be seen as a interstream regression function with the purpose to determine correlations between different streams. Positive correlation is not neccessary to indicate similarity between streams, as the main concern is to predict one stream from another.

### Selective Muscles and Related Methods

In 2000 Sidiropoulos et al. presented an [Online data mining method for co-evolving time sequences](https://ieeexplore.ieee.org/document/839383) called selective muscles. Selective muscles determines the best $k$ representatives from a time series which can be used to predict another time series. This poses a slightly different problem than unsupervised clustering, where you want to select the best set of representatives which can predict all the streams in the data.

**Selective Muscles**: _Given a dependent variable_ $X_i$ and $d - 1$ _independent variables_ $\\{ X_j: j \neq i \\}$, _determine the top_ $b < d$ _streams to pick so as to minimize the expected estimation error._

Here, estimation is usually performed with Autoregressive (AR) or Autoregressive Integrated Moving Average (ARIMA) standard models. The more general problem formulation drops special treatment of any particular variable and is formulated as,

**General Muscles**: _Given_ $d$ _variables_ $X_1, X_2, ..., X_d$, _determine the top_ $b < d$ _streams to pick so as to minimize the average estimation error over all streams._

_General Muscles_ differs slightly from the _Selective Muscles_ problem formulation and is more relevant to the problem of online clustering time series, but is not different in principle.

The following algorithm performs greedy clustering by defining a _regression based_ similarity function, where two time series are similar, when one series can be predicted from the other and does not utilize a cost function like the version from Aggarwal et al. in [On dynamic data-driven selection of sensor streams](https://dl.acm.org/doi/10.1145/2020408.2020595).

```
Algorithm CorrelationCluster(Time Series Streams: [1...n])
 
  NumberOfStreams k;
  
  begin
    J = Randomly sampled set of k time series streams;
    At the next time stamp do
    repeat
      Add a stream to J, which leads to
        maximum decrease in regression error;
      Drop the stream from J which leads to
        least increase of regression error;
    until(J did not change in last iteration)
  end
```

It is interesting to note, that the problem of _sensor selection_ is closely releated to correlation clustering, because in sensor selection one typically tries to select series representatives for predicting other series in the data.

## Shape-based Offline Clustering

According to [Liao](https://dl.acm.org/doi/10.1016/j.patcog.2005.01.025), univariate time-series distance measures, for the more conventional shape-based clustering problem, can be arranged into three categories.

1. Feature based
2. Model-based
3. Shape-based

One has to keep in mind that similarity and distance measures depend on time-series representation, where frequency related representation like, e.g. Discrete Fourier Transformed (DFT) or Discrete Wavelet Transformed (DWT) time series typically require different metrics than time series represented in their raw $T = [t_1, t_2, ..., t_n]$ form. Furthermore, the following univariate metrics differ from multivariate metrics introduced in a later blog post.

**$L_p$ Norm**

A well defined distance metric, like the $L_p$ Norm satisfies nonnegativity, identity and symmetry as well as the triangle inequality property. The $L_p$ Norm benefits from the fact of having linear time complexity $\mathcal{O}(n)$. A drawback of the $L_p$ Norm is, that time series under comparison have to be of equal length. The generalization of the $L_p$ Norm is called Minkowski Norm of order $p$, respectivly $L_p$-norm distance and represents the generalization of the Euclidean distance

$$
L_p(T_1,T_2) = D_{M,p}(T_1,T_2) = \sqrt[p]{\sum_{i=1}^{n}(T_{1_i} - T_{2_i})^p}
$$

,with the Euclidean distance between two time series $T_1$ and $T_2$ of length $n$ being a special case of the Minkowski Norm with $p=2$.

**Dynamic Time Warping Distance**

Dynamic Time Warping (DTW) cannot be considered a norm as it does not fulfill the triangle inequality but usually performs better than the $L_p$ norm, as it realizes a one-to-many mapping between points and therefor allows for time-shifting capturing similar shapes even if they have a time-phase difference. Furthermore, DTW is able to handle different sampling intervals of a time series. With the help of dynamic programming methods DTW between two time series $T_1$ of length $m$ and $T_2$ of length $m$ can be computed as

- $D_{DTW}(T_1,T_2) = 0$, if $m = n = 0$
- $D_{DTW}(T_1,T_2) = \infty$, if $m = n = 0$
- $D_{DTW}(T_1,T_2) = d(T_{1_1}, T_{2_1}) + minFactor$, else
- with $minFactor = \cases\{D_{DTW}(Rest(T_1), Rest(T_2)) \cr D_{DTW}(Rest(T_1), T_2) \cr D_{DTW}(T_1, Rest(T_2))}$.

As DTW is computed with the help of a dynamic programming approach, it could prove as computational prohibitive for very large data sets.

**EDIT Distance**

The Edit Distance (ED), also know as Levenshtein Distance, is proven to be a metric distance also fulfills the triangle inequality in contrast to former DTW or Longest Common Subsequence (LCSS) presented next. It originates from the field of string comparison and measures the number of operations needed to transform a string $S_1$ into a string $S_2$. Valid string manipulations stem from the set of insert, delete, and replace operations.

ED can be calculated as follows,

- $D_{ED}(S_1, S_2) = m$, if $n = 0$
- $D_{ED}(S_1, S_2) = n$, if $m = 0$
- $D_{ED}(S_1, S_2) = D_{ED}(Rest(S_1), Rest(S_2))$, if $S_{1_1} = S_{2_1}$
- $D_{ED}(S_1, S_2) = min \cases\{D_{ED}(Rest(S_1), Rest(S_2)) + 1 \cr D_{ED}(Rest(S_1), S_2) + 1 \cr D_{ED}(S1, Rest(S_2)) + 1}$.

There are extensions for ED, proposed by [Chen et al.](https://dl.acm.org/doi/10.1145/1066157.1066213), introducing a real penalty (ERP) and for allowing to handle real sequences (EDR), which introduce the ability to handle local time shifting and noise in time series clustering.

**Longest Common Subsequence**

Finally, the Longest Common Subsequence (LCSS) is a variation of ED. The benefit of using LCSS is, that it is able to handle stretching of a time series on the time axis and does not need to match all time series data points, thus being less sensitive to outliers than the initially introduced $L_p$-norm and DTW distance measures. LCSS between two real-valued sequences $S_1$ with length $m$ and $S_2$ with length $n$ is computed as,

- $D_{LCSS, \delta, \epsilon}(S_1, S_2) = 0$, if $n = 0$ or $m = 0$
- $D_{LCSS, \delta, \epsilon}(S_1, S_2) = 1 + D_{LCSS, \delta, \epsilon}(Head(S_1), Head(S_2))$, if $|S_{1,m} - S_{2,m}| \le \epsilon$ and $|m - n| \leq \delta$
- $max \cases\{D_{LCSS, \delta, \epsilon}(Head(S_1), S_2) \cr D_{LCSS, \delta, \epsilon}(S_1, Head(S_2))}$,

with $Head(S_1)$ being the subsequence $[S_{1,1}, S_{1,2},...,S_{1,m-1}]$, $\delta$ an integer value controlling the maximum distance on the time axis between two matched elements, and $\epsilon$ a real number $0 \le \epsilon \le 1$ controlling the maximum distance between two elements to be matched.

This was a short introduction to some key distance metrics regarding time series clustering for _univariate time series_ clustering and mainly fetched from the summarized collection of papers from [_Data Clustering - Algorithms and Applications_](http://charuaggarwal.net/clusterbook.pdf).

Kind regards,

Henrik Hain
