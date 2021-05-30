---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Stochastic regression imputation"
subtitle: ""
summary: "Stochastic regression imputation can be considered a refinement of regression imputation because it addresses the correlation bias by adding noise from the regression residuals to the missing value estimations. This post discusses the advantages of stochastic regression imputation with examples in Python."
authors: [mrhenhan]
tags: [missing data, missing value imputation, statistics, unsupervised learning, supervised learning, Python]
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

This blog post attempts to introduce the conceptual advantages of stochastic
regression imputation, as mentioned in the last post [The persistent problem of missing data]({{< relref "/post/the-persistent-problem-of-missing-data" >}}), using practical examples in Python 

Stochastic regression imputation is based on regression imputation in which
imputations are generated based on a model estimated on the observed values.
In regression imputation, the imputed are the most likely values under the
particular model. This often amplifies the relations in the data in an
undesirable way.

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

## Regression imputation vs. stochastic regression imputation

The following example makes use of the classic Box & Jenkins airline data
containing the monthly totals of international airline passengers, 1949 to 1960.
First, the airline data set is loaded into a pandas data frame, and the month
column is dropped, as it is not necessary for the example.

{{< icon name="python" pack="fab" >}} Python 

```python
# Load airline passenger data set
df = pd.read_csv("./AirPassengers.csv", sep=",", encoding="utf-8")
df = df.drop("Month", axis=1)
```

Next, a $0-1$ response indicator matrix (vector) $R$ is generated,
denoting the positions of observed and missing values. The reponse idicator
matrix masks $20\%$ of the dataset as missing.

```python
# Choose 20% of the index positions at random
idx_miss = np.random.randint(0, high=len(df), size=int(len(df)*.2))

# Create response indicator matrix
R = ~df.index.isin(idx_miss)

# create observed and missing data
observed = df[R]
missing = df[~R]
```

The following plot serves to get a feeling for the distribution of the missing
values, which are missing here completely at random (MCAR).  The plot uses the
Abayomi convention for colors (Abayomi et al., 2008), where blue stands for
observed values, red for (real) missing values and black for the complete
dataset with synthetical imputed values.

```python
plt.figure(figsize=(16,6))
sns.scatterplot(x=observed.index, y=observed["#Passengers"], color="blue")
sns.scatterplot(x=missing.index, y=missing["#Passengers"], color="red")
```

{{< figure src="./observedVSmissing.png" title="Observed versus missing monthly airline passenger values.">}}

The missing values seem to be evenly distributed across the data range.

For regression imputation, the scikit-learn LinearRegression model is used.
Finally the missing values are estimated using the model and the index positions
from the missing data.

```python
lrg = LinearRegression()
lrg.fit(observed.index.values.reshape(-1, 1), observed["#Passengers"].values)
imputations = lrg.predict(missing.index.values.reshape(-1, 1))
```

{{< figure src="./regimp.png" title="Regression imputation with observed values in blue, real missing values in red and imputed values in black.">}}

One can see that for the missing values simply the values of the regression
model at the corresponding X position are used. This underestimates the variance
of the data set and does not take into account the uncertainty about missing
values. For stochastic regression imputation a random error or noise term
$\epsilon$ is added to the regression imputation resulat at the position of the
missing value. The error term is drawn from the distribution of the residuals,
here assuming the residuals are normally distributed, thus 
$\dot{\epsilon} ~\sim N(0, \hat{\sigma}^2)$, with $\hat{\sigma}^2$ being
estimated from the residuals of the formerly fitted linear regression model.

```python
# Calculate residuals, variance and noise vector from residual distr.
residuals = observed["#Passengers"].values - rgr.predict(observed.index.values.reshape(-1, 1))
variance = residuals.var()
rnoise = np.random.normal(0,np.sqrt(variance), len(imputations))

# Add noise vector to predicton vector from regression model.
simputations = imputations + rnoise
```

{{< figure src="./stochregimp.png" title="Stochastic regression imputation with observed values in blue, real missing values in red and imputed values in black.">}}

Finally, the following image shows the original data plotted against both
regression and stochastic regression imputation with the real missing values
marked as red dots

{{< figure src="./comimp.png" title="Comparison of the original data to the imputations with regression and stochastic regression imputation. Reals missing values in red.">}}

and the box-and-whiskers plot for all three completed data sets.

{{< figure src="./compbox.png" title="Comparison of the original data to the imputations with regression and stochastic regression imputation. Reals missing values in red.">}}

## Discussion

## Conclusion

## References

- [Flexible imputation of missing data 2018](https://stefvanbuuren.name/fimd/)
