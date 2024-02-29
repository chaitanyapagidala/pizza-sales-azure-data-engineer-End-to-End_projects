Project: Azure Data Migration and Power BI Reporting

This project outlines the steps to migrate data from an SQL Server database to Azure Data Lake Storage (ADLS) and build reports using Power BI.

Steps:                                                                                                                        
Create an Azure SQL Database and upload a CSV file.                                                                                                                   Create an Azure storage account.
Create an Azure Data Factory (ADF).                                                                                               
Set up an Azure Databricks integration runtime.                                                                                   
Create an ADF pipeline to load data from SQL Server to ADLS.                                                                      
Use Databricks notebooks to process and transform the data.
Create Power BI reports to visualize the data


data bricks code:

To read the data from azure blob storage to data bricks note book
    dbutils.fs.mount(
    source = "wasbs://pizzasales@salesstorages.blob.core.windows.net",
    mount_point = "/mnt/pizzasales2",
    extra_configs = {"fs.azure.account.key.salesstorages.blob.core.windows.net":"GzY9ht5lCnSNqZQgikSVu4uoQlmLVOZF4ycg2VM5QP7+N9s8lbxi7ED6AoLMUaEN1hd6TdAXfJNa+AStu9DGaw=="})

creating data frame:

dbutils.fs.ls( "/mnt/pizzasales2")

df= spark.read.format("csv").options(header='True',inferschema='True').load('dbfs:/mnt/pizzasales2/pizza_sales.csv')

creating sql table:

df.createOrReplaceTempView("sales_pizzas")

performing aggregate functions and transforimg the data:

%sql
select
count(distinct order_id) order_id ,
sum(quantity) quantity,
date_format(order_date,'MMM') month_name,
date_format(order_date,'EEEE') day_name,
hour(order_time)order_time,
sum(unit_price) unit_price,
sum(total_price) total_price,
pizza_size,
pizza_category, 
pizza_name
from sales_pizzas
group by 3,4,5,8,9,10


After that connecting to the power bi creating reports based on the KPI.



