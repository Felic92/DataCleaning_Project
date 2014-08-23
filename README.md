Getting And Cleaning Data Project
======

Original Study Overview
------------
The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been aggregated into a dataset with means of data only.

#####Investigators
Jorge L. Reyes-Ortiz, Davide Anguita, Alessandro Ghio, Luca Oneto

Smartlab - Non Linear Complex Systems Laboratory

DITEN - Università degli Studi di Genova

Via Opera Pia 11A, I-16145, Genoa, Italy

activityrecognition@smartlab.ws 

www.smartlab.ws 


Project Overview
-----

The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.  

One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. 

[Here are the data for the project:](https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip)

You should create one R script called run_analysis.R that does the following. 

* Merges the training and the test sets to create one data set.
* Extracts only the measurements on the mean and standard deviation for each measurement. 
* Uses descriptive activity names to name the activities in the data set
* Appropriately labels the data set with descriptive variable names. 
* Creates a second, independent tidy data set with the average of each variable for each activity and each subject. 

Notes on what to do before running the script
--------------

For this assignment I have prepared only one script the required "run_analysis.R". For this script to run properly 
and transforms the raw dataset into the desired tidy dataset few steps must be taken before actually executing the 
script. These steps will be outlined below:

* Download the data using the following link. [Click here for data!](https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip)
    * While I realize that it would be advantageous for the script to automate all the required data acquisition I was not able to get this step automated properly so I have left it to the user. 
    
* Extract the data into a directory names "phoneData" and ensure this directory is in your current working directory
* Download the "run_analysis.R" script from Github and ensure it is also in your current working directory
* Run the script if using R Studio `source("run_analysis.R")` will do this. 
* ???
* Profit

Once the script has finished running you can use the following command to read the tidyDataset back into R:

`tidyData <- read.table("./tidyDataset.txt",header = TRUE)`

Notes on Implementation Decisions 
-----------------

An interesting component of projects like this one is that there exist so many different ways of reaching the desired outcome for this reason I thought it would be appropriate to breifly outline some decisions I personally made in regards to this project and why I chose them opposed to other courses of action. 

I will use the requirements of the script to discuss decisions I made in regards to each of the 5 steps even though my script doesn't exactaly perform each of the steps in a linear order, but rather I performs certain actions when I felt it was most convenient to do so. 

* Merges the training and the test sets to create one data set.
 * This was fairly straight forward I used `read.table("filename")` to load data from each file into dataframes in R
 * Once in R using `dim()` it was easy to see which dataframes could be fit together, `rbind(testData,trainData)` then `cbind(activityData,subjectData,MeasurementData)`, to yield the complete dataset
 
 * Extracts only the measurements on the mean and standard deviation for each measurement.
    * This was a simple exercise in subsetting data I found it advantegous to use `grep()` here to get features which contained the desired patterns in their names: `sensor_data <- sensor_data[,grep("mean|std",colnames(sensor_data))]`
    
* Uses descriptive activity names to name the activities in the data set
    * This was also an exercise in subsetting and while I originally had a line for each of the 6 replacements, which is commented out at the bottom of my script I chose to replace this with a loop to replace numerical activity codes with their textual descriptions
    
* Appropriately labels the data set with descriptive variable names. 
    * I want to note that I added the feature names to the measurement data before cbinding it all together because I find it easier to work with the data in chunks opposed to combining it all then subsetting the parts to want when this is avoidable. 
    * Following the suggestions of the Week 3 lecture on working with textual data I used `gsub()` to replace many patterns in the variable names to make them more readable in my opinion. 
    * I chose to eliminate "_" and "()" sybols because I felt they contributed nothing to reability
    * I chose to replace multiple abbreviations such as "Acc", "Freq", and "Gyro" with their unabbreviaed names "Acceleration","Frequency","Gyroscope". While I acknowledge this lengthens names I believe this enhances readability.
    * I chose to replace the "X","Y", and "Z" symbols with "Xaxis","Yaxis", and "Zaxis"
    * I chose to utilize camelCase as a variable naming convention because while there are many differing opinions on how this contributes to readability and usability with R I personally benefit from this convention. 
    
* Creates a second, independent tidy data set with the average of each variable for each activity and each subject. 
    * I originally tried uzing `melt()` and `ddply()` but wasn't successful I ended up using `aggregate()` to create this second data set using a process inspired from the following source: http://www.statmethods.net/management/aggregate.html 
    * By subsetting the data to only include measurement columns before passing it to aggregate I avoided the introduction of 2 columns which would have been the aggregations of the two non measurement features `subjectId` and `activityLabel`. Aggregate did however change the names of these columes I was able to use the `library(plyr)` and one line of code to fix this however.:
    *`tidyDataset <- rename(tidyDataset, c("Group.1"="subjectId", "Group.2"="activityLabel"))`
    
    
Additional Notes
==============

* In the script I periodically utilized `rm()` to free up memory being taken up by variables which were no longer going to be used
