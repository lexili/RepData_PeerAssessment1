# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

```r
activity <- read.csv("~/Documents/coursera/Reproducible Research/RepData_PeerAssessment1/activity.csv")
x<-is.na(activity$step)
activity1<-cbind(activity$step[!x],activity$date[!x],activity$interval[!x])
```

## What is mean total number of steps taken per day?


```r
steps<-as.numeric(lapply(split(activity$step,activity$date),sum))
plot(steps,type="h")
```

![plot of chunk unnamed-chunk-2](./PA1_template_files/figure-html/unnamed-chunk-2.png) 

```r
step<-as.matrix(steps[!is.na(steps)])
stepmean=apply(step,2,mean)
stepmean
```

```
## [1] 10766
```

```r
apply(step,2,quantile,probs=c(0.5))
```

```
## [1] 10765
```

## What is the average daily activity pattern?

```r
plot(activity$interval,activity$steps,type="l")
```

![plot of chunk unnamed-chunk-3](./PA1_template_files/figure-html/unnamed-chunk-3.png) 


## Imputing missing values

```r
dim(as.matrix(activity))-dim(activity1)
```

```
## [1] 2304    0
```

```r
activity$step[is.na(activity$steps)]<-stepmean
step1<-as.matrix(activity$step)
apply(step1,2,mean)
```

```
## [1] 1444
```

```r
apply(step1,2,quantile,probs=c(0.5))
```

```
## [1] 0
```

## Are there differences in activity patterns between weekdays and weekends?

```r
activity$day <- weekdays(as.Date(activity$date))
A<-split(activity$step,activity$day)
```
