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
   <p align = "center">
      <img src = "https://github.com/LakshyaChauhan/Sales-Analysis/blob/main/assets/all_month.png" ></img>
   </p>


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


