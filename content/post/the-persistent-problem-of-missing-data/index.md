---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "The persistent problem of missing data"
subtitle: ""
summary: "Deploying descriptive, predictive and prescriptive machine learning solution using complete data is difficult, but even more difficult in face of missing data. A gentle introduction to the reasons of missing data and the difficulties generated."
authors: [mrhenhan]
tags: [missing data, missing value imputation, statistics, unsupervised learning, supervised learning]
categories: [machine learning]
date: 2021-05-07T18:00:05+02:00
lastmod: 2021-05-09T14:00:05+02:00
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

# The missing data problem

Missing data is ubiquitous and poses a challenging problem for analysts, data
scientists and machine learning engineers. Among other things, getting analysis
methods, machine learning procedures and corresponding software to run at all in
the presence of missing data, on the other hand, the constant question lurking
in the background as to whether the models created are at all valid and
generalizable. Unfortunately, implicitly applied ad hoc methods such as listwise
deletion, mean imputation, and complete case analysis are still very often used
without analyzing their effect on the results. Most research papers do not even
mention how missing data was handled, probably to get through the review
process and this methodology seems to be common practice in economics as well. 

> "Obviously the best way to treat missing data is not to have them."
> '--- Orchard and Woodbury 1972, p. 697'

It is obvious that this raises legitimate concerns about the correctness of the
respective analysis results and, in my opinion, it is often necessary to take
into account the different scenarios created by the way missing data have been
handled.

## Reasons for missing values

There are plenty of reasons for missing data, from sensor failure to data
transmission errors or more commonly in the software engineering and business
domains, due to inconsequentially and inconsistently captured metrics and KPIs
due to, for example, a missing digitization and data management strategy.
Another example for missing data is data being blocked by the visitor of a web
presence, e.g. user agent information. The reason for missing data which can be
broadly categorized as either missing *intentionally* or missing
*unintentionally*, where intentionally missing data was planned by the data
collector. Example for intentionally missing data would be intentionally
excluded sample units or missing data due to the routing of an questionnaire or
censored data. Unintentionally missing data is often foreseen but essentially an
unplanned occurence and not under control of the data collector. Examples of
this are the missing user agent data due to blocking by the web site visitor
mentioned at the beginning, or sloppily completed and managed process metrics in
tools to support a software engineering process or sales.

Knowing the reasons for missing values is a helpful first step towards
identifying the missingness mechanism underlying the missing values which is
needed to select an appropriate missing value imputation solution.

## Missingness mechanisms 

The missingness mechanism is a foundation concept in working with missing data
defined by Rubin (1976). Rubin classified missing data problems into three
categories according to the likehood of data points being missing. The **missing
data mechanism** or **response mechanism** is the process that governs these
probabilities according to Stef van Buuren (2018).

The missing data mechanism is said to be **missing completely at random (MCAR)**
if the probability of being missing is the same for all data points, which means
that causes for missingness are unrelated to the data. In the fortunate and
mostly unrealistic case that MCAR is true, then we are able to ignore many of
the complexities that arise due to missing data and may even employ simple
ad-hoc solutions for addressing missing data.

The missing data mechanism is said to be **missing at random (MAR)** if the
probability of data points being missing is the same only within groups in the
**observed** data. Thus the data is MCAR only within those groups and the
probability of being missing varies between groups.

If the missingness mechanism is neither MCAR nor MAR the missingness mechanism
is called **missing not at random (MNAR)**. Essentially, this means that the
probability of data points missing varies because of reasons we do not know.

The reasons for missingness as well as the missingness mechanisms are necessary
concepts for being able to reason about why some missing value imputation
solutions may work for our distinct missing value problem and some do not.

## A review on the problems of missing value imputation solutions

Missing value imputaton is different from data synthesis or prediction due
to the underlying missingness reasons and mechanisms. Most solutions, even more
advanced regression and machine learning solutions, only hold, given that the
underlying missingness mechanism MCAR is true, which is very often not the
case.

The problem with imputing predicted values is that they only yield realistic
imputation, if the prediction is close to perfection but that would be like
recreation of the missing data from the available data which naturally
strengthens the relations between the variables and therefor will bias
correlations upwards. Essentially the variability of the data is underestimated
and the imputations are too good to be true according to van Buuren (2018).

For example, in reality the data may be missing because they are outliers with
weak relations to the remaining data. Imputing predited data would not estimate
this weak relation and would result in false positives and spurios relations.

## Solution approaches to the problems of missing value imputation

As already mentioned, the goal of a missing value imputation solution is to
stably impute missing values such that there is no bias introduced under any
of the missingness mechanisms MCAR, MAR, and MNAR. Most simple approaches like
listwise or pairwise deletion only hold within MCAR and reduce the available
information considerably. Mean imputation strongly reduces variance in the data
and results in to small standard error. Regression imputation may hold under
MAR but strengthens the relation of the variables and essentially results in the
standard error being too small. Last observation carried forward (LOCF),
in case of time-to-event data, as well as first observation carried backward
(FOCB) yields a too small standard error as well and they do not hold true
under any missingness mechanism. The same is true for the indicator imputation
solution.

There are some interesting approaches addressing the above short-comings of
ad-hoc solutions with **stochastic regression imputation** introducing the concept
of drawing random noise from the regression residuals, essentially spoiling
the best imputation. This may seem counter-intuitive at a first glance but
prevents imputations from being too good to be true while preserving the
correlation between the variables.

Another promising approach is **multiple imputation**. Using multiple imputation
one creates $m > 1$ complete data sets, where each data set is analyzed
individually. The $m$ analysis results are then pooled into a final estimate
including the standard error by some pooling rules. The imputed values
are drawn from a distribution modeled individually for each missing entry, so
that the $m$ data sets are complete but differ in the imputations only. The
magnitude of differences between the imputations models the uncertainty we have
about the missing values effectively.

**Stochastic regression imputation** and **multible imputation** especially may
be computational prohibitive in face of very large data sets and large
proportions of missingness.

An interesting approach would be to utilize the context a data set was created
for and within to find correlated information for imputation thus reducing
the amount of data needed as well as unnecessary noise with the help of 
unsupervised contextual clustering, while preserving the statistical properties
of the data set.

## Conclusion

Correct and stable missing value imputation is a challenging problem closely
related to prediction and data synthesis approaches. Most of the content here
was drawn from the excellent introduction to missing value imputation from
[Steven van Buuren](https://stefvanbuuren.name) based on the groundbreaking
work on missing value imputation by Donald B. Rubin.

This article introduced the challenging problem of missing value impution
and highlighted some of the difficulties facing missing data. Short-comings
of ad-hoc missing value imputation solutions were addressed and some conceptual
solution approaches were discussed.

In the next article I will discuss **stochastic regression imputation**,
**multiple imputation**, and recent **machine learning** based imputation
approaches including there strengths and short-comings in greater detail.

Kind regards,

Henrik

## References

- [Flexible imputation of missing data 2018](https://stefvanbuuren.name/fimd/)
