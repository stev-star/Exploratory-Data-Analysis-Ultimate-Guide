
# Exploratory-Data-Analysis-Ultimate-Guide## What is Exploratory Data Analysis?


Exploratory data analysis (EDA) is an approach to analyzing data sets in order to understand it in depth and summarize 

their main characteristics, often with the aid of visual methods.

The main aim of EDA is to spot patterns and trends, to identify anomalies, and to test early hypotheses.

before you perform data analysis and run your data through an any algorithm. You need to know the patterns 

in your data and determine which variables are important and which do not play a significant role in the output. 


## Steps involved in Exploratory data analysis.

The steps involved in exploratory data analysis (EDA) can vary depending on the data set and the goals of the analysis,

but some general steps that are typically followed include:


### Data collection: 

Gathering the data from various sources, which may include internal databases, external datasets, or manual data entry.

```python
# important librararies
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

#### Dataset Used

We will use the [IT Salary Survey EU  2020](https://www.kaggle.com/datasets/parulpandey/2020-it-salary-survey-for-eu-region) data for this. It contains 23 columns namely
![head](https://github.com/stev-star/Exploratory-Data-Analysis-Ultimate-Guide/blob/main/head.png)


#### Data cleaning: 

Identifying and correcting errors, missing data, and inconsistencies in the data set. 

This may involve removing or imputing missing data, correcting data entry errors, or dealing with outliers.

let's start by dropping unnecesary Timestamp column which we are not going to use at this point
```python
# dropping the Timestamp column
IT_survey_2020.drop('Timestamp',axis=1,inplace=True)
```
##### - Missing Values

The data has some missing values in it's columns.Let's check the columns with missing values

```python
# checking the missing values
missing_values=IT_survey_2020.isnull().sum().sort_values(ascending=False)
percent=(IT_survey_2020.isnull().sum()/IT_survey_2020.count()).sort_values(ascending=False)
missing_data_df=pd.DataFrame({'missing_values':missing_values,'% of the Total':percent})
missing_data_df
```
![missing_values](https://github.com/stev-star/Exploratory-Data-Analysis-Ultimate-Guide/blob/main/missing_values.png)

There is no specific percentage of data that one should remove during data cleaning .The amount of data that need to be removed 

or modified during data cleaning depends on the nature and quality of the data ,as well as specific rfequirement of analysis or application.

```python
# dropping columns with null values
cols=['Have you been forced to have a shorter working week (Kurzarbeit)? If yes, how many hours per week',
      'Have you received additional monetary support from your employer due to Work From Home? If yes, how much in 2020 in EUR',
      'Yearly bonus + stocks in EUR',                                                                                             
      'Annual brutto salary (without bonus and stocks) one year ago. Only answer if staying in the same country',           
      'Annual bonus+stocks one year ago. Only answer if staying in same country' ]

IT_survey_2020.drop(cols,axis=1,inplace=True)

# dropping the rows with null values
IT_survey_2020.dropna(axis=0,inplace=True)

IT_survey_2020.isnull().sum()
```
![without_missing](https://github.com/stev-star/Exploratory-Data-Analysis-Ultimate-Guide/blob/main/without_missing_values.png)

`- Handling the Outliers`

It's important to visualize the data to identify outliers. Plotting the data in a scatter plot or histogram can help you identify data points that are significantly far from others

```python
sns.scatterplot(x=IT_survey_2020['Age'],y=IT_survey_2020['Yearly brutto salary (without bonus and stocks) in EUR'])
```
![outliers](https://github.com/stev-star/Exploratory-Data-Analysis-Ultimate-Guide/blob/main/outliers.png)


One approach of handling outliers is to remove them from the dataset.However,it's important to be careful when removing outliers as this can bias the an analysis.

Removing outliers can be appropriate due to data erros or when the outliers are not representing the underlying population.

```python
# removal of outliers using standard deviation
lower_limit=IT_survey_2020['Yearly brutto salary (without bonus and stocks) in EUR'].mean()-3*IT_survey_2020['Yearly brutto salary (without bonus and stocks) in EUR'].std()
upper_limit=IT_survey_2020['Yearly brutto salary (without bonus and stocks) in EUR'].mean()+3*IT_survey_2020['Yearly brutto salary (without bonus and stocks) in EUR'].std()
lower_limit,upper_limit

# create a new df 
new_df=IT_survey_2020[(IT_survey_2020['Yearly brutto salary (without bonus and stocks) in EUR']>lower_limit) & (IT_survey_2020['Yearly brutto salary (without bonus and stocks) in EUR']<upper_limit)]
```

### Data visualization:

Creating visualizations of the data to explore patterns, relationships, and trends. 

This can include scatter plots, histograms, box plots, heat maps, and other types of visualizations.

- Univariate Analysis
```python
sns.displot(new_df['Yearly brutto salary (without bonus and stocks) in EUR'],kde=True,bins=20)
```
![displot](https://github.com/stev-star/Exploratory-Data-Analysis-Ultimate-Guide/blob/main/displot%20.png)

Majority of the people who participated in the survey earn a salary between 400000 - 1000000 EUR

```python
# top 5 programming language
top5_main_tech=new_df['Your main technology / programming language'].value_counts(normalize=True).head()
top5_main_tech.plot.barh()
plt.show()
```
![top-5](https://github.com/stev-star/Exploratory-Data-Analysis-Ultimate-Guide/blob/main/top_5_programming.png)

Consider the different programming Languages you have on the data.Plotting the top 5 most used programming 

language by the people who participated in the survey 

- Bivariate Analysis

```python
sns.relplot(x='Age',y='Yearly brutto salary (without bonus and stocks) in EUR',
            data=new_df,hue='Gender')
plt.show()
```
![screenshot](https://github.com/stev-star/Exploratory-Data-Analysis-Ultimate-Guide/blob/main/Screenshot%202023-02-27%20110517.png)

- Multivariate Analysis


