Cleaning Data Course Project
========================================================
The script begins by reading from the relevant datasets: the activity labels, both train and test data sets, subject maps for both test and train, activity maps for both test and train, feature list mapping dataset columns to variable names.


```{r}
activity_labels <- read.table("C:/Users/gbeyderman/Downloads/datacleaning/UCI HAR Dataset/activity_labels.txt", quote="\"")
X_test <- read.table("C:/Users/gbeyderman/Downloads/datacleaning/UCI HAR Dataset/test/X_test.txt", quote="\"")
subject_test <- read.table("C:/Users/gbeyderman/Downloads/datacleaning/UCI HAR Dataset/test/subject_test.txt", quote="\"")
y_test <- read.table("C:/Users/gbeyderman/Downloads/datacleaning/UCI HAR Dataset/test/y_test.txt", quote="\"")
X_train <- read.table("C:/Users/gbeyderman/Downloads/datacleaning/UCI HAR Dataset/train/X_train.txt", quote="\"")
subject_train <- read.table("C:/Users/gbeyderman/Downloads/datacleaning/UCI HAR Dataset/train/subject_train.txt", quote="\"")
y_train <- read.table("C:/Users/gbeyderman/Downloads/datacleaning/UCI HAR Dataset/train/y_train.txt", quote="\"")
features <- read.table("C:/Users/gbeyderman/Downloads/datacleaning/UCI HAR Dataset/features.txt", quote="\"")
```

The column names are loaded from the features dataset.

```{r}
colnames(X_test)<-features$V2
colnames(X_train)<-features$V2
```

Both training and test datasets are filtered for the variables containing mean() and std().

```{r}
myvars<-grep("mean\\(\\)|std\\(\\)", features$V2)
X_test<-X_test[myvars]
X_train<-X_train[myvars]
```


Both training and test datasets are joined using plyr::join to their respective subject labels and activity labvels that are added as columns with the same names in each dataset.


```{r}
y_Test_labels<-plyr::join(y_test, activity_labels, by="V1")
X_test<-cbind(X_test,y_Test_labels$V2)
X_test<-cbind(X_test,subject_test$V1)
names(X_test)[names(X_test)=="y_Test_labels$V2"] <- "activitylabel"
names(X_test)[names(X_test)=="subject_test$V1"] <- "subj"
y_Train_labels<-plyr::join(y_train, activity_labels, by="V1")
X_train<-cbind(X_train,y_Train_labels$V2)
X_train<-cbind(X_train,subject_train$V1)
names(X_train)[names(X_train)=="y_Train_labels$V2"] <- "activitylabel"
names(X_train)[names(X_train)=="subject_train$V1"] <- "subj"
```

Finally, both train and test datasets are bound vertically. To create a tidy dataset, I initially melted the TestTrain dataset for activity labels and subjects (and all the relevant measures). I then used dcast to group the melted dataset by activity label and subject, averaging each variable.  


```{r}
TestTrain<-rbind(X_train,X_test)
TestTrainMelt<-melt(TestTrain, id=c("activitylabel","subj"), measure.vars=features[myvars,2])
tidyTestTrain<-dcast(TestTrainMelt, activitylabel+subj~variable, mean)
tidyTestTrain
```
