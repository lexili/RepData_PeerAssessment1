# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

```r
activity <- read.csv("~/Documents/coursera/Reproducible Research/RepData_PeerAssessment1/activity.csv")
x <- is.na(activity$steps)
activity1 <- data.frame(steps=activity$steps[!x],date=activity$date[!x],interval=activity$interval[!x])
```

## What is mean total number of steps taken per day?


```r
steps <- as.matrix(as.numeric(lapply(split(activity1$steps,activity1$date),sum)))
hist(steps)
```

![plot of chunk unnamed-chunk-2](./PA1_template_files/figure-html/unnamed-chunk-2.png) 

```r
#mean of step
stepmean = apply(steps,2,mean)
stepmean
```

```
## [1] 9354
```

```r
#median of step
apply(steps,2,quantile,probs=c(0.5))
```

```
## [1] 10395
```

## What is the average daily activity pattern?

```r
steps1 <- as.numeric(lapply(split(activity1$steps,activity1$interval),mean))
plot(steps1, type = "l")
```

![plot of chunk unnamed-chunk-3](./PA1_template_files/figure-html/unnamed-chunk-3.png) 

```r
#According to plot, the maximum number of steps belong to around 520
```

## Imputing missing values

```r
#total number of missing values in the dataset 
dim(activity)[1]-dim(activity1)[1]
```

```
## [1] 2304
```

```r
#filling in all missing values with mean
activity$steps[x]<-stepmean
# step1 is the new dataset with the missing data filled in
activity2 <- data.frame(steps=activity$steps,date=activity$date,interval=activity$interval)
steps2 <- as.matrix(as.numeric(lapply(split(activity2$steps,activity2$date),sum)))
hist(steps2)
```

![plot of chunk unnamed-chunk-4](./PA1_template_files/figure-html/unnamed-chunk-4.png) 

```r
apply(steps2,2,mean)
```

```
## [1] 362668
```

```r
apply(steps2,2,quantile,probs=c(0.5))
```

```
## [1] 11458
```

## Are there differences in activity patterns between weekdays and weekends?

```r
activity$day <- weekdays(as.Date(activity$date))
A<-as.matrix(as.numeric(lapply(split(activity$steps,activity$day),mean)))
plot(A,type="l")
```

![plot of chunk unnamed-chunk-5](./PA1_template_files/figure-html/unnamed-chunk-5.png) 
