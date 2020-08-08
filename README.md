# Getting_and_Cleaning_Data_Peer_Graded_Assignment
Submission for Coursera Getting and Cleaning Data Project 

Goal

The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 

1) a tidy data set as described below, 

2) a link to a Github repository with your script for performing the analysis, and 

3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. 

You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.

One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained:

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

Here are the data for the project:

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

You should create one R script called run_analysis.R that does the following.

1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement.
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names.
5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

Getting Started 

1.0 Downloading and unzipping dataset 

if(!file.exists("./data")){dir.create("./data")}

fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"

unzip(zipfile="./data/Dataset.zip",exdir="./data")

1.1 Reading the files 

Reading Training Tables 

X_train <- read.table("~/Work/R_7.6.20/getting_and_cleaning_data/PeerGradedAssignment/train/X_train.txt", quote="\"", comment.char="")

View(X_train)

y_train <- read.table("~/Work/R_7.6.20/getting_and_cleaning_data/PeerGradedAssignment/train/y_train.txt", quote="\"", comment.char="")

View(y_train)

subject_train <- read.table("~/Work/R_7.6.20/getting_and_cleaning_data/PeerGradedAssignment/train/subject_train.txt", quote="\"", comment.char="")

View(subject_train)

Reading Testing Tables 

X_test <- read.table("~/Work/R_7.6.20/getting_and_cleaning_data/PeerGradedAssignment/test/X_test.txt", quote="\"", comment.char="")

View(X_test)

y_test <- read.table("~/Work/R_7.6.20/getting_and_cleaning_data/PeerGradedAssignment/test/y_test.txt", quote="\"", comment.char="")

View(y_test)

subject_test <- read.table("~/Work/R_7.6.20/getting_and_cleaning_data/PeerGradedAssignment/test/subject_test.txt", quote="\"", comment.char="")

View(subject_test)

Reading Feature vector 

features <- read.table("~/Work/R_7.6.20/getting_and_cleaning_data/PeerGradedAssignment/features.txt", quote="\"", comment.char="")

View(features)

Reading Activity Labels 

activity_labels <- read.table("~/Work/R_7.6.20/getting_and_cleaning_data/PeerGradedAssignment/activity_labels.txt", quote="\"", comment.char="")

View(activity_labels)

Assigning Column Names

colnames(X_train) <- features[,2]

colnames(y_train) <-"activityId"

colnames(subject_train) <- "subjectId"

colnames(X_test) <- features[,2] 

colnames(y_test) <- "activityId"

colnames(subject_test) <- "subjectId"

colnames(activity_labels) <- c('activityId','activityType')

1. Merge the data sets 

mrg_train <- cbind(y_train, subject_train, X_train)

mrg_test <- cbind(y_test, subject_test, X_test)

setAllInOne <- rbind(mrg_train, mrg_test)

2. Extract only the measurements on the mean and standard deviation for each measurement.

Reading column names 

colNames <- colnames(setAllInOne)

Create vector for defining ID, mean and standard deviation:

mean_and_std <- (grepl("activityId" , colNames) | 
                   grepl("subjectId" , colNames) | 
                   grepl("mean.." , colNames) | 
                   grepl("std.." , colNames) 
)

Making nessesary subset from setAllInOne:

setForMeanAndStd <- setAllInOne[ , mean_and_std == TRUE]

3. Uses descriptive activity names to name the activities in the data set

setWithActivityNames <- merge(setForMeanAndStd, activity_labels,
                              by='activityId',
                              all.x=TRUE)
                              
4. Appropriately labels the data set with descriptive variable names.       

Done in previous steps, see above 

5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

secTidySet <- aggregate(. ~subjectId + activityId, setWithActivityNames, mean)

secTidySet <- secTidySet[order(secTidySet$subjectId, secTidySet$activityId),]

Writing second tidy data set in txt file
 
write.table(secTidySet, "secTidySet.txt", row.name=FALSE)
