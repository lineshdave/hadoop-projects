# Total Volume of Stocks (subset of Stocks)

## Problem Statement
Find the total stock volume of each traded stock found in the subset of the dataset uploaded in the HDFS directory. Store the results in a fashion ordered by stock volume in a descending order.

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
SELECT stock_symbol, sum(stock_volume) AS tot_stock_volume FROM daily_prices
GROUP BY stock_symbol
ORDER BY tot_stock_volume DESC;
</pre>

## Output Extract
![image](https://user-images.githubusercontent.com/19809692/27839302-3d455700-60be-11e7-80c1-36cf760cfe52.png)
