---
title: RMarkdown is easy to learn!
author: ArgosMs
date: '2019-04-29'
slug: rmarkdown-is-easy-to-learn
categories: []
tags:
  - Data Communication
  - Reproducibility
  - Collaboration
description: ''
topics: []
---

***

*Here you find what is* [RMarkdown](https://vimeo.com/178485416).

*And in this* [thread](https://community.rstudio.com/t/convince-me-to-start-using-r-markdown/1636/20) *some of the benefits of using RMarkdown.*

*So let's start playing with it.*

*Open RStudio. Go to* `File > New File > R Markdown > OK`

*Do you see this?*

***
***
---
title: "Untitled"
author: "ArgosMs"
date: "April 23, 2019"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## R Markdown

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:

```{r cars}
summary(cars)
```

## Including Plots

You can also embed plots, for example:

```{r pressure, echo=FALSE}
plot(pressure)
```

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.

***

*Great!*

*So now replace...*

***

i) `"Untitled"` *with* `"My First RMarkdown: Uhuu!"`

ii) `"ArgosMs"` *with* `"This is ME!"`
              
iii) `"April 23, 2019"` *with* `"My bday: thanks stork!"`

***
             
*Click on* `Knit`

*You should see your changes below...*

***

`title: "My First RMarkdown:Uhuu!"`

`author: "This is ME!"`

`date: "My bday: thanks stork!"`

`output: html_document`

***

*Let's keep jamming!*

*You see this text...*

***

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:

***

*Replace...* 

`<http://rmarkdown.rstudio.com>` *with* `<https://www.rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf>`

 *and* 
 
 `**Knit**` *with* `**Knit cool**`
 
 ***
 
 *Click on* `Knit`
 
 *This is how your new text looks like, right?*
 
 ***
 
This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <https://www.rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf>.

When you click the **Knit cool** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:

***

*How about playing with a new dataset?*

*Replace...*

***

`{r cars}` *with* `{r AirPassengers}`

`summary(cars)` *with* `summary(AirPassengers)`

***

*Click on* `Knit`.

*Yeah! You are now travelling much faster!*

***

```{r}
summary(AirPassengers)
```

***

*Let's create a graph now to see how fast you have been travelling!*

*Replace...*

***

`{r pressure, echo=FALSE}` *with* `{r}`

*and*

`plot(pressure)` *with* `plot(AirPassengers)`

***

*Click on* `Knit`.

*We can see that air travelling has steadily increased overtime. Just like learning R!*

***

```{r}
plot(AirPassengers)
```

***

*So your final edited document should look like this now...*

***

***
---
title: "My First RMarkdown"
author: "This is ME!"
date: "My bday: thanks stork!"
output: html_document
---

## R Markdown

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <https://www.rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf>.

When you click the **Knit cool** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:

```{r AirPassengers}
summary(AirPassengers)
```

## Including Plots

You can also embed plots, for example:

```{r}
plot(AirPassengers)
```

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.

***

*Awesome! You are already using RMarkdown!*

*Now let's have you fly solo.*

*The first challenge is editing an* `.Rmd` *document aka as RMarkdown documents*

*For that you will need to follow this* [reference guide](https://www.rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf).

*You may use the standard text above or any other document. One-page* `.Rmd` *file is more than long enough! :)*

***

### First Challenge

*You need to make at least* **ten** *changes (RMarkdown call these editing options* **syntaxes***) in an* `.Rmd` *document. My suggestion is that you try using as many different syntaxes as possible.*

*Save this document as* `Week_1_Challenge_1_MyLastName_MyFirstName.Rmd` *and share it with the class on Blackboard. (We all want to be inspired by your hard work!)*

***

### Second Challenge

*You must change the* **size** *and* **color** *of the* **title** *and* **author** *in the* `YAML` *section of my* `.Rmd` *document. How can I do that?* [Hint](https://stackoverflow.com/questions/48958406/change-font-for-author-rmarkdown).

*This is the* [list](https://www.quackit.com/css/css_color_codes.cfm) *for* [CSS](https://www.youtube.com/watch?v=4BEyFVufmM8) colors.

*Whoever posts this answer first as an* `.Rmd` *file on blackboard gets a straight* `A`. *More higher marks are up for grabs for those further experimenting with* `CSS` *commands and sharing their* `.Rmd` *documents and comments with the class.*

***

### Third Challenge

*Create a* [slideshow](https://rmarkdown.rstudio.com/lesson-11.html) *with RMarkdown. Add bullet points and at least one image. This image should have been saved in your computer. To help `R`  find this image in your computer you will need to learn how to use* `getwd()` *and *`setwd()`*). Hints for this exercise* [here](https://rmarkdown.rstudio.com/lesson-11.html), [here](https://stackoverflow.com/questions/31886610/add-local-image-file-in-r-presentation) and [here](http://rfunction.com/archives/1001). *Feel free to share your slideshow with the class if you feel like it*.

***

#### I want more!

***

I want to practice more RMarkdown exercises. Glad to hear that. [There](https://www.r-bloggers.com/r-markdown-exercises-part-1/) you go!

I want to know more about RMarkdown. Perfect! Read section `V` of [R for Data Science](https://r4ds.had.co.nz/communicate-intro.html).

I can't have enough of RMarkdown. I am addicted. Hit me with more. Ok, ok, buddy. Here we go: enjoy! :) 
[1](https://rmarkdown.rstudio.com/articles_intro.html) 
[2](http://www.jacolienvanrij.com/Tutorials/tutorialMarkdown.html) 
[3](https://cfss.uchicago.edu/notes/r-markdown/)
[4](https://nuitrcs.github.io/rmarkdown_workshop/exercises.html)

***
