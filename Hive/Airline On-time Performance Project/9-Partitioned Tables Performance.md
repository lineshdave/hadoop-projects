## Partitioned Tables Performance
Test the performance of queries against the partitioned tables in comparison with the non-partitioned tables. Consider especially the number of mappers and reducers used in the MapReducers Tasks rather than just focussing on the query execution time.

#### Query-1

##### Non-partitioned Table Query 
<pre>
select month, count(1) from pq_airline_timing where year = 2006 group by month;
</pre>

##### MapReduce Tasks Execution
<pre>
INFO  : Number of reduce tasks not specified. Estimated from input data size: 2
INFO  : In order to change the average load for a reducer (in bytes):
INFO  :   set hive.exec.reducers.bytes.per.reducer=<number>
INFO  : In order to limit the maximum number of reducers:
INFO  :   set hive.exec.reducers.max=<number>
INFO  : In order to set a constant number of reducers:
INFO  :   set mapreduce.job.reduces=<number>
INFO  : number of splits:2
INFO  : Submitting tokens for job: job_1494894036184_3306
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3306/
INFO  : Starting Job = job_1494894036184_3306, Tracking URL = http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3306/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3306
INFO  : Hadoop job information for Stage-1: number of mappers: 2; number of reducers: 2
INFO  : 2017-07-10 14:51:38,469 Stage-1 map = 0%,  reduce = 0%
INFO  : 2017-07-10 14:51:45,627 Stage-1 map = 50%,  reduce = 0%, Cumulative CPU 8.96 sec
INFO  : 2017-07-10 14:51:47,681 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 20.32 sec
INFO  : 2017-07-10 14:51:50,748 Stage-1 map = 100%,  reduce = 50%, Cumulative CPU 22.76 sec
INFO  : 2017-07-10 14:51:51,770 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 25.19 sec
INFO  : MapReduce Total cumulative CPU time: 25 seconds 190 msec
INFO  : Ended Job = job_1494894036184_3306
+--------+---------+--+
| month  |   _c1   |
+--------+---------+--+
| 2      | 531247  |
| 4      | 585351  |
| 6      | 598315  |
| 8      | 628732  |
| 10     | 611718  |
| 12     | 604758  |
| 1      | 581287  |
| 3      | 605217  |
| 5      | 602919  |
| 7      | 621244  |
| 9      | 584937  |
| 11     | 586197  |
+--------+---------+--+
12 rows selected (19.471 seconds)
<pre>

##### Partitioned Table-1 QUery 
<pre>
select month, count(1) from pq_airline_timing_part where year = 2006 group by month;
</pre>

##### MapReduce Tasks Execution
<pre>
INFO  : Number of reduce tasks not specified. Estimated from input data size: 1
INFO  : In order to change the average load for a reducer (in bytes):
INFO  :   set hive.exec.reducers.bytes.per.reducer=<number>
INFO  : In order to limit the maximum number of reducers:
INFO  :   set hive.exec.reducers.max=<number>
INFO  : In order to set a constant number of reducers:
INFO  :   set mapreduce.job.reduces=<number>
INFO  : number of splits:1
INFO  : Submitting tokens for job: job_1494894036184_3307
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3307/
INFO  : Starting Job = job_1494894036184_3307, Tracking URL = http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3307/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3307
INFO  : Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
INFO  : 2017-07-10 15:02:12,716 Stage-1 map = 0%,  reduce = 0%
INFO  : 2017-07-10 15:02:21,927 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 11.88 sec
INFO  : 2017-07-10 15:02:28,068 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 14.27 sec
INFO  : MapReduce Total cumulative CPU time: 14 seconds 270 msec
INFO  : Ended Job = job_1494894036184_3307
+--------+---------+--+
| month  |   _c1   |
+--------+---------+--+
| 1      | 581287  |
| 2      | 531247  |
| 3      | 605217  |
| 4      | 585351  |
| 5      | 602919  |
| 6      | 598315  |
| 7      | 621244  |
| 8      | 628732  |
| 9      | 584937  |
| 10     | 611718  |
| 11     | 586197  |
| 12     | 604758  |
+--------+---------+--+
12 rows selected (21.585 seconds)
</pre>

##### Partitioned Table-2 QUery 
<pre>
select month, count(1) from pq_airline_timing_part2 where year = 2006 group by month;
</pre>

