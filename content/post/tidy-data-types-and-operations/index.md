---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Tidy Data, Tidy Types, and Tidy Operations"
subtitle: "How to organize your data and choose allowed operations to support data science, machine learning, and data stewardship."
summary: "The notion of tidy data is a concept known from R and used in many available libraries and frameworks today with great success. Tidy data together with proper data types and semantically allowed operations simplifies data science, machine learning and data stewardship by a large margin. In this article we will highlight the core properties of \"Tidy Data, Tidy Types, and Tidy Operations\" with the help of a concise example and how those properties can be successively achieved and maintained."
authors: ["mrhenhan"]
tags: ["tidy data", "statistical data types", "tidy types", "tidy operations","data wrangling" , "data stewardship", "R", "Python"]
categories: ["data science", "machine learning", "data analytics", "statistical software"]
date: 2020-06-15T13:10:29+02:00
lastmod: 2020-06-15T13:10:29+02:00
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
Hadley Wickham adapted a quote from Leo Tolstoy, when he proclaimed "Like families, tidy datasets are all alike but every messy dataset is messy in its own way"-[[Wickham2014]](https://scholar.google.com/scholar?oi=bibs&cluster=7796623832662932979&btnI=1&hl=en) and he stated that around 80% of all work is spent data wrangling. More recent studies still indicate that 50% to 70% of work time is spent during data wrangling, which essentially means that only 30% to 50% of the work time is left for generating insights and value. Untidy or messy data thus obstruct business and research ventures and represents a successively growing cost factor in the process of data stewardship with negative side effects, not only for your current business and value stream but also during opportunity exploration and research.

The second problem area is the use of correct data types and permitted mathematical operations for those types to obtain semantically valid and stable statements from your analysis. Thus the use of wrong data types and operations subtly endangers the validity of an analysis or the results of a machine learning model.

With the aim of mitigating or completely eliminating the above-mentioned risks, we will describe how to obtain "Tidy Data, Tidy Types, and Tidy Operations" with the aim of reducing data stewardship costs and increasing flexibility for actual data science and machine learning.

## Tidy Data

The foundtions of tidy data are closely related to the principles of [Codd's relational algebra](https://dl.acm.org/doi/pdf/10.1145/362384.362685), especially w.r.t Codd's 3rd normal form and defines a standard "way of mapping the meaning of a dataset to its structure" - [[Wickham2014]](https://scholar.google.com/scholar?oi=bibs&cluster=7796623832662932979&btnI=1&hl=en). Thus tidy data, e.g. data prepared for analysis, is defined as,

1. Each variable forms a column.
2. Each observation forms a row.
3. Each type of observational unit forms a table.

while _data persisted in any other way is considered messy data!_

Providing tidy data facilitates data scientist and machine learning engineers to easily extract necessary variables for their cause, because it provides a standard way of structuring data.

Ordering variables is not strictly necessary, but a well thought of order makes it easier to get a first overview of the data to be analyzed, thus it is recommended to put fixed variables first, followed by the measured variables,

1. Fixed variables.
2. Measured variables.
3. Order by fixed, then measured variables.

with the five most common violations being

- Column headers are values, not variable names.
- Multiple variables are stored in one column.
- Variables are stored in both rows and columns.
- Multiple types of observational units are stored in the same table.
- A single observational unit is stored in multiple tables.

according to [Wickham](https://scholar.google.com/scholar?oi=bibs&cluster=7796623832662932979&btnI=1&hl=en)

If a relational database adheres to Codd's 3rd normal form, it should be easy to extract arbitrary observational units for analysis and to persist results. Furthermore, operating on tidy data is a fundamental step for adopting data stewardship practices.

Before presenting specific violation examples and how to tidy them using Python and alternatively R, we introduce typical data manipulation methods used for the purpose of data manipulation.

### Data Manipulation

### Examples

#### Variables as Column Headers

#### Columns with Multiple Variables

#### Variables in Rows and Columns

#### Multiple Types in one Table

#### One Type in multiple tables

## Tidy Types

## Tidy Operations

## Conclusion

## References
