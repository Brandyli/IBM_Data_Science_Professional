
### Python for Data Science and AI
Key Concepts
__Types__
* Demonstrate an understanding of types in python by converting/casting data types: strings, floats, integers.
* Interpret variables and solve expressions by applying mathematical operations.
* Describe how to manipulate strings by using a variety of methods and operations.

__Lists and Tuple & Dictionary & Sets__

* Understand tuples and lists by describing and manipulating tuple combinations and list data structures.
* Demonstrate understanding of dictionaries by writing structures with correct keys and values.
* Understand the differences between sets, tuples, and lists by creating sets.

### Intro of Data Analysis of Python
Key Concepts
* Understanding the Data
* Importing and Exporting Data in Python
* Getting Started Analyzing Data in Python
* Python Packages for Data Science

### Data Wrangling 
Key Concepts
* Identify and Handle Missing Values
* Data Formatting
* Data Normalization
* Binning
* Indicator variables

### Exploratory Data Analysis on Car Price
#### What are the main characteristics which have the most impact on the car price?
#### 1. Import Data
```
import pandas as pd
import numpy as np

path='https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/DA0101EN/automobileEDA.csv'
df = pd.read_csv(path)
df.head()
```
#### Visualizing via Seaborn
```
%%capture
! pip install seaborn
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
```
How to choose the right visualization method? - Check .dtypes
```
# list the data types for each column
print(df.dtypes)
```
#### 1. What is the data type of the column "peak-rpm"?
```
df["peak-rpm"].dtypes
df.corr()
```
#### 2. Find the correlation between the following columns: bore, stroke,compression-ratio , and horsepower.
```
df[['bore','stroke' ,'compression-ratio','horsepower']].corr()
```
#### Continuous numerical variables:
the scatterplot of "engine-size" and "price"
```
sns.regplot(x = "engine-size", y = "price", data = df)
plt.ylim(0,)
df[["engine-size", "price"]].corr()
```
the scatterplot of "highway-mpg" and "price"
```
sns.regplot(x = "highway-mpg", y = "price", data = df)
df[["highway-mpg", "price"]].corr()
```
Let's see if "Peak-rpm" as a predictor variable of "price".
```
sns.regplot(x = "peak-rpm", y = "price", data = df)
df[["peak-rpm", "price"]].corr()
```
#### 3 Find the correlation between x="stroke", y="price".
```
sns.regplot(x="stroke", y="price", data = df)
df[["stroke","price"]].corr()
```
### Categorical variables
```
sns.boxplot(x = "body-style", y = "price", data = df)
sns.boxplot(x = "engine-location", y = "price", data = df)
sns.boxplot(x = "drive-wheels", y = "price", data = df)
```
### 3. Descriptive Statistical Analysis
```
df.describe()
# Method 1
df.describe(include = object)
# Method 2
df.describe(include=['object'])
```
#### Value Counts
```
df['drive-wheels'].value_counts()
# We can convert the series to a Dataframe as follows :
df['drive-wheels'].value_counts().to_frame()
```
```
drive_wheels_counts = df['drive-wheels'].value_counts().to_frame()
drive_wheels_counts.rename(columns = {'drive-wheels':'value_counts'}, inplace = True)
# Now let's rename the index to 'drive-wheels':
drive_wheels_counts.index.name = 'drive-wheels'
drive_wheels_counts
engine_loc_counts = df['engine-location'].value_counts().to_frame()
engine_loc_counts.rename(columns = {'engine-location':'value_counts'}, inplace = True)
engine_loc_counts.index.name = 'engine-location'
engine_loc_counts
```
### 4. Basics of Grouping
```
df['drive-wheels'].unique()
df_group_one = df[['drive-wheels', 'body-style','price']]
# We can then calculate the average price for each of the different categories of data.
df_group_one = df_group_one.groupby(['drive-wheels'], as_index = False).mean()
grouped_test1 = grouped_test1.groupby(['drive-wheels', 'body-style'], as_index = False).mean()

# This grouped data is much easier to visualize when it is made into # a pivot table. A pivot table is like an Excel spreadsheet, with one # variable along the column and another along the row. We can convert # the dataframe to a pivot table using the method "pivot " to create # a pivot table from the groups.

t_pivot = grouped_test1.pivot(index = 'drive-wheels', columns = 'body-style')
t_pivot = t_pivot.fillna(0) # fill missing data with 0
```
#### Q4. Use the "groupby" function to find the average "price" of each car based on "body-style" ?
```
body_style_group = df[["body-style","price"]]
body_style_group = body_style_group.groupby(["body-style"], as_index = False).mean()
body_style_group

import matplotlib.pyplot as plt
%matplotlib inline
```
```
fig, ax = plt.subplots()
im = ax.pcolor(t_pivot, cmap = "RdBu")

#label names
row_labels = t_pivot.columns.levels[1]
col_labels = t_pivot.index

#move ticks and labels to the center
ax.set_xticks(np.arange(t_pivot.shape[1]) + 0.5, minor = False)
ax.set_yticks(np.arange(t_pivot.shape[0]) + 0.5, minor = False)

#insert lables
ax.set_xticklabels(row_labels, minor = False)
ax.set_yticklabels(col_labels, minor = False)

#rotate label if too long
plt.xticks(rotation = 90)

fig.colorbar(im)
plt.show()
```
---
#### 5. Correlation and Causation
Correlation: a measure of the extent of interdependence between variables.

Causation: the relationship between cause and effect between two variables.
P-value
```
df.corr()
from scipy import stats
# Let's calculate the Pearson Correlation Coefficient and P-value of 'wheel-base' and 'price'.
coef, p_value = stats.pearsonr(df['wheel-base'], df['price'])
print("the correlation coefficient is: ", coef, "P_value is: ", p_value)
```
### 6. ANOVA

ANOVA: Analysis of Variance
The Analysis of Variance (ANOVA) is a statistical method used to test whether there are significant differences between the means of two or more groups. ANOVA returns two parameters:

F-test score: ANOVA assumes the means of all groups are the same, calculates how much the actual means deviate from the assumption, and reports it as the F-test score. A larger score means there is a larger difference between the means.

P-value: P-value tells how statistically significant is our calculated score value.

If our price variable is strongly correlated with the variable we are analyzing, expect ANOVA to return a sizeable F-test score and a small p-value.
```

# ANOVA
f_val, p_val = stats.f_oneway(grouped_test2.get_group('fwd')['price'], grouped_test2.get_group('rwd')['price'], grouped_test2.get_group('4wd')['price'])  
 
print( "ANOVA results: F=", f_val, ", P =", p_val)
```
