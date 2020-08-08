Code Book

Variables Description 

X_train = X_train.txt 

y_train = y_train.txt 

subject_train = subject_train.txt

X_test = X_test.txt 

y_test = y_test.txt 

subject_test = subject_test.txt

features = features.txt

activity_labels = activity_labels.txt

mrg_train = merge y_train, subject_train, and X_train

mrg_test = merge y_test, subject_test, and X_test

setAllInOne = merge mrg_train and mrg_test

mean_and_std = defines Id mean and standard deviation 

setForMeanAndStd = makes subset from setAllInOne

setWithActivityNames = uses descriptive activiy names to name the activites in the data set 

secTidySet = second tidy data set 

