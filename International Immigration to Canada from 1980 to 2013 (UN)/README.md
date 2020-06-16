# International Immigration to Canada from 1980 to 2013 (UN)

### I. Data Cleaning 
- 1. Remove columns that are not imformative 
```df_can.drop(['AREA', 'REG', 'DEV', 'Type', 'Coverage'], axis = 1, inplace = True)```

### II. Data Transformation
- 1. Rename some of the columns so that they make sense
```df_can.rename(columns={'OdName':'Country', 'AreaName':'Continent','RegName':'Region'}, inplace=True)```
- 1. For consistency, change types of all column labels to string type
```df_can.columns = list(map(str, df_can.columns))```
```all(isinstance(column, str) for column in df_can.columns)```
- 1. Set the country name as index - useful for quickly looking up countries
```df_can.set_index('Country', inplace = True)```
- 1. Add total column
```df_can['Total'] = df_can.sum(axis =1)```

### III. Data Visualization

#### Line Plots
What are top 5 countries contributing the most immigrants to Canada from 1980 to 2013?

<img width="929" alt="Line Graph" src="https://user-images.githubusercontent.com/46945617/84721008-9ed01100-af4d-11ea-8bab-5d05e74db24a.png">

#### Area Plots or Stacked Line Plot 
- Select top 5 countries 
```df_can.sort_values(['Total'], ascending = False, axis = 0, inplace = True)```
```# transpose the dataframe
df_top5 = df_top5[years].transpose()```
```df_top5.index = df_top5.index.map(int) # let's change the index values of df_top5 to type integer for plotting
Option 1: This is what we have been using so far
df_top5.plot(kind='area', alpha=0.35, figsize=(20, 10)) 
plt.title('Immigration trend of top 5 countries')
plt.ylabel('Number of immigrants')
plt.xlabel('Years')
```
<img width="652" alt="Screen Shot 2020-06-15 at 8 59 55 PM" src="https://user-images.githubusercontent.com/46945617/84721005-9d064d80-af4d-11ea-9828-4ec2ba82a563.png">

Option 2: preferred option with more flexibility
```
ax = df_top5.plot(kind='area', alpha=0.35, figsize=(20, 10))

ax.set_title('Immigration Trend of Top 5 Countries')
ax.set_ylabel('Number of Immigrants')
ax.set_xlabel('Years')
```
What are least 5 countries contributing the most immigrants to Canada from 1980 to 2013?
- Select least 5 countries 

```
Get the 5 countries with the least contribution
df_least5 = df_can.tail()

Transpose the dataframe
df_least5 = df_least5[years].transpose()
```
```
Method 1
df_least5.index = df_least5.index.map(int)
ax = df_least5.plot(kind = 'area', stacked=False, alpha = 0.45, figsize = (20,10))
ax.set_title('Immigration Trend of 5 Countries with Least Contribution to Immigration')
ax.set_ylabel('Number of Immigrants')
ax.set_xlabel('Years')
```
<img width="852" alt="Screen Shot 2020-06-15 at 9 00 28 PM" src="https://user-images.githubusercontent.com/46945617/84721012-a0013e00-af4d-11ea-8d6b-7585e0fa42ff.png">

```
Method 2
df_least5.index = df_least5.index.map(int) # let's change the index values of df_least5 to type integer for plotting
df_least5.plot(kind='area', alpha=0.45, figsize=(20, 10)) 

plt.title('Immigration Trend of 5 Countries with Least Contribution to Immigration')
plt.ylabel('Number of Immigrants')
plt.xlabel('Years')

plt.show()
```
<img width="848" alt="Screen Shot 2020-06-15 at 9 00 41 PM" src="https://user-images.githubusercontent.com/46945617/84721013-a1cb0180-af4d-11ea-950c-24582809a0bd.png">

#### Histograms
##### What is the frequency distribution of the number (population) of new immigrants from the various countries to Canada in 2013?

```
count, bin_edges = np.histogram(df_can['2013'])

df_can['2013'].plot(kind = 'hist', figsize = (10, 8), xticks = bin_edges)

plt.title('Histogram of Immigration from 195 countries in 2013') # add a title to the histogram
plt.ylabel('Number of Countries') # add y-label
plt.xlabel('Number of Immigrants') # add x-label

plt.show()
```
##### What is the immigration distribution for Denmark, Norway, and Sweden for years 1980 - 2013?
 ```
Transpose dataframe
df_t = df_can.loc[['Denmark', 'Norway', 'Sweden'], years].transpose()
 ```
 ```
Let's get the x-tick values
count, bin_edges = np.histogram(df_t, 15)

Un-stacked histogram
df_t.plot(kind ='hist', 
          figsize=(10, 6),
          bins=15,
          alpha=0.6,
          xticks=bin_edges, 
          color=['coral', 'darkslateblue', 'mediumseagreen'] 
         ) # coral 珊瑚色的 darkslateblue 暗灰蓝色的 mediumseagreen 中海绿色的

plt.title('Histogram of Immigration from Denmark, Norway, and Sweden from 1980 - 2013')
plt.ylabel('Number of Years')
plt.xlabel('Number of Immigrants')

plt.show()
```
<img width="656" alt="Screen Shot 2020-06-15 at 9 05 07 PM" src="https://user-images.githubusercontent.com/46945617/84721021-a42d5b80-af4d-11ea-97de-5f52e698a502.png">

