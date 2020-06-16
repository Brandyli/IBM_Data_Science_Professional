# International Immigration to Canada from 1980 to 2013 (UN)

## I. Data Cleaning 

-  Remove columns that are not imformative \
```df_can.drop(['AREA', 'REG', 'DEV', 'Type', 'Coverage'], axis = 1, inplace = True)
```
## II. Data Transformation

- Rename some of the columns so that they make sense \
```df_can.rename(columns={'OdName':'Country', 'AreaName':'Continent','RegName':'Region'}, inplace=True)
```
-  For consistency, change types of all column labels to string type
```df_can.columns = list(map(str, df_can.columns))
all(isinstance(column, str) for column in df_can.columns)
```
-  Set the country name as index - useful for quickly looking up countries \
```df_can.set_index('Country', inplace = True)
```
-  Add total column \
```df_can['Total'] = df_can.sum(axis =1)
```
- Years that we will be using for plotting later on \
```years = list(map(str, range(1980, 2014)))
print('data dimensions:', df_can.shape)
```

## III. Data Visualization
- Line Plots 
- Area Plots 
- Histograms 
- Bar Charts - Horizontal & Vertical bars 
- Pie Charts 
- Box Plots 
- Scatter Plots
- Bubble Plots 
### Line Plots
##### What are top 5 countries contributing the most immigrants to Canada from 1980 to 2013?
<img width="929" alt="Line Graph" src="https://user-images.githubusercontent.com/46945617/84721008-9ed01100-af4d-11ea-8bab-5d05e74db24a.png">

### Area Plots or Stacked Line Plots 
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

Option 2: preferred option with more flexibility
ax = df_top5.plot(kind='area', alpha=0.35, figsize=(20, 10))

ax.set_title('Immigration Trend of Top 5 Countries')
ax.set_ylabel('Number of Immigrants')
ax.set_xlabel('Years')
```
![image](https://user-images.githubusercontent.com/46945617/84796824-18581580-afc7-11ea-857f-51f7e9c08707.png)

##### What are least 5 countries contributing the most immigrants to Canada from 1980 to 2013?
 
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
![image](https://user-images.githubusercontent.com/46945617/84797318-ad5b0e80-afc7-11ea-8e6a-11cd0cca7c5d.png)

```
Method 2
df_least5.index = df_least5.index.map(int) # let's change the index values of df_least5 to type integer for plotting
df_least5.plot(kind='area', alpha=0.45, figsize=(20, 10)) 

plt.title('Immigration Trend of 5 Countries with Least Contribution to Immigration')
plt.ylabel('Number of Immigrants')
plt.xlabel('Years')

plt.show()
```
![image](https://user-images.githubusercontent.com/46945617/84797369-bba92a80-afc7-11ea-8875-a494710a794a.png)

### Histograms
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
![image](https://user-images.githubusercontent.com/46945617/84721214-2289fd80-af4e-11ea-808e-7c25b504ddb2.png)
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
![image](https://user-images.githubusercontent.com/46945617/84798037-7afde100-afc8-11ea-9eb1-2653430e4ad2.png)


### Bar Charts
#### Vertical bar plot
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

#### Horizontal Bar Plot
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

### Pie Charts
##### What is the immigration to canada by continent in 2013?
```
# group countries by continents and apply sum() function 
df_continents = df_can.groupby('Continent', axis=0).sum()

colors_list = ['gold', 'yellowgreen', 'coral', 'skyblue', 'green', 'pink']
explode_list = [0.1, 0, 0, 0, 0.1, 0.1] # ratio for each continent with which to offset each wedge.

df_2013.plot(kind = 'pie', figsize = (15,6),
             shadow = True, pctdistance = 1.12,
             startangle = 90, autopct = '%1.1f%%',
             colors = colors_list, labels = None,
             explode = explode_list, subplots = True
            )

plt.title('Immigration to Canada by Continent in 2013', y = 1.12)
plt.axis('equal')

plt.show()
```
<img width="619" alt="Screen Shot 2020-06-16 at 8 05 54 AM" src="https://user-images.githubusercontent.com/46945617/84795828-f90cb880-afc5-11ea-9adc-2cc4c04583a8.png">

### Box Plots
##### What is the distribution of the number of new immigrants from India and China for the period 1980 - 2013?
```
df_CI = df_can.loc[['China', 'India'], years].transpose()
df_CI.plot(kind = 'box', figsize = (8,6))

