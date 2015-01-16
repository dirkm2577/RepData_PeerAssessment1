# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

```r
activity <- read.csv("~/Documents/Coursera/Data Science/Reproducible Research/Course Project 1/activity.csv", header = TRUE, sep = ",")
activity$date <- strptime(activity$date, "%Y-%m-%d")
```


## What is mean total number of steps taken per day?


```r
activity2 <- data.frame("Date" = activity$date, "Steps" = activity$steps)
activity3 <- aggregate(Steps ~ Date, data = activity2, FUN = "sum")
with(activity3, plot(activity3$Date, activity3$Steps, main = "Total Steps per Day", xlab = "Days", ylab = "Steps", type = "h")) # "h" for type 'histogram'
with(subset(activity3), lines(mean(activity3$Steps), y = NULL, type = "l"))
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png) 

```r
mean(activity3$Steps) # result is 10766.19
```

```
## [1] 10766.19
```


## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
