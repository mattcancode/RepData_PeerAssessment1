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

There are 2304 missing values in the original data. We will create a copy of that data and fill in any missing values using the mean values for the respective intervals.


```r
newActivity <- activity
newActivity[is.na(newActivity$steps), 'steps'] <- apply(newActivity[is.na(newActivity$steps), ], 1, function(x) {
        meanStepsPerInterval[x['interval']] 
    })
```

We again create a histogram showing the total number of steps taken each day (this time with all values filled in).

```r
newStepsPerDay <- with(newActivity, tapply(steps, date, sum))
hist(newStepsPerDay, xlab = "Steps Per Day", main = "Histogram of Steps Taken Each Day", breaks = 20)
```

![](PA1_template_files/figure-html/new-steps-per-day-histogram-1.png) 

And again calculate the **mean** and **median** total number of steps taken per day.

```r
newMeanStepsPerDay <- mean(newStepsPerDay)
newMedianStepsPerDay <- median(newStepsPerDay)
```
- Mean number of steps taken per day is now 1.0766189\times 10^{4} (previously 1.0766189\times 10^{4}).
- Median number of steps taken per day is now 1.0766189\times 10^{4} (previously 10765)..

The total number of steps has increased from 570608 to 6.5673751\times 10^{5}. 
Note that the mean is unchanged, which is somewhat surprising since we added so many new rows. However, it appears that in many cases where data was missing, it was missing for the entire day so those days were completely ignored in the original calculations. And, since those new values were populated with the means from the original calculations, the means remain unchanged. (The original data only included values for 53 days, while filling in missing values expanded that number to 61.)

There was, however, a slight change to the median steps per day since adding new values will obviously shift where the middle value will be found.

## Are there differences in activity patterns between weekdays and weekends?

To analyze activity on weekends versus weekdays, we need to add a new factor.


```r
newActivity$wday <- as.factor(ifelse(weekdays(newActivity[['date']]) %in% c("Saturday", "Sunday"), "weekend", "weekday"))
```

We can now plot comparing average weekend and weekday activity for each time interval.



*Note: Still haven't figured out the plot. Hopefully you won't see this since I will have figured it out before the deadline.*