plt.title('Box plot of Chinese and Indian Immigrants to Canada from 1980 - 2013')
plt.ylabel('Number of Immigrants')
plt.show()
```
<img width="605" alt="Screen Shot 2020-06-16 at 11 41 29 AM" src="https://user-images.githubusercontent.com/46945617/84796176-57399b80-afc6-11ea-8938-b30284ab1b73.png">

##### What is the distribution of the top 15 countries (based on total immigration) among 1980s, 1990s, and 2000s?

Create a new dataframe which contains the aggregate for each decade:

* Create a list of all years in decades 80's, 90's, and 00's.
* Slice the original dataframe df_can to create a series for each decade and sum across all years for each country.
* Merge the three series into a new data frame.
```
yr_80s = list(map(str, range(1980, 1990)))
yr_90s = list(map(str, range(1990, 2000)))
yr_00s = list(map(str, range(2000, 2010)))

df_80s = top15.loc[:, yr_80s].sum(axis = 1)
df_90s = top15.loc[:, yr_90s].sum(axis = 1)
df_00s = top15.loc[:, yr_00s].sum(axis = 1)

new_df = pd.DataFrame({'1980s':df_80s, '1990s':df_90s, '2000s':df_00s})

new_df.plot(kind='box', color='blue',figsize=(10, 6)) 
plt.title('Top 15 immigrantion countries in 1980s, 1990s, and 2000s.')
plt.xlabel('Years')
plt.ylabel('Number of Immigrants')
```
<img width="636" alt="Screen Shot 2020-06-16 at 11 40 30 AM" src="https://user-images.githubusercontent.com/46945617/84796044-36714600-afc6-11ea-9ef3-b8bbd0595540.png">

### Scatter Plots
##### What is the total immigration from Denmark, Norway, and Sweden to Canada from 1980 to 2013?
```
df_countries = df_can.loc[['Denmark', 'Norway', 'Sweden'], years].transpose()
df_total = pd.DataFrame(df_countries.sum(axis = 1))
df_total.reset_index (inplace = True)
df_total.index = map(str, df_total.index)
df_total.columns = ['year', 'total']

df_total.plot(kind = 'scatter', x ='year', y = 'total', color = 'darkblue',figsize=(10, 6))

plt.title('Total Immigration of  to Canada from 1980 - 2013')
plt.xlabel('Year')
plt.ylabel('Number of Immigrants')

plt.show()
```
<img width="703" alt="Screen Shot 2020-06-16 at 11 23 03 AM" src="https://user-images.githubusercontent.com/46945617/84795853-0164f380-afc6-11ea-993d-e0159f91ee05.png">

#### Bubble Plots
##### What is the immigration from China and India from 1980 to 2013?
```
df_can_t = df_can[years].transpose() # transposed dataframe

# cast the Years (the index) to type int
df_can_t.index = map(int, df_can_t.index)

# let's label the index. This will automatically be the column name when we reset the index
df_can_t.index.name = 'Year'

# reset index to bring the Year in as a column
df_can_t.reset_index(inplace=True)

# view the changes
df_can_t.head()
```
```
# normalize China data
norml_China = (df_can_t['China'] - df_can_t['China'].min()) / (df_can_t['China'].max() - df_can_t['China'].min()) 
# normalize India data
norml_India = (df_can_t['India'] - df_can_t['India'].min()) / (df_can_t['India'].max() - df_can_t['India'].min())
```
```
# China
ax0 = df_can_t.plot(kind='scatter',
                    x='Year',
                    y='China',
                    figsize=(14, 8),
                    alpha=0.5,                  # transparency
                    color='red',
                    s=norml_China * 2000 + 10,  # pass in weights 
                    xlim=(1975, 2015)
                   )

# India
ax1 = df_can_t.plot(kind='scatter',
                    x='Year',
                    y='India',
                    alpha=0.5,
                    color="darkblue",
                    s=norml_India * 2000 + 10,
                    ax = ax0
                   )

ax0.set_ylabel('Number of Immigrants')
ax0.set_title('Immigration from China and India from 1980 - 2013')
ax0.legend(['China', 'India'], loc='upper left', fontsize='x-large')
```
<img width="874" alt="Screen Shot 2020-06-16 at 11 37 40 AM" src="https://user-images.githubusercontent.com/46945617/84795875-06c23e00-afc6-11ea-86d4-34c4aa35db22.png">
