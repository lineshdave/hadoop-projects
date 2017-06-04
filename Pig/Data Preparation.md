## Data Preparation

### Nasdaq Daily Prices

#### Format
CSV File publicly available

#### Structure/Columns
<pre>
exchange,stock_symbol,date,stock_price_open,stock_price_high,stock_price_low,stock_price_close,stock_volume,stock_price_adj_close
NASDAQ,DELL,1997-08-26,83.87,84.75,82.50,82.81,48736000,10.35
NASDAQ,DITC,2002-10-24,1.56,1.69,1.53,1.60,133600,1.60
NASDAQ,DLIA,2008-01-28,1.91,2.31,1.91,2.23,760800,2.23
NASDAQ,DWCH,2002-07-10,3.09,3.14,3.09,3.14,2400,1.57
</pre>

#### Data File Placement

The data file <i>NASDAQ_daily_prices_subset.csv</i> is placed in one of the user-created directories in HDFS: - <i>/user/cloudera/rawdata/handson_train/nasdaq_daily_prices/</i>.