##### What are the immigration distribution for Greece, Albania, and Bulgaria for years 1980 - 2013?

```
df_t2 = df_can.loc[['Greece', 'Albania', 'Bulgaria'], years].transpose()
count, bin_edges = np.histogram(df_t2, 15)
xmin = bin_edges[0] - 10   #  first bin value is 31.0, adding buffer of 10 for aesthetic purposes 
xmax = bin_edges[-1]+ 10 #  last bin value is 308.0, adding buffer of 10 for aesthetic purposes
```
```
df_t2.plot(kind = 'hist',
           alpha = 0.35,
           figsize = (10,6),
           bins = 15,
           stacked = True,
           xticks=bin_edges,
           xlim = (xmin, xmax),
           color = ['coral', 'darkslateblue', 'mediumseagreen']         
          )
plt.title('Histogram of Immigration from Greece, Albania, and Bulgaria from 1980 - 2013')
plt.ylabel('Number of Years')
plt.xlabel('Number of Immigrants') 

plt.show()
```
![image](https://user-images.githubusercontent.com/46945617/84721214-2289fd80-af4e-11ea-808e-7c25b504ddb2.png)
<img width="621" alt="Screen Shot 2020-06-15 at 9 06 18 PM" src="https://user-images.githubusercontent.com/46945617/84721025-a7284c00-af4d-11ea-9839-27851c6ad580.png">

#### Bar Charts
##### Vertical bar plot
##### What are Icelandic immigrants (country = 'Iceland') to Canada from year 1980 to 2013?
```
Step 1: get the data
df_iceland = df_can.loc['Iceland', years]
df_iceland.head()
```
```
df_iceland.plot(kind='bar', figsize=(10, 6), rot=90) 

plt.xlabel('Year')
plt.ylabel('Number of Immigrants')
plt.title('Icelandic Immigrants to Canada from 1980 to 2013')

Annotate arrow
plt.annotate('',                      # s: str. will leave it blank for no text
             xy=(32, 70),             # place head of the arrow at point (year 2012 , pop 70)
             xytext=(28, 20),         # place base of the arrow at point (year 2008 , pop 20)
             xycoords='data',         # will use the coordinate system of the object being annotated 
             arrowprops=dict(arrowstyle='->', connectionstyle='arc3', color='blue', lw=2)
            )

# Annotate Text
plt.annotate('2008 - 2011 Financial Crisis', # text to display
             xy=(28, 30),                    # start the text at at point (year 2008 , pop 30)
             rotation=72.5,                  # based on trial and error to match the arrow
             va='bottom',                    # want the text to be vertically 'bottom' aligned
             ha='left',                      # want the text to be horizontally 'left' algned.
            )

plt.show()
```
<img width="620" alt="Screen Shot 2020-06-15 at 9 08 12 PM" src="https://user-images.githubusercontent.com/46945617/84721029-aabbd300-af4d-11ea-88bd-5114d8cce6ac.png">

##### Horizontal Bar Plot
##### What are the top 15 countries?
```
df_can.sort_values(by = 'Total', ascending = True, inplace = True)
top15 = df_can['Total'].tail(15)
```
```
top15.plot(kind = 'barh',figsize=(12, 12), rot=90, color = 'darkslateblue', alpha = 0.7)

plt.xlabel('Number of Immigrants')
plt.ylabel('Year')
plt.title('Immigrants of Top 15 countries to Canada from 1980 to 2013')

Annotate value labels to each country
for index, value in enumerate(top15): 
    label = format(int(value), ',') # format int with commas
    
    # place text at the end of bar (subtracting 47000 from x, and 0.1 from y to make it fit within the bar)
    plt.annotate(label, xy=(value - 47000, index - 0.10), color='white')
    
plt.show()
```
<img width="708" alt="Screen Shot 2020-06-15 at 9 15 54 PM" src="https://user-images.githubusercontent.com/46945617/84721032-ad1e2d00-af4d-11ea-8e82-c760fd6eda78.png">
