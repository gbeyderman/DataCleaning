Cleaning Data Course Project Codebook
========================================================
The script begins by reading from the relevant datasets: the activity labels, both train and test data sets, subject maps for both test and train, activity maps for both test and train, feature list mapping dataset columns to variable names.

- 'activity_labels.txt': Links the class labels with their activity name. 6 observations.
- 'train/X_train.txt': Training set. 7352 observations of 561 variables
- 'train/y_train.txt': Training labels. 7352 observations mapping activity level to observation.
- 'test/X_test.txt': Test set. 2947 observations of 561 variables
- 'test/y_test.txt': Test labels. 2947 observations mapping activity level to observation.
- 'features.txt': List of all features. A 561-feature vector with time and frequency domain variables. 
- 'train/subject_train.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 

The column names are loaded from the features dataset. Each dataset is augmented with the 561 variable names as column names.

Both training and test datasets are filtered for the variables containing mean() and std(). 66 variables are left:
> features[myvars,2]
 [1] tBodyAcc-mean()-X           tBodyAcc-mean()-Y           tBodyAcc-mean()-Z          
 [4] tBodyAcc-std()-X            tBodyAcc-std()-Y            tBodyAcc-std()-Z           
 [7] tGravityAcc-mean()-X        tGravityAcc-mean()-Y        tGravityAcc-mean()-Z       
[10] tGravityAcc-std()-X         tGravityAcc-std()-Y         tGravityAcc-std()-Z        
[13] tBodyAccJerk-mean()-X       tBodyAccJerk-mean()-Y       tBodyAccJerk-mean()-Z      
[16] tBodyAccJerk-std()-X        tBodyAccJerk-std()-Y        tBodyAccJerk-std()-Z       
[19] tBodyGyro-mean()-X          tBodyGyro-mean()-Y          tBodyGyro-mean()-Z         
[22] tBodyGyro-std()-X           tBodyGyro-std()-Y           tBodyGyro-std()-Z          
[25] tBodyGyroJerk-mean()-X      tBodyGyroJerk-mean()-Y      tBodyGyroJerk-mean()-Z     
[28] tBodyGyroJerk-std()-X       tBodyGyroJerk-std()-Y       tBodyGyroJerk-std()-Z      
[31] tBodyAccMag-mean()          tBodyAccMag-std()           tGravityAccMag-mean()      
[34] tGravityAccMag-std()        tBodyAccJerkMag-mean()      tBodyAccJerkMag-std()      
[37] tBodyGyroMag-mean()         tBodyGyroMag-std()          tBodyGyroJerkMag-mean()    
[40] tBodyGyroJerkMag-std()      fBodyAcc-mean()-X           fBodyAcc-mean()-Y          
[43] fBodyAcc-mean()-Z           fBodyAcc-std()-X            fBodyAcc-std()-Y           
[46] fBodyAcc-std()-Z            fBodyAccJerk-mean()-X       fBodyAccJerk-mean()-Y      
[49] fBodyAccJerk-mean()-Z       fBodyAccJerk-std()-X        fBodyAccJerk-std()-Y       
[52] fBodyAccJerk-std()-Z        fBodyGyro-mean()-X          fBodyGyro-mean()-Y         
[55] fBodyGyro-mean()-Z          fBodyGyro-std()-X           fBodyGyro-std()-Y          
[58] fBodyGyro-std()-Z           fBodyAccMag-mean()          fBodyAccMag-std()          
[61] fBodyBodyAccJerkMag-mean()  fBodyBodyAccJerkMag-std()   fBodyBodyGyroMag-mean()    
[64] fBodyBodyGyroMag-std()      fBodyBodyGyroJerkMag-mean() fBodyBodyGyroJerkMa-std()

Both training and test datasets are joined using plyr::join to their respective subject labels and activity labels that are added as columns with the same names in each dataset. This step adds two coumns to each dataset.


Finally, both train and test datasets are bound vertically (10,299 observations of 68 variables). To create a tidy dataset, I initially melted the TestTrain dataset for activity labels and subjects (and all the relevant measures) this step creates a narrow (4 variables - activity label, subject, variable and value) and long (679,734 observations). I then used dcast to group the melted dataset by activity label and subject, averaging each variable - bringing back the 68 variables for 180 observations for each subject and activity across the combined dataset.  

