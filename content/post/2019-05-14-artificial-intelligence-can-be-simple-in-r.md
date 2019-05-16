---
title: Artificial intelligence is straightforward in R!
author: ArgosMs
date: '2019-05-14'
slug: artificial-intelligence-is-straightforward-in-r
categories:
  - caret
tags:
  - Artificial Intelligence
  - Machine Learning
description: ''
topics: []
---

***

Few topics raise so much interested nowadays than [artificial intelligence (AI)](https://www.techopedia.com/definition/190/artificial-intelligence-ai). One of the applications of AI is [machine learning (ML)](https://singularityhub.com/2017/08/28/machine-learning-is-everywhere-but-what-is-it-exactly/).

So let's take a look at how ML works in `R`.

Big thanks to [Rebecca Barter](http://www.rebeccabarter.com/blog/2017-11-17-caret_tutorial/) for this great tutorial.

***

```{r}
library(caret)
library(ranger)
library(tidyverse)
library(e1071)
```

***

Get `abalone.data` [here](http://archive.ics.uci.edu/ml/machine-learning-databases/abalone/)

***

Save the downloaded file `abalone.data` in your working directory. If you do not know what abalone is, you might to try it. It is [tasty](https://www.youtube.com/watch?v=EB8ecdalbDE)!

***

```{r}
getwd()
abalone_data <- read.table("C:/Users/NewUser/Documents/Blogdown/abalone.data", sep = ",")
```

***

Load in column names.

***

```{r}
colnames(abalone_data) <- c("sex", "length", "diameter", "height", 
                            "whole.weight", "shucked.weight", 
                            "viscera.weight", "shell.weight", "age")
```
                            
***

Select abalones that are older than 10. Remove "age" variable.

***

```{r}
abalone_data <- abalone_data %>%
  mutate(old = age > 10) %>%
  select(-age)
```

***

Create traininig and testing datasets

***

```{r}
set.seed(23489)
train_index <- sample(1:nrow(abalone_data), 0.9 * nrow(abalone_data))
abalone_train <- abalone_data[train_index, ]
abalone_test <- abalone_data[-train_index, ]
```

***

Remove original dataset.

***

```{r}
rm(abalone_data)
```

***

Vew the first rows of training dataset.

***

```{r}
head(abalone_train)
```

***

Check the dimensions of the training dataset.

***

```{r}
dim(abalone_train)
```

***

Fit [random forest model](https://www.coursera.org/lecture/machine-learning-data-analysis/what-is-a-random-forest-and-how-is-it-grown-FL9wx).

***

```{r}
rf_fit <- train(as.factor(old) ~ ., 
                data = abalone_train, 
                method = "ranger")
rf_fit
```

***

Now predict outcome in testing dataset.

***

```{r}
abalone_rf_pred <- predict(rf_fit, abalone_test)
```

***

Compare predicted and true outcome.

***

```{r}
confusionMatrix(abalone_rf_pred, as.factor(abalone_test$old))
```

***

We could have stoppped here. But let's do a bit more!

***

Center, scale and perform a YeoJohnson transformation. Identify and remove variables. with near zero variance. Perform pca.

***

```{r}
abalone_no_nzv_pca <- preProcess(select(abalone_train, - old), 
                                 method = c("center", "scale", "YeoJohnson", "nzv", "pca"))
abalone_no_nzv_pca
```

***

Identify variables.

***

```{r}
abalone_no_nzv_pca$method
```

***

Identify principal components.

***

```{r}
abalone_no_nzv_pca$rotation
```

***

Identify the indices of 10 80% subsamples of the iris data

***

```{r}
train_index <- createDataPartition(iris$Species,
                                   p = 0.8,
                                   list = FALSE,
                                   times = 10)
head(train_index)
```

***

Add a madeup grouping variable that groupes each subsequent 5 abalone together. Filter to the first 50 abalone for simplicity.

***

```{r}
abalone_grouped <- cbind(abalone_train[1:50, ], group = rep(1:10, each = 5))
head(abalone_grouped, 10)
```

***

Perform grouped K means.

***

```{r}
group_folds <- groupKFold(abalone_grouped$group, k = 10)
group_folds
```

***

Let's tune parameters. Create testing and training sets.

***

```{r}
set.seed(998)
in_training <- createDataPartition(abalone_train$old, p = .75, list = FALSE)
training <- abalone_train[ in_training,]
testing  <- abalone_train[-in_training,]
```

***

Specifiy resampling method.

***

```{r}
fit_control <- trainControl(## 10-fold CV
  method = "cv",
  number = 10)
```
  
***

Run random forest model.

***

```{r}
set.seed(825)
rf_fit <- train(as.factor(old) ~ ., 
                data = abalone_train, 
                method = "ranger",
                trControl = fit_control)
rf_fit
```

***

Use now grouped folds. 

***

```{r}
group_fit_control <- trainControl(## use grouped CV folds
  index = group_folds,
  method = "cv")

set.seed(825)
rf_fit <- train(as.factor(old) ~ ., 
                data = select(abalone_grouped, - group), 
                method = "ranger",
                trControl = group_fit_control)
rf_fit
```

***

Define grid parameter options.

***

```{r}
rf_grid <- expand.grid(mtry = c(2, 3, 4, 5),
                       splitrule = c("gini", "extratrees"),
                       min.node.size = c(1, 3, 5))
rf_grid
```

***

Re-fit model with the parameter grid.

***

```{r}
rf_fit <- train(as.factor(old) ~ ., 
                data = select(abalone_grouped, - group), 
                method = "ranger",
                trControl = group_fit_control,
                # provide a grid of parameters
                tuneGrid = rf_grid)
rf_fit
```

***

Now you have a feeling of how ML and AI are used in `R`. Get new datasets and start practicing.

***

**I want more!**

Check out these exercises: 
[1](https://towardsdatascience.com/the-random-forest-algorithm-d457d499ffcd)
[2](https://www.r-bloggers.com/a-quick-introduction-to-machine-learning-in-r-with-caret/)
[3](https://www.datacamp.com/community/blog/3-reasons-to-learn-caret)
[4](https://www.machinelearningplus.com/machine-learning/caret-package/)
[5](https://rpubs.com/williamsurles/310197)
