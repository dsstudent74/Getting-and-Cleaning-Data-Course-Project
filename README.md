#Looks for a folder and creates the folder if needed; sets the working directory, downloads the data from the URL and retrieves the download date.

if(!file.exists("C:/Users/Peter/Coursera Data Science/JH data science/cleaning data/final project data")) {
   dir.create("C:/Users/Peter/Coursera Data Science/JH data science/cleaning data/final project data")
}
setwd("C:/Users/Peter/Coursera Data Science/JH data science/cleaning data/final project data")
getwd()
list.files(getwd())
library(downloader)
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download(url, dest="dataset.zip", mode="wb") 
list.files(getwd())
dateDownloaded <- date()
dateDownloaded

#Unpacks the data and changes the working directory to the data location, then reads the necessary files.

unzip("dataset.zip")
list.files(getwd())
list.files("UCI HAR Dataset")
setwd("C:/Users/Peter/Coursera Data Science/JH data science/cleaning data/final project data/UCI HAR Dataset")
list.files(getwd())
list.files("./test")
X_test_to_merge <- read.table("./test/X_test.txt")
y_test_to_merge <- read.table("./test/y_test.txt")
subject_test_to_merge <- read.table("./test/subject_test.txt")
list.files("./train")
X_train_to_merge <- read.table("./train/X_train.txt")
y_train_to_merge <- read.table("./train/y_train.txt")
subject_train_to_merge <- read.table("./train/subject_train.txt")
features <- read.table("./features.txt")
activity_labels <- read.table("./activity_labels.txt")
colnames(X_test_to_merge) <- features[,2]
colnames(y_test_to_merge) <- "Activity_ID"
colnames(subject_test_to_merge) <- "Subject_ID"
colnames(X_train_to_merge) <- features[,2]
colnames(y_train_to_merge) <- "Activity_ID"
colnames(subject_train_to_merge) <- "Subject_ID"
colnames(activity_labels) <- c("Activity_ID", "Activity")

#Merges the training and the test sets to create one data set.

Test_set_merged <- cbind(subject_test_to_merge, y_test_to_merge, X_test_to_merge)
Training_set_merged <- cbind(subject_train_to_merge, y_train_to_merge, X_train_to_merge)
Final_Data_Set <- rbind(Test_set_merged, Training_set_merged)

#Extracts only the measurements on the mean and standard deviation for each measurement.

Column_Identifiers <- colnames(Final_Data_Set)
Find_Subject_Activity_Mean_STD <- (grepl("Subject_ID", Column_Identifiers) | grepl("Activity_ID", Column_Identifiers) | grepl("mean...", Column_Identifiers) | grepl("std...", Column_Identifiers))
Subset_Subject_Activity_Mean_STD <- Final_Data_Set[ , Find_Subject_Activity_Mean_STD == TRUE]

#Appropriately labels the data set with descriptive variable names.

Descriptive_Names_Data_Set <- merge(Subset_Subject_Activity_Mean_STD, activity_labels, by = "Activity_ID")
names(Descriptive_Names_Data_Set) <- gsub("\\()", "_", names(Descriptive_Names_Data_Set))
names(Descriptive_Names_Data_Set) <- gsub("^t", "Time_", names(Descriptive_Names_Data_Set))
names(Descriptive_Names_Data_Set) <- gsub("^f", "Frequency_", names(Descriptive_Names_Data_Set))
names(Descriptive_Names_Data_Set) <- gsub("-mean", "_Mean_", names(Descriptive_Names_Data_Set))
names(Descriptive_Names_Data_Set) <- gsub("-std", "_Standard_Deviation_", names(Descriptive_Names_Data_Set))

#Creates a second, independent tidy data set with the average of each variable for each activity and each subject.

library(dplyr)
Tidy_Data <- Descriptive_Names_Data_Set %>%
        group_by(Subject_ID, Activity) %>%
        summarise_each(funs(mean))

write.table(Tidy_Data, "./Tidy_Data.txt", row.names = FALSE)
list.files(getwd())