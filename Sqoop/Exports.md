### Export hdfs files into MySQL Database

#### Step:1
Create database and the underlying tables

<pre>
create database sqoop_handson;

use sqoop_handson;

create table nyse_dividends (exchange varchar(20), stock_symbol varchar(5), datestring varchar(20), value float(10,2));

create table altered_nyse_dividends (stock_symbol varchar(5), value float(10,2), datestring varchar(20), exchange varchar(20));
</pre>

#### Step:2
Use sqoop-export command to export the hdfs data available in file(s) to the MySQL database and tables created in Step-1

<pre>
sqoop-export --connect jdbc:mysql://quickstart:3306/sqoop_handson --username root --table nyse_dividends --export-dir /user/cloudera/rawdata/handson_train/nyse --mapreduce-job-name 'nyse dividend mysql export' --fields-terminated-by ',' -m 2 --password-file /user/cloudera/passwordfile

sqoop-export --connect jdbc:mysql://quickstart:3306/sqoop_handson --username root --table altered_nyse_dividends --export-dir /user/cloudera/rawdata/handson_train/nyse --mapreduce-job-name 'nyse dividend mysql export' --fields-terminated-by ',' -m 1 --password-file /user/cloudera/passwordfile --columns exchange,stock_symbol,datestring,value
</pre>
