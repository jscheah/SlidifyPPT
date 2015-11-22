---
title       : Developing Data Products Shiny App Presentation
subtitle    : Building a prediction algorithm for BodyFat Percentage
author      : Joune Seng Cheah
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
---

## OVERVIEW

- The Application aims to predict a person's % body weight based on certain input parameters.
- The bodyfat data used to build the prediction algorithm can be found in the mfp package.
- Description of the data can be found at http://www.inside-r.org/node/65917

--- 

## Preprocessing the data

The following libraries were used:

```r
library(caret)
library(rpart)
library(randomForest)
library(mfp)
library(rpart.plot)
```

Retain only the columns of interest for prediction

```r
data(bodyfat)
keepColumns<- c("siri","age","weight","height","abdomen")
dataFat<-bodyfat[keepColumns]
set.seed(1000)
siri <- dataFat$siri
```

---

## Data Modeling
Here we will parition the training set into a pure training data set (80%) and a validation data set(20%).


```r
inTrain <- createDataPartition(y=dataFat$siri, p=0.90, list=FALSE)
myTraining <- dataFat[inTrain, ]; 
myTesting <- dataFat[-inTrain, ]
```

We fit a predictive model using Random Forest algorithm.

```r
modFitRandomforest <- train(siri ~ ., data=myTraining, method="rf", ntree=250)
```

---

## Prediction & Performace

```r
predictionsRandomforest <- predict(modFitRandomforest, myTesting)
accuracy <- postResample(predictionsRandomforest, myTesting$siri)
accuracy
```

```
##      RMSE  Rsquared 
## 4.3569097 0.7453536
```

---

## Conclusions

- The App provides a simple estimate of one's body fat percentage based on one's age, weight, height and abdomen circumfrance.
- The model only explains ~75% of the variability, hence its accuracy is limited to a certain extend. More data needs to be collected to improve the accuracy of the model
- The participants of the data project were male and this would most likely not work female participants.
