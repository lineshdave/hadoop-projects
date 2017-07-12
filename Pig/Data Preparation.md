# Data Preparation

## Nasdaq Daily Prices

### Format
CSV File publicly available

### Structure/Columns
<pre>
exchange,stock_symbol,date,stock_price_open,stock_price_high,stock_price_low,stock_price_close,stock_volume,stock_price_adj_close
NASDAQ,DELL,1997-08-26,83.87,84.75,82.50,82.81,48736000,10.35
NASDAQ,DITC,2002-10-24,1.56,1.69,1.53,1.60,133600,1.60
NASDAQ,DLIA,2008-01-28,1.91,2.31,1.91,2.23,760800,2.23
NASDAQ,DWCH,2002-07-10,3.09,3.14,3.09,3.14,2400,1.57
</pre>

### Data File Placement

The data file <i>NASDAQ_daily_prices_subset.csv</i> is placed in one of the user-created directories in HDFS: - <i>/user/cloudera/rawdata/handson_train/nasdaq_daily_prices/</i>.

## Movie Lens Dataset

### Format
CSV files publicly available

### List of Files
<OL>
<LI> movies.csv </LI>
<LI> ratings.csv </LI>
<LI> tags.csv </LI>
<LI> genome-scores.csv </LI>
<LI> genome-tags.csv </LI>
</OL>

### Structure/Columns
<pre>
# movies.csv
movieId,title,genres
1,Toy Story (1995),Adventure|Animation|Children|Comedy|Fantasy
2,Jumanji (1995),Adventure|Children|Fantasy
3,Grumpier Old Men (1995),Comedy|Romance

# ratings.csv
userId,movieId,rating,timestamp
1,122,2.0,945544824
1,172,1.0,945544871
1,1221,5.0,945544788

# tags.csv
userId,movieId,tag,timestamp
28,63062,angelina jolie,1263047558
40,4973,Poetic,1436439070
40,117533,privacy,1436439140

# genome-scores.csv
movieId,tagId,relevance
1,1,0.02400000000000002
1,2,0.02400000000000002
1,3,0.05475000000000002

# genome-tags.csv
tagId,tag
1,007
2,007 (series)
3,18th century
</pre>

## Shell Script
<pre>
# Download the ml-latest zip file from the movielens web site
wget http://files.grouplens.org/datasets/movielens/ml-latest.zip

# unzip the downloaded ml-latest zip file
unzip ml-latest.zip

hdfs dfs -mkdir -p /user/cloudera/rawdata/handson_train/movielens/latest/movies
hdfs dfs -mkdir -p /user/cloudera/rawdata/handson_train/movielens/latest/ratings
hdfs dfs -mkdir -p /user/cloudera/rawdata/handson_train/movielens/latest/tags
hdfs dfs -mkdir -p /user/cloudera/rawdata/handson_train/movielens/latest/genome.scores
hdfs dfs -mkdir -p /user/cloudera/rawdata/handson_train/movielens/latest/genome.tags

hdfs dfs -moveFromLocal ml-latest/movies.csv /user/cloudera/rawdata/handson_train/movielens/latest/movies
hdfs dfs -moveFromLocal ml-latest/ratings.csv /user/cloudera/rawdata/handson_train/movielens/latest/ratings
hdfs dfs -moveFromLocal ml-latest/tags.csv /user/cloudera/rawdata/handson_train/movielens/latest/tags
hdfs dfs -moveFromLocal ml-latest/genome-tags.csv /user/cloudera/rawdata/handson_train/movielens/latest/genome.tags
hdfs dfs -moveFromLocal ml-latest/genome-scores.csv /user/cloudera/rawdata/handson_train/movielens/latest/genome.scores
</pre>

## Data File Placement

The data files list above are placed in multiple user-created sub-directories in HDFS named above within the main directory: - <i>/user/cloudera/rawdata/handson_train/movielens/latest/</i>.
