---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Scipy 2020 Day One"
subtitle: "Dabl(ling) with new Libraries and an Introduction to Baysian Data Science"
summary: ""
authors: ["mrhenhan"]
tags: ["Scipy 2020", "machine learning", "dabl", "Bayesian statistics", "PyMC3", "data science"]
categories: ["SciPy", "machine learning", "data science", "bayesian statistics"]
date: 2020-07-06T17:52:42+02:00
lastmod: 2020-07-06T17:52:42+02:00
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
I am very happy :smile: to participate at the 2020 edition of the SciPy conference, which is held online thanks to the measures that prevent the spread of the COVID-19 virus. Although it is the first online version of the SciPy conference, everything works fine and fluently due to the tremendous help from the organizers and community. The first SciPy conference videos were pre-recorded and uploaded on Sunday by [Enthought](https://www.enthought.com/) on [Youtube](https://www.youtube.com/user/EnthoughtMedia/playlists?view=50&sort=dd&shelf_id=4), whereas tutorials and sessions are held live. Q&A regarding specific topics will take place following the live sessions, today on Monday for the machine learning sessions.

As I participated at the awesome 4 hour Bayesian stats modelling session held by Eric Ma, I had only time for some additional content and the plenum discussions. The remainder of this blog will give a quick intro to the sessions I participated in.

*Note*: _This blog and the references will be extended, as it currently only represents a quick summary of my first day at SciPy 2020!_

## dabl: Automatic Machine Learning with a Human in the Loop

Scikit-learn core developer [Andreas C. Müllers](http://amueller.github.io/) library [_dabl_](https://amueller.github.io/dabl/dev/), an acronym for _data analysis baseline library_, intends to make supervised learning and fast AutoMl easier for machine learning beginners through reducing common boilerplate code. The library addresses the _clean_, _visualize_, and data _explain_ segments of a data science life cycle. One of the main advantages of _dable_ is that it does not introduce high time requirements, as usual for AutoML approaches, and is therefore very suitable for prototyping.

To get a quick introduction to _dabl_, make sure to watch the following SciPy 2020 talk.

{{< youtube TQnxH90PqFc >}}

## Bayesian Stats Modelling Tutorial

Wow, what can I say? [Eric Ma](https://ericmjl.github.io/), a bio medical researcher at Novartis Institutes, did a wonderful job introducing classical frequentist statistical modelling with the help of [NumPy](https://numpy.org/doc/stable/reference/random/index.html), [pandas](https://pandas.pydata.org/) and simply [Matplotlib](https://matplotlib.org/) for visualization purposes, followed by the more _modern_ ;) Bayesian inference approach with the help of [PyMC3](https://docs.pymc.io/). The whole tutorial had a duration of four hours and he delivered a plethora of great references and advises, actually so much, that I will need some time to sort and evaluate them. In the reference section you will find the links and notebooks I have collected.

The accompanying repository, where you will find all covered materials is available from [Github: bayesian-stats-modelling-tutorial](https://github.com/ericmjl/bayesian-stats-modelling-tutorial).

Sadly the video of his session is currently not available online, but I will add it as soon as it is available at Youtube.

## Optimizing Humans and Machines to Advance Science

Finally, [Ana Comesana](https://www.linkedin.com/in/ana-comesana-62b524117/), a Scientific Engineering Associate at Lawrence Berkeley National Laboratory presented her approach for identifying a systematic and robust approach for feature selection, e.g. interpretable dimensionality reduction on 1800 mulecular descriptors for predicting performance properties of bio-jet fuels. She addressed fundamental problems that arise when using machine learning in connection with scientific research, especially the _correlation is not causation_ problem. Make sure to watch her talk! :smile:

{{< youtube ENOf0IZDla8 >}}

## References

- [_dabl - The Data Analysis Baseline Librry_](https://amueller.github.io/dabl/dev/)
- [Bayesian stats modelling tutorial](https://github.com/ericmjl/bayesian-stats-modelling-tutorial)
