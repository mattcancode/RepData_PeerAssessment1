# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

1. First, we need to load the activity data from the csv file.

```r
activity = read.csv("activity.csv")
```

2. And we'll transform the values in the 'date' column so they are actual Dates; and the values in the 'interval' column as more user-friendly formatted factors.

```r
activity$date <- as.Date(activity$date)
activity$interval <- as.factor(activity$interval)
```

## What is mean total number of steps taken per day?

1. We start with a histogram showing the total number of steps taken each day.

```r
stepsPerDay <- with(activity[!is.na(activity$steps), ], tapply(steps, date, sum))
hist(stepsPerDay, xlab = "Steps Per Day", main = "Histogram of Steps Taken Each Day", breaks = 20)
```

![](PA1_template_files/figure-html/steps-per-day-histogram-1.png) 

2. And then calculate the **mean** and **median** total number of steps taken per day.

```r
meanStepsPerDay <- mean(stepsPerDay)
medianStepsPerDay <- median(stepsPerDay)
```
- Mean number of steps taken per day is 1.0766189\times 10^{4}.
- Median number of steps taken per day is 10765.

## What is the average daily activity pattern?

We need to calculate the average steps per interval (averaged across all days).

```r
meanStepsPerInterval <- with(activity, tapply(steps, interval, mean, na.rm = TRUE))
```

And then plot it.

```r
plot(rownames(meanStepsPerInterval), meanStepsPerInterval, type = "l", main = "Average Daily Activity Pattern", xlab = "Interval", ylab = "Average Steps Taken")
```

![](PA1_template_files/figure-html/daily-pattern-plot-1.png) 

Finally, figure out which interval had the most steps.

```r
mostActiveInterval <- meanStepsPerInterval[which.max(meanStepsPerInterval)]
```

The 5-minute interval with the maximum average number of steps is 835.

## Imputing missing values


```r
totalMissingValues = sum(is.na(activity$steps))
```

## Are there differences in activity patterns between weekdays and weekends?
