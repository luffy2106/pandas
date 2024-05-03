# Top essential pandas functions should know

Reference:
```
https://www.kdnuggets.com/10-essential-pandas-functions-every-data-scientist-should-know
```

#### 0. Importing data

Pay attention to these parameters:
- sep : , the "sep" parameter is used to specify the delimiter that separates the columns in the CSV file. By default, the comma (,) is used as the delimiter.
- encoding :  default ‘utf-8’
- memory_map : bool, default False, map the file object directly onto memory and access the data directly from there. Using this option can improve performance because there is no longer any I/O overhead.
- low_memory : bool, default True 
* low_memory = True : Internally process the file in chunks, resulting in lower memory use while parsing, but possibly mixed type inference
* low_memory = False : We are instructing pandas not to use a memory-saving dtype inference mechanism, which might result in faster performance but higher memory usage.


#### 1. Data Viewing
- df.head(): It displays the first five rows of the sample data
- df.tail(): It displays the last five rows of the sample data
- df.sample(n): It displays the random n number of rows in the sample data
- df.shape: It displays the sample data's rows and columns (dimensions).
- df.memory_usage() : It will tell you how much memory is being consumed by each column(in bytes)

#### 2. Selection
- df.iloc[row_num] : select row based on its index
- df['column_name'] / df['column_name_1', 'column_name_2'] : select column/multiple columns
- Select based on the condition :
```
condition = df['column_name'] > 50  # Define your condition here
selected_data = df[condition]
```
or 
```
selected_data = df[df['column_name'] > 50]
```

#### 3. Data cleaning
- df.isnull(): This will identify the missing values in your dataframe.
- df.dropna(): This will remove the rows containing missing values in any column.
- df.fillna(val): This will fill the missing values with val given in the argument.
- df[‘col’].astype(new_data_type): It can convert the data type of the selected columns to a different data type.


#### 4. Applying Custom Functions
#####  4.1. Apply 
You can apply custom functions according to your needs in either a row or a column.
- Apply the custom function on a column or column-wise(axis = 0)
```
def cus_fun(x):
    return x * 3
df["Sales_Tripled"] = df["SALES"].apply(cus_fun, axis=0)
```
or
```
df["Sales_Tripled"] = df["SALES"].apply(lambda x : x * 3, axis=0)
```

- Apply the custom function on a row or or row-wise(axis = 1)
```
import pandas as pd
# Create a sample DataFrame
data = {'A': [1, 2, 3],
        'B': [4, 5, 6],
        'C': [7, 8, 9]}

df = pd.DataFrame(data)
# Calculate the mean for each row using the mean() function with axis=1
row_means = df.mean(axis=1)
print(row_means)

0    4.0
1    5.0
2    6.0
dtype: float64
```

#####  4.2 apply() with predefined function with parameter
Ex :
```
def scale_experience_five(row,scale):
    return row["Experience"] * scale
times = 5
df.apply(scale_experience_five, args=(times,), axis=1)
```

#####  4.3. Applymap 
applymap is a DataFrame method in pandas that operates element-wise on a DataFrame(all elements in the dataframe)
Ex :
```
import pandas as pd

# Create a sample DataFrame
data = {
    'A': [4, 9, 16],
    'B': [25, 36, 49]
}

df = pd.DataFrame(data)

# Define a function to calculate square root
def sqrt(x):
    return x ** 0.5

# Apply the square root function using applymap
result_df = df.applymap(sqrt)
print(result_df)
	A	B
0	2.0	5.0
1	3.0	6.0
2	4.0	7.0
```

