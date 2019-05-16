---
title: "Inferential statistics is uncomplicated with R!"
author: "ArgosMs"
date: '2019-05-09'
description: ''
slug: inferential-statistics-is-uncomplicated-with-r
tags: 
- Inferential Statistics
- Decision Making
categories:
- statsr
- ggplot2
- dplyr
- kableExtra
- foreign
topics: []
---

***

It is very important to understand how large groups of people behave. However, it is not always possible to interview, collect data, from every single person of a population to understand collective behavior. Thus we resort to [inferential statistics](https://www.thoughtco.com/differences-in-descriptive-and-inferential-statistics-3126224).

Inferential statistics allows us to infer, reach some conclusions, by observing the behavior of part, a [sample](https://www.simplilearn.com/statistical-concepts-and-their-application-in-business-tutorial) or subset, of a selected population. 

The outcomes of inferential statistics are stated in terms of `probability` language since they are generalizations deriving from observations made from a sample of the target population. 

Nonetheless, these statements can be quite informative depending on the [`robustness`](https://www.thoughtco.com/what-is-robustness-in-statistics-3126323) of the statistical methods leading to these results.

`R` is the perfect language for statistical inferences. It is powerful, fast, and easy!

There are many good tutorials out there. I have selected this [one](https://sajalsharma.com/portfolio/gss_inferential), though, because it is brief and provide an interesting case study. The only problem is that it has some bugs in its code. I have fixed them and simplified the workflow for this lab.

Hope you all enjoy it!

***

```{r}
install.packages("ggplot2")
install.packages("dplyr")
install.packages("statsr")
install.packages("kableExtra")
install.packages("foreign")
library(ggplot2)
library(dplyr)
library(statsr)
library(knitr)
library(scales)
library(gridExtra)
library(kableExtra)
library(foreign)
```

***

First, get the data on this [website](http://gss.norc.org/get-the-data/stata). Select this dataset: `GSS 1972-2018 Cross-Sectional Cumulative Data (Release 1, March 18, 2019)`

***

Then use [`getwd()`](http://rfunction.com/archives/1001) to find your work directory. Transfer or save the downloaded dataset to where `getwd()` is pointing to. Alternatively, use `setwd()`.

***

```{r}
getwd()
dat <- read.dta("C:/Users/NewUser/Documents/Blogdown/GSS7218_R1.DTA")
```

***

Explore the dataset a bit.

***

```{r}
ls(dat)
dat[ ,"sex"]
dat[ ,"sexeduc"]
dat["2012", ]
```

***

Filter out data from the year `2012` and select interested `variables`.

***

```{r}
dat <- filter(dat, year == 2012) %>% select(sex,sexeduc)
str(dat)
```

***

Plot the dataset. What do you find here?

***

```{r}
ggplot(dat, aes(x=sexeduc)) + geom_bar() + ggtitle('Favourability to Sex Education in Public Schools') + xlab('Favour or Oppose Sex Education') + theme_bw()
```

***

Remove `NA` values from dataset. What conclusions can you make now?

***

```{r}
dat <- na.omit(dat)
table(dat)
```

***

Remove `Depends` category. What statements can you make now?

***

```{r}
dat <- droplevels(dat)
mosaicplot(prop.table(table(dat),1), main = 'Sex vs Favourability of Sex Education')
```

***

Let's make `inferences` based on this sample now. What are the [`null`](https://keydifferences.com/difference-between-null-and-alternative-hypothesis.html) and `alternative` hypotheses in this study? 

***

Lets take a look at the difference in proportions of `men` and `women` who oppose `sex education` by calculating [`p-value`](https://www.dummies.com/education/math/statistics/what-a-p-value-tells-you-about-statistical-data/) first. Do you `accept` or `reject` the `null` hypothesis?

***

```{r}
inference(x= sex,y = sexeduc, data = dat, statistic = "proportion", type = "ht", null = 0 , method = "theoretical", alternative="greater", success = "oppose")
```

***

Let's now take a look at the [`confidence interval`](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3271529/) of this sample. Why does this study need `confidence interval`? What does the `confidence interval` reveal about this sample?

***

```{r}
inference(x= sex,y = sexeduc, data = dat, statistic = "proportion", type = "ci", method = "theoretical", success = "oppose")
```

***

**I want more!**

Check these tutorials on inferential statistics: [0](https://stat.utexas.edu/videos/r/statistical-inference/video/19)
[1](http://personality-project.org/r/basics.t.html)
[2](https://learningstatisticswithr.com/book/epilogue.html)
[3](https://rpubs.com/alejandrobalderas/375191)
[4](http://www.rpubs.com/sujith/IS)
[5](https://www.r-bloggers.com/data-science-for-doctors-inferential-statistics-exercises-part-3/)
[6](https://bookdown.org/home/tags/statistics/)
