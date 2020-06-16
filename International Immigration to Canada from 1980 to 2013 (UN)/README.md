# International Immigration to Canada from 1980 to 2013 (UN)

### 1. Data Cleaning 
- Remove columns that are not imformative 
```df_can.drop(['AREA', 'REG', 'DEV', 'Type', 'Coverage'], axis = 1, inplace = True)```
- Rename some of the columns so that they make sense
```df_can.rename(columns={'OdName':'Country', 'AreaName':'Continent','RegName':'Region'}, inplace=True)```
- For consistency, change types of all column labels to string type
```df_can.columns = list(map(str, df_can.columns))```
