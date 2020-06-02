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

We assume that we are the operators of a large online business, called All.In Retail a company of the [All.In Data](https://www.all-in-data.de/de/) group, and we have a contiuous large stream of webshop visitors of different psychological types, tastes, and so on. In addition, despite a wide range of products, we have many products of almost functional equivalence, but associated with different margins, storage, procurements or processing costs.

After a certain period of time we begin to notice that although the search function of our online shop returns the products requested by the customer just fine, it does not prioritize or devalue the products that are associated with high margins, storage costs or high procurement or processing costs for us.

We also find that there is no difference in the conversion or churn rate between the number of customers who come directly to a product page from an external source and those who use the internal search. This also calls into question the way the recommendation measures are working and how they are related to the internal search engine.

## Example Case: Learning to Rank and Personalization

Analyzing the previous description it becomes rather obvious that we have a rather specific balancing problem at hand. Without complicating the problem by taking areas like, e.g. after-sales into account, we intend to optimize revenue by selling those products with the highest margin and lowest possible processing or maintaining costs as often as possible. This means that we would like to have them appear in a prominent position in related search queries.

{{% alert note %}}
Prioritize and weight products with high profit margins and low costs in internal related search queries with the help of learning to rank algorithms!
{{% /alert %}}

The aforementioned goal is a typical target for so-called learning to rank algorithms, which are used currently more and more prominently in the area of online retail business and web searches. Relatedly, these algorithms can also be used to influence or bias recommendation engine results.

The second part of the optimization procedure is based on the hypothesis, that the probability of closing a sales together with the probability of visitor revisits will increase with higher search result and visualization personalization according to the psychological type of the visitor.

{{% alert note %}}
Probability of closing a sale and the probability of visitor revisits will increase with higher search result and visualization personalization according to the psychological type of the visitor!
{{% /alert %}}

Usually visitor product preferences are conflicting with our goal of increasing revenue, thus we need to balance our preferences against personal visitor preferences leading to the $volume \times frequency$ [^1] optimization problem. The $volume \times frequency$ formulation states that we ideally want to increase the single sales transaction $volume$, e.g. the margin and want to increase the $frequency$ this transaction happens, given that there are no _transaction costs_ involved.

{{% alert warning %}}
We need to balance our search proposal preferences against personal visitor preferences to optimize the typical $volume \times frequency$ formulation.
{{% /alert %}}

In order to get an optimal result, meaning a sufficient number of visitors ($frequency$) buy products with high revenue ($volume$), we need to bias the search engine results, i.e. the ranking of the internal search or recommendation egine, so that we meet the needs of the respective psychological customer type according to the aformentioned personalization hypothesis and thus sell more often ($frequency$) and optimize our profit ($volume$) parallely.

The next part introduced learning to rank in a generic way before we introduce how deep learning, or more specifically, deep reinforcement learning can be used to dynamically shift search engine results in a way that will optimize the $volume \times frequency$ optimization problem according to the different psychological visitor types.
  
## Learning to Rank

Usually, learning to rank is formalized as supervised machine learning problem where the training data consist of a set of objects together with their total or partial orders and a specific ranking model, e.g. a ranking function approximation is learned via a neural net for example.  The learning to rank method is the place where we introduce our personal preferences by parameterizing the used algorithm. 

{{% alert note %}}
Personal ranking preferences can be introduced to the ranking method through parameterization of the ranking algorithm.
{{% /alert %}}

Since a supervised learning model consists of the training data and a specific procedure, there are various possibilities for parameterization, e.g. by targeted modification of the training data. The training data consists of queries and documents, where each query is associated with a number of documents and the relevance of the documents with respect to the query. Typically _relevance_ is given as label denoting the _grade_ of a document, where _high grades_ are associated with _high document relevance_.

During prediction, the predictor receives a new set of objects as input and subsequently generates a ranking list of the received objects using the learned ranking model.

{{% alert note %}}
Although we are talking about documents, the basic principle remains the same for product pages, blog entries, recommendation items and any item associated with some kind of (meta-)information.
{{% /alert %}}

More formally, we assume $Q$ to be the query set, $D$ to be the document set, and $Y=\\{1,2,...,l\\}$ to be the label set, where labels are associated with totally ordered grades, meaning that $l \succ l-1 \succ ... \succ 1$. Now we define $Q=\\{q_1,q_2,...,q_m\\}$, with $q_i$ as the $i$-th query and $D_i=\\{d_{i,1}, d_{i,2}, ..., d_{i,n_{i}}\\}$ as the set of documents associated with the $i$-th query. Furthermore let $y_i=\\{y_{i,1}, y_{i,2},...,y_{i,n_{i}}\\}$ be the set of labels associated with the $i$-th query and document set. Therefor, the training set can be defined as $T=\\{(q_i,D_i), y_i\\}^{m}_{i=1}$.

Former definitions implicate that feature vectors $x_{i,j} = \varnothing(q_i,d_{i,j})$ used for training the learning to rank model are created from already existing or simulated query-document tuples $(q_i,d_{i,j})$, essentially meaning, that features are defined as functions of query-document tuples. Features, defined as functions of query-document tuples represent one possibility to intrinsically bias search results for the purpose to rank internally preferred products higher, although this approach has the disadvantage that a learning to rank system would have to be retrained every time a change of ranking preferences is desired.

{{% alert note %}}
Frequent preference changes of the training data would probably require prohibitively expensive rebuilds of an formerly trained learning to rank model.
{{% /alert %}}

Our goal is it to train a ranking model $f(q,d) = f(x)$ or a scorer who is able to assign a specific score to a provided query-document tuple $(q, d)$ - More precisely the model should be able to assign a score to a given feature vector $x$. Finally, ranking can be defined as selecting a permutation $\pi_i \in \Pi_i$ for a given query-document tuple with the help of the scores given by the ranking model $f(q_i,d_i)$.

{{% alert note %}}
Ranking can be considered as selecting a distinct permutation $\pi_i$ from a set of possible permutations $\Pi_i$ with the help of a scorer, which means that ranking preferences can be superimposed both via scorer and result re-weighting or permutation.
{{% /alert %}}

Together with the formerly introduced possibilities to bias ranking results, there are three locations to superimpose personalized shifts in a models ranking.

1. Introduce changes to the training data.
3. Introduce parameters to the ranking model (scorer) itself, e.g. via the loss function.
2. Permute or re-weight the ranking result.

One may have noticed that the supervised ranking problem is somehow similar to supervised learning w.r.t. train- and test data but differs in that query-document tuples form a group, where inter-group relations are considered i.i.d. data and intra-group data is not i.i.d.. In order to take this into account, adjustments are necessary in corresponding training and testing procedures.

## Evaluating Ranking Performance in Business

Although they are well known evaluation measures in the field of document retrieval, e.g. **Normalized Discounted Cumulative Gain (NDCG)** or **Discounted Cumulative Gain (DCG)**, those are usually not the **key performance indices (KPI)** we are interested in, in the area of business. Nonetheless we introduce those measures as we are able to _extend evaluation measures to account for revenue gain_ associated with a positional query result ranking, which will be useful when we will introduce _reinforcement learning_ as ranking optimization procedure. There are more evaluation measures of course, like e.g. **Kendall's Tau** or **Mean Average Precision (MAP)**, each with distinct benefits and drawbacks. The best working evaluation measure is use case specific and has to be evaluated for each distinct application case.

### Discounted Cumulative Gain (DCG)

Given a query-document tuple $(q_i,D_i)$ with $\pi_i$ being the chosen ranking permutation on $D_i$ and $y_i$ is the set of grades on $D_i$, then DCG measures the goodness of $\pi_i$ w.r.t. $y_i$. That means DCG at a position k can be defined as

$$
DCG(k) = \sum_{j:\pi_i(j) \leq k} G(j)D(\pi_i(j)),
$$

where $G_i(.)$ is an arbitrary gain function, $D_i(.)$ a positional discount function, and $\pi_i(j)$ is the position of $d_{i,j}$ in $\pi_i$ with the summation being over the top $k$ positions in the ranking list $\pi_i$. Normally, the gain function is defind as some kind of exponential function of grade $G(j) = 2^{y_{i,j}} - 1$, with $y_{i,j}$ represents the label of a specific document $d_{i,j}$ in a ranking $\pi_i$. Additionally, a position discount function is typically of a logarithmic type of a position $D(\pi_i(j)) = \frac{1}{log_2(1 + \pi_i(j))}$, with $\pi_i(j)$ indicating the position of document $d_{i,j}$ in ranking $\pi_i$. This means that the DCG is representing the cumulative gain of accessing the information from position one to position k with discounts on the position. Subsequently, the DCG at position k will be

$$
DCG(k) = \sum_{j:\pi_i(j) \leq k} \frac{2^{y_{i,j}}}{log_2(1 + \pi_i(j))}.
$$

As a normalized evaluation measure has the property of being more easily to compare, a normalized evaluation measure should be used for evaluating performance between different ranking models.

### Normalized Discounted Cumulative Gain (NDCG)

Extending the formerly introduced definition of the DCG, the NDCG at position k is defined as

$$
NDCG(k) = G^{-1}_{max,i}(k)DCG(k),
$$

with $G_{max,i}(k)$ being a normalization factor chosen in a way that a perfect ranking $\pi^{*}_i$ yields a NDCG score of $1$ at position $k$. Thus NDCG at position $k$ is defined as

$$
NDCG(k) = G_{max,i}^{-1}(k) \sum_{j:\pi_i(j) \leq k} \frac{2^{y_{i,j}}}{log_2(1 + \pi_i(j))}.
$$

DCG as well as NDCG values are further averaged over queries in model evaluation.

# two learning to rank algorithms

# Learning to rank influence parameters

# Reinforcement Learning Introduction

# Reinforcement Learning for learning to rank

# Software Engineering aspect/Systems integrations

# CLI sample project

# Offer to solve learning to rank problems
  

Part of intro
- Learning to rank itself challenging and far from beeing solved but 2 other sides to learning to rank which should be balanced / balancing problem -> provider presents things that he wants to be found and that closely match what the querier wants to find
  1. Personalization, e.g. retrieving Information I prefer to find
  2. External bias Information the provider likes to be found (e.g. products who have a large margin, high storage costs and so forth.)
  

[^1]: Simple formulation without discriminatory parameters.
