---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Stochastic regression imputation"
subtitle: ""
summary: "Stochastic regression imputation can be considered a refinement of regression imputation because it addresses the correlation bias by adding noise from the regression residuals to the missing value estimations. This post discusses the advantages of stochastic regression imputation with examples in Python."
authors: [mrhenhan]
tags: [missing data, missing value imputation, statistics, unsupervised learning, supervised learning, Python]
categories: [machine learning]
date: 2021-05-07T18:00:05
lastmod: 2021-06-03T15:43:05
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

{{% alert note %}}
The [JupyterLab source code](https://github.com/mrhenhan/stochastic-regression-imputation) for this article is now available via [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/mrhenhan/stochastic-regression-imputation/master?filepath=notebooks%2Fstochastic-regression-imputation.ipynb)
{{% /alert %}}

# An introduction to (stochastic) regression imputation

This blog post attempts to introduce the conceptual advantages of stochastic
regression imputation, as mentioned in the last post 
[The persistent problem of missing data]({{< relref "/post/the-persistent-problem-of-missing-data" >}}),
with the help of practical examples written in Python. 

Deterministic regression imputation is an imputation method based on 
(linear) regression. To estimate missing values a regression model is fitted
to the observed part of the data and then the estimates of the model are used as
imputations at the points where missing values occur. Deterministic
regression imputation has multiple drawbacks. On the one hand, it strengthens
the relationships in the data, and on the other hand, it does not take into
account the uncertainty about missing data. Another disadvantage of 
deterministic regression imputation, as well as related more recent methods
from the area of machine learning, is that they may lead to imputations
which seem to be perfect for the model generated from the observed data.

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
denoting the positions of observed and missing values. This response indicator
matrix will mask $25$% of the dataset as missing.

{{< icon name="python" pack="fab" >}} Python 

```python
# Choose 25% of the index positions at random
idx_miss = np.random.randint(0, high=len(df), size=int(len(df)*.25))

# Create response indicator matrix
R = ~df.index.isin(idx_miss)

# create observed and missing data
observed = df[R]
missing = df[~R]
```

The following plot serves to get a feeling for the distribution of the missing
values, which are missing here completely at random (MCAR).  The plot uses the
Abayomi convention for colors (Abayomi et al., 2008), where blue is for
observed values, red for (real) missing values and black for the complete
dataset with imputed values.

{{< icon name="python" pack="fab" >}} Python 

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

{{< icon name="python" pack="fab" >}} Python 

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
$\epsilon$ is added to the regression imputation result at the position of the
missing value. The error term is drawn from the distribution of the residuals,
here assuming the residuals are normally distributed, thus 
$\dot{\epsilon} ~\sim N(0, \hat{\sigma}^2)$, with $\hat{\sigma}^2$ being
estimated from the residuals of the formerly fitted linear regression model.

{{< icon name="python" pack="fab" >}} Python 

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
marked as red dots.

{{< figure src="./compimp.png" title="Comparison of the original data to the imputations with regression and stochastic regression imputation. Reals missing values in red.">}}

The box-and-whiskers plot for all three completed data sets shows, that the

{{< figure src="./compbox.png" title="Box-and-whiskers plot comparison of the original data distribution with the deterministic and stochastic imputation distributions.">}}

distributions between the original data and both imputation methods do not
deviate much from each other.

## Discussion

How is it that stochastic regression imputation often gives more realistic
results, although the best results are contaminated by noise or, at least
apparently, outliers are imputed?

In fact, several problem dimensions exist in imputing missing values. First,
it is not possible to test missing value imputation methods on real data with
missing values as, in fact, we don't know the missing values. Thus, a missing
value imputation model can only be trained on simulation data, where missing
values are simulated according the missingness distributions in the real data.
Secondly, and more interesting in the case of deterministic and stochastic
regression imputation, a missingness model can only be trained on observed
values. In case of deterministic regression imputation, missing values are
imputed directly from the regression line, reducing the variance of the
dataset in relation to the missing rate not accounting for the uncertainty
of missing values nor unseen data, which could result in low
generalizability of the model. 

In contrast, stochastic regression imputation accounts for the uncertainty
w.r.t. missing values by adding a random error term from the regression model
residual distribution and thus does not reduce variance or covariance 
significantly. Imputations are more realistic and  more generalizable, depending
on the quality of the training data set the regression model was trained under.

Finally, it must be noted that, at least in the present simulated case, the
mean absolute percentage (MAPE) and root mean squared error are higher
for stochastic regression imputation than for deterministic regression
imputation.

$$
MAPE(Y,\hat{Y}) = 100/N \sum_i^N{ \lvert \frac{Y_i - \hat{Y}_i}{Y_i} \rvert}
$$

$$
RMSE(Y,\hat{Y}) = \sqrt{\frac{\sum_i^N{(y_i - \hat{y_i})^2}}{N}}
$$

The following table contains the MAPE and RMSE results for both imputation
methods and $25$ % of missing values following the MCAR mechanism.

| Imputation method                   | RMSE    | MAPE     |
| ----------------------------------- | ------- | -------- |
| Deterministic regression imputation | $56.18$ | $12.6$ % |
| Stochastic regression imputation    | $75.69$ | $21.4$ % |

## Conclusion

This blog post introduced stochastic regression imputation in comparison to
deterministic regression imputation together with some code examples
in Python. Although this was not a formal investigation, it showed the
conceptual benefits of stochastic regression imputation through spoiling the
best estimation results by adding noise from the residual distribution.
Furthermore, the dangers of blindly trusting the often all too perfect
imputations of regression imputation and its modern incarnations from the area 
of machine learning were introduced.

In the next blog post multiple imputation will be introduced.

Best regards,

Henrik

## References

- [Flexible imputation of missing data 2018](https://stefvanbuuren.name/fimd/)
