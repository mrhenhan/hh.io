---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Scipy 2020 Day Three"
subtitle: "Deep Learning from Scratch, Pandera, Awkward Arrays, and Scalabe ML with Ray"
summary: ""
authors: ["mrhenhan"]
tags: ["SciPy 2020", "Python", "machine learning", "deep learning", "PyTorch", "NumPy", "Score-P", "Ray", "Awkward Array", "pandera", "RayServe"]
categories: ["SciPy", "Python", "Deep Learning", "Machine Learning"]
date: 2020-07-09T10:18:07+02:00
lastmod: 2020-07-09T10:18:07+02:00
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
The third day of SciPy 2020 was filled with interesting and foundational tutorial content regarding deep learning with a short primer to the [PyTorch library](https://pytorch.org/) and I found the time to watch some interesting SciPy talks from [Enthoughts SciPy Youtube channel](https://www.youtube.com/c/enthought/playlists?view=50&sort=dd&shelf_id=4) as well. Fortunately, my internet provider Vodafone decided late in the evening to have massive connectivity problems all over Mannheim so that I didn't miss too much.

My third day began with the talks _Pandera: Statistical Data Validation of Pandas Dataframes_ from Niels Bantilan, _Ray: A System for Scalable Python and ML_ held by Robert Nishihara, and _Machine Learning Model Serving: The Next Step_ from Simon Mo, before I participated at the tutorial _Deep Learning from Scratch with PyTorch_. After the tutorial I managed to watch some additional conten and watched the talks _Analyzing the Performance of Python Applications Using Multiple Levels of Parallelism_ from Christian Harold, a fellow researcher from Dresden and the introductionary talk _Awkward Array: Manipulating JSON like Data with NumPy like Idioms_ held by Jim Pivarski.

Together with the content from the former two days, my reading and research list ist starting to grow extremely fast and I guess I need to find some time slots for isolated batch processing :smile:

So, let's start with a little bit of content review.

## Pandera: Statistical Data Validation of Pandas Dataframes | Niels Bantilan

[pandera](https://pandera.readthedocs.io/en/stable/) is a data validation library for correctness and hypothesis tests on Pandas dataframes in tidy (long-form) and wide data format (see [Tidy Data, Tidy Types, and Tidy Operations](https://henrikhain.io/post/tidy-data-types-and-operations/) for more informations on tidy data). For runtime validation of pandas data structures pandera uses informations pandas provides.

Pandera is able to _check types_ and properties of columns in a [pd.DataFrame](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html) or values in a [pd.Series](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.html).

Furthermore, pandera is able to perform more complex statistical validations like _hypothesis testing_ and integrates with existing data science processing pipelines through _function decorators_.

Make sure to watch the very good talk from Niels Bantilam.

{{< youtube PxTLD-ueNd4 >}}

## Ray: A System for Scalable Python and ML | Robert Nishihara

Robert Nishihara presented [Ray](https://ray.io/), which tries to consolidate the merging areas of big data stores, deep learning, web services and high performance computing by helping with distributed computing and model serving.

Ray allows for execution and utilizing parallelism on your local computer as well as scaling out to computing clusters on [Legion](https://legion.stanford.edu/), [AWS](https://aws.amazon.com/de/), [Azure](https://azure.microsoft.com/en-us/), and [Google Cloud](https://cloud.google.com/) and proves to be similar to tools like Dask while mainting ease of use. 

{{< youtube XIu8ZF7RSkw >}}

## Machine Learning Model Serving: The Next Step | Simon Mo

[RayServe](https://docs.ray.io/en/releases-0.8.5/rayserve/overview.html), an independent compontent of the formerly introduced Ray, addresses the problem of model serving for interactive scoring or batch predictions. Simon Mo does a great job highlighting the issues one may experience with obvious solution like serving a model via, e.g. a [Flask](https://flask.palletsprojects.com/en/1.1.x/) web service and furthermore discusses the common alternativ and short-comings of using an externalized tensor prediction service as utilized by [TFServing](https://www.tensorflow.org/tfx/guide/serving), [SageMaker](https://aws.amazon.com/de/sagemaker/), and others.

RayServe claims to overcome the deficiencies of both approaches via an easy to use CLI interface and recipes. Make sure to watch his video if you are interested in model serving at scale!

{{< youtube rFZhwsSxZ_Q >}}

## Tutorial: Deep Learning from Scratch with PyTorch

The four hour tutorial about Deep Learning from Scratch with PyTorch is a really great ressource about the foundations of implementing neural networks with [NumPy](https://numpy.org/) an the error prone complexity it involves and why [PyTorch](https://pytorch.org/) should be used in the first place. Additionally, one of the most noteworthy and commendable facts is that the tutors Hugo Bowne-Anderson and Dhavide Aruliah started with possible social issues deep learning could pose.

Unfortunately after the basics there were only 15 minutes left for the PyTorch part and Deep Learning with its own advantages and disadvantages was not or almost not introduced anymore. Nevertheless the linked resources are a great learning resource with many facts, tips and tricks.

As with the other tutorials, the YouTube video will be linked as soon as it is generally available. In the meantime, be sure to check out the Jupyter notebooks, they are awesome :heart:

[Deep Learning from Scratch with PyTorch]https://github.com/hugobowne/deep-learning-from-scratch-pytorch

## Analyzing the Performance of Python Applications Using Multiple Levels of Parallelism | Christian Harold

[Score-P](https://www.vi-hps.org/projects/score-p) introduces a scalable performance measurement infrastructure for parallel codes and comes with a practical Python module - Score-P Python or instrumenting Python code. In general, Score-P provides serveral instrumentation wrappers for process-level, thread-level, accelerator bases parallelism as well as wrappers for external code from [OpenBLAS](https://www.openblas.net/), [LAPACK](http://www.netlib.org/lapack/), and [FFTW](http://www.fftw.org/). Basically, Score-P provides a view on parallelism and ressource usage for highly distributed application code. Additionally, there are multiple extensions providing different views on application behavior and it integrates with [Vampir](https://vampir.eu/) and [Cube](http://apps.fz-juelich.de/scalasca/releases/cube/4.5/docs/guide/html/).

{{< youtube hTmXpvws52M >}}

## Awkward Array: Manipulaing JSON like Data with NumPy like Idioms | Jim Pivarski

[Awkward Array](https://github.com/scikit-hep/awkward-array) is a library from the scikit-hep environment that introduces NumPy like idioms for [JSON](https://www.json.org/json-en.html) like data. Awkward Array is useful if you need to have JSON or dictionary like data in array format for scalability and performance purposes while being able to do mathematics and sclicing.

Additionally, Awkward Array integrates with Numba optimizations, e.g. [Numba](http://numba.pydata.org/) compiled functions. The library is exceptionally well suited to handle [GeoJSON](https://de.wikipedia.org/wiki/GeoJSON) data at scale!

{{< youtube WlnUF3LRBj4 >}}


Kind regards,

Henrik Hain
