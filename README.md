# Sales Analysis on 12 months Data Set

## Overview
In this project, we analyze sales data for a company over the span of 12 months. The main goal is to extract useful insights such as identifying the best month for sales, top-selling products, and patterns in customer purchasing behavior. This is achieved by cleaning and merging data from multiple CSV files, followed by various analyses.

## Question 1 : What was the best month for sale? How much was earned during that month?

### Data preparation Steps
1. **Merging Monthly Sales Data**
   - Combined all 12 months of sales data into a single file using `pandas.concat`.
   - Ensured the merged data is clean and complete.
   ```python
   files  =  [file for file in os.listdir('analysis/Sales_Data')]
   all_months_data = pd.DataFrame()
   for file in files:
    df = pd.read_csv('./analysis/Sales_Data/'+file)
    all_months_data = pd.concat([all_months_data ,df])
    all_months_data.head()
   ```
  
   <img src = "https://github.com/LakshyaChauhan/Sales-Analysis/blob/main/assets/all_month.png" width = "75%" ></img>
  


2. **Cleaning the Data**
   * Removed rows with missing values using `dropna`.
   * Filtered out invalid rows based on conditions like incorrect 'Order Date'.
   * Converted necessary columns (like 'Month') to numeric types for analysis.
   ```python
   nan_df = all_months_data[all_months_data.isna().any(axis=1)]
   nan_df.head()
 
   all_months_data = all_months_data.dropna(how='all')
   all_months_data

   all_months_data.loc[:,'Month'] = all_months_data['Order Date'].str[0:2]

   all_months_data = all_months_data[all_months_data['Month'].str[0:2] != 'Or']
   all_months_data['Month'] = pd.to_numeric(all_months_data['Month'])
   all_months_data.head()

   ```
   
3. **Sales Analysis**
   - Calculated total sales by adding a new column 'Sales' (`Quantity Ordered` × `Price Each`).
   - Grouped data by month and calculated total monthly sales.
   ```python
   all_months_data['Price Each'] = pd.to_numeric(all_months_data['Price Each'])
   all_months_data['Quantity Ordered'] = pd.to_numeric(all_months_data['Quantity Ordered'])
   all_months_data.head(50)
   ```

4. **Visualization**
   + Plotted the monthly sales data using `matplotlib` to identify trends.
   + Created bar charts to visualize the best month for sales.
   ```python
   months = ['JAN','FEB','MAR','APR','MAY','JUN','JUL','AUG','SEP','OCT','NOV','DEC']
   plt.bar(months,results['Sales'])
   plt.xticks(months)
   plt.ylabel('Sales in USD($)')
   plt.xlabel('Months')
   plt.show()
   ```
   <img src = "https://github.com/LakshyaChauhan/Sales-Analysis/blob/main/assets/ans1.png">

## Question 2 : Which city had highest number of sales?

### 1. Data Preparation:

The dataset didn't already have a City column, the city information was extracted from the Purchase Address column. The city and state were combined for better clarity.
```python
def get_city(address):
    return address.split(',')[1]
def get_state(address):
    return address.split(',')[2].split(' ')[1]
all_months_data['City'] = all_months_data['Purchase Address'].apply(lambda x:f"{get_city(x)} ({get_state(x)})")
all_months_data
```

### 2. Grouping Data by City:

The data was grouped by the City column to calculate the total sales for each city using the Sales column.

### 3. Summing the Sales:

The total sales for each city were calculated by summing the Sales column after grouping by City.
```python
city = all_months_data.groupby('City').sum()
city.head()
```

### 4. Identifying the City with the Highest Sales:

The city with the highest total sales was identified using Python's idxmax() function, which returns the city with the maximum sales.
```python
cities = [city for city, df in all_months_data.groupby('City')]
plt.bar(cities,city['Sales'])
plt.xticks(cities, rotation = 'vertical',size =8 )
plt.ylabel('Sales in USD($)')
plt.xlabel('City ')
plt.show()
```
<img src= "https://github.com/LakshyaChauhan/Sales-Analysis/blob/main/assets/ans2.png">

## Question 3 : At what time should we display advertisements to maximize the likelihood of customer's buying product?

### 1. Convert Order Date to DateTime Format:

First, we needed to ensure that the Order Date column was in the proper datetime format to extract hour information.
```python
all_months_data['Order Date'] = pd.to_datetime(all_months_data['Order Date'])
```

### 2. Extract the Hour from the Order Date:

We extracted the hour from the Order Date to understand when most purchases were made.
```python
all_months_data['Hour'] = all_months_data['Order Date'].dt.hour
all_months_data['Minutes'] = all_months_data['Order Date'].dt.minute

all_months_data
```

### 3. Plotting the Data:

A bar chart was created to visualize the number of orders during each hour. This helped us determine peak hours for customer purchases.
```python
hours =  [hour for hour , df in all_months_data.groupby('Hour')]

plt.plot(hours,all_months_data.groupby(['Hour']).count())
plt.xticks(hours)
plt.ylabel('Number of Orders')
plt.xlabel('Hours')
plt.grid()
plt.show()
```
<img src ="https://github.com/LakshyaChauhan/Sales-Analysis/blob/main/assets/ans3.png">
