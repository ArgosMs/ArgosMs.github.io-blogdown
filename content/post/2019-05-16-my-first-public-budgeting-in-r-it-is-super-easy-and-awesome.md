---
title: 'My first public budgeting in R: it is super easy and awesome!'
author: ArgosMs
date: '2019-05-01'
slug: my-first-public-budgeting-in-r-it-is-super-easy-and-awesome
categories:
  - budgetr
tags:
  - Public Budgeting
  - Accountability
  - Decision Making
description: ''
topics: []
---

***

[Here](https://www.managementstudyguide.com/budget-in-public-administration.htm) you find what public budgeting is. 

In this [post](https://www.hhcpa.com/blogs/non-profit-accounting-services/a-look-into-the-importance-of-budgets-for-governments-and-not-for-profits/) a bit of the importance of budgeting for nonprofit orrganizations.

And [here](https://economictimes.indiatimes.com/budget-faqs/why-is-it-important-for-the-government-to-have-a-budget/articleshow/67450000.cms?from=mdr) a bit of the crucial role that budgeting plays in economic development.

So now that you know that public budgeting is a very important tool for decision making, let me introduce it to [budgetr](https://derek-damron.github.io/budgetr/).

Derek Damron, the creator of `budgetr`, provides a very good intoduction to this package and how it works. So I will skip it but strongly suggest you to read, practice and play with his instructions before enaging in the exercise below.

The exercise we will be practing today is a quick introduction to the very initial step in building a [participatory budget](https://www.govtech.com/fs/infrastructure/Is-Participatory-Budgeting-the-Answer-to-Cities-Biggest-Questions.html) platform.

Click [here](https://www.citizenlab.co/blog/civic-engagement/steps-to-effective-participatory-budgeting/) and [here](https://www.participatorybudgeting.org/) to learn more about participatory budgeting. [Examples](https://www.shareable.net/15-participatory-budgeting-projects-that-give-power-to-the-people/).

***

Let's begin working on the first step then by creating a budget.

***

`install.packages("remotes")`

`remotes::install_github("derek-damron/budget")`

`library(budgetr)`

***

Let's say that we want to help the council decide how to allocate resources for the renovation of a public park.

***

Let's first create the items of the budget. Let's also assume that the total budget is $100,000

***

```{r}
expense_super_awesome_playground <- create_item( name = "Expense Super Awesome Playground"
                   , amount = -3000
                   , day = "2019-01-01"
                  , recurring = "monthly")

expense_safe_lighting <- create_item( name = "Expense Safe Lighting"
                       , amount = -2000
                       , day = "2019-01-01"
                       , recurring = "monthly")
                       
expense_cool_bike_lanes <- create_item( name = "Expense Cool Bike Lanes"
                       , amount = -5000
                       , day = "2019-01-01"
                       , recurring = "monthly")
```
***

Let's now create the budget for the renovation of this public park.

***
                        
```{r}
park_schedule <- create_schedule(expense_super_awesome_playground,
                                 expense_safe_lighting,
                                 expense_cool_bike_lanes) 
```

`park_schedule`

***

![](C:/Users/NewUser/Documents/Blogdown/Rplot_2.png)

***

Great! You have now your initial budget schedule. You know how much the city has allocated to the renovation of the park and the total amount initially expected to be monthly spent on each of items added in the budget. See below.

***

Let's now take a look at the budget.

***

```{r}
park_budget <- create_budget(park_schedule, start="2019-01-01", initial=100000)
```

***

`park_budget`

***

![](C:/Users/NewUser/Documents/Blogdown/Rplot_1.png)


***

`plot(park_budget)`

***

![](C:/Users/NewUser/Documents/Blogdown/Rplot.png)


***

Okay. So now we know that by April 2019 the total amount left for the park renovation of the three items would be $60,000, if expenses on each item are kept the same.

***

But wait! We have just learned that the cost of renovating the bike lane has increased even before we started it. It will be now $6000 per month. We need to update this amount in our budget!

***

`expense_cool_bike_lanes <- update_item(expense_cool_bike_lanes, amount = -6000)`

***

This increase also impacted our `schedule`, so we need to update it as well.

***

```{r}
park_schedule <- create_schedule(get_items())
park_schedule
```

***

![](C:/Users/NewUser/Documents/Blogdown/Rplot_3.png)

***

```
park_budget <- create_budget(park_schedule, start="2019-01-01", initial = 100000)
park_budget
```

***

![](C:/Users/NewUser/Documents/Blogdown/Rplot_4.png)

***

So now we know that we budget will be at $5600 with this unexpected increase on the renovation of bike lanes. 

***

`plot(park_budget)`

***

![](C:/Users/NewUser/Documents/Blogdown/Rplot_5.png)

***

Good news! The has just announced that the state government will help the renovation of this public park with $50,000.

Great! Let's update our schedule! It is easy!

***

state_support <- create_item( name = "State Support"
                           , amount = 50000
                           , day = "2019-01-01"
                           )
park_schedule_updated <- update_schedule(park_schedule, add=state_support)

***

Few more changes we will need to do before we start this renovation. The council now wants to delay the start of this rebovation until June 2019 and finish by the end of the year.

Okay! Let's update our schedule.

***

`park_budget_updated <- update_budget(park_budget, start = "2019-06-01", end = "2019-12-01")`

`plot(park_budget_updated)`

***

![](C:/Users/NewUser/Documents/Blogdown/Rplot_6.png)

***

Well, if everything goes according to plan, the amount initially allocated for the renovation of the public park should be more than enough and one can expect it to be completed in six months with starting date expected to be mid 2019.

***

I hope you have enjoyed this exercise. The objective was to introduce to `R` language in a friendly and meaninful way. Once you start feeling more comforatble with this programming language, you will naturally explore other possibilities to design public budgets that help you and your organization make better decisions.

***

To those you want some challenges, you may want to create an interactive public budgeting using the package `Shiny`. This is a very good firs step to build a full-on participatory budgeting platform. 

Find some initiatives here: [1](https://pbstanford.org/) [2](https://abalancingact.com/) [3](https://www.demsoc.org/participatory-budgeting-in-scotland/) [4](https://www.budgetallocator.com/)
[5](http://community.openspending.org/resources/gift/chapter6-3/)


