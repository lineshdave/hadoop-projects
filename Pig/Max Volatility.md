## Maximum Volatility of Stocks (subset)

### Problem Statement
Find the record with max volatility for each stock. Volatility is  stock_price_high - stock_price_low.

### Algorithm
<pre>
-- 1. get the data
-- 2. remove header
-- 3. project stock_symbol, (stock_price_high - stock_price_low) as volatility
-- 4. group by stock_symbol
-- 5. reduced by MAX
-- 6. from data generate (stock_symbol + volatility) ,......
-- 7. join records using the new key
-- 8. retrieve trading rec from joined rec

-- select concat(sym, (high_price - low_price)) key, ...... from nasdaq_daily_prices n
-- join (select concat(sym, max(high_price - low_price)) key from nasdaq_daily_prices group by sym) v
-- on v.key = n.key
</pre>

### Pig Script
<pre>
data = LOAD '/user/cloudera/rawdata/handson_train/nasdaq_daily_prices' using PigStorage(',')
       AS (exchange:chararray,stock_symbol:chararray,date:chararray,stock_price_open:float,
           stock_price_high:float,stock_price_low:float,stock_price_close:float,
           stock_volume:int,stock_price_adj_close:float);

wo_header_data = FILTER data BY exchange != 'exchange';

projected_data = FOREACH wo_header_data 
                 GENERATE stock_symbol, (stock_price_high - stock_price_low) AS volatility;

grped_vol_data = GROUP projected_data BY stock_symbol;

reduced_data = FOREACH grped_vol_data 
               GENERATE group as stock_sym, MAX(projected_data.volatility) AS  max_stock_volatility;

projected_data_2 = FOREACH wo_header_data GENERATE CONCAT(stock_symbol, 
                   (chararray)(stock_price_high - stock_price_low)) AS key, 
                   stock_symbol,date,stock_price_open,stock_price_high,
                   stock_price_low,stock_price_close,stock_volume;

proj_red_data = FOREACH reduced_data GENERATE CONCAT(stock_sym, (chararray)max_stock_volatility) AS key;

jned_proj_data = JOIN projected_data_2 BY key, proj_red_data BY key;

j_data = FOREACH jned_proj_data 
         GENERATE projected_data_2::stock_symbol AS stock_symbol,projected_data_2::date AS date,
         projected_data_2::stock_price_open AS stock_price_open, 
         projected_data_2::stock_price_high AS stock_price_high, 
         projected_data_2::stock_price_low AS stock_price_low, 
         projected_data_2::stock_price_close AS stock_price_close, 
         projected_data_2::stock_volume AS stock_volume;

final_data = ORDER j_data BY stock_symbol;

STORE j_data INTO '/user/cloudera/output/handson_train/pig/nasdaq_daily_prices/max_volatility_rec' USING PigStorage('|');
</pre>

### Output
An output file <i>part-r-00000</i> is created in the following HDFS directory - <i>/user/cloudera/output/handson_train/nasdaq_daily_prices/tot_stock_volume</i> along with the <i>_SUCCESS</i> file to mark the successful completion and running of the above script. This script can be run in the GRUNT interactive Pig environment.
