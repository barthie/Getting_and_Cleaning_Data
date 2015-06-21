The fully commented code used to read in all raw data, perform transformations and create the final tidyData file can be found in run_analysis.R in this repository and is also included below. This is the only script necessary to create the final dataset.





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




