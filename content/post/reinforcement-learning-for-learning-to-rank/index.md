---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Personalize Learning to Rank Results through Reinforcement Learning"
subtitle: "Introduction to Learning to Rank and Reinforcement Learning for Personalization Purposes"
summary: "Learning to optimally rank and personalize search results is a difficult and important topic in scientific information retrieval as well as in online retail business, where we typically want to bias customer query results with respect to specific preferences for the purpose of increasing revenue. Reinforcement learning, as a generic-flexible learning model, is able to bias, e.g. personalize, learning-to-rank results at scale, so that externally specified goals, e.g. an increase in sales and probably revenue, can be achieved. This article introduces the topics learning-to-rank and reinforcement learning in a problem-specific way and is accompanied by the example project 'cli-ranker', a command line tool utilizing reinforcement learning principles for learning user information retrieval preferences regarding text document ranking."
authors: ["mrhenhan"]
tags: ["personalization", "LambdaMart", "search engine", "LambdaRank", "RankNet", "document retrieval", "online retail", "BM25", "PageRank", "eCommerce"]
categories: ["reinforcement learning", "learning to rank", "machine learning"]
date: 2020-06-03T09:30:11+02:00
lastmod: 2020-06-03T09:30:11+02:00
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
projects: ["cli-ranker"]
---
Information retrieval, data mining tasks, the search feature of an online retailer, the presentation have one thing in common! They are essentially **ranking problems**, which are typically tackled with the help of so called **learning to rank** algorithms. **Learning to rank** is considered a subarea of **machine learning**, concentrating on the development of request data based models for solving ranking problems.

Although learning to rank is not a finally solved problem, there is a great interest in extending the current capabilities of learning to rank algorithms for real-world applications in such a way that the interests of query and response providers are equally satisfied. Because the previous fact sounds a bit abstract at first, I will try to explain this with a simple example from the online business domain, that will accompany us throughout the rest of this and the next article and will show **how our personalized learning to rank solution will increase your online business revenue stream**.

## Example Case: All.In Retail Online Shop

We assume we are operators of a large online business, called All.In Retail a company of the [All.In Data](https://www.all-in-data.de/de/) group, and we have a continuous large stream of webshop visitors of different psychological types, tastes, and so on. In addition, despite a wide range of products, we have many products of almost functional equivalence, but associated with different margins, storage, procurements or processing costs.

After a certain period of time we begin to notice that although the search function of our online shop returns the products requested by the customers just fine, it does not prioritize or devalue products associated with high margins, storage costs or high procurement or processing costs.

We also find that there is no difference in the conversion or churn rate between the number of customers who directly visit a product page from an external source and those who use the internal search. This also calls into question the way the recommendation measures are working and how they are related to our internal search engine.

{{% alert info %}}
Did you know that you can consult [All.in Data GmbH](https://www.all-in-data.de/de/kontakt/) about how to increase the performance of your online business and that we are happy to make you an individual offer regarding the use of our ranking solution? 
{{% /alert %}}

## Example Case: Learning to Rank and Personalization

Analyzing the previous description it becomes obvious that we have a very specific balancing problem at hand. Without complicating the issue by taking areas like, e.g. after-sales into account, we intend to optimize revenue by selling product variants with the highest margin and lowest possible processing or maintaining costs, as often as possible. This means that we would like to have them appear in a prominent position in related search queries or recommendations.

{{% alert note %}}
Prioritize and weight products with high profit margins and low costs in internal related search queries with the help of learning to rank algorithms!
{{% /alert %}}

The aforementioned goal is a typical target for so-called learning to rank algorithms, which are used currently more and more prominently in the area of online retail business and web searches. Relatedly, the algorithms can also be adapted in a way to influence or bias recommendation engine results in our favor.

The second part of the optimization procedure is based on the hypothesis, that the probability of closing a sale together with the probability of visitor revisits will increase with higher search result and visualization personalization, according to the psychological type of the visitor.

{{% alert note %}}
Probability of closing a sale and the probability of visitor revisits will increase with higher search result and visualization personalization according to the psychological type of the visitor!
{{% /alert %}}

Quite often visitor product preferences conflict with our goal of increasing revenue, thus we need to balance our product preferences against personal visitor preferences, subsequently leading to the $volume \times frequency$ [^1] optimization problem. The $volume \times frequency$ formulation states that we ideally want to increase the single sales transaction $volume$, e.g. the margin and want to increase the $frequency$ this transaction happens in parallel, given that there are no _transaction costs_ involved.

{{% alert warning %}}
We need to balance our search proposal preferences against personal visitor preferences to optimize the typical $volume \times frequency$ formulation.
{{% /alert %}}

In order to get an optimal result, meaning a sufficient number of visitors ($frequency$) buy products with high revenue ($volume$), we need to bias our search engine results, i.e. the ranking of the internal search or recommendation engine, so that we meet the needs of the respective customer psychology and preferences, according to the aformentioned personalization hypothesis and thus sell more often ($frequency$) and optimize our profit ($volume$) parallely.

The next part introduced learning to rank in a generic way before we introduce how deep learning, or more specifically, deep reinforcement learning can be used to dynamically shift search engine results in a way that will optimize the $volume \times frequency$ optimization problem according to the different psychological visitor types.
  
## Learning to Rank

Usually, learning to rank is formalized as supervised machine learning problem where the training data consist of a set of objects together with their total or partial orders and a specific ranking model, e.g. a ranking function approximation is learned via a neural net for example.  The learning to rank method is the place where we introduce our personal preferences by parameterizing the used algorithm with the help of (deep) reinforcement learning.

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

## Supervised Objective Learning Function

In the present case, the learning to rank task is defined within the machine learning framework of supervised learning. Remembering formerly introduced differences w.r.t the input and output data in comparison to normally specified supervised machine learning tasks, we will consider $X$ as the feature space build from list of feature vectors and $Y$ as the output space in form of lists of grades, with $x \in X$ being a list of feature vectors and $y \in Y$ being a list of grades. Assume $P(X,Y)$ is an unknown joint probability over the random variables $X$ and $Y$. Subsequently $F(.)$ is a function realizing a mapping $X \rightarrow Y$. As usual we want to learn an estimation $\hat{F}(.)$ of $F(.)$ [^2]. Furthermore, we need a specific loss function $L(.,.)$ or $L(\hat{F}(x),y)$ to evaluate the ranking results of $\hat{F}(.)$. One has to bear in mind, that the loss function is defined a little bit different than in other statistical learning tasks, as it intrinsically involves sorting. Loss based on NDCG can be defined as

$$
L(\hat{F}(x), y) = 1 - NDCG,
$$
Did you know that we are able to evaluate
with the empirical risk or objective learning function being

$$
\hat{R}(F) = \frac{1}{m} \sum_{i=1}^{m} L(\hat{F}(x_i),y_i).
$$

As in other learning tasks, learning corresponds to the minimization of the risk function, though minimization could prove difficult because the loss function is usually not continuous and additionally involves sorting.

## Conclusion

In this article we have introduced learning to rank for online retail, eCommerce, respectively for sales in general and introduced the balancing or optimization problem between sales and customer preferences in a simple form. Furthermore, we introduced learning to rank and emphazised the parameterization possibilities we use to address former optimization problem dynamically with the help of (deep) reinforcement learning. In the following article we will present two state of the art learning to rank algorithms and how our solution will improve online retail revenue by optimizing former balancing problem dynamically on an existing visitor stream.


Kind regards,

Henrik Hain


[^1]: Simple formulation without discriminatory parameters.
[^2]: $F(.)$ typically denotes the global ranking function and $f(.)$ denotes the local ranking function.
