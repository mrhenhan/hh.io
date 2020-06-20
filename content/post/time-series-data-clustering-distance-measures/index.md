---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Time Series Data Clustering Distance Measures"
subtitle: "Typical distance metrics for unsupervised time series clustering"
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

In 2000 Sidiropoulos et al. presented an [Online data mining method for co-evolving time sequences](https://ieeexplore.ieee.org/document/839383) called selective muscles. Selective muscles determines the best $k$ representatives from a time series which can be used to predict another time series.

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

## Shape-based Offline Clustering
