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

# Plotting the histogram of total number of steps per day
with(activity3, plot(activity3$Date, activity3$Steps, main = "Total Steps per Day", xlab = "Days", ylab = "Steps", type = "h")) # "h" for type 'histogram'
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png) 

```r
mean(activity3$Steps) # result is 10766.19
```

```
## [1] 10766.19
```

```r
median(activity3$Steps) # result is 10765
```

```
## [1] 10765
```

*The mean total number of steps taken per day is 10766.19.*

*The median for the total number of steps taken per day is 10765.*

## What is the average daily activity pattern?

### Time Series Plot for average steps per interval, averaged across all days


```r
activity4 <- aggregate(steps ~ interval, data = activity, FUN = "mean")
with(activity4, plot(activity4$interval, activity4$steps, main = "Time Series Plot", xlab = "5-minute Intervals", ylab = "Steps", type = "l"))
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

### 5-minute interval, on average across all the days in the dataset, which contains the maximum number of steps


```r
activity5 <- aggregate(steps ~ interval, data = activity, FUN = "sum")
max(activity5)
```

```
## [1] 10927
```

```r
activity5[activity5$steps == 10927,]
```

```
##     interval steps
## 104      835 10927
```

*The interval '835' contains the maximum number of steps (10927).*

## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
