---
title: 'Building Interactive Maps in R: I can do it!'
author: ArgosMs
date: '2019-05-07'
slug: building-interactive-maps-in-r-i-can-do-it
categories:
  - leaflet
tags:
  - Interactive Maps
  - Data Visualization
description: ''
topics: []
---

***

Maps, or [spatial analytics](https://www.forbes.com/sites/esri/2017/12/20/the-importance-of-applying-spatial-analytics-in-business/#e84e7183e01b), are a very useful method to share many stories. It is even better when these maps are [interactive](https://leafletjs.com/examples.html).

Today we will learn how to easily build a map in `R` using the popular package of [`leaflet`](https://rstudio.github.io/leaflet/).

There are many tutorials online but they are either designed for advanced users or feature some issues in their codes. This is the case of this very good [one](https://allthisblog.wordpress.com/2016/10/12/r-311-with-leaflet-tutorial/) I selected after fixing some of its bugs.

This post is a bit long and may seem a bit too complicated for beginners. Do not fear!

The main goal of this [vignette](http://r-pkgs.had.co.nz/vignettes.html) is to introduce the most important aspects of `leaflet` so that you can start building your own interactive maps as quickly as possible.

Let's get to work!

***

```{r}
#install.packages("leaflet")
library(leaflet)
```

***

```{r}
m <- leaflet()
m <- addTiles(m)
m <- addMarkers(m, lng=-123.256168, lat=49.266063, popup="T")
m
```

***

Let's now get and prepare the dataset.

***

```{r}
#install.packages("culr")
library(curl)
```

***

```{r}
data_filter = read.csv(curl('https://raw.githubusercontent.com/joeyklee/aloha-r/master/data/calls_2014/201401CaseLocationsDetails-geo-filtered.csv'), header=TRUE)
```

***

`Subset` the dataset

***

```{r}
df_graffiti_noise <- subset(data_filter, cid == 1)
df_street_traffic_maint <- subset(data_filter, cid == 2)
df_garbage_recycling_organics <- subset(data_filter, cid == 3)
df_water <- subset(data_filter, cid == 4)
df_animal_vegetation <- subset(data_filter, cid == 5)
df_other <- subset(data_filter, cid == 0)
```

***

Add `tiles` to the map.


***

```{r}
m %>%
  addTiles() 
```
  
***

Define `colors` based on `cid`. 

***

```{r}
colorFactors <- colorFactor(c('red', 'orange', 'purple', 'blue', 'pink', 'brown'),
                           domain = data_filter$cid)
```

***

Add `markers` on the map based on `subsets`. Check the other [`arguments`](https://data-flair.training/blogs/r-arguments-introduction/) of this function.

***

```{r}
m <- addCircleMarkers(m, 
                     lng = df_graffiti_noise$lon_offset,  
                     lat = df_graffiti_noise$lat_offset,
                     popup = df_graffiti_noise$Case_Type, 
                     radius = 2, 
                     stroke = FALSE, 
                     fillOpacity = 0.75, 
                     color = colorFactors(df_graffiti_noise$cid),
                     group = "1 - graffiti & noise")

m <- addCircleMarkers(m, 
                     lng = df_street_traffic_maint$lon_offset, lat=df_street_traffic_maint$lat_offset, 
                     popup = df_street_traffic_maint$Case_Type, 
                     radius = 2, 
                     stroke = FALSE, fillOpacity = 0.75,
                     color = colorFactors(df_street_traffic_maint$cid),
                     group = "2 - street & traffic & maintenance")

m <- addCircleMarkers(m, 
                     lng = df_garbage_recycling_organics$lon_offset, lat=df_garbage_recycling_organics$lat_offset, 
                     popup = df_garbage_recycling_organics$Case_Type, 
                     radius = 2, 
                     stroke = FALSE, fillOpacity = 0.75,
                     color = colorFactors(df_garbage_recycling_organics$cid),
                     group = "3 - garbage related")

m <- addCircleMarkers(m, 
                     lng = df_water$lon_offset, lat=df_water$lat_offset, 
                     popup = df_water$Case_Type, 
                     radius = 2, 
                     stroke = FALSE, fillOpacity = 0.75,
                     color = colorFactors(df_water$cid),
                     group = "4 - water related")

m <- addCircleMarkers(m, 
                     lng = df_animal_vegetation$lon_offset, lat=df_animal_vegetation$lat_offset, 
                     popup = df_animal_vegetation$Case_Type, 
                     radius = 2,
                     stroke = FALSE, fillOpacity = 0.75,
                     color = colorFactors(df_animal_vegetation$cid),
                     group = "5 - animals & vegetation")

m <- addCircleMarkers(m, 
                     lng = df_other$lon_offset, lat=df_other$lat_offset, 
                     popup = df_other$Case_Type, 
                     radius = 2,
                     stroke = FALSE, fillOpacity = 0.75,
                     color = colorFactors(df_other$cid),
                     group = "0 - other")
```
                     
***

Add `tiles` to the `map`.

***

```{r}
m <- addTiles(m, group = "OSM (default)") 
m <- addProviderTiles(m,"Stamen.Toner", group = "Toner")
m <- addProviderTiles(m, "Stamen.TonerLite", group = "Toner Lite")

m <- addLayersControl(m,
                     baseGroups = c("Toner Lite","Toner"),
                     overlayGroups = c("1 - graffiti & noise", "2 - street & traffic & maintenance",
                                       "3 - garbage related","4 - water related", "5 - animals & vegetation",
                                       "0 - other")
)

m
```

***

Let's now `optimize` the map by making a [`list`](https://www.tutorialspoint.com/r/r_lists.htm) of variables so we can [`loop`](https://www.datamentor.io/r-programming/for-loop/) through them.

***

These are the `subsets` as seen before. Note that `=` can replace `<-`.


***

```{r}
df_graffiti_noise = subset(data_filter, cid == 1)
df_street_traffic_maint = subset(data_filter, cid == 2)
df_garbage_recycling_organics = subset(data_filter, cid == 3)
df_water = subset(data_filter, cid == 4)
df_animal_vegetation = subset(data_filter, cid == 5)
df_other = subset(data_filter, cid == 0)
```

***

This is the `list`.

***

```{r}
data_filterlist <- list(df_graffiti_noise = subset(data_filter, cid == 1),
                       df_street_traffic_maint = subset(data_filter, cid == 2),
                       df_garbage_recycling_organics = subset(data_filter, cid == 3),
                       df_water = subset(data_filter, cid == 4),
                       df_animal_vegetation = subset(data_filter, cid == 5),
                       df_other = subset(data_filter, cid == 0))
```
                       
***

These are the `layers` and `colors`.

***

```{r}
layerlist <- c("1 - graffiti & noise", "2 - street & traffic & maintenance",
              "3 - garbage related","4 - water related", "5 - animals & vegetation",
              "0 - other")

colorFactors <- colorFactor(c('red', 'orange', 'purple', 'blue', 'pink', 'brown'), 
                            domain=data_filter$cid)
```
                            
***

And this is the `loop`. Find `loop` exercises [here](https://www.r-exercises.com/2018/03/30/loops-in-r-exercises/).

***

```{r}
for (i in 1:length(data_filterlist)){
    m <- addCircleMarkers(m, 
                         lng=data_filterlist[[i]]$lon_offset,
                         lat=data_filterlist[[i]]$lat_offset, 
                         popup=data_filterlist[[i]]$Case_Type, 
                         radius=2,
                         stroke = FALSE, 
                         fillOpacity = 0.75,
                         color = colorFactors(data_filterlist[[i]]$cid),
                         group = layerlist[i]
    )
}
```

***

Let's build the new `map` now.

***

```{r}
m <- addTiles(m, "Stamen.TonerLite", group = "Toner Lite") 
m <- addLayersControl(m,
                     overlayGroups = c("Toner Lite"),
                     baseGroups = layerlist)
m
```

***

[`Polygons`](https://cengel.github.io/rspatial/4_Mapping.nb.html) are important aspects of `maps`. 

***

```{r}
#install.packages(c("rgdal", "GISTools", "downloader"))
library(rgdal)
library(GISTools)
library(downloader)
```

***

Get and read about `polygons`. If you have problem reading this file, follow these [instructions](http://rstudio-pubs-static.s3.amazonaws.com/84577_d3eb8b4712b64dbdb810773578d3c726.html).

***

`hex_1401_fn <- "https://raw.githubusercontent.com/joeyklee/aloha-r/master/data/calls_2014/geo/hgrid_250m_1401_counts.geojson`
`downloader::download(url = hex_1401_fn, destfile = "/Users/NewUser/Documents/hgrid_250m_1401_counts.GeoJSON")`
`hex_1401 <- readOGR(dsn = "./hgrid_250m_1401_counts.GeoJSON", layer = "hgrid_250m_1401_counts")`
```

***

Initiate `map` with `Stamen Toner`.

***

```{r}
m <- leaflet()
m <- addProviderTiles(m, "Stamen.TonerLite", group = "Toner Lite")
```

***

Set `color scale`.

***

`pal <- colorNumeric( palette = "Greens", domain = hex_1401$data)`

***

Add `polygons` to the `map`.

***

```{r eval=F}
m <- addPolygons(m, 
                data = hex_1401,
                stroke = FALSE, 
                smoothFactor = 0.2, 
                fillOpacity = 1,
                color = ~pal(hex_1401$data),
                popup = paste("Number of calls: ", hex_1401$data, sep="")
)
```

***

Add `legend` to this [`cloropeth`](https://www.r-graph-gallery.com/chloropleth-map/) `map`.

***

Put all `maps` together.

***

```{r eval=F}
m <- addLegend(m, "bottomright", pal = pal, values = hex_1401$data,
              title = "3-1-1 call density",
              labFormat = labelFormat(prefix = " "),
              opacity = 0.75
)

m

m <- leaflet()
m <- addProviderTiles(m, "Stamen.TonerLite", group = "Toner Lite") 

hex_1401_fn <- 'https://raw.githubusercontent.com/joeyklee/aloha-r/master/data/calls_2014/geo/hgrid_250m_1401_counts.geojson'
hex_1401 <- readOGR(dsn = "./hgrid_250m_1401_counts.GeoJSON", layer = "hgrid_250m_1401_counts")

pal <- colorNumeric(
  palette = "Greens",
  domain = hex_1401$data
)

m <- addPolygons(m, 
                data = hex_1401,
                stroke = FALSE, 
                smoothFactor = 0.2, 
                fillOpacity = 1,
                color = ~pal(hex_1401$data),
                popup= paste("Number of calls: ",hex_1401$data, sep=""),
                group = "hex"
)

m <- addLegend(m, "bottomright", pal = pal, values = hex_1401$data,
              title = "3-1-1 call density",
              labFormat = labelFormat(prefix = " "),
              opacity = 0.75
)

df_graffiti_noise = subset(data_filter, cid == 1)
df_street_traffic_maint = subset(data_filter, cid == 2)
df_garbage_recycling_organics = subset(data_filter, cid == 3)
df_water = subset(data_filter, cid == 4)
df_animal_vegetation = subset(data_filter, cid == 5)
df_other = subset(data_filter, cid == 0)

data_filterlist <- list(df_graffiti_noise = subset(data_filter, cid == 1),
                       df_street_traffic_maint = subset(data_filter, cid == 2),
                       df_garbage_recycling_organics = subset(data_filter, cid == 3),
                       df_water = subset(data_filter, cid == 4),
                       df_animal_vegetation = subset(data_filter, cid == 5),
                       df_other = subset(data_filter, cid == 0))

layerlist <- c("1 - graffiti & noise", "2 - street & traffic & maintenance",
              "3 - garbage related","4 - water related", "5 - animals & vegetation",
              "0 - other")


colorFactors <- colorFactor(c('red', 'orange', 'purple', 'blue', 'pink', 'brown'), domain=data_filter$cid)
for (i in 1:length(data_filterlist)){
  m <- addCircleMarkers(m, 
                       lng=data_filterlist[[i]]$lon_offset, lat=data_filterlist[[i]]$lat_offset, 
                       popup=data_filterlist[[i]]$Case_Type, 
                       radius=2,
                       stroke = FALSE, fillOpacity = 0.75,
                       color = colorFactors(data_filterlist[[i]]$cid),
                       group = layerlist[i]
  )
  
}


m <- addLayersControl(m,
                     overlayGroups = c("Toner Lite", "hex"),
                     baseGroups = layerlist
)

m
```

***

Congratulations! You are awesome!

***

**I want more!**. 

Practice with these [exercises](https://www.r-exercises.com/2018/08/27/specalize-in-geo-spatial-visualizations-with-leaflet-part-1-exercises/).

***
