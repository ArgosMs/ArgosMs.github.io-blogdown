---
title: I made my first web application in just few minutes!
author: ArgosMs
date: '2019-04-29'
slug: i-made-my-first-web-application-in-just-few-minutes
categories:
  - Shiny
tags:
  - Interactive Web Apps
  - Data Communication
description: ''
topics: []
---

***

Find here what is [Shiny](https://www.rstudio.com/products/shiny-2/).

If you want a comprehesive tutorial, click [here](https://shiny.rstudio.com/tutorial/written-tutorial/lesson1/).

If you learn better listening, check this excellent [3-part](https://shiny.rstudio.com/tutorial/) video tutorials.

Okay, let's roll!

***

In RStudio: `File > New File > Shiny App Web`

Create a name in `Application Name` and click on `Create`.

This is what to see, right?

***

![](C:/Users/NewUser/Documents/Shiny_Tutorial/Shiny_Tutorial_image_0.png)

***

Click on `Knit`

***

You see this now, correct?

***

![](C:/Users/NewUser/Documents/Shiny_Tutorial/Shiny_Tutorial_image_1.png)

***

Excellent! 

You have built your first Shiny app! The first of your many web apps, right?

***

Let's now show your great work to the world! 

More information on how to deploy your app to the web [here](https://shiny.rstudio.com/deploy/).

***

`Sign up` to `shinyapps.io` if you do not have an account yet.

***

Log in and follow the three steps:

i) Install 'rsconnect' `install.packages('rsconnect')`
ii) Authorize Account
iii) Deploy

***

If you run into error messages like this:

`Error in library(...) : there is no package called ...`

Just install `install.packages()` the package `R` is asking you 
and make it available using `library()`

Wait for a few seconds... Voilà!

Your app is up and running: `https://argosms.shinyapps.io/shiny_tutorial/` for the world to see!

***

![](C:/Users/NewUser/Documents/Shiny_Tutorial/Shiny_Tutorial_image_2.png)

***

### Challenge 1

in `app.R`: What is `ui`? What is `server`?

***

### Challenge 2

in `app.R`, under `sliderInput`: play with the numbers in `min` `max` `value`

then `save` and click on `Run app`. Can you see the changes?

***

### Challenge 3

Add a [widget](https://shiny.rstudio.com/gallery/widget-gallery.html) to your app.
**Hint**: think about `input` and `output`

***

[Examples](https://www.showmeshiny.com/) of Shiny apps.

***

#### I want more!

Another [tutorial](https://deanattali.com/blog/building-shiny-apps-tutorial/), and one [more](https://www.listendata.com/2018/02/shiny-tutorial-r.html), and one [more](http://zevross.com/blog/2016/04/19/r-powered-web-applications-with-shiny-a-tutorial-and-cheat-sheet-with-40-example-apps/)

[Resources](https://bookdown.org/weicheng/shinyTutorial/resources.html)

***

