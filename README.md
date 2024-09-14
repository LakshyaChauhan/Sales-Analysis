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
   ```
   <p align = "center">
      <img src = "https://github.com/LakshyaChauhan/Sales-Analysis/blob/main/assets/all_month.png" ></img>
<!--       <img src = "" width="45%"></img> -->
   </p>

all_months_data.head()

2. **Cleaning the Data**
   * Removed rows with missing values using `dropna`.
   * Filtered out invalid rows based on conditions like incorrect 'Order Date'.
   * Converted necessary columns (like 'Month') to numeric types for analysis.

3. **Sales Analysis**
   - Calculated total sales by adding a new column 'Sales' (`Quantity Ordered` × `Price Each`).
   - Grouped data by month and calculated total monthly sales.

4. **Visualization**
   + Plotted the monthly sales data using `matplotlib` to identify trends.
   + Created bar charts to visualize the best month for sales.

## Result
![Result]()
