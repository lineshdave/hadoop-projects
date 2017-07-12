## Hive SerDe
![image](https://user-images.githubusercontent.com/19809692/28099608-8a6e84fc-668b-11e7-8ac1-4510192063a8.png)
### Examples
#### Setting up airport table
<pre>
-- create external table for airports
create external table airports (
    iata string,
    airport string,
    city string,
    state string,
    country string,
    geolat float,
    geolong float
)
row format serde 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
with serdeproperties (
   "separatorChar" = ",",
   "quoteChar"     = "\"",
   "escapeChar"    = "\\"
)
stored as textfile
location '/user/ldave2001/rawdata/handson_train/airline_performance/airports';
</pre>

#### Description and Sample Table Query Output From Above Table
<pre>
0: jdbc:hive2://iop-bi-master.imdemocloud.com> desc airports;
+-----------+------------+--------------------+--+
| col_name  | data_type  |      comment       |
+-----------+------------+--------------------+--+
| iata      | string     | from deserializer  |
| airport   | string     | from deserializer  |
| city      | string     | from deserializer  |
| state     | string     | from deserializer  |
| country   | string     | from deserializer  |
| geolat    | string     | from deserializer  |
| geolong   | string     | from deserializer  |
+-----------+------------+--------------------+--+
7 rows selected (0.04 seconds)

0: jdbc:hive2://iop-bi-master.imdemocloud.com> select count(1) from airports;
INFO  : Number of reduce tasks determined at compile time: 1
INFO  : In order to change the average load for a reducer (in bytes):
INFO  :   set hive.exec.reducers.bytes.per.reducer=<number>
INFO  : In order to limit the maximum number of reducers:
INFO  :   set hive.exec.reducers.max=<number>
INFO  : In order to set a constant number of reducers:
INFO  :   set mapreduce.job.reduces=<number>
INFO  : number of splits:1
INFO  : Submitting tokens for job: job_1494894036184_3381
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/prox                       y/application_1494894036184_3381/
INFO  : Starting Job = job_1494894036184_3381, Tracking URL = http://iop-bi-mast                       er.imdemocloud.com:8088/proxy/application_1494894036184_3381/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894                       036184_3381
INFO  : Hadoop job information for Stage-1: number of mappers: 1; number of redu                       cers: 1
INFO  : 2017-07-11 21:45:05,255 Stage-1 map = 0%,  reduce = 0%
INFO  : 2017-07-11 21:45:10,380 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU                        2.25 sec
INFO  : 2017-07-11 21:45:15,496 Stage-1 map = 100%,  reduce = 100%, Cumulative C                       PU 4.53 sec
INFO  : MapReduce Total cumulative CPU time: 4 seconds 530 msec
INFO  : Ended Job = job_1494894036184_3381
+-------+--+
|  _c0  |
+-------+--+
| 3377  |
+-------+--+
1 row selected (16.461 seconds)
</pre>

#### Setting up carriers table
<pre>
-- create external table for carriers
create external table carriers (
    cdde varchar(4),
    description varchar(30)
)
row format serde 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
with serdeproperties (
   "separatorChar" = ",",
   "quoteChar"     = "\"",
   "escapeChar"    = "\\"
)
stored as textfile
location '/user/ldave2001/rawdata/handson_train/airline_performance/carriers';
</pre>

#### Description and Sample Table Query Output From Above Table
<pre>
0: jdbc:hive2://iop-bi-master.imdemocloud.com> desc carriers;
++--------------+------------+--------------------+--+
|   col_name   | data_type  |      comment       |
+--------------+------------+--------------------+--+
| cdde         | string     | from deserializer  |
| description  | string     | from deserializer  |
+--------------+------------+--------------------+--+
2 rows selected (0.073 seconds)

0: jdbc:hive2://iop-bi-master.imdemocloud.com> select count(1) from carriers;
INFO  : Number of reduce tasks determined at compile time: 1
INFO  : In order to change the average load for a reducer (in bytes):
INFO  :   set hive.exec.reducers.bytes.per.reducer=<number>
INFO  : In order to limit the maximum number of reducers:
INFO  :   set hive.exec.reducers.max=<number>
INFO  : In order to set a constant number of reducers:
INFO  :   set mapreduce.job.reduces=<number>
INFO  : number of splits:1
INFO  : Submitting tokens for job: job_1494894036184_3382
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3382/
INFO  : Starting Job = job_1494894036184_3382, Tracking URL = http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3382/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3382
INFO  : Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
INFO  : 2017-07-11 22:02:11,420 Stage-1 map = 0%,  reduce = 0%
INFO  : 2017-07-11 22:02:17,566 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 1.84 sec
INFO  : 2017-07-11 22:02:22,678 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 4.42 sec
INFO  : MapReduce Total cumulative CPU time: 4 seconds 420 msec
INFO  : Ended Job = job_1494894036184_3382
+-------+--+
|  _c0  |
+-------+--+
| 1492  |
+-------+--+
</pre?

#### Setting up plane_info table
<pre>
-- create external table for plane information
create external table plane_info (
    tailnum varchar(4),
    type varchar(30),
    manufacturer string,
    issue_date varchar(16),
    model varchar(10),
    status varchar(10),
    aircraft_type varchar(30),
    pyear int
)
row format serde 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
with serdeproperties (
   "separatorChar" = ",",
   "quoteChar"     = "\"",
   "escapeChar"    = "\\"
)
stored as textfile
location '/user/ldave2001/rawdata/handson_train/airline_performance/plane_data';
</pre>

#### Description and Sample Table Query Output From Above Table
<pre>
0: jdbc:hive2://iop-bi-master.imdemocloud.com> desc plane_info;
+----------------+------------+--------------------+--+
|    col_name    | data_type  |      comment       |
+----------------+------------+--------------------+--+
| tailnum        | string     | from deserializer  |
| type           | string     | from deserializer  |
| manufacturer   | string     | from deserializer  |
| issue_date     | string     | from deserializer  |
| model          | string     | from deserializer  |
| status         | string     | from deserializer  |
| aircraft_type  | string     | from deserializer  |
| pyear          | string     | from deserializer  |
+----------------+------------+--------------------+--+
8 rows selected (0.058 seconds)

0: jdbc:hive2://iop-bi-master.imdemocloud.com> select count(1) from plane_info;
INFO  : Number of reduce tasks determined at compile time: 1
INFO  : In order to change the average load for a reducer (in bytes):
INFO  :   set hive.exec.reducers.bytes.per.reducer=<number>
INFO  : In order to limit the maximum number of reducers:
INFO  :   set hive.exec.reducers.max=<number>
INFO  : In order to set a constant number of reducers:
INFO  :   set mapreduce.job.reduces=<number>
INFO  : number of splits:1
INFO  : Submitting tokens for job: job_1494894036184_3383
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3383/
INFO  : Starting Job = job_1494894036184_3383, Tracking URL = http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3383/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3383
INFO  : Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
INFO  : 2017-07-11 22:05:55,363 Stage-1 map = 0%,  reduce = 0%
INFO  : 2017-07-11 22:06:00,486 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 2.39 sec
INFO  : 2017-07-11 22:06:05,604 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 4.73 sec
INFO  : MapReduce Total cumulative CPU time: 4 seconds 730 msec
INFO  : Ended Job = job_1494894036184_3383
+-------+--+
|  _c0  |
+-------+--+
| 5030  |
+-------+--+
1 row selected (17.417 seconds)
</pre>


#### Setting up a new HDFS file with non default delimiter
<pre>
-- inserting into hdfs directory as text file with non-default delimiter
insert overwrite directory '/user/ldave2001/output/handson_train/hive/insrt_directory'
row format delimited
fields terminated by '::::'
select * from airports limit  100;
</pre>

#### Output
<pre>
INFO  : Number of reduce tasks determined at compile time: 1
INFO  : In order to change the average load for a reducer (in bytes):
INFO  :   set hive.exec.reducers.bytes.per.reducer=<number>
INFO  : In order to limit the maximum number of reducers:
INFO  :   set hive.exec.reducers.max=<number>
INFO  : In order to set a constant number of reducers:
INFO  :   set mapreduce.job.reduces=<number>
INFO  : number of splits:1
INFO  : Submitting tokens for job: job_1494894036184_3384
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3384/
INFO  : Starting Job = job_1494894036184_3384, Tracking URL = http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3384/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3384
INFO  : Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
INFO  : 2017-07-11 22:07:14,000 Stage-1 map = 0%,  reduce = 0%
INFO  : 2017-07-11 22:07:20,161 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 1.82 sec
INFO  : 2017-07-11 22:07:25,284 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 4.06 sec
INFO  : MapReduce Total cumulative CPU time: 4 seconds 60 msec
INFO  : Ended Job = job_1494894036184_3384
INFO  : Moving data to: /user/ldave2001/output/handson_train/hive/insrt_directory from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/output/handson_train/hive/insrt_directory/.hive-staging_hive_2017-07-11_22-07-08_888_4700161562485339321-63/-ext-10000
No rows affected (17.465 seconds)

HDFS File Listing:
[ldave2001@iop-bi-master airline_ontime_perf]$ hdfs dfs -ls /user/ldave2001/output/handson_train/hive/insrt_directory
Found 1 items
-rwxrwx--x+  3 ldave2001 ldave2001       6184 2017-07-11 22:07 /user/ldave2001/output/handson_train/hive/insrt_directory/000000_0
</pre>


