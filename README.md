# Assignment11

---
title: "Assignment 11 Markdown File"
author: "Jonathan"
date: "March 30, 2017"
output: html_document
---

# SNP 500

This document is an HTM file generated by the markdown file availible at the following link: <https://github.com/Jonathan-Knowles/Assignment11>.

## Install required packages and load them.

### Install Packages

```{r InstallPackages, echo=TRUE}
install.packages("tseries", repos='http://cran.us.r-project.org')
```

### Load Packages

```{r LoadPackages, echo=TRUE}
library(tseries)
library(tseries)
```

## Gather Data

```{r EnterData, echo=TRUE}
SNPdata <- get.hist.quote('^gspc',quote="Close")
```


## Calculate the Log returns.

```{r CalculateLogReturns, echo=TRUE}
SNPret <- log(lag(SNPdata)) - log(SNPdata)
```

## Calculate the volatility measure.


```{r CalculateVolatilityMeasure, echo=TRUE}
SNPvol <- sd(SNPret) * sqrt(250) * 100
```

## Create function determining volatility.


```{r CalculateVolatilityMeasure3factors, echo=TRUE}
getVol <- function(d, logrets) {
  var = 0
  lam = 0
  varlist <- c()
  
  for (r in logrets) {
    lam = lam*(1 - 1/d) + 1
    var = (1 - 1/lam)*var + (1/lam)*r^2
    varlist <- c(varlist, var)
  }
  
  sqrt(varlist)
}
```


## Calculate the volatility measure of the entire series using 3 different decay factors.

### Factor of 10
```{r CalculateVolatilityMeasureFactorOf10, echo=TRUE}
volest <- getVol(10, SNPret)
```


### Factor of 30
```{r CalculateVolatilityMeasureFactorOf30, echo=TRUE}
volest2 <- getVol(30, SNPret)
```

### Factor of 100
```{r CalculateVolatilityMeasureFactorOf100, echo=TRUE}
volest3 <- getVol(100, SNPret)
```


## Plotting the data.

```{r Plot, echo=TRUE}
plot(volest,type="l")
lines(volest2, type="l",col="red")
lines(volest3, type="l",col="blue")
print.eval = TRUE
```