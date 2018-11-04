setwd("F:/R-3.5.1/coursera")
#1. read and merge data
## step1:download the zip file into "data"

if(!file.exists("./data")) dir.create("./data")
fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileUrl, destfile = "./data/projectData_getCleanData.zip")

## step2:unzip data
listZip <- unzip("./data/projectData_getCleanData.zip", exdir = "./data")

## step3:read data
train.x <- read.table("./data/UCI HAR Dataset/train/X_train.txt")
train.y <- read.table("./data/UCI HAR Dataset/train/y_train.txt")
train.subject <- read.table("./data/UCI HAR Dataset/train/subject_train.txt")
test.x <- read.table("./data/UCI HAR Dataset/test/X_test.txt")
test.y <- read.table("./data/UCI HAR Dataset/test/y_test.txt")
test.subject <- read.table("./data/UCI HAR Dataset/test/subject_test.txt")

## step4:merge training and test data
testdata <- cbind(test.x, test.y, test.subject)
traindata <- cbind(train.x, train.y, train.subject)
fulldata <- rbind(traindata, testdata)

#-------------------------------------------------------------------------
#2.Extract only the measurements on the mean and standard deviation for each measurement.

## step1:read feature name
featureName<-read.table("./data/UCI HAR Dataset/features.txt", stringsAsFactors=FALSE)[,2]
## step2: extract mean and standard deviation of each measurements
featureIndex <- grep(("mean\\(\\)|std\\(\\)"), featureName)
finalData <- fulldata[, c(1, 2, featureIndex+2)]
colnames(finalData) <- c("subject", "activity", featureName[featureIndex])

#--------------------------------------------------------------------------------
# 3. Uses descriptive activity names to name the activities in the data set

## step1: load activity data into R
activityName <- read.table("./data/UCI HAR Dataset/activity_labels.txt")

## step2: replace 1 to 6 with activity names
finalData$activity <- factor(finalData$activity, levels = activityName[,1], labels = activityName[,2])

#-------------------------------------------------------------------------------
# 4. Appropriately labels the data set with descriptive variable names.

names(finalData) <- gsub("\\()", "", names(finalData))
names(finalData) <- gsub("^t", "time", names(finalData))
names(finalData) <- gsub("^f", "frequence", names(finalData))
names(finalData) <- gsub("-mean", "Mean", names(finalData))
names(finalData) <- gsub("-std", "Std", names(finalData))

#-------------------------------------------------------------------------------
# 5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.
library(dplyr)
groupData <- finalData %>%
  group_by(subject, activity) %>%
  summarise_each(funs(mean))

write.table(groupData, "./Data/MeanData.txt", row.names = FALSE)
