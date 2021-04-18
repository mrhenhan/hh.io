---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "SciPy 2020 Day Two"
subtitle: "Half of the Dask tutorial, Continuous Integration for Scientific Projects and more..."
summary: ""
authors: ["mrhenhan"]
tags: ["SciPy 2020", "Python", "machine learning", "Dask", "CI", "scientific projects", "data science", "histogram", "boost", "C++14", "Pandas", "Numba", "Plotly", "xarray", "sklearn", "matplotlib", "bokeh"]
categories: ["SciPy", "Python", "parallel computation", "continuous integration"]
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

Today I also participated at the 'Welcome & SciPy Tools Plenary Sessions'. During the Welcome session the remainder of the daily schedule was presented and the gold sponsors were honoured. It was relatively quick session mainly intended for introducing the SciPy Tools Plenary session. During the SciPy Tools Pleanry session the following SciPy Python libraries where mentioned.

### Pandas

[Pandas](https://pandas.pydata.org/) version 1.1 will be released in July 2020 and the funding was secured with the help of CZI and NumFOCUS. With the Pandas 1.0 release a new Pandas logo was introduced with improvements regarding the Pandas website theme and the documentation.

Pandas got serveral larger and smaller improvements w.r.t Numba integration and new data types like dedicated nullable _pd.NA_ for missing data and ExtensionArrays. User defined functions (UDF) are now able to reduce python overhead when using the Numba engine.

### Numba

[Numba](http://numba.pydata.org/) is adopted by more large projects, like Awkward and Pandas and underwent massive refactorings as the code base was reorganized and old Python and NumPy version support and hacks were removed. Numba now has better error reporting too and benefits from a growing user community with > 1.4M downloads/month. It is noteworthy that the people behind Numba work on a new governance model to encourage new maintainers and contributers to participate in development.

### Plotly

[Plotly](https://plotly.com/) a plotting library which integrates with [Python](https://www.python.org/), [R](https://www.r-project.org/) and [Julia](https://julialang.org/) and core of the [Dash](https://plotly.com/dash/) framework has introduced a lot of improvements, like the Express high level API, removal of the binding to external servers, improved tile maps with support for all tile servers, hierarchical data plot, imshow, and serveral speedups. Furthermore, they integrated a new jupytertext backend for documentation purposes and a new Python API reference interface. 

Furthermore, they announced a new static image export system, Python to JavaScript access, expanding Plotly express and better support for sequence and periodic data.

Currently Plotly has about 3M downloads/month and more than 50 Github contributers.

### xarray

[xarray](http://xarray.pydata.org/en/stable/) a library for working with labelled multi-dimensional arrays, improved through implementing sparse integrations and other [NEP-18](https://numpy.org/neps/nep-0018-array-function-protocol.html) array integrations and support for dataset argmin, argmax as well as dataset weighted mean functionality.

They got funded now by CZI and currently need help with improving the documentation.

### sklearn

[sklearn](https://scikit-learn.org/stable/) refreshed their website and they further improved the user guide. A new plotting API based on [Matplotlib](https://matplotlib.org/) was introduced. sklearn now has support for stacked classifiers (output of one classifier is a feature for the next one), HistGradientBoosting with missing value support and monotonic constraints as well as nearest neighbor graphs don't need a complete recomputation upon introducing new data points. Additionally, the are minor improvements regarding the kmeans algorithm scalability and generalized linear models were implemented.

One awesome feature is, that sklearn now has rich visual interactive representations of estimators and computation pipelines!

### matplotlib

[Matplotlib](https://matplotlib.org/) got one year of funding from the [Chan Zuckerber Initiative](https://chanzuckerberg.com/) and introduced test baselin image relocation (GSoC). There were multiple bugfix releases last year and they are preparing for a 3.4 release in September 2020 with new bar3d light source support, simplified tick formatters, and a new mosaic subplot as well as post-hoc axes sharing. They further try to improve the current documentation.

### bokeh

[Bokeh](https://docs.bokeh.org/en/latest/index.html) now supports static and dynamic plots and improved WebGL support using ReGL. They start to support server side rendering of large datasets and are promoting the Bokeh protocol for interactions between Python and JavaScript and are trying to decouple Bokeh from [Tornado](https://www.tornadoweb.org/en/stable/) and Websockets. Furthermore they want to allow Python callbacks without a separate Bokeh server. For future updates, they want to introduce generalized and abstract widgets and interactions, improve the user experience regarding building dashboards and tools, and want to adopt LaTeX and MathText support. It is intended to tighten the BokehJS libraries and improve the memory performance.

## Lightning Talks

I also watched the punalicious lightning talks at day 2 of SciPyConf 2020 - Many thanks to the hilariously funny hosts. I espacially remembered the funny talks about Rapids an end2end accellerated GPU data science library, Frappucino, a library helping with API changes and a study about visual pleasing color cycles intended for improving coloring for people suffering from duteranopia, protanopa or tritanopia. Colors must be accessible to anybody!

Kind regards,

Henrik Hain
