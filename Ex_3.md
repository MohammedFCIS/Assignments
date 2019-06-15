Data Wrangling Exercise 3: Human Activity Recognition
================
Mohammed Ali; Marwa Mohamed
6/2/2019

## Overview

This data wrangling project is known to be a bit challenging since it’s
a good example of a messy, real-world data set that you’d encounter in a
data science job.

The goal of this project is to get you some practice in processing real
world datasets using the tools and techniques you have learnt so far.

You can download the dataset from
[here](https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip).
A full description of that data is available
[here](http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones)
and also in the README file included with the data.

This data set is organized in a way that makes it hard to use at first.
You will need to use several data transformation techniques to put it
into a usable, tidy state. Some extra guidelines and hints are in
associated file called `1547028719_Samsung_data_wrangling_hints.pdf` in
the data folder

## Exercise

### 0: Load the data in RStudio

Load the training and test data sets into RStudio, each in their own
data frame.

### 1: Merge data sets

Merge the training and the test sets to create one data set.

2: Mean and standard deviation

Create two new columns, containing the mean and standard deviation for
each measurement respectively. *Hint: Since some feature/column names
are repeated, you may need to use the make.names() function in R*.

### 3: Add new variables

Create variables called `ActivityLabel` and `ActivityName` that label
all observations with the corresponding activity labels and names
respectively

### 4: Create tidy data set

From the data set in step 3, creates a second, independent tidy data set
with the average of each variable for each activity and each subject.