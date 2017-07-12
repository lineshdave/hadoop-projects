# Total Volume of Stocks (subset of Stocks)

## Problem Statement
Find the total stock volume of each traded stock found in the subset of the dataset uploaded in the HDFS directory. Store the results in a fashion ordered by stock volume in a descending order.

## Algorithm
![pig first example algorithm](https://cloud.githubusercontent.com/assets/19809692/26765343/2561ac20-4948-11e7-9956-40d2b3a3101d.JPG)

## Pig Script
<pre>
raw_data = LOAD '/user/cloudera/rawdata/handson_train/nasdaq_daily_prices' using PigStorage(',')
           AS (exchange:chararray,stock_symbol:chararray,date:chararray,stock_price_open:float,
               stock_price_high:float,stock_price_low:float,stock_price_close:float,
               stock_volume:int,stock_price_adj_close:float);
wo_header_data = FILTER raw_data BY exchange != 'exchange';
proj_data = FOREACH wo_header_data GENERATE stock_symbol, stock_volume;
group_data = GROUP proj_data BY stock_symbol;
aggr_data = FOREACH group_data 
            GENERATE group AS stock_symbol, SUM(proj_data.stock_volume) AS stock_total;
sorted_data = ORDER aggr_data BY stock_total DESC;
STORE sorted_data INTO '/user/cloudera/output/handson_train/pig/nasdaq_daily_prices/tot_stock_volume'
      USING PigStorage(',');
</pre>

## Output
An output file part-r-00000 is created in the following HDFS directory - <i>/user/cloudera/output/handson_train/pig/nasdaq_daily_prices/tot_stock_volume</i> along with the <i>_SUCCESS</i> file to mark the successful completion and running of the above script. This script can be run in the GRUNT interactive Pig environment.

## HDFS Snapshot
![image](https://user-images.githubusercontent.com/19809692/27015497-c9083b02-4edc-11e7-9f06-466f723ea3d4.png)
