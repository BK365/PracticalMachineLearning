Machine Learning
========================================================

This is a machine learning algorithim to predict which class of exercise an individual is performing based on measurements from sensors located on variour parts of the arm.

**Preprocessing**

```r
pmltraining <- read.csv("pml-training.csv")  ##import the training set
pmltesting <- read.csv("pml-testing.csv")  ##import the testing set

## Subset both the training and testing set to select only complete columns
## and excluding columns with no variability
pmltraining <- subset(pmltraining, select = c(7:11, 37:49, 60:68, 84:86, 102, 
    113:124, 140, 151:160))
pmltesting <- subset(pmltesting, select = c(7:11, 37:49, 60:68, 84:86, 102, 
    113:124, 140, 151:160))
```



```r
library(randomForest)
```

```
## randomForest 4.6-7
## Type rfNews() to see new features/changes/bug fixes.
```

```r

Modrf = randomForest(factor(classe) ~ ., data = pmltraining, ntree = 200, nodesize = 1, 
    PROX = TRUE, oob_score = TRUE)  ##random forest
```


**Table of counts for each category A-E:**

```r
table(predict(Modrf, pmltesting))
```

```
## 
## A B C D E 
## 7 8 1 1 3
```


**Category for each row:**

```r
PredTable <- data.frame(predict(Modrf, pmltesting[1:20, ]))
names(PredTable) <- "classe"
```



```r
PredTable
```

```
##    classe
## 1       B
## 2       A
## 3       B
## 4       A
## 5       A
## 6       E
## 7       D
## 8       B
## 9       A
## 10      A
## 11      B
## 12      C
## 13      B
## 14      A
## 15      E
## 16      E
## 17      A
## 18      B
## 19      B
## 20      B
```


**Error Rate by Number of Trees**

```r
plot(Modrf)
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6.png) 

