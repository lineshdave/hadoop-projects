## NASDAQ and NYSE Trading Database Creation

### Problem Statement
Create a database in Hive called <i>handson_nasdaq</i> that will hold the tables created in Hive in different formats (text, avro, and parquet). Also, demonstrate altering and renaming of these Hive tables.

### Hive Script

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

### Output
