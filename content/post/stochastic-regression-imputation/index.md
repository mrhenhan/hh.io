---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Stochastic regression imputation"
subtitle: ""
summary: "Stochastic regression imputation can be considered a refinement of regression imputation because it addresses the correlation bias by adding noise from the regression residuals to the missing value estimations. This post discusses the advantages of stochastic regression imputation with examples in Python and R."
authors: [mrhenhan]
tags: [missing data, missing value imputation, statistics, unsupervised learning, supervised learning, Python, R]
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

# An introduction to (stochastic) regression imputation

This blog post attempts to introduce the, mentioned in the last post [The persistent problem of missing data]({{< relref "/post/the-persistent-problem-of-missing-data" >}}), conceptual advantages of stochastic regression imputation using practical examples in Python and R.

Stochastic regression imputation is based on regression imputation in which imputations are generated based on a model estimated on the observed values. In regression imputation, the imputed are the most likely values under the particular model. This often amplifies the relations in the data in an undesirable way.

The drawbacks of regression imputation, as well as related more recent methods
from the area of machine learning, are dangerous as they may lead to imputations
which seem to be perfect for the model generated from the observed data.
However, these methods unfortunately amplify relationships in the data,
artificially increasing correlations and underestimating variation.

The basic idea behind stochastic regression imputation is to account for the
inherent uncertainty about missing values by adding noise. This is a very
powerful concept, which also builds the basis of many modern missing value
imputation approaches. In stochastic regression imputation, the noise is
simulated by drawing random values from the residuals of the estimated
regression model for each missing value and subsequently add them to the 
predicted missing value.



## Conclusion


## References

- [Flexible imputation of missing data 2018](https://stefvanbuuren.name/fimd/)
