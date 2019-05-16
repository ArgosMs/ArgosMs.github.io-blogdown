---
title: I want to quickly make beautiful graphs. You got it!
author: ArgosMs
date: '2019-05-06'
slug: i-want-to-quickly-make-beautiful-graphs-you-got-it
categories:
  - ggplot2
  - tidyverse
tags:
  - graphs
  - Data Visualization
description: ''
topics: []
---

***

Graphs allow us to share stories in a way that our audience can intuitively understand. `R` is perfect for it.

`ggplot2` is *the* package for building these graphs. There are also many, a lot of great, tutorials online, so there is no need to develop a new one. 

I have checked many of these tutorials and ended up selecting this [one](http://www.rebeccabarter.com/blog/2017-11-17-ggplot2_tutorial/) because it is well-written, concise, and provide an end-goal that makes the learning experience more meaningful.

So let's start *drawing* now!

***

Install `ggplot2` and make it available.

***

`install.packages("ggplot2")`
`library(ggplot2)`

***

Select `dataset`.

***

`gapminder <- read.csv("https://raw.githubusercontent.com/swcarpentry/r-novice-gapminder/gh-pages/_episodes_rmd/data/gapminder-FiveYearData.csv")`

***

Take a look at the first lines of the dataset.

***

`head(gapminder)`

***

Plot dataset and use `aes()` to indicate graph coordinates.

***

```{r}
ggplot(gapminder, aes(x = gdpPercap, y = lifeExp))
```

***

Use `geom_point()` to add points layer to the graph.

***

```{r}
ggplot(gapminder, aes(x = gdpPercap, y = lifeExp)) + geom_point()
```

***

Use `alpha` to change transparency of all points, 
`col` to change color points, and
`size` to change the size of these points.

***

```{r}
ggplot(gapminder, aes(x = gdpPercap, y = lifeExp)) +
  geom_point(alpha = 0.5, col = "cornflowerblue", size = 0.5)
```
  
***

Change color points by selecting the column `continent` in the dataset `gapminder`. Use `unique()`.

***

```{r}
unique(gapminder$continent)
```

***

Add `continent` in `aes()`. Notice that `color` was removed from `geom_point()` because the color of points are now different. They depend on `continents`.


***

```{r}
ggplot(gapminder, aes(x = gdpPercap, y = lifeExp, color = continent)) +
  geom_point(alpha = 0.5, size = 0.5)
```
  
***

Change the size of points based on the column `pop` (population) from `gapminder`.

***

```{r}
ggplot(gapminder, aes(x = gdpPercap, y = lifeExp, color = continent, size = pop)) +
  geom_point(alpha = 0.5)
```
  
***

Replace the `geom` `points` with `lines`. Use `geom_line()`.

***

```{r}
ggplot(gapminder, aes(x = year, y = lifeExp, group = country, color = continent)) +
  geom_line(alpha = 0.5)
```
  
***

Try `geom_boxplot()`.

***

```{r}
ggplot(gapminder, aes(x = continent, y = lifeExp, fill = continent)) +
  geom_boxplot()
```
  
***

How about `geom_histogram()`? (Change the values of `binwidth` to see what happens).

***

```{r}
ggplot(gapminder, aes(x = lifeExp)) + 
  geom_histogram(binwidth = 3)
```
  
***

Try `geom_smooth()`.

***

```{r}
ggplot(gapminder, aes(x = gdpPercap, y = lifeExp, size = pop)) +
  geom_point(aes(color = continent), alpha = 0.5) +
  geom_smooth(se = FALSE, method = "loess", color = "grey30")
```
  
***

`install.packages("dplyr")`
`library(dplyr)`

***

Let's use only data from the year `2007` in `gapminder`. 

***


`gapminder_2007 <- gapminder %>% filter(year == 2007)`. *ps.* [`%>%`](https://uc-r.github.io/pipe)?

***

Plot `gapminder_2007`.

***

```{r}
ggplot(gapminder_2007, aes(x = gdpPercap, y = lifeExp, color = continent, size = pop)) +
  geom_point(alpha = 0.5)
```

***

Display `gdpPercap` on a [logarithmic scale](https://www.forbes.com/sites/naomirobbins/2012/01/19/when-should-i-use-logarithmic-scales-in-my-charts-and-graphs/#358fab165e67). Use `scale()`. 

***

```{r}
ggplot(gapminder_2007, aes(x = gdpPercap, y = lifeExp, color = continent, size = pop)) +
  geom_point(alpha = 0.5) +
  scale_x_log10()
```
  
***

Manipulate `x-axis` with `breaks` and `limits`. These are called `arguments`.

***

```{r}
ggplot(gapminder_2007, aes(x = gdpPercap, y = lifeExp, color = continent, size = pop)) +
  geom_point(alpha = 0.5) +
  scale_x_log10(breaks = c(1, 10, 100, 1000, 10000),
                limits = c(1, 120000))
```
                
***

Add labels to the graph using `labs()`. 

***

```{r}
ggplot(gapminder_2007, aes(x = gdpPercap, y = lifeExp, color = continent, size = pop)) +
  geom_point(alpha = 0.5) +
  scale_x_log10() +
  labs(title = "GDP versus life expectancy in 2007",
       x = "GDP per capita (log scale)",
       y = "Life expectancy",
       size = "Population",
       color = "Continent")
```

***

Manipulate the scale of the `size` variable using `scale_size()`.

***

```{r}
ggplot(gapminder_2007, aes(x = gdpPercap, y = lifeExp, color = continent, size = pop)) +
  geom_point(alpha = 0.5) +
  scale_x_log10() +
  labs(title = "GDP versus life expectancy in 2007",
       x = "GDP per capita (log scale)",
       y = "Life expectancy",
       size = "Population (millions)",
       color = "Continent") +
  scale_size(range = c(0.1, 10),
             breaks = 1000000 * c(250, 500, 750, 1000, 1250),
             labels = c("250", "500", "750", "1000", "1250")) 
```
             
***

Make the plot for each continent. Use `facet_wrap()`.

***

```{r}
ggplot(gapminder_2007, aes(x = gdpPercap, y = lifeExp, color = continent, size = pop)) +
  geom_point(alpha = 0.5) +
  scale_x_log10() +
  labs(title = "GDP versus life expectancy in 2007",
       x = "GDP per capita (log scale)",
       y = "Life expectancy",
       size = "Popoulation (millions)",
       color = "Continent") +
  scale_size(range = c(0.1, 10),
             breaks = 1000000 * c(250, 500, 750, 1000, 1250),
             labels = c("250", "500", "750", "1000", "1250")) +
  facet_wrap(~continent)
```
  
***

Make the graph more beautiful by changing `themes`.

***

```{r}
ggplot(gapminder_2007, aes(x = gdpPercap, y = lifeExp, color = continent, size = pop)) +
  geom_point(alpha = 0.5) +
  scale_x_log10(breaks = c(1000, 10000),
                limits = c(200, 120000)) +
  labs(title = "GDP versus life expectancy in 2007",
       x = "GDP per capita (log scale)",
       y = "Life expectancy",
       size = "Population (millions)",
       color = "Continent") +
  scale_size(range = c(0.1, 10),
             breaks = 1000000 * c(250, 500, 750, 1000, 1250),
             labels = c("250", "500", "750", "1000", "1250")) +
  theme_classic(base_family = "Avenir")
```
  
***

Customize the graph further by using `theme()`.

***

```{r}
ggplot(gapminder_2007) +
  geom_point(aes(x = gdpPercap, y = lifeExp, color = continent, size = pop),
             alpha = 0.5) +
  geom_text(aes(x = gdpPercap, y = lifeExp + 3, label = country),
            color = "grey50",
            data = filter(gapminder_2007, pop > 1000000000 | country %in% c("Nigeria", "United States"))) +
  scale_x_log10(limits = c(200, 60000)) +
  labs(title = "GDP versus life expectancy in 2007",
       x = "GDP per capita (log scale)",
       y = "Life expectancy",
       size = "Popoulation",
       color = "Continent") +
  scale_size(range = c(0.1, 10),
             # remove size legend
             guide = "none") +
  theme_classic() +
  theme(legend.position = "top",
        axis.line = element_line(color = "grey85"),
        axis.ticks = element_line(color = "grey85"))
```
        
***

Save the graph using `ggsave()`.

***

```{r}
p <- ggplot(gapminder_2007) +
  geom_point(aes(x = gdpPercap, y = lifeExp, color = continent, size = pop),
             alpha = 0.5) +
  geom_text(aes(x = gdpPercap, y = lifeExp + 3, label = country),
            color = "grey50",
            data = filter(gapminder_2007, pop > 1000000000 | country %in% c("Nigeria", "United States"))) +
  scale_x_log10(limits = c(200, 60000)) +
  labs(title = "GDP versus life expectancy in 2007",
       x = "GDP per capita (log scale)",
       y = "Life expectancy",
       size = "Popoulation",
       color = "Continent") +
  scale_size(range = c(0.1, 10),
             # remove size legend
             guide = "none") +
  theme_classic() +
  theme(legend.position = "top",
        axis.line = element_line(color = "grey85"),
        axis.ticks = element_line(color = "grey85"))

ggsave("beautiful_plot.png", p, dpi = 300, width = 6, height = 4)
```

***

**I want more!**. 

I suggest these readings and exercises: [0](https://r4ds.had.co.nz/data-visualisation.html)
[1](http://r-statistics.co/ggplot2-Tutorial-With-R.html)
[2](https://www.edureka.co/blog/ggplot2-tutorial/) [3](https://www.ling.upenn.edu/~joseff/avml2012/) [4](https://ggplot2.tidyverse.org/) [5](http://tutorials.iq.harvard.edu/R/Rgraphics/Exercises.html) [6](http://blog.echen.me/2012/01/17/quick-introduction-to-ggplot2/) [7](https://www.bioinformatics.babraham.ac.uk/training/ggplot_course/Introduction%20to%20ggplot.pdf) [8](https://uc-r.github.io/ggplot_intro) 
[9](http://www.sthda.com/english/wiki/be-awesome-in-ggplot2-a-practical-guide-to-be-highly-effective-r-software-and-data-visualization)
[10](https://www.r-graph-gallery.com/)
Enjoy!

  
  
  
  
  




