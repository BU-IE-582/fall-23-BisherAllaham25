## Spambase
Classifying Email as Spam or Non-Spam

Dataset Characteristics
Multivariate

Associated Tasks
Classification

Feature Type
Integer, Real

Instances
4601

Features
57

The classification task for this dataset is to determine whether a given email is spam or not.



The last column of 'spambase.data' denotes whether the e-mail was considered spam (1) or not (0), i.e. unsolicited commercial e-mail.  Most of the attributes indicate whether a particular word or character was frequently occuring in the e-mail.  The run-length attributes (55-57) measure the length of sequences of consecutive capital letters. 
Here are the definitions of the attributes:

48 continuous real [0,100] attributes of type word_freq_WORD 
= percentage of words in the e-mail that match WORD, i.e. 100 * (number of times the WORD appears in the e-mail) / total number of words in e-mail.  A "word" in this case is any string of alphanumeric characters bounded by non-alphanumeric characters or end-of-string.

6 continuous real [0,100] attributes of type char_freq_CHAR] 
= percentage of characters in the e-mail that match CHAR, i.e. 100 * (number of CHAR occurences) / total characters in e-mail

1 continuous real [1,...] attribute of type capital_run_length_average 
= average length of uninterrupted sequences of capital letters

1 continuous integer [1,...] attribute of type capital_run_length_longest 
= length of longest uninterrupted sequence of capital letters

1 continuous integer [1,...] attribute of type capital_run_length_total 
= sum of length of uninterrupted sequences of capital letters 
= total number of capital letters in the e-mail

### code:

The code includes hyperparameter optimization for four base classifiers: K-Nearest Neighbors (KNN), Decision Tree (CART), Random Forest (RF), and Gradient Boosting (GBM).

For each classifier, a set of hyperparameters is specified, and grid search is performed using GridSearchCV to find the best combination of hyperparameters.

Here are the hyperparameters and their respective ranges:

1. K-Nearest Neighbors (KNN):
Parameter: n_neighbors
Range: [1, 3, 5, 7, 9]
2. Decision Tree (CART):
Parameters:
min_samples_leaf
ccp_alpha
min_samples_split
Ranges:
min_samples_leaf: [1, 2, 5, 10, 20]
ccp_alpha: [0]
min_samples_split: [2, 4, 10, 20, 40]
3. Random Forest (RF):
Parameters:
n_estimators: [500]
max_features: ['sqrt', 'log2', None]
min_samples_leaf: [5]
4. Gradient Boosting (GBM):
Parameters:
max_depth: [3, 4, 5]
n_estimators: [50, 100, 150, 200, 250]
learning_rate: [0.01, 0.1]
### Results:

The script evaluates several base models using cross-validation with the "roc_auc" scoring metric. Here are the mean roc_auc scores for each base model:

Logistic Regression (LR): 0.954
K-Nearest Neighbors (KNN): 0.9243
Support Vector Machine (SVC): 0.9611
Decision Tree (CART): 0.8608
Random Forest (RF): 0.9704
AdaBoost: 0.9616
Gradient Boosting (GBM): 0.974
LightGBM: 0.9763

The script then performs hyperparameter optimization for KNN and CART and prints the mean roc_auc scores before and after optimization, along with the best hyperparameters found:

K-Nearest Neighbors (KNN):
Before Optimization: 0.9243
After Optimization: 0.929
Best Parameters: {'n_neighbors': 7}
Decision Tree (CART):
Before Optimization: 0.8743
After Optimization: 0.9365
Best Parameters: {'ccp_alpha': 0, 'min_samples_leaf': 20, 'min_samples_split': 10}
These results provide insights into how hyperparameter tuning improves the performance of the base models. The mean roc_auc scores are higher after optimization, and the best hyperparameters for each model are reported.

The hyperparameter optimization is performed using GridSearchCV to explore different combinations of hyperparameters within the specified ranges and select the combination that maximizes the performance metric (roc_auc in this case). The final models with the optimized hyperparameters are then used for the subsequent steps in the script, such as creating a voting ensemble.

******************************************

