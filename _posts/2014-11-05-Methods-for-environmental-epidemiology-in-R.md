---
layout: post
title: "Methods for Environmental Epidemiology in R"
date: 2014-08-14
comments: true
categories: 
- statistical analysis
tags:
- Time series
- Environmental Epidemiology
---



Recently, the short effect of air pollution on the mortality have drawn more and more attention in China. Many experts have try different methods to analysis the impact of air pollution on health. The study design has fallen into four types: ecological time series, case-crossover, panel and cohort studies. In general, the first three methods are the best way to analysis the short effect (acute effect), whereas the cohort study is used to estimate the acute and chronic effect of the air pollution. Here, i will introduce the ecological time series and case-crossover methods based on the R software. And later, i will cover the hierarchical model for multi-sites time series study to pooling the risk across locations. It is also noticed that two method also can be applied to other study, fox example, the short effect of meteorological factors on mortality or acute infectious disease. 
（Some methods can be found in the book: ****Statistical methods in environmental epidemiology with R: A case study in air pollution and health****. You can download it form [here](http://bbs.pinggu.org/thread-710855-1-1.html)）

-------------------------------

## Time series and case-crossover

 Time series and case-crossover analysis are the most common methods used to estimate the short effects of air pollution on health. Both methods typically treat the outcome as the counts representing the number of times a particular event occurred on a given day. Time series allows for over-dispersion  associated with the poisson distribution and controls for the long-term trend and seasonality using the nonparametric or parametric splines. The case-crossover method compares the exposure during a case day when the event occurred with the exposure in nearby control days and examine whether the event is associated with the exposure. It is obvious that the confounding related the individual characteristics are controlled by the design. Both methods have the advantage and weakness, therefore, the choose to the analysis method depends your purpose. Some experts also compared two methods and you can download the paper from [here](http://www.sciencedirect.com/science/article/pii/S0048969710010983)
 
 No matter which method you choose, you should download the **tsModel** package into R software. Thanks to Roger D. Peng, who have develop the package for statistical methods in environmental epidemiology. Another attribution for him is the reproducible research. His homepage can be found [here](http://www.biostat.jhsph.edu/~rpeng/). 

---------------------

## Models in R software ##

  Generally, the origin data from the Center for Disease Control and Prevention (CDC) was recorded as case with the information such as death date, cause of death and address. You can tidy up the data more conveniently by the package [**dplyr**](http://cran.r-project.org/web/packages/dplyr/index.html). Fox example, using the code as followed you can get the daily count for each city:

    dat.count<-data.frame(summarise(group_by(dat.dis,date,city),count=n()));

  In China, how to get the air pollution data and meteorologic factors such as daily temperature and humidity is indeed a big problem. The air pollution data can be obtained form the website. The way is showed in [my homepage](http://spatial-r.github.io/en/2014/06/How-to-get-air-quality-data-from-website/). However, you need make the compute work all the time for the updated data every hour. The meteorologic data can be obtained from the [China Meteorological Data Sharing Service System](http://cdc.cma.gov.cn/home.do). After all the data is ok, you can merge the air pollution data, meteorological data and the mortality data into one data. The code can be here:
     
     dat.airmet<-merge(dat.air,dat.mer,by=c("date","city"),all.y=T)
     dat.fin<-merge(dat.airmet,dat.count,by=c("date","city"),all.y=T)

  Here, The way to merge air pollution data and meteorological data is based on the function**merge** and set the argument **by** with **c("date","city")**. Therefore, you should make sure that both the **dat.air** (air pollution dat) and **dat.mer** (meteorological data) included the two variables **date** and **city**. The argument **all.y=T** mean that the dat.air was added to the dat.mer according to the date and city, just for the reason that the date in meteorological data is always completed. The final data may have some missing data for example the concentrations of air pollutants, you can complete it using the interpolation methods in **xts** package. There are some [example](http://cos.name/cn/topic/129915/) you can learn.

  Now, we will try the statistical methods and estimate the short effect of the air pollution on health. The time-series method is as followed:
 
    res.ts<-glm(count~pm10+ns(temperature,3)+ns(humidity,3)+ns(date,6*7)  
     +factor(dow)+factor(holiday),family=poisson,data=dat.fin)   
    

  Here, **ns()** means a nature cubic spline; 7 degree for time ( ns(date,6&7) represents the year in data is 6 year). Dichotomous variables indicating the day of week(Dow) and the public holidays (holiday) are also included in the model. The final composition is a nature cubic spline for temperature and humidity with 3 df, respectively. Sometimes, the model also include another variable **factor(infectious)** to adjust the impact from the infectious disease. 

  There are many different designs for choosing the control days in case-crossover study. **Time-stratified case crossover ** is commonly used for the fixed and disjointed time strata(eg., month). The code to case-crossover is as followed:
       
     res.case<-glm(count~pm10+ns(temperature,3)+ns(humidity,3)+factor(strata)  
      +factor(dow)+factor(holiday),family=poisson,data=dat.fin)

  Here, the argument **factor(strata)** is the big difference compared the time-series method. 
  An appropriate time to stratify the date depends on your experience. The way to create the variable **strata** is not a difficult thing after the time strata is fixed.    
  
  --------------------------------------------------------------------