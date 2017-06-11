## Greater Stock Volatility than Average Volatility for each Stock (subset of Stocks)

### Problem Statement
Find records that have volatility greater than its average volatility for each stock in the entire dataset. Order the results by the stock symbol and store in a pipe delimited file

### Algorithm
1. Load all records from the dataset.
2. Filter the header line from the loaded records.
3. Add calculated volatiltiy information to each record.
4. Find average volatility for each stock symbol.
5. Merge average volatility for each stock record in the dataset.
6. Filter stock records that have volatility greater than the average volatility.
7. Add the other record columns to the filtered stock records from step-6 above.
8. Save the entire updated stock records using the PigStorage.

### Pig Script
<pre>
data = LOAD '/user/cloudera/rawdata/handson_train/nasdaq_daily_prices' using PigStorage(',')
       AS (exchange:chararray,stock_symbol:chararray,date:chararray,stock_price_open:double,
           stock_price_high:double,stock_price_low:double,stock_price_close:double,
           stock_volume:int,stock_price_adj_close:double);

wo_header_data = FILTER data BY exchange != 'exchange';

projected_data = FOREACH wo_header_data
                 GENERATE stock_symbol AS key,
                          (stock_price_high - stock_price_low) AS volatility,
                          stock_volume AS stock_volume;

grouped_projected_data = GROUP projected_data BY key;

reduced_grouped_data = FOREACH grouped_projected_data
                        GENERATE group as stock_symbol,
                                AVG(projected_data.volatility) AS avg_stock_volatility;

join_prelim = JOIN projected_data BY key, reduced_grouped_data BY stock_symbol;

join_prelim_data = FOREACH join_prelim GENERATE
                        CONCAT(reduced_grouped_data::stock_symbol,
                                (chararray)projected_data::volatility,
                                (chararray)stock_volume) AS key,
                        reduced_grouped_data::stock_symbol AS stock_symbol,
                        projected_data::volatility AS stock_volatility,
                        reduced_grouped_data::avg_stock_volatility AS avg_stock_volatility;

join_prelim_data_filtered =  FILTER join_prelim_data
                                BY stock_volatility > avg_stock_volatility;


projected_data_2 = FOREACH wo_header_data GENERATE
                   CONCAT(stock_symbol,
                          (chararray)(stock_price_high - stock_price_low),
</pre>

### Output
An output file part-r-00000 is created in the following HDFS directory - <i>/user/cloudera/output/handson_train/pig/nasdaq_daily_prices/avg_volatility_rec</i> along with the <i>_SUCCESS</i> file to mark the successful completion and running of the above script. This script can be run in the GRUNT interactive Pig environment OR executed from the shell command line using the "-f" option (pig -f xxxx.pig).

![image](https://user-images.githubusercontent.com/19809692/27015430-903294ae-4edb-11e7-862d-97b40b6bb87e.png)
