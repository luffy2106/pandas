# Top essential pandas functions should know

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

#####  2.2 apply() with predefined function with parameter
Ex :
```
def scale_experience_five(row,scale):
    return row["Experience"] * scale
times = 5
df.apply(scale_experience_five, args=(times,), axis=1)
```

#####  2.3. Applymap 
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

#### 3. Statistics
##### 3.1 General statistics 
- df.describe(): Get the basic statistics of each column of the sample data(count, mean, std, min, 25%, 50%, 75%, max)
- df.info() : Get the information about the various data types used and the non-null count of each column.
- df.corr(): This can give you the correlation matrix between all the numeric columns in the data frame(note that it's only apply for numeric columns) (*)
##### 3.2 Statistical calculations
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