#### 5. Statistics
##### 5.1 General statistics 
- df.describe(): Get the basic statistics of each column of the sample data(count, mean, std, min, 25%, 50%, 75%, max)
- df.info() : Get the information about the various data types used and the non-null count of each column.
- df.corr(): This can give you the correlation matrix between all the numeric columns in the data frame(note that it's only apply for numeric columns) (*)
##### 5.2 Statistical calculations
- mean(): Calculates the mean value for each column.
- sum(): Computes the sum of values for each column.
- std(): Calculates the standard deviation of values for each column.
- min(): Finds the minimum value in each column.
- max(): Finds the maximum value in each column.
- count(): Counts the number of non-null values in each column.
- idxmax(): Returns the index of the first occurrence of the maximum value in each column.
- idxmin(): Returns the index of the first occurrence of the minimum value in each column.
- quantile(): Computes the quantile for each column, default x = 0.5(**)

Reference:
- Correlation Coefficient Formula(*):
r = (Σ((X_i - X_mean) * (Y_i - Y_mean))) / (sqrt(Σ(X_i - X_mean)^2) * sqrt(Σ(Y_i - Y_mean)^2))
- quantile(**)
```
# Create a sample DataFrame
data = {'A': [10, 20, 30, 40, 50],
        'B': [15, 25, 35, 45, 55]}

df = pd.DataFrame(data)

# Calculate the 25th percentile (first quartile) for each column
q1 = df.quantile(0.25)
print(q1)
A   20.0
B   25.0
```
In the provided code snippet, the df.quantile(0.25) function calculates the 25th percentile (Q1) for each column in the DataFrame df.

For column 'A', the values are [10, 20, 30, 40, 50]. When sorted in ascending order, the 25th percentile falls between 20 and 30. This means that approximately 25% of the values are less than 30, and 75% are greater than or equal to 20. Therefore, the calculated 25th percentile (Q1) for column 'A' is 20.0 because it is the interpolated value that corresponds to the position where 25% of the data falls below it and 75% above it.

For column 'B', the values are [15, 25, 35, 45, 55]. Similarly, when sorted in ascending order, the 25th percentile lies between 25 and 35. Therefore, the calculated Q1 for column 'B' is 25.0.

The df.quantile(0.25) function automatically computes the 25th percentile for each numeric column in the DataFrame, which explains why you get the output as shown.

(listings['host_is_superhost'] == "t" & listings['neighbourhood_cleansed'] == neighbor)

#### 6. Data analysis
6.1 Aggregation Functions 
- Single aggregation
You can group a column by its name and then apply some aggregation functions like sum, min/max, mean, etc.
Ex :
```
df.groupby("CITY").agg({"SALES": "sum"})
```
- Multiple aggregations
Ex:
```
aggregation = df.agg({"SALES": "sum", "QUANTITYORDERED": "mean"})
```
6.2 Sorting data
You can sort the data based on a specific column, either in the ascending order or in the descending order.
Ex:
```
df.sort_values('SALES', ascending = False) # Sorts the data in descending order
```
6.3 Pivot tables
We can create pivot tables that summarize the data using specific columns. This is very useful in analyzing the data when you only want to consider the effect of particular columns. There are 4 parameters you need to modified based on your expect result
- values: It contains the column for which you want to populate the table's cells.
- index: The column used in it will become the row index of the pivot table, and each unique category of this column will become a row in the pivot table.
- columns: It contains the headers of the pivot table, and each unique element will become the column in the pivot table. 
- aggfunc: This is the same aggregator function we discussed earlier.

ex:
```
pd.pivot_table(df, values="SALES", index="CITY", columns="year_id", aggfunc="sum")
```
#### 7. Combining Data Frames
- Combine horizontially(by rows)
```
combined_df = pd.concat([df1,df2])
```
- Combine vertically(by columns):It is useful when you want to combine two data frames that share a common identifier.
```
merger_df = pd.merge(df1, df2, on="common_col")
```

#### 8. Time Series Analysis
- Conversion to DateTime Object Model: We can convert the date column into a datetime format for easier data manipulation.
 ```
 import pandas as pd

# Sample DataFrame
data = {'ORDERDATE': ['2022-01-15', '2022-02-20', '2022-03-25']}
df = pd.DataFrame(data)

# Convert "ORDERDATE" column to datetime
df["ORDERDATE"] = pd.to_datetime(df["ORDERDATE"])

# Extract month from "ORDERDATE"
df['MONTH'] = df['ORDERDATE'].dt.month

print(df)
```
- Calculate Rolling Average: Using this method, we can create a rolling window to view data. We can specify a rolling window of any size. If the window size is 5, then it means a 5-day data window at that time.
```
rolling_avg = df["SALES"].rolling(window=5).mean()
```

#### 9. Cross Tabulation
We can perform cross-tabulation between two columns of a table. It is generally a frequency table that shows the frequency of occurrences of various categories. It can help you to understand the distribution of categories across different regions.
Ex : Suppose that you wan to see the distribution "DEALSIZE" of each "COUNTRY"
```
cross_tab = pd.crosstab(df["COUNTRY"], df["DEALSIZE"])
```
The result will be

           | Large | Medium | Small   
Country
Australia      7       86       2 
Belgium        4       5        12
Canada         15      10       24
Denmark        12      15       10
Finland        10      8        9        
France         4       5        6

#### 10. Handling Outliers
Outliers in data means that a particular point goes far beyond the average range. Let’s understand it through an example. Suppose you have 5 points, say 3, 5, 6, 46, 8. Then we can clearly say that the number 46 is an outlier because it is far beyond the average of the rest of the points. These outliers can lead to wrong statistics and should be removed from the dataset.

Here pandas come to the rescue to find these potential outliers. We can use a method called Interquartile Range(IQR), which is a common method for finding and handling these outliers. You can also read about this method if you want information on it. You can read more about them here.

Let’s see how we can do that using pandas.
```
Q1 = df["SALES"].quantile(0.25)
Q3 = df["SALES"].quantile(0.75)
IQR = Q3 - Q1
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

outliers = df[(df["SALES"] < lower_bound) | (df["SALES"] > upper_bound)]
```

Q1 is the first quartile representing the 25th percentile of the data and Q3 is the third quartile representing the 75th percentile of the data.

lower_bound variable stores the lower bound that is used for finding potential outliers. Its value is set to 1.5 times the IQR below Q1. Similarly, upper_bound calculates the upper bound, 1.5 times the IQR above Q3.

After which, you filter out the outliers that are less than the lower or greater than the upper bound.





 