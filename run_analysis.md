Title: run_analysis.R
Description: This is the code for analysing raw data and creating tidy data.
Author: Lakshmi Kasturi
Please copy this code in your Rstudio Browser and save it as run_analysis.R and execute it.

run_analysis <- function(){
  # Checks for data directory and creates data1 directory if it doesn't exist
  if (!file.exists("C:/data1")){
    message("Creating data directory")
    dir.create("C:/data1")
  }
  if (!file.exists("C:/data1/UCI HAR DATA SET")){
    # Download the data
    fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
    zipfile <- "c:/data1/UCI_HAR_data.zip"
    message("Downloading Data")
    download.file(fileUrl, destfile = zipfile)
    unzip(zipfile, exdir = "C:/data1")
  }
  # Read data from train and test datasets
  message("This part of the code first reads the data from the train and test datasets")
  message("Reading X_train.txt")
  training.x <- read.table("C:/data1/UCI HAR Dataset/train/X_train.txt")
  message("Reading y_train.txt")
  training.y <- read.table("C:/data1/UCI HAR Dataset/train/y_train.txt")
  message("Reading subject_train.txt")
  training.subject <- read.table("C:/data1/UCI HAR Dataset/train/subject_train.txt")
  message("Reading X_test.txt")
  test.x <- read.table("C:/data1/UCI HAR Dataset/test/X_test.txt")
  message("Reading y_test.txt")
  test.y <- read.table("C:/data1/UCI HAR Dataset/test/y_test.txt")
  message("Reading subject_test.txt")
  test.subject <- read.table("C:/data1/UCI HAR Dataset/test/subject_test.txt")
  # Merge train and test data sets
  message("Merge the train and test datasets")
  merge1.x <- rbind(training.x, test.x)
  merge1.y <- rbind(training.y, test.y)
  merge1.subject <- rbind(training.subject, test.subject)
  # Column bind into a single dataset called dataframe1
  message("Column bind the x, y, subject datasets into a single dataset")
  dataframe1 <- cbind(merge1.x, merge1.y)
  dataframe1 <- cbind(dataframe1, merge1.subject)
  # Read the features file
  message("Read features.txt file")
  features <- read.table("C:/data1/UCI HAR Dataset/features.txt")
  message("Now get the mean and std column numbers from features.txt")
  mean.col <- sapply(features[,2], function(x) grepl("mean()", x, fixed = TRUE))
  std.col <- sapply(features[,2], function(x) grepl("std()", x, fixed = TRUE))
  # Extract mean and std values from dataframe1 based on col names from fetures.txt data
  message("Extract the mean & std values from dataframe1 Using col nos from mean.col and std.col")
  edf <- dataframe1[, (mean.col|std.col)]
  # Set variable name for the activity name and person performing the activity
  colnames(edf) <- features[(mean.col|std.col), 2]
  colnames(edf)[67] <- "activity.name"
  colnames(edf)[68] <- "person.id"
  # Assign descriptive names to the activities
  message("Giving descriptive activity names to the activities in the dataset")
  edf$activity.name[edf$activity.name == 1] <-"WALKING"
  edf$activity.name[edf$activity.name == 2] <-"WALKING_UPSTAIRS"
  edf$activity.name[edf$activity.name == 3] <-"WALKING_DOWNSTAIRS"
  edf$activity.name[edf$activity.name == 4] <-"SITTING"
  edf$activity.name[edf$activity.name == 5] <-"STANDING"
  edf$activity.name[edf$activity.name == 6] <-"LAYING"
  # Convert all variable names to lower case and remove special characters
  names(edf) <- tolower(names(edf))
  names(edf) <- gsub("-", "", names(edf))
  names(edf) <- sub("[(][)]", "", names(edf))
  # Find the colmeans for each activity performed by each person and store the tidy data
  message("Calculating column means and tidying data")
  tidy.dataframe <- ddply(edf, .(person.id, activity.name), function(x) colMeans(edf[, 1:66]))
  # Write the tidy dataset to a text file
  message("Writing tidy dataset to a text file")
  write.table(tidy.dataframe, "C:/data1/UCR_HAR_tidy.txt", row.names = FALSE)
}
  
