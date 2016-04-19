---
layout: post
title: "Static Map in R"
date: 2014-07-19
comments: true
categories: 
- Visualization
tags:
- Map
- R
---


Numerous packages in R sofware can help you creat the geographic map. Here, i will summarize the packages related to the map so as to visualizate your geographical data more conveniently.

## Origin map data

If don't have the map data, you can download them from the [GADM](http://www.gadm.org/) by the function **getDate** in the **raster** package. The website supplies the "RData" files (including four levels) which can be used in R environment directly. Alternatively, you can download the "shapefile" file and then use the function **readShapePoly** (in the maptools package) to read the data. Considering the specificity for  the china map, i had put all the relevant data in my github: [https://github.com/Spatial-R/China-Map-Data](https://github.com/Spatial-R/China-Map-Data). The codes to download the data from GADM are as follow:

    library(raster)
    adm <- getData('GADM', country='China', level=1)
    adm1 <- getData('GADM', country='Taiwan', level=0)
    adm2 <- getData('GADM', country='Hong kong', level=0)
    adm3 <- getData('GADM', country='Macao', level=0)
    plot(adm);plot(adm1, add = T);plot(adm2, add = T);plot(adm3, add = T)


The **maps** package also supplies the map data for the world. However, the data is out of date and the China data doesn't show the Chongqing city. Generally speaking, the data from GAMD is the best choose.



## Sp and maptools package

Sp and maptools package are the basic packages in R sofware to deal with the shapefile file. After you have read the map data, you just use the **plot** function to get the map.



    library(sp);library(maptools)
    china.map<-readShapePoly("bou2_4p.shp")
    plot(china.map,main="China map in maptools package")
    points(x=120.2,y=30.3,col="red");text(x=120.2,y=30.3,"Hangzhou",col="Blue")


The way to fill the polys and points with different colors can be found in [here](http://site.douban.com/182577/widget/notes/10568279/note/257898418/) and [here](http://yihui.name/cn/2008/10/china-map-and-city-locations-with-r/).

## Rworldmap package

Rworldmap is a package for visualising global data, concentrating on data referenced by country codes or gridded at half degree resolution. The mapping process then involves 3 steps (or 2 if your data are already in an R dataframe):

  1.  read data into R
  2.  join data to a map (using function **joinCountryData2Map()**)
  3.  display the map (using function **mapCountryData**)
 
The example is as follow:

    library(rworldmap)
    data(countryExData)
    sPDF <- joinCountryData2Map( countryExData,joinCode = "ISO3", nameJoinColumn = "ISO3V10")
    mapParams <- mapCountryData( sPDF, nameColumnToPlot="BIODIVERSITY",addLegend=FALSE )
    do.call( addMapLegend, c(mapParams, legendWidth=0.5, legendMar =4))


## Ggplot2 package
Ggplot2 package offers great power to plot data in R. The plots are designed to comply with the grammar of graphics philosophy and can be produced to a publishable level relatively easily. After you have read the shapefile data in R, the **fortify** function can be applied to transformed the shapefile to dataframe which can be directly used to the ggplot2 plot. The example is follow:



     library(maptools);library(ggplot2)
     china_map<-readShapePoly("bou2_4p.shp")
     china_map1<-fortify(china_map,region='BOU2_4M_ID') ### transform to the dataframe
     ggplot(china_map1, aes(long, lat)) +theme_bw(base_family="serif",base_size=10)+
     geom_path(aes(long,lat, group=group, fill=hole), color="black", size=0.3)


## Googlemap

Ggmap,plotGoogleMaps and RgoogleMaps packages can use the googlemap API to get or display the map data. Moreover, the downloaded map can combine with ggplot package. However, the googla map doesn't work in China for a long time. 

