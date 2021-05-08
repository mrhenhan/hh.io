---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "The persistent problem of missing data"
subtitle: ""
summary: "Deploying descriptive, predictive and prescriptive machine learning solution using complete data is difficult, but even more difficult in face of missing data. A gentle introduction to the reasons of missing data and the difficulties generated."
authors: [mrhenhan]
tags: [missing data, missing value imputation, statistics, unsupervised learning, supervised learning]
categories: [machine learning]
date: 2021-05-07T18:00:05+02:00
lastmod: 2021-05-07T18:00:05+02:00
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

If the missingness mechanism is not MCAR or MAR the missingness mechanism is
called **missing not at random (MNAR)**. Essentially, this means that the
probability of data points missing varies because of reasons we do not know.

The reasons for missingness and the missingness mechanisms are necessary
concepts for being able to reason about why some missing value imputation
solutions may work for our distinct missing value problem and some do not.

## A review on some missing value imputation solutions

