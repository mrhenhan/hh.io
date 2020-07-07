---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "SciPy 2020 Day Two"
subtitle: "Half of the Dask tutorial, Continuous Integration for Scientific Projects and more..."
summary: ""
authors: ["mrhenhan"]
tags: ["SciPy 2020", "machine learning", "Dask", "CI", "scientific projects", "data science", "histogram", "boost", "C++14"]
categories: ["SciPy", "parallel computation", "continuous integration"]
date: 2020-07-07T20:24:55+02:00
lastmod: 2020-07-07T20:24:55+02:00
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
Day two of the SciPy 2020 conference was also very informative. Except of some connectivity issues with my internet provider, which lead to missing the latter half of the awesome [Dask](https://dask.org/) tutorial and prevented me from listening to other talks,  everything went equally smooth. My SciPy day started nicely with a very informative talk with my assigned mentor [Hongsup Shin](https://www.linkedin.com/in/hongsupshin/), an experienced data scientist at [Arm](https://www.arm.com/), who gave me tips and hints on how to get more involved in the field of open source scientific development, which was very helpful for me. Also at this point I would like to thank Hongsup for his support. Apart from these highlights, I watched a talk about Boost-histograms, Continious Integration for Scientific Python Projects, and watched the current daily Welcome, SciPy Tools Plenary Session, and HPC Python Q&A session which I will briefly introduce in the next sections. 

## Boost-histogram: High-Performance Histograms as Objects |SciPy 2020| Schreiner, Pivarski & Dembinski

Schreiner et. al generalized the usual single operation, producing serveral arrays which together build a histogram, by introducing histograms as high performance Python object. Boost-histograms are built on top of [Boost](https://www.boost.org/) libraries' Histogram in C++14. Their work is basically meant as foundation others are able to build on. Actually in the [Scikit-HEP](https://scikit-hep.org/) project, there are already a physicist friendly front-end called ['Hist'](https://github.com/scikit-hep/hist) and a conversion package with the name ['Aghast'](https://github.com/scikit-hep/aghast) designed around boost-histogram.

One of the benefits is, that boost-histograms are Python objects allowing fancy indexing with tidy names and callbacks additionally to simple manipulation through [NumPy](https://numpy.org/) and that they are easily handable in a threaded program workflow. Furthermore, they are fast, usually twice as fast as naive, unoptimized NumPy alternatives, and threaded even faster. Make sure to watch the talk!

{{< youtube ERraTfHkPd0 >}}

## Tutorial: Parallel and Distributed Computing in Python with Dask

## Continuous Integration for Scientific Python Projects |SciPy 2020| Stanley Seibert

This talk is actually a set of good software engineering continuous integration best practices and tips and tricks applied to scientific python projects. For me, there wasn't anything surprising or unknown here, but it's very good to recall all the necessary important balancing decisions sometimes. The key takeaway should be, that continuous integration is essentially a process with task which should be ordered along the importance/impact axes and ticked of in sequence. If you want to make your life easier and don't know the best practices or want to recall the important parts -> watch this talk! :smile:

{{< youtube OAlr9vY5XLU >}}

## Welcome & SciPy Tools Plenary Session

Today I also participated at the 'Welcome & SciPy Tools Plenary Sessions'. During the Welcome session the remainder of the daily schedule was presented and the gold sponsors were honoured.


## Lightning Talks

## References
