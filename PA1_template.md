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
# Plotting the Time Series Plot
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

## 5. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)


```r
sum(is.na(activity$steps) == TRUE)
```

```
## [1] 2304
```

*The total number of rows with NAs is 2304.*

## 6. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

*To make the calculation for the complete dataset more accurate, I will replace the NAs with the corresponding mean for the particular interval.*

## 7. Create a new dataset that is equal to the original dataset but with the missing data filled in.


```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
## 
## The following object is masked from 'package:stats':
## 
##     filter
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
activity_imp <- merge(activity, activity4, by = "interval")
activity_imp$steps <- activity_imp$steps.x
my_na <- is.na(activity_imp$steps.x)
activity_imp$steps[my_na] <- activity_imp$steps.y[my_na]
activity_imp <- select(activity_imp, date, steps, interval, -c(steps.x, steps.y))
```

## 8. Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?


```r
new_imp <- data.frame("Date" = activity_imp$date, "Steps" = activity_imp$steps)
new_steps_day <- aggregate(Steps ~ Date, data = new_imp, FUN = "sum")
with(new_steps_day, plot(new_steps_day$Date, new_steps_day$Steps, main = "Total Steps per Day", xlab = "Days", ylab = "Steps", type = "h"))
```

![](PA1_template_files/figure-html/unnamed-chunk-7-1.png) 


```r
mean(new_steps_day$Steps)
```

```
## [1] 10766.19
```

```r
median(new_steps_day$Steps)
```

```
## [1] 10766.19
```

*The mean for the total steps per day has not changed after imputing the NA values, just the median has changed and is now equal to the mean.*

## Are there differences in activity patterns between weekdays and weekends?
