<h1 align="center"> Hey there, i'm Lass, and this is my Store data analysis</h1>

<h4 align="center"> <i> Firstly, let me clarify that all this data is fictitious and was used only for my studies. </i> </h4>

<p align="center">
  <img src="https://github.com/DevLass/Store-data-analysis/blob/main/readmeimg/g0.png" alt="Page Preview">
</p>

<hr>

<h3 align="center"> <i> Let me show my code!!</i> </h3>

<h4 align="justify"> <i> This script reads a CSV file into a Spark DataFrame using specified options (no schema inference, the first row as the header, and a comma delimiter). <br><br> It then displays the DataFrame and creates a temporary SQL table named "StoreData" from it. The file location and type are specified at the beginning of the script.
</i> </h4>

```
# File location and type

file_location = "/FileStore/tables/Teste1/Online_Store_Sales_Data_xlsm___Online_Store_Sales.csv"
file_type = "csv"

# CSV options

infer_schema = "false"
first_row_is_header = "true"
delimiter = ","

# The applied options are for CSV files. For other file types, these will be ignored.

df = spark.read.format(file_type) \
  .option("inferSchema", infer_schema) \
  .option("header", first_row_is_header) \
  .option("sep", delimiter) \
  .load(file_location)

display(df)

temp_table_name = "StoreData"

df.createOrReplaceTempView(temp_table_name)

```
<hr>

<h4 align="justify"> <i> 
Here, we aggregate sales data by product category.
</i> </h4>

```
%sql

SELECT `Product Category`, SUM(`Units Sold`) AS UnitsSold
FROM `StoreData`
GROUP BY `Product Category`;

```
<p align="center">
  <img src="https://github.com/DevLass/Store-data-analysis/blob/main/readmeimg/g1.png" alt="Page Preview">
</p>

<hr>

<h4 align="justify"> <i> 
This SQL code is executed to aggregate revenue data by product category, but it encountered an issue due to the presence of a `$` symbol in the `Total Revenue` column values.
</i> </h4>

```
%sql

SELECT `Product Category`, SUM(`Total Revenue`) AS TotalRevenue
FROM `StoreData`
GROUP BY `Product Category`;
```

<h4 align="justify"> <i> 
Now the SQL query calculates the total revenue for each product category by first removing the `$` symbol from the `Total Revenue` column values and then converting them to a decimal format. The cleaned values are summed and the results are grouped by `Product Category`.
</i> </h4>

```
%sql

SELECT `Product Category`, SUM(CAST(REPLACE(`Total Revenue`, '$', '') AS DECIMAL(10, 2))) AS TotalRevenue
FROM `StoreData`
GROUP BY `Product Category`;
```

<p align="center">
  <img src="https://github.com/DevLass/Store-data-analysis/blob/main/readmeimg/g2.png" alt="Page Preview">
</p>

<hr>

<h4 align="justify"> <i> 
Calculate average revenue per product category from StoreData, rounding to one decimal place.</i> </h4>

```
%sql

SELECT  `Product Category`, ROUND(AVG(CAST(REPLACE(`Total Revenue`, '$', '') AS DECIMAL(10, 2))), 1) AS `Average Revenue`
FROM `StoreData`
GROUP BY `Product Category`;

```

<p align="center">
  <img src="https://github.com/DevLass/Store-data-analysis/blob/main/readmeimg/g3.png" alt="Page Preview">
</p>

<hr>

<h4 align="justify"> <i> 
Sum units sold per product name and category from StoreData, sorted by units sold in descending order.</i> </h4>

```
%sql

SELECT `Product Name`, `Product Category`, SUM(`Units Sold`) AS UnitsSold
FROM `StoreData`
GROUP BY `Product Name`, `Product Category`
ORDER BY UnitsSold DESC;

```

<p align="center">
  <img src="https://github.com/DevLass/Store-data-analysis/blob/main/readmeimg/g4.png" alt="Page Preview">
</p>

<hr>

<h4 align="justify">Count occurrences of each region. <i> 
</i> </h4>

```
%sql

SELECT `Region`, COUNT(*) AS `RegionCount`
FROM `StoreData`
GROUP BY `Region`;

```
<p align="center">
  <img src="https://github.com/DevLass/Store-data-analysis/blob/main/readmeimg/g5.png" alt="Page Preview">
</p>

<hr>

<h4 align="justify">Calculate total revenue per payment method.<i> 
</i> </h4>

```
%sql

SELECT `Payment Method`, SUM(CAST(REPLACE(`Total Revenue`, '$', '') AS DECIMAL(10, 2))) AS TotalRevenue
FROM `StoreData`
GROUP BY `Payment Method`;

```
<p align="center">
  <img src="https://github.com/DevLass/Store-data-analysis/blob/main/readmeimg/g6.png" alt="Page Preview">
</p>

<hr>

<h4 align="justify">Calculate total revenue per date from StoreData, ordered chronologically.<i> 
</i> </h4>

```
%sql

SELECT `Date`, SUM(CAST(REPLACE(`Total Revenue`, '$', '') AS DECIMAL(10, 2))) AS TotalRevenue
FROM `StoreData`
GROUP BY `Date`
ORDER BY to_date(`Date`, 'M/d/yyyy') ASC;

```
<p align="center">
  <img src="https://github.com/DevLass/Store-data-analysis/blob/main/readmeimg/g7.png" alt="Page Preview">
</p>

