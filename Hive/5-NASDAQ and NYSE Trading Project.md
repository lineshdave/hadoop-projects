# NASDAQ and NYSE Trading Project

## Problem Statement
Create a database in Hive called <i>handson_nasdaq</i> that will hold the NASDAQ daily stock and NYSE daily stock dividends tables in Hive in different formats (text, avro, and parquet). Also, demonstrate altering and renaming of these Hive tables.

## Objective
To demonstrate database and table creation capabilities in Hive, which includes creation of data in these tables in different format (text, avro, and parquet).

## Hive Script

This is a 3-parts script. The first script is used to mostly used to create, rename, and alter tables; second is used to load the tables; and, the third is used to query these tables.

### First Script - Table Creation

<pre>
--Drop tables and database if exists
drop table handson_nasdaq.nasdaq_daily_prices;
drop table handson_nasdaq.avro_dividends;
drop table handson_nasdaq.avro_dividend;
drop table handson_nasdaq.nasdaq_dividends;
drop table handson_nasdaq.tbl_nasdaq_daily_prices;
drop table handson_nasdaq.tbl_nasdaq_daily_prices_avro;
drop table handson_nasdaq.tbl_nasdaq_daily_prices_parquet;
drop database handson_nasdaq;

-- create a database for this project
create database handson_nasdaq 
location '/user/cloudera/hive/warehouse/handson_nasdaq';

use handson_nasdaq;

--Create a managed table for NASDAQ daily prices data set.
create table tbl_nasdaq_daily_prices (
	exchange_name string,
	stock_symbol string, 
	tdate string,
	stock_price_open float,
	stock_price_high float,
	stock_price_low float,
	stock_price_close float,
	stock_volume int, 
	stock_price_adj_close float
)
row format delimited
fields terminated by ',';

--Create a managed table for NASDAQ daily prices data set with parquet data format
create table tbl_nasdaq_daily_prices_parquet (
	exchange_name string,
	stock_symbol string, 
	tdate string,
	stock_price_open float,
	stock_price_high float,
	stock_price_low float,
	stock_price_close float,
	stock_volume int, 
	stock_price_adj_close float
)
stored as parquet;

--create and load an avro table simultaneously
create table tbl_nasdaq_daily_prices_avro 
stored as avro
as 
select * from tbl_nasdaq_daily_prices_parquet;

--Create an external table for NASDAQ daily prices data set.
create external table nasdaq_daily_prices (
	exchange_name string,
	stock_symbol string, 
	tdate string,
	stock_price_open float,
	stock_price_high float,
	stock_price_low float,
	stock_price_close float,
	stock_volume int, 
	stock_price_adj_close float
)
row format delimited
fields terminated by ','
location '/user/cloudera/rawdata/handson_train/nasdaq_daily_prices';

-- Create an external table for NASDAQ dividends data set.
create external table nasdaq_dividends (
	exchange_name string,
	stock_symbol string, 
	tdate string, 
	dividends float
)
row format delimited
fields terminated by ','
location '/user/cloudera/rawdata/handson_train/nyse';

-- create a managed avro table
create table nasdaq_dividends_avro (
	exchange_name string,
	stock_symbol string, 
	tdate string, 
	dividends float
)
stored as avro;

--alter table to add the schema location
alter table nasdaq_dividends_avro
set location '/user/cloudera/rawdata/handson_train/nasdaq_dividend_avro';

-- alter a table to add a table property
alter table nasdaq_dividends_avro 
set tblproperties('author'='linesh dave');

-- alter table to rename the table
alter table nasdaq_dividends_avro 
rename to avro_dividend;	

-- to recreate the sql statement used to create a table
-- also use the output to view the real details for building a hive table
show create table nasdaq_daily_prices;

-- view details of our new avro table
show create table avro_dividend;

</pre>

### Second Script - Loading Tables

<pre>
use handson_nasdaq;

--load data into the managed table tbl_nasdaq_daily_prices
load data inpath '/user/cloudera/rawdata/handson_train/nasdaq_daily_prices'
overwrite into table tbl_nasdaq_daily_prices;

--load data into the managed parquet table tbl_nasdaq_daily_prices 
insert overwrite table tbl_nasdaq_daily_prices_parquet
select * from tbl_nasdaq_daily_prices;

--load data into our new avro table from another previous table or the same column specification
insert overwrite table avro_dividend select * from nasdaq_dividends;

</pre>

### Third Script - Query Tables

<pre>
USE handson_nasdaq;

-- Find out total volume sale for each stock symbol which has closing price more than $5.
SELECT stock_symbol, sum(stock_volume) total_volume 
FROM nasdaq_daily_prices 
WHERE stock_price_close > 5.0 
GROUP BY stock_symbol;

-- 0 Records found

-- Find out highest price and highest dividends for each stock symbol.
-- If one of them does not exist then keep Null values.
SELECT prices.stock_symbol, dividends.stock_symbol,
       prices.record_closing_price, dividends.record_dividend 
FROM
(SELECT stock_symbol, max(stock_price_close) record_closing_price 
 FROM tbl_nasdaq_daily_prices 
 GROUP BY stock_symbol) prices
FULL OUTER JOIN
(SELECT stock_symbol, max(dividends) record_dividend 
 FROM nasdaq_dividends 
 GROUP BY stock_symbol) dividends
ON prices.stock_symbol = dividends.stock_symbol

-- Snapshot of the output from the above query is shown below
</pre>

## Hive Browser Output (Database/Table View)
![image](https://user-images.githubusercontent.com/19809692/27840823-548f1372-60ca-11e7-864a-0be1bb16aed6.png)

## Hive Browser Output (Database/Table View)
![image](https://user-images.githubusercontent.com/19809692/27840815-31c34034-60ca-11e7-88e3-e7c8dae07eac.png)

## Query Output Snapshot
![image](https://user-images.githubusercontent.com/19809692/27843156-db54f93a-60de-11e7-8612-2b5826f863dc.png)

