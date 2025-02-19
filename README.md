# ComparingClassifiers
Practical Application III: Comparing Classifiers

Link to Jupytr Notebook: https://github.com/mjlee177/ComparingClassifiers
Dataset: The dataset was acquired from the UCI Machine Learning repository here: https://archive.ics.uci.edu/dataset/222/bank+marketing.  The data is from a Portugese banking institution and is a collection of the results of multiple marketing campaigns. We will make use of the article accompanying the dataset here (https://drive.google.com/file/u/0/d/CRISP-DM-BANK.pdf/edit) for more information on the data and features.

## Summary of Findings: 
Our business goal is to see which Classifier (dummy, logistic regression, k-nearest neighbors, decision trees, SVC with linear, SVC with poly, SVC with rbf, and SVC with sigmoid) works best to predict the outcome (yes = buy CD or no = does not buy CD).  20 features have data collected and we will provide the best model as well as determine which features are the most important for the business to consider.  Having this information can also inform businesses which data is not important to collect, saving them time and money.

We first observed the data so we could visualize what is in the csv.  Figures are saved in the 'images' folder.  Of note, we discovered that the distribution of the target column had only 11.27% "yes".  This required us to use stratify=y in train/ test split.

Without hyperparameter exploration, the results are below:
                 Model  Train Time (s)  Train Accuracy  Test Accuracy
0                Dummy         0.02768         0.88732        0.88732
1  Logistic Regression         2.05312         0.90979        0.90979
2                  KNN         0.12593         0.92763        0.90675
3        Decision Tree         0.33003         1.00000        0.89291
4           SVM Linear        38.24149         0.90173        0.90129
5             SVM Poly        22.19268         0.92580        0.90784
6         SVM Gaussian        72.99464         0.91991        0.91136
7          SVM Sigmoid        79.50230         0.86661        0.86632

Knn was determined to be the 'best' classifier based on training time (lowest besides Dummy), train accuracy (highest besides overfit Decision Tree), and test accuracy (on par with the best).

Then, GridSearchCV was implemented on Logistic Regression and KNN to optimize hyperparameters.  The conclusion was that n_nearest_neighbors = 3 was optimal and is what should be used in a model.  In the interest of time and the fact that accuracies are already excellent, GridSearchCV was not performed on the remaining classifiers.

To interpret the results, we used Recursive Feature Elimination (RFE) to show the most impactful features.  Then we ranked the linear regression coefficients and listed the top decision tree nodes.

The top 3 features (with actions) are:
1. duration: employees who are in contact with clients should note that the longer time you spend on them, the more likely they are to purchase.
2. cons.price.idx: when the consumer price index is high, it's a good time to sell CDs. 
3. euribor3m: when the euribor 3 month rate is high, it's a good time to sell CDs.

A confusion matrix and accompanying metrics were generated for the knn classifier.  The accuracy is very high at .905 so 90.5% of the predictions are correct.<br>
The precision is only 0.627, which means that only 62.7% of the positives were correctly identified.  False positives are not a big issue because the consequence is just that the sales person spent some time on someone that did not say yes.<br>
The recall is 0.392, which means only 39.2% of the positives were correctly identified.  This is of greater consequence because this situation is if a 'yes' slips by.  Given that only 11.27% of leads say 'yes', this is a big deal<br>
The specificity of 0.970 shows that 97% of negatives were correctly identified.  This is not very important because if someone is a 'no' it is of little consequence to the business.<br>
Finally, the AUC-ROC score of .905 means that there's a 90.5% chance that the model will be able to distinguish between positive and negative classes.  A very good score.<br>

## Future Improvements
If there was more time, GridSearchCV could have been performed on the remaining SVM models and then the train time, train accuracy, and test accuracy could be re-evaluated.

Also if there was more time, the precision and recall for the models with optimized hyperparameters could have been explored.  Higher precision and recall are both desired, but recall is a more important metric for this business so that 'yes' replies do not slip through.
