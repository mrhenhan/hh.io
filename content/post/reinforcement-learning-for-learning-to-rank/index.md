---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Personalize Learning to Rank Results through Reinforcement Learning"
subtitle: "An approach to bias information retrieval result rankings with respect to user preferences by applying reinforcement learning principles on the problem area of learning to rank."
summary: "Learning to optimally rank and personalize search results is a difficult important topic in scientific information retrieval as well as in online retail business, where we typically want to bias specific customer query results for the purpose of increasing revenue. Reinforcement learning, as a generic-flexible learning model, is able to bias, e.g. personalize, learning-to-rank results at scale, so that externally specified goals, e.g. an increase in sales and probably revenue, can be achieved. This article introduces the topics learning-to-rank and reinforcement learning in a problem-specific way and is accompanied by the example project 'cli-ranker', a command line tool utilizing reinforcement learning principles for learning user information retrieval preferences regarding text document ranking."
authors: ["mrhenhan"]
tags: ["personalization", "LambdaMart", "search engine", "LambdaRank", "RankNet", "document retrieval", "online retail", "BM25", "PageRank", "eCommerce"]
categories: ["reinforcement learning", "learning to rank", "machine learning"]
date: 2020-05-14T11:08:11+02:00
lastmod: 2020-05-14T11:08:11+02:00
featured: false
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
projects: ["cli-ranker"]
---
Information retrieval and data mining tasks, e.g. online search, the presentation order of queried goods in a webshop, or the preferred order of online advertisements have a thing in common. They are essentially **ranking problems** which are typically tackled with the help of so called **learning to rank** algorithms. **Learning to rank** is considered a subarea of **machine learning**, concentrating on the development of request data based models for solving ranking problems.

Although learning to rank is not a finally solved problem, there is a great interest in extending the current capabilities of learning to rank algorithms for real-world applications in such a way that the interests of query and response providers are equally satisfied. Because the previous fact sounds a bit abstract at first, I will try to explain this with a simple example, from the online retailer business domain, that will accompany us throughout the rest of this article.

## Example Case: All.In Retail Online Shop

We assume that we are the operators of a large online business, called All.In Retail a company of the [All.In Data](https://www.all-in-data.de/de/) Group, and we have a contiuous large amount of customers of different psychological types, tastes, and so on. In addition, despite a wide range of products, we have many products of almost functional equivalence, but associated with different margins, storage, procurements or processing costs.

After a certain period of time we begin to notice that the search function of our online presence returns the products requested by the customer, but it does not prioritize or devalue the products that are associated with high margins or storage costs or high procurement or processing costs for us.

We also find that there is no difference in the conversion or churn rate between the number of customers who come directly to a product page from an external source and those who use the internal search.

## Example Case: Learning to Rank and Personalization




Part of intro
- Learning to rank itself challenging and far from beeing solved but 2 other sides to learning to rank which should be balanced / balancing problem -> provider presents things that he wants to be found and that closely match what the querier wants to find
  1. Personalization, e.g. retrieving Information I prefer to find
  2. External bias Information the provider likes to be found (e.g. products who have a large margin, high storage costs and so forth.)
  
# Introduction to learning to rank

# two learning to rank algorithms

# Learning to rank influence parameters

# Reinforcement Learning Introduction

# Reinforcement Learning for learning to rank

# Software Engineering aspect/Systems integrations

# CLI sample project

# Offer to solve learning to rank problems
  
