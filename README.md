```{R}

#Script for the analysis of the project data 

library(stringr)
library(dplyr)

# Import X, Y and subject for test
x_test <- read.delim("test/X_test.txt", header = FALSE, sep="")
y_test <- read.delim("test/y_test.txt", header = FALSE, sep="")
subject_test <- read.delim("test/subject_test.txt", header = FALSE, sep="")

# Import X, Y and subject for training
x_train <- read.delim("train/X_train.txt", header = FALSE, sep="")
y_train <- read.delim("train/y_train.txt", header = FALSE, sep="")
subject_train <- read.delim("train/subject_train.txt", header = FALSE, sep="")

# Labeling dataset with descriptive variable names
# get names
features <- read.delim("features.txt", header = FALSE, sep="")
feat_names <- features[,2]

# assign features names to columns in data
names(x_test) <- feat_names
names(x_train) <- feat_names

# merge X, Y and subject
test <- cbind(y_test,subject_test, x_test)
train <- cbind(y_train, subject_train, x_train)

# merge train and test and rename first two columns
mergedData <- rbind(test,train)
names(mergedData)[1] <- "activity"
names(mergedData)[2] <- "subject"
# Task 1 and 4 are completed

# Import activity_labels, features
act_labels <- read.delim("activity_labels.txt", header = FALSE, sep="")
features <- read.delim("features.txt", header = FALSE, sep="")

# replace activity numbers by activity labels
mergedData[,1] <- act_labels[match(mergedData[,1],act_labels[,1]),2]
# Task 3 is completed

# get list of names to check for mean() and std()
mergedData.names <- names(mergedData)

# list columns with name containing "mean(" or "std("
# reduces dataset to variables with mean and std
mergedData <- mergedData[,c((1:2),grep("(std|mean)\\(", mergedData.names))]
# Task 2 is completed

# organize data by activity, subject and the mean of the variables
averages <- mergedData %>% group_by(activity, subject) %>% summarize_all(funs(mean))
# Task 5 is completed

write.csv(averages,file="tidy_data.csv")


