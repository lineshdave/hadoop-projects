# Average Stock Price (subset of Stocks)

## Problem Statement
Find the average closing price for each stock in the entire dataset. Order the results by the stock symbol and store in a pipe delimited file

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
SELECT stock_symbol, avg(stock_price_close) AS avg_stock_close_price FROM daily_prices
GROUP BY stock_symbol;

## Output Extract
![image](https://user-images.githubusercontent.com/19809692/27836774-e1d6d6dc-60ae-11e7-8dbd-f7905cf02c8d.png)
![image](https://user-images.githubusercontent.com/19809692/27836834-29729cce-60af-11e7-983d-2ad56eefd258.png)

