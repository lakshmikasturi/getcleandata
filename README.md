# getcleandata
This repo is created for "tidying data" project assignment
Part-1 Download and read data
i. Check for data directory and create "M" directory if it doesn't exist
  if (!file.exists("M")){
    message("Creating data directory")
    dir.create("M")}
ii. Check for file and create "N" if it doesn't exist
iii. Download the data from the "source = fileurl" into the "destination = zipfile" using download.file
iv. Unzip the zipped dataset
  if (!file.exists("M/N")){
    # Download the data
    fileUrl <- "https://Dataset.zip"
    zipfile <- "c:/M/N.zip"
    message("Downloading Data")
    download.file(fileUrl, destfile = zipfile)
    unzip(zipfile, exdir = "C:/M")}
v. Read the Train and Test datasets using read.table
      train.x <- read.table("trainx.txt")
      train.y <- read.table("trainy.txt")
      train.subject <- read.table("trainsubject.txt")
vi. Merge the train and test datasets using rbind which returns 3 datasets: x,y and subject
      merge.x <- rbind(train.x, test.x)
      merge.y <- rbind(train.y, test.y)
      merge.subject <- rbind(train.subject, test.subject)
vii. Merge x, y, subject datasets using cbind
      df1 <- cbind(x,y)
      df1 <- cbind(df1,subject)
viii. Read the features file and get col numbers of mean and std measurements
      meancol <- sapply(features, grepl("mean"))
      stdcol <- sapply(features, grepl("std"))
viii. Extract only the measurements of the mean and std values for each measurement
ix. Extract the mean and std values for each measurement
    dataframe <- extract.mean and .std
x. Name activities
xi. Create a tidy dataset 
    tidydataset <- ddply(datframe, .(subject, activity), colMeans)
xii. Write the tidy dataset to text file
      write.table(tidy.txt, destination, row.names = FALSE)
