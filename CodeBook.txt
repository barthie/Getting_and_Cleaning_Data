Getting and Cleaning Data: Course Project

The tidyData.txt file in this repository was created to satisfy the requirements of the course project for the Coursera Getting and Cleaning Data course.

The data was downloaded from the link provided in the assignment description (https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip). The file was unzipped to the local drive and stored in C:\Downloads\Coursera\Cleaning Data\getdata-projectfiles-UCI HAR Dataset\UCI HAR Dataset. The files in the Inertial Signals folders (located in the test and train subfolders) were not needed for the assignment. The train and test folders each contained three files. The files were text files that contained the subject codes (30 subjects, numbered accordingly), the activity codes and the data. The descriptions for the activity codes were found in the activity_labels.txt file.

Each of the three files were read into R, along with the activity labels and the features.txt file, which contained the labels for the data fields. The activity labels were matched to the activity codes in the corresponding tables. All fields were labeled. The three files were then combined. The subjects in the first column, the activity in the second and the 561 data fields next. This was done for the train and test data. These two sets were then row bound to create one complete data set.

With this data set, the field names were searched to find those that contained the mean and standard deviations of each measure. This was done using the grep function in R. These columns, along with the subject and activity fields were extracted from the previously created data set. The data was then aggregated to find the mean of each measure for the individual subjects and activities.

The remaining tidy data set (appropriately named tidyData.txt) contains 68 columns and 180 rows (30 subjects times six activities). 

The columns are named as follows:

[1]	subject
[2]	activity
[3]	tBodyAcc-mean()-X
[4]	tBodyAcc-mean()-Y
[5]	tBodyAcc-mean()-Z
[6]	tGravityAcc-mean()-X
[7]	tGravityAcc-mean()-Y
[8]	tGravityAcc-mean()-Z
[9]	tBodyAccJerk-mean()-X
[10]	tBodyAccJerk-mean()-Y
[11]	tBodyAccJerk-mean()-Z
[12]	tBodyGyro-mean()-X
[13]	tBodyGyro-mean()-Y
[14]	tBodyGyro-mean()-Z
[15]	tBodyGyroJerk-mean()-X
[16]	tBodyGyroJerk-mean()-Y
[17]	tBodyGyroJerk-mean()-Z
[18]	tBodyAccMag-mean()
[19]	tGravityAccMag-mean()
[20]	tBodyAccJerkMag-mean()
[21]	tBodyGyroMag-mean()
[22]	tBodyGyroJerkMag-mean()
[23]	fBodyAcc-mean()-X
[24]	fBodyAcc-mean()-Y
[25]	fBodyAcc-mean()-Z
[26]	fBodyAccJerk-mean()-X
[27]	fBodyAccJerk-mean()-Y
[28]	fBodyAccJerk-mean()-Z
[29]	fBodyGyro-mean()-X
[30]	fBodyGyro-mean()-Y
[31]	fBodyGyro-mean()-Z
[32]	fBodyAccMag-mean()
[33]	fBodyBodyAccJerkMag-mean()
[34]	fBodyBodyGyroMag-mean()
[35]	fBodyBodyGyroJerkMag-mean()
[36]	tBodyAcc-std()-X
[37]	tBodyAcc-std()-Y
[38]	tBodyAcc-std()-Z
[39]	tGravityAcc-std()-X
[40]	tGravityAcc-std()-Y
[41]	tGravityAcc-std()-Z
[42]	tBodyAccJerk-std()-X
[43]	tBodyAccJerk-std()-Y
[44]	tBodyAccJerk-std()-Z
[45]	tBodyGyro-std()-X
[46]	tBodyGyro-std()-Y
[47]	tBodyGyro-std()-Z
[48]	tBodyGyroJerk-std()-X
[49]	tBodyGyroJerk-std()-Y
[50]	tBodyGyroJerk-std()-Z
[51]	tBodyAccMag-std()
[52]	tGravityAccMag-std()
[53]	tBodyAccJerkMag-std()
[54]	tBodyGyroMag-std()
[55]	tBodyGyroJerkMag-std()
[56]	fBodyAcc-std()-X
[57]	fBodyAcc-std()-Y
[58]	fBodyAcc-std()-Z
[59]	fBodyAccJerk-std()-X
[60]	fBodyAccJerk-std()-Y
[61]	fBodyAccJerk-std()-Z
[62]	fBodyGyro-std()-X
[63]	fBodyGyro-std()-Y
[64]	fBodyGyro-std()-Z
[65]	fBodyAccMag-std()
[66]	fBodyBodyAccJerkMag-std()
[67]	fBodyBodyGyroMag-std()
[68]	fBodyBodyGyroJerkMag-std()


