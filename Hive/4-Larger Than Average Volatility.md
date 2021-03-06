# Larger than Average Volatility for each Stock (subset of Stocks)

## Problem Statement
Find records that have volatility greater than its average volatility for each stock in the entire dataset. Order the results by the stock symbol and store in a pipe delimited file

## Problem Statement
Find the record with max volatility for each stock. Volatility is  stock_price_high - stock_price_low.

## Data file
This is the same dataset/file that is placed is HDFS (<i>/user/cloudera/rawdata/handson_train/nasdaq_daily_prices</i>) and is used for the Pig exercise

## Hive Table Creation
Out of the four techniques to creat Hive tables - 1) using sqoop, 2) hive web or beemax; 3) beeline/java/shell script; and, 4) HBASE metastrore.

The table for this exercise is created using command line -- beeline -u jdbc:hive://quickstart:cloudera:10000:

<pre>
   jdbc:hive2://quickstart.cloudera:10000> CREATE EXTERNAL TABLE daily_prices
. . . . . . . . . . . . . . . . . . . . .> (
. . . . . . . . . . . . . . . . . . . . .>   `exchange` string ,
. . . . . . . . . . . . . . . . . . . . .>   `stock_symbol` string ,
. . . . . . . . . . . . . . . . . . . . .>   `date` string ,
. . . . . . . . . . . . . . . . . . . . .>   `stock_price_open` float ,
. . . . . . . . . . . . . . . . . . . . .>   `stock_price_high` float ,
. . . . . . . . . . . . . . . . . . . . .>   `stock_price_low` float ,
. . . . . . . . . . . . . . . . . . . . .>   `stock_price_close` float ,
. . . . . . . . . . . . . . . . . . . . .>   `stock_volume` int ,
. . . . . . . . . . . . . . . . . . . . .>   `stock_price_adj_close` float ) 
. . . . . . . . . . . . . . . . . . . . .> ROW FORMAT   DELIMITED FIELDS TERMINATED BY ','
. . . . . . . . . . . . . . . . . . . . .> LOCATION "/user/cloudera/rawdata/handson_train/nasdaq_daily_prices/"
. . . . . . . . . . . . . . . . . . . . .> TBLPROPERTIES("skip.header.line.count" = "1");
</pre>

## Hive Query
<pre>
SELECT a.stock_symbol, b.stock_price_high, b.stock_price_low, b.stock_volume, a.avg_volatility
FROM 
(SELECT stock_symbol, avg(stock_price_high-stock_price_low) AS avg_volatility 
FROM daily_prices
GROUP BY stock_symbol) AS a, 
daily_prices AS b
WHERE a.stock_symbol = b.stock_symbol
AND b.stock_price_high-b.stock_price_low > a.avg_volatility;
</pre>

## Output (Hue) Extract
![image](https://user-images.githubusercontent.com/19809692/27838826-061fdfe6-60bb-11e7-8143-0819991a676f.png)
