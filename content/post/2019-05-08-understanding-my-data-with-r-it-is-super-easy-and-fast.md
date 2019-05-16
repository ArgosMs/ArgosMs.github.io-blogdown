---
title: 'Understanding my data with R: It is super easy and fast!'
author: ArgosMs
date: '2019-05-08'
slug: understanding-my-data-with-r-it-is-super-easy-and-fast
categories:
  - e1701
  - mlbench
  - dplyr
  - tidyr
tags:
  - Descriptive Statistics
  - Data Analysis
  - Data Manipulation
description: ''
topics: []
---

***

[Descriptive statistics](https://statistics.laerd.com/statistical-guides/descriptive-inferential-statistics.php) is the very first step of [data analysis](https://ori.hhs.gov/education/products/n_illinois_u/datamanagement/datopic.html). Descriptive statstics thus help you understand your data. And the more you understand it, the better the results will be. This is because by looking closely at your data, you will buid some intuition that eventually point you to some tasks such as [data cleansing](https://www.edq.com/glossary/data-cleansing/) and [data transformation](https://www.alooma.com/blog/what-is-data-transformation).

Therefore descriptive statistics is essential in data science and thanks to `R` it is easy and fast.

There are many ways and tutorials online introducing descriptive statistics in `R`. I came across this excellent [one](https://machinelearningmastery.com/descriptive-statistics-examples-with-r/) for beginners. I will simplify this tutorial even further so that you can start playing with your data as soon as possible in `R`.

Let's go!

***

```{r}
install.packages("mlbench")
install.packages("e1071")
library(mlbench)
library(e1071)
```

***

Select data `PimaIndiansDiabetes`. Take a look at the first twenty lines of this data by using `head()`. Check the number of `rows` and `columns` of this dataset by using `dim()`.

***

```{r}
data(PimaIndiansDiabetes)
head(PimaIndiansDiabetes, n=20)
dim(PimaIndiansDiabetes)
```

***

Check the class [attribute](http://adv-r.had.co.nz/Data-structures.html#attributes) of the dataset using [`sapply()`](https://www.r-bloggers.com/using-apply-sapply-lapply-in-r/).

***

```{r}
data(BostonHousing)
sapply(BostonHousing, class)
```

***

Check the `frequency` of `values` in the column `diabetes` of the dataset by using [`cbind()`](http://www.datasciencemadesimple.com/cbind-in-r/).

***

```{r}
data(PimaIndiansDiabetes)
y <- PimaIndiansDiabetes$diabetes
cbind(freq=table(y), percentage=prop.table(table(y))*100)
```

***

Summarize each attribute of the dataset.

***

```{r}
data(iris)
summary(iris)
```

***

Check the [`standard deviation`](https://www.dummies.com/education/math/statistics/why-standard-deviation-is-an-important-statistic/) of the dataset.

***

```{r}
data(PimaIndiansDiabetes)
sapply(PimaIndiansDiabetes[,1:8], sd)
```

***

Calculate the [skewness](http://www.fusioninvesting.com/2010/09/what-is-skew-and-why-is-it-important/) of your dataset.

***
```{r}
data(PimaIndiansDiabetes)
skew <- apply(PimaIndiansDiabetes[,1:8], 2, skewness)
print(skew)
```

***

It is easier to find if your data is `skewed` with a `graph` showing the `frequency` of `values`. For instance, let's `plot()` the values of column `diabetes` from the dataset.

***

```{r}
y <- PimaIndiansDiabetes$diabetes
plot(y)
```

***

Check how the attributes of the dataset are [correlated](https://statisticsbyjim.com/basics/correlations/).

***

```{r}
data(PimaIndiansDiabetes)
correlations <- cor(PimaIndiansDiabetes[,1:8])
print(correlations)
```

***

**I want more!**. 

Check out these ones: 
[0](https://rstudio-pubs-static.s3.amazonaws.com/291237_ccac14a9e129479c8d07e1dcfac5a697.html)
[1](https://www.statmethods.net/stats/descriptives.html)
[2](http://www.sthda.com/english/wiki/descriptive-statistics-and-graphics)
[3](https://cran.r-project.org/doc/contrib/Verzani-SimpleR.pdf)
[4](http://www-1.ms.ut.ee/BDA/BDA4.pdf)

***

`dplyr` and `tidyr` are two of the most powerful packages for [data manipulation](https://support.rstudio.com/hc/en-us/articles/201057987-Quick-list-of-useful-R-packages).

You *must* get familar with both packages if you will be manipulating datasets. This [tutorial](https://datacarpentry.org/r-socialsci/03-dplyr-tidyr/index.html) featuring both `packages` is the best one I could find online. If you carefully go through this excellent tutorial, the leaning curve to pick up this essential skill will not be too steep. Take your time and enjoy yourself!

If you need more tutorials on `dplyr`. Check these ones:
[0](https://cran.r-project.org/web/packages/dplyr/vignettes/dplyr.html)
[1](https://www.r-bloggers.com/express-intro-to-dplyr/)
[2](https://rpubs.com/justmarkham/dplyr-tutorial)
[3](https://dplyr.tidyverse.org/)
[4](http://stat545.com/block009_dplyr-intro.html)
[5](https://genomicsclass.github.io/book/pages/dplyr_tutorial.html)

And for `tidyr` check these ones:
[0](https://rpubs.com/bradleyboehmke/data_wrangling)
[1](https://garrettgman.github.io/tidying/)
[2](http://ohi-science.org/data-science-training/tidyr)
[3](http://tclavelle.github.io/dplyr-tidyr-tutorial/)