The fully commented code used to read in all raw data, perform transformations and create the final tidyData file can be found in run_analysis.R in this repository and is also included below.

## Getting and Cleaning Data : Course Project

## Path of the data downloaded from 
## https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 
## to be used for the project.
filePath='C:\\Downloads\\Coursera\\Cleaning Data\\getdata-projectfiles-UCI HAR Dataset\\UCI HAR Dataset'
setwd(filePath)

## Read in the test and train data
data_X_test=read.table(paste(getwd(),'/test/X_test.txt',sep=''),header=FALSE)
data_X_train=read.table(paste(getwd(),'/train/X_train.txt',sep=''),header=FALSE)

## Read in the y variables, which are the activity factors
data_y_test=read.table(paste(getwd(),'/test/y_test.txt',sep=''),header=FALSE)
data_y_train=read.table(paste(getwd(),'/train/y_train.txt',sep=''),header=FALSE)

## Read in the subject files for the test and train data
data_sub_test=read.table(paste(getwd(),'/test/subject_test.txt',sep=''),header=FALSE)
data_sub_train=read.table(paste(getwd(),'/train/subject_train.txt',sep=''),header=FALSE)

## Read in the features, to be used as the column names and the 
## activity labels
features = read.table('features.txt')
activity_labels=read.table('activity_labels.txt')

## Add a column name to the subject frames
names(data_sub_test)='subject'
names(data_sub_train)='subject'

## Add a column to the activity list containing the descriptions
## of the factors
data_y_test <- cbind(data_y_test,factor(data_y_test[,1],labels=activity_labels[,2]))
data_y_train <- cbind(data_y_train,factor(data_y_train[,1],labels=activity_labels[,2]))

## label the columns in the above frames
names(data_y_test)=c('actnum','activity')
names(data_y_train)=c('actnum','activity')

## Label the columns in the data frames with the labels from
## the features file
names(data_X_test)=features[,2]
names(data_X_train)=features[,2]

## Bind together subject list, activity list and the data
## for test and train
dataTrain<-cbind(data_sub_train, data_y_train, data_X_train)
dataTest<-cbind(data_sub_test, data_y_test, data_X_test)

## Bind the train and test data
data<-rbind(dataTrain,dataTest)

## Find the columns with mean() and std() in their labels.
## These will be combined with columns 1 and two to extract 
## the columns needed for the assignment
meanCols<-grep('mean()',names(data),fixed=TRUE)
stdCols<-grep('std()',names(data),fixed=TRUE)
cols<-c(1,3,meanCols,stdCols)

## Extract the data to create the tidyData set
data<-(data[,cols])

## Aggregate the data as needed
tidyData<-aggregate(data,list(data$subject,data$activity),mean)

## we end up with two additional columns - a duplicate of the
## subject and the numbers from the activities. these are
## cleaned from the tidyData set
tidyData<-tidyData[,c(1,2,5:70)]

## rename the two columns that were aggregated over
names(tidyData)<-c('subject','activity',names(tidyData)[3:68])

## write out the tidyData set
write.table(tidyData,file='tidyData.txt',row.names=FALSE)





