---
title: 'My first time series in R: uhuuu!'
author: ArgosMs
date: '2019-05-03'
slug: my-first-time-series-in-r-uhuuu
categories:
  - aTSA
  - tseries
tags:
  - Time Series Analysis
  - Decision Making
description: ''
topics: []
---

***

The importance of [time series](https://towardsdatascience.com/trend-seasonality-moving-average-auto-regressive-model-my-journey-to-time-series-data-with-edc4c0c8284b) for the strategic decisions organizations need to make cannot be overstated enough. 

Fortunately `R` helps us with that and there are many tutorials online to help us build our first time series while becoming a bit more familiar with key terms used in this type of modeling.

After looking around the web for a while, I recommend this [one](https://www.analyticsvidhya.com/blog/2015/12/complete-tutorial-time-series-modeling/) for beginners. 

The author explain time series jargons in clear and straighforward language while also introducing us to some codes that we can run in our console.

Enough said. Let's get into our travel time machine!

***

First off, it is important to understand the concept of `stationarity`. You **need** a *stationary* time series to build a time series model.

So what makes a time series *stationary*? A time series is *stationary* if... 

i) the `mean` of the series is constant; that is, it does not change over time;

ii) the `variance` (*i.e.* how spread the data is to the `mean`) also does not change over time. In other words, the variation in the spread of data around the `mean` (*aka* as `distribution`) is also constant;

iii) the `covariance`, or the measure of how two random variables change together, also does not change over time. It is also constant.

In sum, if the time series you are looking at is constant, the `mean`, `variance`, and `covariance` do not change much over time, it is most likely that you are looking at a *stationary* time series. This is good!

But do not panic if your time series seems to be `non-stationary`. You can `stationarize` your time series using the function [`ts()`](https://www.statmethods.net/advstats/timeseries.html), for example.

***

So let's take a look now at a times series that is `stationary` so that we can run a time series model.

If you run the codes below you will find some initial information from this dataset comprising international airline passengers from 1949 to 1960.

***

```{r}
data(AirPassengers)
class(AirPassengers)
start(AirPassengers)
end(AirPassengers)
frequency(AirPassengers)
summary(AirPassengers)
plot(AirPassengers)
abline(reg=lm(AirPassengers~time(AirPassengers)))
cycle(AirPassengers)
plot(aggregate(AirPassengers,FUN=mean))
boxplot(AirPassengers~cycle(AirPassengers))
```

***

After taking a look at the graphs resulting from the `functions` above, you can confirm that this `time series` is `stationary`. So let's continue working on this dataset.

***

So now we know what `stationary` is. The next step is to differentiate between `autoregressive` and `moving average` `time series` models.

***

`Autoregressive` (`AR`) Time Series Model occurs when an output (or prediction of a value) depends on previous observations (or inputs). For instance, the amount of goods and services that a country produces over a period of time (*aka* Gross Domestic Product -- GDP) depends on the investments made in previous years. You need to water the flower to help it grow!

`Moving Average` (`MA`) Time Series Model, however, does not use previous observations, or past forecasts, to predict future values. `Moving Average` models use the `errors` from past forecasts to predict future values. The GDP of a country collapsed not because of lack of investmet but because of unexpected climate factors. Watering flowers may not be enough to make them bloom!  

So what is the main difference between `AR` and `MA` models? `AR` models are more sensitive to `noise` or `error` than `MA` models. That is, the effect of `errors` (or unexpected water shortages) has a much lasting impact on the predictions (or flowers) of `AR` models than of `MA` models.   

***

Two other important concepts we need to learn before building our `time series` model are: `AutoCorrelation Function` (`ACF`) and `Partial AutoCorrelation Function` (`PACF`).

`ACF` measures the `correlation` (*the strength of the relationship between two variables*) between different `lag` (*the correlation for time series observations with observations with previous time steps*) functions.

[`PACF`](https://machinelearningmastery.com/gentle-introduction-autocorrelation-partial-autocorrelation/) is the summary of the relationship between an observation in a time series with observations *at prior* time steps with the relationships of intervening observations removed.

***

So plotting `ACF` and `PACF` tell us whether we are dealing first with a `AR` or `MA` `time series` model.

If `PACF` [displays](https://people.duke.edu/~rnau/411arim3.htm) a sharp cut off while `ACF` decays more slowly, this stationarized series features an `AR` signature and vice-versa.

***

So now we know that the steps to conduct a `time series` analysis are:


i) Visualize the `time series`
ii) Stationarize the series
iii) Plot `ACF`/`PACF` charts to find the right parameters by identifying if `time series` is `AR` or `MA`.
iv) Build `ARIMA` Model
v) Make Predictions

        
***

Let's now create a `time series` model

***

i) Test the Resultant Series

***

```{r eval = FALSE}
install.packages("aTSA")
library(aTSA)
install.packages("tseries")
library(tseries)
```

`adf.test(diff(log(AirPassengers)), alternative="stationary", k=0)`

***

ii) Find Right Parameters

***

`acf(log(AirPassengers))`

***

iii) Remove Unequal Parameters by Using `Log` of the Series

***

`acf(diff(log(AirPassengers)))`

***

iv) Address Trend Component by Taking `Difference` of the Series

***

`pacf(diff(log(AirPassengers)))`

***

v) Fit an `ARIMA` Model

***

`(fit <- arima(log(AirPassengers), c(0, 1, 1),seasonal = list(order = c(0, 1, 1), period = 12)))`

***

vi) Predict the Future 10 years

***

`pred <- predict(fit, n.ahead = 10*12)`

***

vii) Fitting in a Seasonal Component

***

`ts.plot(AirPassengers,2.718^pred$pred, log = "y", lty = c(1,3))`

***

And voilà! Here is your first `time series`. 

***

**I want more!**. 

Here you find more ways to practice `time series`: [1](https://a-little-book-of-r-for-time-series.readthedocs.io/en/latest/src/timeseries.html)
[2](https://robjhyndman.com/talks/MelbourneRUG.pdf)
[3](https://otexts.com/fpp2/graphics.html)
