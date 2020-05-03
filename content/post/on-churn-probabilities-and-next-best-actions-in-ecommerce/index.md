---
title: "On Churn Probabilities And Next Best Actions In Ecommerce"
subtitle: "Why do decision makers only ever want information about the status quo?"
summary: "An alarminlgy high percentage of [All.In Data's](https://www.all-in-data.de) customers prefer machine learning systems that classify or quantify a distinct condition, e.g. customer churn, to systems that are capable of acting immediately in an optimal way on a given situation. This post intends to explain the principles of a system that is able to automatically react as optimal as possible to ecommerce customer actions."
authors: [mrhenhan]
tags: [ecommerce, ml, baysian bandit, reinforcement learning, next best action, churn prevention,churn rate, conversion rate, recommender systems]
categories: [ecommerce, machine learning, reinforcement learning]
date: 2020-05-03T00:13:57+02:00
lastmod: 2020-05-03T00:13:57+02:00
featured: true
draft: true

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
There are serveral ways to increase the turnover of a typical online retailer. One of the many possible optimization is to influence the proportion between buyers and non-buyers in such a way that turnover and profit increases increase. The proportion of buyers in a visitor population over a period of time can be determined by the _conversion rate_, whereas the proportion of non-buyers can be characterized by the complement of the conversion rate, the so-called _churn rate_. Thus, naive conversion rate for the number of visitors in a time interval $t$ can be defined as 

$$Rate_{conversion}=\\frac{|Visitors_{buyers}|}{|Visitors|}$$
   
and the complementary churn rate being $1-Rate_{conversion}$.
