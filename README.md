Readme
===================
This README file describes how to use the files within this repository.

The repository includes the following files:
-------------------
- 'README.md': This file

- 'CodeBook.md': Shows information about the variables used on the feature vector.

- 'cleanedData.txt': File containing intermediate "Clean" data

- 'tidyData.txt': File containing the final "Tidy" data.

- 'run_analysis.R': R file with the code to generate the output files ( 'cleanedData.txt' and 'tidyData.txt').


Detailed explanation on how to treat the data.
-------------------
First, unzip the data from https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip and rename the folder with "data".
Then, move the script "run_analysis.R" inside the folder "data", so the script "run_analysis.R" is as the same level as the folders "test" and "train".
R working directory has to be the path where the file "run_analysis.R" is located, as all paths are relative from there.

Second, use source("run_analysis.R") command.

Third, you will find two output files are generated in the current working directory:
cleanedData.txt (8.3 MB): it contains a data frame called cleanedData with 10299*68 dimension.
tidyData.txt (224 KB): it contains a data frame called result with 180*68 dimension.

Lastly, the tidy data was created with a write.table command so can be read into R with a read.table command.
You can use the following command to read the file to get the average of each variable for each activity and each subject
data <- read.table("tidyData.txt") 
There are 6 activities and 30 subjects, so there're 180 (6*30) rows containing all possible combinations for subject and activities. In this file there's also one column for each one of the 66 features.


Detailed explanation on how I processed the data.
-------------------
The steps in which I processed the data are detailed according to the source code to make it easier to follow the explanations.

## Step 1: Merges the training and the test sets to create one data set.
First of all, the test files ("subject_test.txt", "X_test.txt" and "y_test.txt") are read, and all of them are stored on a data frame.
Then, the same process is done to the training files ("subject_train.txt", "X_train.txt" and "y_train.txt").
Following, these 2 data frames ("training" -allTrains- and "testing" -allTests-) are combined to create the data set.

To give a proper name to the feature columns within the data set, the file "features.txt" is checked.

## Step 2: Extracts only the measurements on the mean and standard deviation for each measurement. 
To use only the mean and standard deviations the names of the columns are searched for.
To avoid problems the regular expression that is used has the parenthesis escaped, so the resulting expression is as follows:
"-mean\\(\\)|-std\\(\\)"

## Step 3: Uses descriptive activity names to name the activities in the data set
To convert the names to a more readable format some transformations take place:
- Names are converted to lower case
- "_" symbol is deleted
- The string "downstairs" is replaced with the UpperCase "Downstairs"
- The string "upstairs" is replaced with the UpperCase "Upstairs"
After these changes have been performed, the number identifying the activity is replaced with the corresponding string.
By doing this, the activities are easily understandable.

## Step 4: Appropriately labels the data set with descriptive variable names. 
Here some more transformations take place. In this case I removed these characters from the variable names: "-" "," "(" ")", also capitalizing "Mean" and "Std".
This data frame is combined with the data frame containing the activity information form the previous step, and also combined with the subjects information to generate a "clean" dataset

## Step 5: Creates a second, independent tidy data set with the average of each variable for each activity and each subject. 
The data from the clean data set (generated on step 4) is aggregated by activity and subject, outputting the mean grouped by these variables.
The file containing this information is the file named "tidyData.txt"