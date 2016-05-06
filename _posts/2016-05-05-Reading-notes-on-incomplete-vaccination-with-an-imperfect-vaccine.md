---
layout: post
title: "Incomplete vaccination with an imperfect vaccine"
date: 2016-05-05
comments: true
categories: 
- ReadingNotes
tags:
- Vaccine
- Epidemiology
---

#  Incomplete vaccination with an imperfect vaccine

This is my reading notes from the paper: **Pertussis immunity and epidemiology: mode and duration of vaccine-induced immunity**, which was subjected on the **nature**, **degree** and **durability** of the vaccine protection. 

## Background

- The resurgence of pertussis in some countries that maintain high vaccination coverage have drawn attention to the gaps in our understanding of the epidemiology effects of the pertussis vaccine.
-  Candiate explantations range from the vaccine-driven evaluation of the aetiological agent, to the changes of the reporting and surveillance and lossing the efficience due to the swith from the whole  cell pertussis vaccine to aP.
- The nature of the vaccine-induced protection can leave district footprints in the transient disease incidence patterns following the roll out of a vaccination program.
- The mode of the vaccine failure can determine the depth and duration of the honeymoon period, as well as the characteristics of the resurgence.
-  Alought the immune memory shapes the epidemiological dynamics, the uncertainty can be reudced by the mechanistic model.

## Vaccine effect 

The vaccine effect on the vaccine-preventable disease includes two part: Immunity against infection and immunity against transmission and disease.  

### Immunity against infection

Three, not mutually exclusive, modes by which vaccines might fail in this goal are:  

 - **Primary vaccine failure**, a vaccine exhibit primary failure if it fails to provide any form of protection against infection to some fraction of the vaccinated people. Therefore, the **primary vaccine failure** is quantified by the fraction of vaccined individuals the vaccine fails to protect.
 - **Leakiness**, a vaccine is said to be leakness when it readuced, but not eliminate the potential infection. The leakiness of a vaccine is measured by the probability of infection upon exposure for a vaccined individual relative to the same probability for an unvaccinated individual.
 - **Waning**, vaccine indduced protection is said to wane when it cease after some time. Here, we quantify the speed of waning by the mean duration of protection, or, equivalently, its reciprocal, **the rate at which immunity is lost**.

### Immunity against tramsimission and disease

Even when a vaccine fail to provide the protection against infection, it might still reduced the infections' transmissibility and or the severity of the disease sympoton. These effects can be qualityfied by:  

- **relative infectiousness**, which is model as the ratio of the tramsimission rate of a vaccinated person to that of an unvaccinated person.
- **relative reporting probability**,  the ratio of the reporting probability of a vaccinated individual relative to that of an unvaccinated individual.


## Method

 **Some statement**:   
 
-  In the absence of primary vaccine failure, vaccinated individuals whose protection against infection has failure are likely to be recorded as cases (possibly due to the vaccine-induced protection against server disease) but may be just as infectious as unvaccinated individuals.  Here, we relate our assumption of zero primary failure, considering models with modest levels of aP primary failure.
- For simplicity, infection and vaccine-derived immunity was assumed to be perfect and lifelong, althought the infection-derived immunity lasts longer than the vaccine-derived immunity.
- Transmission rate was assumed to be periodic function of time, with a period of 1 year.
- Parameters were estimated by fitting the deterministic models using the maximum likehood  estimating via the trajectory matching. The best fits from the initial runs were used to starting points for the subsequent iterations of the trajectory matching to reveal the ML estimate.
- A profile is derived by creat an array of fixed values for a parameter and maximizing the likehood at each fixed value over the remaining model parameters.
 
### Data

 - Monthly reportes for six regions from the beginning of 1996 until the end of 2009.
 - Regional demographic data: population size, annual number of live births and deaths.
 - The covaerage of aP and the vaccination schedule.

### Model

A standard Susceptible-exposed-infected-recoverd (SEIR) model with eight compartments. It includes two compartments each of susceptible (S_{i}), exposed(E_{I}) and infected (I_{i})  compartments in order to distinguish individuals who were never vaccinated (i=1) from those who vaccinated (i=2).  The vaccinated compartment contained individuals who were vaccinated and still maintain some vaccine-derived protection against infection. In others words, there are a spectrum of models, differing in certain parameters and not in others, that were all roughly equally well-supported by the data.   In others words, there are a spectrum of models, differing in certain parameters and not in others, that were all roughly equally well-supported by the data.   

## Results

- Deterministic  model do not explain the data as well as the stochastic models.
- Relative infectiousness of vaccinated infected individual is equal to that of unvaccinated ones.
- Full model has to many degress of freedom for these parameters(wanning and leakiness)
to be uniquely indentiable.  In other words, 
- In others words, there are a spectrum of models, differing in certain parameters and not in others, that were all roughly equally well-supported by the data.   
- Low relatively reporting probability can be interpreted  that the preponderance of infection in the vaccinated individuals are mild or asymptomatic, which indicated that the vaccine does benefit individuals directly by diminishing disease severity. 
 
  
