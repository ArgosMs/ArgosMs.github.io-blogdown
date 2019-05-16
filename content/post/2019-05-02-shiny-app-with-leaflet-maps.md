---
title: Shiny App with Leaflet Maps
author: ArgosMs
date: '2019-05-02'
slug: shiny-app-with-leaflet-maps
categories:
  - Shiny
  - leaflet
tags:
  - Education
  - Map
  - Data Communication
  - Data Visualization
  - Interactiveness
description: ''
topics: []
---

***
I came across this interesting [blog](http://sillasgonzaga.com/post/mapeando-a-abertura-de-escolas-municipais-em-sao-paulo-ao-longo-dos-anos/) and started playing with it.

The main issue is that you now need to provide your credit card number to Google to use `ggmap`. I did not want to, so I recreated the second part of code using maps from `leaflet` and decided do the animation using `shiny`.

The beauty of *shiny apps* is that they are interactive, so they help create simple and compelling narratives from large datasets.

The map below, for instance, shows the number of public schools opened anually in the city of Sao Paulo. As you move the slider from 1902 to 2018, the dots in the map are updated with the location of public schools opened in that particular year across the city.

Quite [cool](https://argosms.shinyapps.io/SP_Schools_GIF/)!

***

**Data Cleaning**

***

```{r}
library(tidyverse)
library(lubridate)
library(leaflet)
library(shiny)
library(xts)
library(dplyr)
library(sp)
library(assertthat)
library(crosstalk)
library(cellranger)
library(colorspace)
library(broom)

lcl <- locale(encoding = "ISO-8859-1")

df <- read_csv2("C:/Users/NewUser//Documents/SP_Schools_GIF/escolasr34jun18.csv", locale = lcl)

dim(df)

df <- df %>% 
  select(DATA = DT_CRIACAO, LAT = LATITUDE, LON = LONGITUDE)

converter_mes <- function(x){
  nomes <- c("jan", "fev", "mar", "abr", "mai", "jun",
             "jul", "ago", "set", "out", "nov", "dez")
  
  numeros <- str_pad(1:12, width = 2, pad = "0")
  
  x <- str_replace_all(x, nomes[1], numeros[1])
  x <- str_replace_all(x, nomes[2], numeros[2])
  x <- str_replace_all(x, nomes[3], numeros[3])
  x <- str_replace_all(x, nomes[4], numeros[4])
  x <- str_replace_all(x, nomes[5], numeros[5])
  x <- str_replace_all(x, nomes[6], numeros[6])
  x <- str_replace_all(x, nomes[7], numeros[7])
  x <- str_replace_all(x, nomes[8], numeros[8])
  x <- str_replace_all(x, nomes[9], numeros[9])
  x <- str_replace_all(x, nomes[10], numeros[10])
  x <- str_replace_all(x, nomes[11], numeros[11])
  x <- str_replace_all(x, nomes[12], numeros[12])
  
  x
  
}

df <- df %>% 
  mutate(DATA_CLEAN = dmy(converter_mes(DATA)),
         LAT = LAT/1e6,
         LON = LON/1e6
  ) %>% 
  mutate(ANO = year(DATA_CLEAN)) %>% 
  na.omit()

summary(df)

df %>% 
  count(ANO) %>% 
  ggplot(aes(x = ANO, y = n)) + 
  geom_col(fill = "darkorange1") + 
  theme_minimal() +
  labs(x = NULL, y = NULL,
       title = "The number of public schools founded in Sao Paulo") +
  scale_x_continuous(breaks = scales::pretty_breaks(n = 10))

praca_se <- leaflet() %>%
  addTiles() %>%  
  addCircles(lng=-46.634123, lat=-23.548408,
             popup="Praca da Se")

all_points <- leaflet(df) %>%
  addTiles() %>%  
  addCircles(lng= df$LON, lat= df$LAT,
             popup="Praca da Se")

date <- df$ANO

table(df$ANO)
```
***

**Shiny's user interface** (`ui`)

***

```{r}
ui <- fluidPage(
  sliderInput("obs", "Year", min(date), 
              max(date),
              value = max(date),
              step=1,
              animate=F),
  leafletOutput("all_points")
)
```
***

**Shiny's server and** `leaflet` **map**

***

```{r}
server <- function(input, output, session) {
  obs <- reactive({
    df %>% 
      filter(date==input$obs)
  })
  
  output$all_points <- renderLeaflet({
    df <- obs()
    
    leaflet(df) %>%
      addTiles() %>%  
      addCircles(lng= df$LON, lat= df$LAT,
                 radius = .1,
                 color= "black",
                 stroke = T, 
                 fillOpacity = 10)
  })
}

shinyApp(ui, server)
```
***