##### MapReduce Tasks Execution
<pre>
INFO  : Number of reduce tasks not specified. Estimated from input data size: 1
INFO  : In order to change the average load for a reducer (in bytes):
INFO  :   set hive.exec.reducers.bytes.per.reducer=<number>
INFO  : In order to limit the maximum number of reducers:
INFO  :   set hive.exec.reducers.max=<number>
INFO  : In order to set a constant number of reducers:
INFO  :   set mapreduce.job.reduces=<number>
INFO  : number of splits:1
INFO  : Submitting tokens for job: job_1494894036184_3308
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3308/
INFO  : Starting Job = job_1494894036184_3308, Tracking URL = http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3308/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3308
INFO  : Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
INFO  : 2017-07-10 15:13:13,773 Stage-1 map = 0%,  reduce = 0%
INFO  : 2017-07-10 15:13:22,979 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 8.59 sec
INFO  : 2017-07-10 15:13:28,093 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 11.18 sec
INFO  : MapReduce Total cumulative CPU time: 11 seconds 180 msec
INFO  : Ended Job = job_1494894036184_3308
+--------+---------+--+
| month  |   _c1   |
+--------+---------+--+
| 1      | 581287  |
| 2      | 531247  |
| 3      | 605217  |
| 4      | 585351  |
| 5      | 602919  |
| 6      | 598315  |
| 7      | 621244  |
| 8      | 628732  |
| 9      | 584937  |
| 10     | 611718  |
| 11     | 586197  |
| 12     | 604758  |
+--------+---------+--+
12 rows selected (20.366 seconds)
</pre>

#### Query-2

##### Non-partitioned Table Query 
<pre>
select count(1) from pq_airline_timing where year = 2006 and month=1;
</pre>

##### MapReduce Tasks Execution
<pre>
INFO  : Number of reduce tasks determined at compile time: 1
INFO  : In order to change the average load for a reducer (in bytes):
INFO  :   set hive.exec.reducers.bytes.per.reducer=<number>
INFO  : In order to limit the maximum number of reducers:
INFO  :   set hive.exec.reducers.max=<number>
INFO  : In order to set a constant number of reducers:
INFO  :   set mapreduce.job.reduces=<number>
INFO  : number of splits:2
INFO  : Submitting tokens for job: job_1494894036184_3309
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3309/
INFO  : Starting Job = job_1494894036184_3309, Tracking URL = http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3309/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3309
INFO  : Hadoop job information for Stage-1: number of mappers: 2; number of reducers: 1
INFO  : 2017-07-10 15:19:45,207 Stage-1 map = 0%,  reduce = 0%
INFO  : 2017-07-10 15:19:53,384 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 19.43 sec
INFO  : 2017-07-10 15:19:58,494 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 19.43 sec
INFO  : MapReduce Total cumulative CPU time: 19 seconds 430 msec
INFO  : Ended Job = job_1494894036184_3309
+---------+--+
|   _c0   |
+---------+--+
| 581287  |
+---------+--+
1 row selected (20.535 seconds)
</pre>

##### Partitioned Table-1 QUery 
<pre>
select count(1) from pq_airline_timing_part where year = 2006 and month=1;
</pre>

##### MapReduce Tasks Execution
<pre>
INFO  : Number of reduce tasks determined at compile time: 1
INFO  : In order to change the average load for a reducer (in bytes):
INFO  :   set hive.exec.reducers.bytes.per.reducer=<number>
INFO  : In order to limit the maximum number of reducers:
INFO  :   set hive.exec.reducers.max=<number>
INFO  : In order to set a constant number of reducers:
INFO  :   set mapreduce.job.reduces=<number>
INFO  : number of splits:1
INFO  : Submitting tokens for job: job_1494894036184_3310
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3310/
INFO  : Starting Job = job_1494894036184_3310, Tracking URL = http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3310/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3310
INFO  : Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
INFO  : 2017-07-10 15:26:39,589 Stage-1 map = 0%,  reduce = 0%
INFO  : 2017-07-10 15:26:46,756 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 8.96 sec
INFO  : 2017-07-10 15:26:52,890 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 11.47 sec
INFO  : MapReduce Total cumulative CPU time: 11 seconds 470 msec
INFO  : Ended Job = job_1494894036184_3310
+---------+--+
|   _c0   |
+---------+--+
| 581287  |
+---------+--+
1 row selected (19.53 seconds)
</pre>

##### Partitioned Table-2 QUery 
<pre>
select count(1) from pq_airline_timing_part2 where year = 2006 and month=1;
</pre>

##### MapReduce Tasks Execution
<pre>
+---------+--+
|   _c0   |
+---------+--+
| 581287  |
+---------+--+
1 row selected (0.057 seconds)
</pre>
