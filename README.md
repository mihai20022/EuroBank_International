# EuroBank_International
Analyzing the dataset to understand the causes of customer attrition and identify the customers who are at risk.


# Business Context 

EuroBank International (EBI) is facing the challenge of customer churn, which means that its customers are leaving their service for various reasons. The bank seeks to understand the underlying causes of customer attrition and proactively identify customers who are at risk of leaving. EBI has engaged your group to conduct a comprehensive data analytics study. Your task is to deliver useful insights to the bank.


# Data Pre-processing 

Are there any outliers / anomalies in the dataset that could distort the results?


The describe method will be applied to the dataset to understand if the mean, standard deviation, min, max are consistent.

We will explore first numeric column to check for anomalies or outliers.

1. Credit Score

The boxplot was plotted and some outliers were detected (below the lower quartile). It was decided to not remove the outliers but to replace the extreme values with the boundaries one (384.5 in this case).

2. Age

The boxplot was plotted and some outliers were detected: age above 62. However, they could be valid customers so it was decided to keep them in the column for further insight.

3. Tenure

The boxplot for the tenure columns has no outliers or possible anomalies. The minimum is 0 and maximum is 10, only positive numbers as it should be.


4. Balance

No anomalies were detected for the balance column. Customers do not have negative balance. The boxplot was plotted to check if there are  outliers. No outliers were detecting.

5. Products Number

Boxplot was plotted to look for outliers. Some outliers were spotted,one of them being 4 products. Realistically speaking, it is possible to have 4 bank products. Consequently, it was decided to keep the outliers.

6. Estimated Salary

There are no negative values for the estimated salary. The boxplot does not have any outliers or anomalies.



Credit Card, Active Member and Churn do not have any outliers. The standard deviation is low for both of them. The columns consists of only 1 or 0 values.


Are

1. Country

The country columns has no null values. It consists of three unique values: France, Spain, Germany. No anomalies were detected. All values are existing countries. The majority of the customers are from France.

2. Gender

The gender column consists of 2 unique values: Male and Female. There are no null values and the value counts are spreaded almost equally (4900 - Male, 4100 - Female).


### Are there variables that are not relevant to the analysis

It was decided that customer id is not relevant to the further analysis. It does not explain the customer churn. Consequently it will dropped from the dataset.


### Duplicates

The dataset was also checked for duplicates using df.duplicated().sum()



### Are there variables in the dataset that are not relevant to the analysis

Our team has decided to keep all the columns. However, we believe that some columms will have low feature importance: 

- Gender. It might not influence directly the churning rate.
- Credit Card. It can indicate a stronger engagement, but usually not a major churn driver by itself.


### Encoding

Binary encoding will be applied for the Gender because it has two values: Male and Female. A map will be created to map Female  to 0, and Male to 1.

Next, we will apply one hot encoding for the country column by creating separate columns containing dummy variables. Two columns were created: country_Germany, country_Spain, whereas you can deduce from these two columns from which country is the customer.

### Scaling

The scaling is necessary when distance-based or gradient-based models are being trained. They are sensitive to the scale of input data.

We will be scaling with the Standard Scaler. The method is working best when the data is normally distributed. In case the data is not normally distributed, then we will apply robust scaler


Normality tests were conducted Q-Q plots. Credit score is approximately normal and consequently will be scaled using Stantard Scaler. Other numeric columns such as age, balance and estimated salary will be scaled using Robust Scaler. Robust Scaler is a better solution, while standard scaling is using mean and standard deviation, the robust one is scaling using the median and interquartile range.


### Exploratory Data Analysis

### What is the overall customer churn in the dataset?

In order to find the customer churn rate, we divide the number of clients that churn (churn = 1) by the total number of clients.

20.36% is the overall customer churn.

### Churn Rate by demographic variables

### Categorical variables

### Gender

Male Churn Rate is 16.49% . Female Churn Rate is higher, having a 24.98%.

### Country

The German customers are churning the most, 32%. Spain and France have a similar churning rate, being 17% and 16%, lower than the german country

### Tenure

The % of customers churning is relatively similar if we take into account the number of years that the customer had an account. The lowest percentage of churned are the clients owning an account for 7 years.

### Products Number

All customers owning 4 products had churned. We can see a trend that if the customer has 2 products, that they are less customers that churn. However, if they purchase new products (3-4) then it increases exponentially: for 3 - 83%, 4 - 100%


### Credit Card

Customers having a credit card have a similar churn rate 20% comparing to the customers that do not have.


### Active Member

The customers that are not active members are churning more (27% out of all non active members) comparing to the active members (churn rate of 14%)
### Numeric values

In order to plot the bar plots, we divided the column data in multiple bins in order to assess the churn rate for each group.

### Age 

The age was divided in 6 bins. The highest percentage (55%) of customers churning are between 51-61. The age group


### Credit Score

Customers segment consisting of a credit score ranging from 384- 451 has the highest churn rate of 34%. The other credit score groups have a churn rate of 18-21%.

### Balance

Half of the all customers that have a high-balance (200800 - 2509000) are churning. In addition, 34% of customers with low balance (0 - 50180) also churns. Other balance segments have a similar churn rate of 20-26%.

### Estimated Salary

There is a stable churn rate across the estimated salary groups ranging between 19% and 22%


### Is there a relationship between tenure and churn rate?

The correlation between tenure and churning is -0.01. This would mean that there is no corelation between the feature tenure and target variable.


### Patterns in the dataset

The highest positive correlation is between the age and churn (0.28). The elderly people tend to churn more than the younger clients. However, in the correlation matrix, we can also detect a negative relationship between balance and number of products (-0.31). This means that if the customer has more products then the balance will decrease.

