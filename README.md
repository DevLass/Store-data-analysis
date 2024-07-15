<h1 align="center"> Hey there, i'm Lass, and this is my Store data analysis</h1>

<h3 align="justify"> <i> Firstly, let me clarify that all this data is fictitious and was used only for my studies.</i> </h3>

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

# Imagem da Tabela


```
%sql

SELECT `Product Category`, SUM(`Units Sold`) AS UnitsSold
FROM `StoreData`
GROUP BY `Product Category`;

```

# Imagem do Grafico de Unidades vendidas x Categoria

<p align="center">
  <img src="https://github.com/DevLass/Store-data-analysis/blob/main/readmeimg/g1.png" alt="Page Preview">
</p>

```
%sql

SELECT `Product Category`, SUM(`Total Revenue`) AS TotalRevenue
FROM `StoreData`
GROUP BY `Product Category`;
```

# Falar que não funcionou

```
%sql

SELECT `Product Category`, SUM(CAST(REPLACE(`Total Revenue`, '$', '') AS DECIMAL(10, 2))) AS TotalRevenue
FROM `StoreData`
GROUP BY `Product Category`;
```

# Imagem do Grafico de Valor Vendido por Categoria

<p align="center">
  <img src="https://github.com/DevLass/Store-data-analysis/blob/main/readmeimg/g2.png" alt="Page Preview">
</p>

```
%sql

SELECT  `Product Category`, ROUND(AVG(CAST(REPLACE(`Total Revenue`, '$', '') AS DECIMAL(10, 2))), 1) AS `Average Revenue`
FROM `StoreData`
GROUP BY `Product Category`;

```

# Imagem do Grafico de Média de Preços por Categoria
<p align="center">
  <img src="https://github.com/DevLass/Store-data-analysis/blob/main/readmeimg/g3.png" alt="Page Preview">
</p>

```
%sql

SELECT `Product Name`, `Product Category`, SUM(`Units Sold`) AS UnitsSold
FROM `StoreData`
GROUP BY `Product Name`, `Product Category`
ORDER BY UnitsSold DESC;

```

# Imagem da tabela dos produtos mais vendidos
<p align="center">
  <img src="https://github.com/DevLass/Store-data-analysis/blob/main/readmeimg/g4.png" alt="Page Preview">
</p>

```
%sql

SELECT `Region`, COUNT(*) AS `RegionCount`
FROM `StoreData`
GROUP BY `Region`;

```

# Imagem do Grafico de Lugares Vendidos
<p align="center">
  <img src="https://github.com/DevLass/Store-data-analysis/blob/main/readmeimg/g5.png" alt="Page Preview">
</p>

```
%sql

SELECT `Payment Method`, SUM(CAST(REPLACE(`Total Revenue`, '$', '') AS DECIMAL(10, 2))) AS TotalRevenue
FROM `StoreData`
GROUP BY `Payment Method`;

```

# Imagem do Grafico dos Metódos de pagamento
<p align="center">
  <img src="https://github.com/DevLass/Store-data-analysis/blob/main/readmeimg/g6.png" alt="Page Preview">
</p>

```
%sql

SELECT `Date`, SUM(CAST(REPLACE(`Total Revenue`, '$', '') AS DECIMAL(10, 2))) AS TotalRevenue
FROM `StoreData`
GROUP BY `Date`
ORDER BY to_date(`Date`, 'M/d/yyyy') ASC;

```

# Imagem do Grafico de vendas por dia
<p align="center">
  <img src="https://github.com/DevLass/Store-data-analysis/blob/main/readmeimg/g7.png" alt="Page Preview">
</p>

