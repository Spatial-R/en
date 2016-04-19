---
layout: post
title: "Coordinate Data from Website"
date: 2014-06-11
comments: true
categories: 
- Data source
tags:
- Spatial
- R
---

The longlat data is essential prerequisite for the spatial analysis. Especially in epidemiology, the detail address data is there. The only thing you need do is to transfrom the address into the coordinate. If the data is not large, you can get the coordinate data one by one in [google map](http://maps.google.com/) or [baidu map](http://maps.baidu.com/). However, when number of the data is above 50, the "input-copy" way is extremly boring and insecurity. Thanks to the open API in google map and baidu map, you can get the coordinate data very conveniently. Note the format of coordinate data in google map and baidu map is different. If you get the coordinate data from google map and then show it in baidu map, the deviation comes. 

## Google Map
Google place API is powerful in the tranformation for the english address. Make sure the format of the address is OK. You can copy the code into your R console before input your google place API(Apply for your API in [https://code.google.com/apis/console/]( https://code.google.com/apis/console/)). Note that the google maps api has a daily limit of 2,500 so that your vector shouldn't be too long. The code you can find in Neal D. Goldstein, Amy H. Auchincloss and Brian K. Lee.



## Baidu Map
Baed on the baidu map geocoding API v2.0, you can search for the coordinate for the chinese address. There is no daily limitation for this API. At the same time, you also can transform the Chinese address into english, then use the google place API. The code is as follow:


    library(XML);library(stringr);library(RCurl);library(reshape)
    key = ""  #### Copy your own key here
    get.latlong <- function(data) {
      url.base <- "http://api.map.baidu.com/geocoder/v2/?ak="
      url <- paste(url.base, key, "&callback=renderOption&output=xml&address=",data, sep = "")
      url.result <- getURL(url)
      longlat <- unlist(str_match_all(url.result, "[0-9]+[.]*[0-9]*[<>]"))[c(2:3)]
       }
     lt <- data.frame(t(sapply(dat, get.latlong)))
     lt[, 1] <- gsub("<", "", lt[, 1]);lt[, 2] <- gsub("<", "", lt[, 2])  ### The final result is in lt

Moreover, you can use the geoconv API 2.0 to transform the format of the geocoordinate data(form google map or sogou map to baidu map)

## Conclusion
Which way you choose is based on your data and the platform to show the result. Baidu map api is suitable for chinese address and google map is for english address. 