A weak negative correlation can be spotted between active member and churn (-0.15). The active members are churning less often than the passive ones. In addition, balance has a weak positive correlation with the target variable (0.12). It might be the case that customers owning a higher balance have a higher probability of churning.


### Model building

Which variables are the strongest predictors of customer churn? How did you conclude these are the strongest predictors?


The strongest predictors can be identified using correlation matrix. The correlation is compatible only with numeric features. In our dataset ebi_base_processed_model.csv the categorical features were transformed using one hot encoding - get_dummies (if the column had 3 or more unique values) or binary encoding in case the column has only 2 unique values.

The strongest predictors are:
- age (0.28)
- country_Germany (0.17)
- balance (0.11)
- active_member (-0.15)
- gender (-0.1)

In the following questions we will explore 3 models: Decision Tree Classifier (Tree-based), Random Forest (Tree-based ensemble), Neural Network (basic Multi-Layer Perceptron) and the feature importance for each model.

### Decision Tree Classifier (Tree-based)

Decision Tree is a supervised learning algorithm. It consists of a hierarchical tree structure containg branches, nodes, leaf nodes and root nodes. This model was chosen for its well-known flexibility and interpretability.

First, we will test the model using the default decision tree classifier without any parameters.

The train score is 1 and the test score is 0.8. There is clearly an overfitting scenario, the model is not generalized well. In order to fix the overfitting problem, we will introduce parameters tp the model such as max_depth, min_samples_leaf, min_samples_split.

After training the model, our team compued the classification report:

Accuracy 0.8 - the model predicted 80% of all results. However the data is imbalanced (1434 and 366), the model can get a high accuracy by predicting "No Churn" for most cases.

F1-score for churn 0.51 - The model identifies only half of the churners. Precision and Recall are both 0.51 meaning that 51% of the predicted churners were actually churners and 51% of real churners were detexted.

F1-score for non-churners 0.87 - The model identifies 87% of all non-churning. This is a good score, but it might be due to the imbalanced dataset. (1434 vs 366).

Now we will test the decision tree classifier using several parameters.

First, we have started to increase from a max depth of 1 to 7, whereas with the 7, the train and test score are the highest and more similar. Therefore, no more overfitting. If we increase the minimum of samples leaf to 50, then the F1-score is also increasing from 0.51 to 0.57. However, a value above 50 will decrease the F1-score back to the old value.

Instead of changing the parameters, we will try to resample the dataset using SMOTE due to the class imbalance: 1434 non-churners and 366 churners. The SMOTE sampling creates new synthetic minority samples by interpolation.

After applying the SMOTE, we can see a decrease in precision, but an increase in recall to a score of 0.7. Overall the F1-score remains almost the same 0.55. THe overall accuracy also decreased to 0.77. Now the model can detect a larger amount of customers that are churning.

In order to see the feature importance of the model, our team has acccessed throuugh the parameter feature_importances attribute of the model. These are the following results:
age                 0.471738
products_number     0.221091
active_member       0.135318
balance             0.090999
gender              0.050010
country_Germany     0.015698
estimated_salary    0.005595
tenure              0.004001
credit_card         0.003185
credit_score        0.002365
country_Spain       0.000000
The age has the highest importance (0.47) when the model splits the node. The products number has a moderate influence (0.22) on the model. Starting wit the active member that has an importance of 0.14, other features have a importance below 10%.

A bar chart has been ploted to better visualize the critical feature when using decision tree classifier.

### Random Forest (Tree-based ensemble)

Random Forest is a machine learning algorithm, par of the Decision Tree-based models, that uses many decision trees to make better predictions. Each tree has a different part of the data, combining the results by using voting for classification in order to reduce errors and improve accuracy of the model. The model was chosen due to several advantages:
- High accuracy. Provides reliable and precise predictions.
- Handles missing values. The model can predict with incomplete data.
- Reduces overfitting. Ensures generalization to the test data.

We will start with a simple model to understand the default performance of the Random Forest Classifier.

The results obtained from the default random forest classifier are similar to the fine-tuned decision classifier. However, the model is overfitting according to the train accuracy that is 1 and the test accuracy that is 0.86, respectively.

In order to decrease the overfitting and generalize the mode, we will tune the hyperparameter. Our focus will be on number of trees, depth of each tree, minimum samples of leafs and nodes, number of features per split and class weight. To make it more efficient, we will apply the grid search to find the optimal settings without the manual trial and error.

The grid search will try every possible combination of the following parameter grid:
- n_estimators : [200,300] 
- max_depth : [3,5,7]
- min_samples_leaf: [20,50]
- max_features': [sqrt,log2]
- class_weight': [balanced]

In total 24 models.

After training the model , we have computed the train score and test score, both of them being almost similar 81%. The best parameters selected by grid search are: 
- max_depth: 7
- max_features: sqrt 
- min_samples_leaf: 20
- n_estimators: 200

Comparing the results with the default model, we can see a strong recall (0.72): the model identifies 72% of the actual churners, moderate precision (0.53), half of the customers predicted are actual churners, and a good F1 for churn (0.61).

The feature importance are similar to the decision tree classifier besides the balance. Random Forest seems to prioritize churners based on their balance if they will churn or not.

### Neural Network (basic Multi-Layer Perceptron)


Neural network is a machine learning model that tries to mimic the functions of the huuman breain. The model processes the data, learns patterns and allows tasks such as pattern recognition and decision-making. Our team has decided to use Perceptron, the simplest model of Neural Networks. This model is designed to output a binary ouput (1 or 0), in our case would be if the customer churns or not.

