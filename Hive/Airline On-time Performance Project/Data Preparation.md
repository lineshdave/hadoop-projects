### Data Preparation
This project used IBM's imdemocloud platform instead of the local Cloudera platform to take advantag of high disk space and CPU available in the cloud environment than on the local machine.<br>
<br>
The connection to the cloud environment is established using PuTTY. Prior to that, an account is already setup on IBM's cloud sandbox. Ensure that HDFS, Hive, Sqoop, and Pig are up and running.

### Data Loading Script
<pre>
wget http://stat-computing.org/dataexpo/2009/2006.csv.bz2
wget http://stat-computing.org/dataexpo/2009/2007.csv.bz2
wget http://stat-computing.org/dataexpo/2009/2008.csv.bz2


hdfs dfs -mkdir -p /user/ldave2001/rawdata/handson_train/airline_performance/flights

bzip2 -d 2006.csv.bz2
bzip2 -d 2007.csv.bz2
bzip2 -d 2008.csv.bz2

hdfs dfs -moveFromLocal 2006.csv /user/ldave2001/rawdata/handson_train/airline_performance/flights
hdfs dfs -moveFromLocal 2007.csv /user/ldave2001/rawdata/handson_train/airline_performance/flights
hdfs dfs -moveFromLocal 2008.csv /user/ldave2001/rawdata/handson_train/airline_performance/flights


wget http://stat-computing.org/dataexpo/2009/airports.csv
wget http://stat-computing.org/dataexpo/2009/carriers.csv
wget http://stat-computing.org/dataexpo/2009/plane-data.csv

hdfs dfs -mkdir -p /user/ldave2001/rawdata/handson_train/airline_performance/airports
hdfs dfs -mkdir -p /user/ldave2001/rawdata/handson_train/airline_performance/carriers
hdfs dfs -mkdir -p /user/ldave2001/rawdata/handson_train/airline_performance/plane_data

hdfs dfs -moveFromLocal airports.csv    /user/ldave2001/rawdata/handson_train/airline_performance/airports
hdfs dfs -moveFromLocal carriers.csv    /user/ldave2001/rawdata/handson_train/airline_performance/carriers
hdfs dfs -moveFromLocal plane-data.csv  /user/ldave2001/rawdata/handson_train/airline_performance/plane_data
</pre>

### Validation
Run the following command "hdfs dfs -ls /users/ldave2001/rawdata/handson_train/airline_performance/". If this directory exist, and there are files underneath it, then the above data loading has been successful.
