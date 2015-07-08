# Reproducible Research: Peer Assessment 1

## Introduction
Dear Peer Reviewer,

What follows is the first course assignment for the Coursera module "Reproducible Research". On occasion, I have gone slightly beyond what is required by the question; I'm aware that plagiarism is a constant worry to us all, and I hope that by adding one or two extra pieces of information I should keep it 'different'. 

Thank you for your time.

## Loading and preprocessing the data
###Loading the data
The data are stored in a zip file, located in the working directory. Our first step is to extract it.


```r
data1 <- read.csv(unz("activity.zip","activity.csv"))
```
###Processing the data
We need to convert to the correct class of R object, which we can do with:


```r
data1$steps    <- as.numeric(data1$steps)
data1$date     <- as.Date(data1$date, "%Y-%m-%d")
data1$interval <- factor(data1$interval)
```

## What is mean total number of steps taken per day?
Here I've used the dplyr package to group by date and summarize. As the data stands, we have the number of steps taken in particular intervals. We need to sum over the intervals on each date.

###Histogram


```r
library(dplyr)

data2 <- tbl_df(data1)  %>%
        group_by(date)  %>%
        summarise(total = sum(steps,na.rm=TRUE))
```

To show this graphically, I've used the ggplot2 package.


```r
library(ggplot2)

g <- ggplot(data2, aes(total))
g + geom_histogram(fill="white", colour="black") +
    labs(x = "Total number of steps taken in a day") +
    labs(y = "Number of days") +
    labs(title = "Histogram of steps taken")
```

![](PA1_template_files/figure-html/hist-1.png) 

We can see that there are a large number of days in which no activity occurs (perhaps the pedometer was sitting in a drawer for those days) but most of the time the individual concerned took between 10000 and 15000 steps.

###Mean and Median
This is most efficiently achieved with the summary() command. (I've stored the result so I can call on it in the text below.)


```r
data2sum <- summary(data2$total)
data2sum
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##       0    6778   10400    9354   12810   21190
```
We can thus see that the mean number of steps taken in a day is 9354 steps, while the median is 1.04\times 10^{4} steps. The fact that the mean is smaller reflects the large number of days with no steps taken.

## What is the average daily activity pattern?
###Time series plot
###Which interval (on average) has most steps?

## Imputing missing values
###Total missing values

###Strategy for missing values

###Imputing missing values

###Histogram, mean and median


## Are there differences in activity patterns between weekdays and weekends?

###Creating a factor for weekends and weekdays

###Plot weekends and weekdays
