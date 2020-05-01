---
title: 'Time Series - A Primer'
subtitle: 'A quick introduction to time series analysis with python'
summary: A very quick primer for facilitating understanding and handling of time series and time series decomposition in pandas
authors:
- mrhenhan
tags:
- Academic
categories:
- Analytics
date: "2019-11-27"
lastmod: "2020-05-01"
featured: false
draft: false
# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

**Understand the core nature of a time series and how pandas helps you with manipulating and analyzing it!**

Time series, sequentially ordered numerical values, are omnipresent throughout nearly every field of interest. They occur in the financial sector, in e-commerce, in medicine and astronomy, and many other fields of interest. Time series naturally arise from observations made at subsequent, usually equispaced, points in time, e.g. by sensor or log readings. Being able to predict or forecast time series is a valuable asset in nearly every operational field as it allows for adaption to upcoming developments, e.g. timely scaling of web server resources, buying or selling distinct assets or restocking of items in a shop. However, handling, analysis and forecasting of time series poses unique challenges we need to address for tasks like the ones mentioned above. This article will define equispaced time series fundamental properties, how to detect these properties with Python and is accompanied by a Jupyter notebook at Github. Hyperlinks and useful references, e.g. the [Jupyter Notebooks](https://mybinder.org/v2/gh/hhain/time-series/master?filepath=01%20Time%20Series%20-%20A%20Primer.ipynb) for this time series article can be found at the end of this text.

**Time series properties:**

Univariate equi-spaced time series are a member of the class of contextual data representations and consist of two attributes.

	1. The so called contextual attribute, the time dimension.
	2. The behavioral attribute, the measured value.

Mathematically a time series is usually defined as $X=\{X_1,X_2,...\}$ or $Y=\{Y_t|t \in T\}$, where $T$ is the so called index set and. Times series thus represent a kind of stochastic process where a distinct time series is a realization of the stochastic process. We usually speak of time series (TS), when the behavioral attribute is a numerical value $x_t \in R$ and of a temporal sequences in case the behavioral attribute is a member of a symbolic set. Equi-spaced means that time-spans between two neighbor points are of equal length $|t_1-t_2|=|t_2-t_3|= ... = |t_{n-1} - t_n|$. From above definition we can infer that time series are usually of very high dimensionality and are therefore subject to the so called curse of high dimensionality.


**Visualizing a Time Series**

To get a first feeling for a TS it is best to visualize it in a meaningful way. Luckily visualizing a TS is very easy to accomplish with the help of Jupyter and Pandas. The following Python code plots a TS representing the minimum temperature for each day in Melbourne, Australia over the course of 10 years.

	import statsmodels.api as sm

	res = sm.tsa.seasonal_decompose(df['Melbourne Min. Temp.'], model='additive', freq=365)
	resplot = res.plot()

{{< figure src="./melbourne_min.png" title="Daily Minimum Temperature in Australia, Melbourne 1981 - 1990">}}

A quick glance at the image reveals some kind of cyclic seasonality. Identifying foundational TS properties is helpful for building predictors and simulators. The next subsection will show how to identify and prove 3 foundational time series properties, namely seasonality, trend, irregularity.

**Time Series Decomposition**

For analysis it is useful to isolate patterns in time series. A TS itself could be thought of as a additive or multiplicative composition of a seasonal, trend, and irregular part, where the original TS is given by one of the following models.

	- $Y_t=Y(offset) + Y(seasonality) + Y(irregularity)$
	- $Y_t=Y(offset) * Y(seasonality) * Y(irregularity)$

A useful tool to capture those properties is the Loess seasonal decompose from statsmodels, which decomposes a time series into its trend, seasonal, and irregular (residual) components. The following Python code will create a seasonal Loess decomposition plot of the Melbourne minimum temperature dataset.

	import statsmodels.api as sm

	res = sm.tsa.seasonal_decompose(df['Melbourne Min. Temp.'], model='additive', freq=365)
	resplot = res.plot()

Seasonal decomposition plot of Melbourne temperature data Melbourne Seasonal Decomposition Plot. The plot itself contains the observed time series, the trend component, the seasonal component and the irregular component of the analysed time series. In this case we used the additive model, as negative or zero values are not supported in case of the multiplicative model.

{{< figure src="./melbourne_tsa.png" title="Seasonal decomposition plot of Melbourne temperature data Melbourne Seasonal Decomposition Plot">}}

**What's next**

This was a very quick primer on time series and seasonal decomposition in Python. In the following articles we examine time series,autocorrelation, and seasonal decomposition more throughly, before we start with using machine learning and deep learning for time series analysis, prediction and forecasting.


**Hyperlinks & References**

    - [Python](https://www.python.org/)
    - [Time Series Primer Jupyter Notebook](https://mybinder.org/v2/gh/hhain/time-series/master?filepath=01%20Time%20Series%20-%20A%20Primer.ipynb)
    - [Pandas](https://pandas.pydata.org/)
    - [Statsmodels](https://www.statsmodels.org/stable/index.html)
    - [Jupyter](https://jupyter.org/)
    - [Github](https://github.com/)
    - [STL: Loess Decomposition Paper](https://www.wessa.net/download/stl.pdf)

