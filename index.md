---
title       : Developing Data Products Asisgnment 2
subtitle    : Related to Slidify
author      : Shiv Onkar (shiv_kumar10@infosys.com)
job         : Analytics Analyst
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax, quiz, bootstrap]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Slide 2: Introduction

This projects takes data from Galton datset and build prediction model. It takes height of Father, Mother and Gender of the the child. Based on these input data the model is built using glm function. Inputs for predicting from Galton data.

R Code starts from here with description

```r
setwd("D:/Projects/Analytics/CourseEra/DataScience/09_DevelopingDataProducts/Proj")

library(shiny)
library(knitr)
library(ggplot2)

```

---

## Slide 3: Data Processing and Model building

```r
#Data Processing
Data <- read.csv("Galton.csv",  header=TRUE) # stringsAsFactors=FALSE, , na.strings = "?", dec = "."

#Creating prediction function
fit <- glm(Height ~ Mother+ Father  + Gender -1 , data = Data)

# Test Data
Mother_h <- 50
Father_h <- 60
Gender_c <- "M"
predData <- data.frame(Mother = Mother_h, Father = Father_h, Gender = Gender_c)

predResult <- predict(fit, newdata = predData)

# It shuold be equivalent to 50* 0.32150 + 60 * 0.40598 + 20.57071

predResult

```

---

## Slide 4: Preparing for Graph Data and Plot

```r
#Transforming the data to view in graph
Data_temp_F <- subset(Data, ,c(Father, Gender, Height))
Data_temp_M <- subset(Data, ,c(Mother, Gender, Height))

names(Data_temp_F)[1] <- "ParentHeight"
names(Data_temp_M)[1] <- "ParentHeight"

Data_temp_F$Parent <- factor("F",levels = c("F","M"))
Data_temp_M$Parent <- factor("M",levels = c("F","M"))
Data_Plot <- rbind(Data_temp_F, Data_temp_M)

rm(Data_temp_M)
rm(Data_temp_F)


```

---

## Plot the graph

gg <- ggplot(data=Data_Plot, aes(x= ParentHeight, y= Height)) +  geom_point( aes(color = Parent, shape = Gender)) 
gg <- gg + xlab("Parent Height") + ylab("Children Height")
gg <- gg + geom_smooth(method = "glm")
gg
