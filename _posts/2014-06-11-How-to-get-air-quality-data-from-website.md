---
layout: post
title: "Air Quality Data from Website"
date: 2014-06-11
comments: true
categories: 
- Data source
tags:
- Spatial
- R
---

More people were concerned about the air quality in China, espicially the concentration of PM2.5 that is believed to pose the greatest health risks. If you want to analysis the relationship between the air pollution or tempreture and the mortality, the access to the data on the air pollutants is requsite. Here, i will introduce three ways to get the air quality data from the website.

## Frist website
The website: [http://aqicn.org/map/cn/](http://aqicn.org/map/cn/) supplies the real-time air quality index for PM10,PM2.5,SO2,NO2,O3 and NO. You can backstepping the concentration for each air pollutant based on the air quality index, respectively. The format to air quality index is in the China air quality standard (2012). Moreover, the website gives the meteorology data such as Temperature, dew point temperature, pressure,humidity and wind speed. The code to catch the data is as follow:


    require(RCurl);require(XML);require(rjson);library(reshape);library(data.table)
    options(verbose = TRUE); URL = "http://aqicn.org/city/all/"
    doc = htmlParse(URL);  nodes = getNodeSet(doc, "//a[@href]")
    hrefs = sapply(nodes, function(x) xmlGetAttr(x, "href"))  ###get all the site
    ks = which(hrefs == "http://aqicn.org/city/beijing/dongchengdongsi/")
    ks1 = which(hrefs == "http://aqicn.org/city/hongkong/")
    ref2 = hrefs[ks:ks1]  ### get the site in China

    data = list();i = 1
    while (i < (n/3 + 1)) {
       url = ref2[i]; names = substr(url, 23, (nchar(url) - 1))
       try(y <- readHTMLTable(url, warn = FALSE, which = 6), silent = T)
     if (length(y) == "5") {
       mo = data.frame(y[, c(1, 3)])
       dy = data.frame(t(as.numeric(as.character(mo$Current))))
       names(dy) <- as.character(mo$Var.1); dy$site = rep(names, 1)
       data[[i]] = dy;i = i + 1
      } else {
         i = i + 1
        }
     }
    now.time = substr(Sys.time(), 1, 13)
    file.name = paste(now.time, "-pollutant", ".csv", sep = "")
    dat = ldply(data, rbind.fill)


## Second website
The website: [http://www.pm25.in/api_doc](http://www.pm25.in/api_doc) supply an API to the data in National Bureau of Environmental Protection. You just apply for a appkey and then you can get the air quality data in 190 cities in China conveniently. The website also provide detailed materials about the way to ues the appkey. Thanks to the [anson](http://weibo.com/gzanson?sudaref=www.pm25.in) and his team. The code is as follow:


     library(RCurl);library(XML);library(plyr)
     url="http://www.pm25.in/api/querys/all_cities.json?token=qrzh7e8i9k9mmvf6cQ48"
     pm=getURL(url);pm2 <-fromJSON(pm);pm3=data.frame(do.call("rbind",pm2))
     pm4=apply(pm3,2,as.character); dt=gsub("-","",substr(pm4[1,21],1,13))


## Third website
Some website also punish the air quality data for certain city. For example, the Haikou municipal environmental protection bureau provide the data in its website:[http://220.174.250.107:9808/Views/Home/AirCityIndex.aspx](http://220.174.250.107:9808/Views/Home/AirCityIndex.aspx). You also can use R to download the data. The code is follow:


    library(XML);library(RCurl)
    url="http://220.174.250.107:9808/Views/Home/AirCityIndex.aspx"
    dat<-readHTMLTable(getURL(url),as.data.frame=TRUE,which=5)
     names(dat)<-c("site","so2","no2","pm10","pm10-24","co","03-1","o3-8","pm2.5","pm2.5-24")
     y=paste(as.Date(now),"-",as.POSIXlt(now)$hour,".csv",sep="")

## Conclusion
Based on the R sofware, you can get the air quality data very conveniently. However, if you want to collect the data consistently, the while loop will work well and let your compute run the code in the certain time, for example in the integral point. 



