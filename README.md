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