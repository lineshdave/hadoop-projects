# Custom Built Functions

There are multiple publicly available and/or open-source projects or repositories available for Hive Custom Built Functions. One such example is [HiveSwam](https://github.com/livingsocial/HiveSwarm/blob/master/README.markdown).

## JAR Addition in Hive
The JAR files can be downloaded and built using maven on the local machine and then added to Hive in 3 different ways depending on the machine setup.<br><br>
If Cloud environment such as imdemocloud is used:<br>
<OL>
<LI>Copy the JAR file to the HDFS file system (hdfs dfs -put HiveSwarm-1.0-SNAPSHOT.jar .)</LI>
<LI>Login to Hive using the beeline command (use appropriate database)</LI>
<LI>Execute the following command "add jar hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/HiveSwarm-1.0-SNAPSHOT.jar"</LI>
</OL>

If the same Cloud environment is used as above, then the JAR file can be added as follows:<br>
<OL>
<LI>Login to Hive using simple "hive" command<LI>
<LI>Execute the command "add jar /mnt/home/ldave2001/hadoop-training-projects/hive/airline_ontime_perf/HiveSwarm-1.0-SNAPSHOT.jar"</LI>
</OL>
Just a word of caution - JAR file added in Hive using this method does NOT make it available in beeline. So, beware!
<br><br>

Lastly, if you are working on a local VM such as Cloudera, then the following steps may be required to add the JAR file:<br>
<OL>
<LI>Login to Hive using the beeline command (use appropriate database)</LI>
<LI>Execute the command "add jar file:///user/cloudera/hadoop-training-projects/hive/airline_ontime_perf/HiveSwarm-1.0-SNAPSHOT.jar"</LI>
</OL>

## Multiple Join Query without Custom Built Functions

### Query Statement
<pre>
select flightnum, year, month, dayofmonth, dayofweek, c.description, f.tailnum, p.aircraft_type,
        CONCAT(a.airport, ' ', a.city, ', ', a.state, ', ', a.country ) origin,
        CONCAT(b.airport, ' ', b.city, ', ', b.state, ', ', b.country ) dest
from pq_airline_timing f
        join carriers c on f.uniquecarrier = c.cdde
        join airports a on f.origin = a.iata
        join airports b on f.dest = b.iata
        join plane_info p on p.tailnum = f.tailnum
 where f.year = 2006 and month = 1;
</pre>

### Map Reduce Tasks Execution
<pre>
INFO  : Execution completed successfully
INFO  : MapredLocal task succeeded
INFO  : Number of reduce tasks is set to 0 since there's no reduce operator
INFO  : number of splits:2
INFO  : Submitting tokens for job: job_1494894036184_3531
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3531/
INFO  : Starting Job = job_1494894036184_3531, Tracking URL = http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3531/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3531
INFO  : Hadoop job information for Stage-9: number of mappers: 2; number of reducers: 0
INFO  : 2017-07-15 19:55:24,017 Stage-9 map = 0%,  reduce = 0%
INFO  : 2017-07-15 19:56:11,059 Stage-9 map = 16%,  reduce = 0%, Cumulative CPU 109.19 sec
INFO  : 2017-07-15 19:56:29,452 Stage-9 map = 32%,  reduce = 0%, Cumulative CPU 149.77 sec
INFO  : 2017-07-15 19:56:50,900 Stage-9 map = 49%,  reduce = 0%, Cumulative CPU 200.26 sec
INFO  : 2017-07-15 19:57:12,333 Stage-9 map = 61%,  reduce = 0%, Cumulative CPU 248.82 sec
INFO  : 2017-07-15 19:57:24,595 Stage-9 map = 78%,  reduce = 0%, Cumulative CPU 276.25 sec
INFO  : 2017-07-15 19:57:49,107 Stage-9 map = 89%,  reduce = 0%, Cumulative CPU 303.1 sec
INFO  : 2017-07-15 19:58:22,771 Stage-9 map = 100%,  reduce = 0%, Cumulative CPU 341.47 sec
INFO  : MapReduce Total cumulative CPU time: 5 minutes 41 seconds 470 msec
INFO  : Ended Job = job_1494894036184_3531
GC overhead limit exceeded
Closing: 0: jdbc:hive2://iop-bi-master.imdemocloud.com:10000/ok_airline_ld
</pre>

## Multiple Join Query with Custom Built Function

### Query Statement and Setup
<pre>
add jar hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/HiveSwarm-1.0-SNAPSHOT.jar;

create temporary function gps_distance_from as 'com.livingsocial.hive.udf.gpsDistanceFrom';

--- creating a hive view
drop view airport_without_header;
create view airport_without_header as
select * from airports where iata <> 'iata';

-- query
select from_airport, to_airport, MIN(dist) minimum_distance
from
 (select tbl1.airport from_airport, tbl2.airport to_airport,
         gps_distance_from(tbl1.geolat, tbl1.geolong, tbl2.geolat, tbl2.geolong ) dist
  from (select  iata, airport, geolong, geolat from airport_without_header) tbl1
        full outer join
       (select  iata, airport, geolong, geolat from airport_without_header) tbl2
       where tbl1.iata <> tbl2.iata
 ) v
group by from_airport, to_airport
order by minimum_distance
limit 10;
</pre>

### Map Reduce Tasks Exectuion
<pre>
INFO  : Number of reduce tasks determined at compile time: 1
INFO  : In order to change the average load for a reducer (in bytes):
INFO  :   set hive.exec.reducers.bytes.per.reducer=<number>
INFO  : In order to limit the maximum number of reducers:
INFO  :   set hive.exec.reducers.max=<number>
INFO  : In order to set a constant number of reducers:
INFO  :   set mapreduce.job.reduces=<number>
INFO  : number of splits:1
INFO  : Submitting tokens for job: job_1494894036184_3540
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036 184_3540/
INFO  : Starting Job = job_1494894036184_3540, Tracking URL = http://iop-bi-master.imdemocloud.com:8088/ proxy/application_1494894036184_3540/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3540
INFO  : Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
INFO  : 2017-07-15 20:51:30,120 Stage-1 map = 0%,  reduce = 0%
INFO  : 2017-07-15 20:51:36,296 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 4.1 sec
INFO  : 2017-07-15 20:51:47,549 Stage-1 map = 100%,  reduce = 88%, Cumulative CPU 16.12 sec
INFO  : 2017-07-15 20:51:55,727 Stage-1 map = 100%,  reduce = 93%, Cumulative CPU 27.7 sec
INFO  : 2017-07-15 20:52:07,994 Stage-1 map = 100%,  reduce = 98%, Cumulative CPU 41.49 sec
INFO  : 2017-07-15 20:52:23,323 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 58.77 sec
INFO  : MapReduce Total cumulative CPU time: 58 seconds 770 msec
INFO  : Ended Job = job_1494894036184_3540
INFO  : Number of reduce tasks not specified. Estimated from input data size: 3
INFO  : In order to change the average load for a reducer (in bytes):
INFO  :   set hive.exec.reducers.bytes.per.reducer=<number>
INFO  : In order to limit the maximum number of reducers:
INFO  :   set hive.exec.reducers.max=<number>
INFO  : In order to set a constant number of reducers:
INFO  :   set mapreduce.job.reduces=<number>
INFO  : number of splits:3
INFO  : Submitting tokens for job: job_1494894036184_3541
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036 184_3541/
INFO  : Starting Job = job_1494894036184_3541, Tracking URL = http://iop-bi-master.imdemocloud.com:8088/ proxy/application_1494894036184_3541/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3541
INFO  : Hadoop job information for Stage-2: number of mappers: 3; number of reducers: 3
INFO  : 2017-07-15 20:52:31,734 Stage-2 map = 0%,  reduce = 0%
INFO  : 2017-07-15 20:52:48,098 Stage-2 map = 44%,  reduce = 0%, Cumulative CPU 55.06 sec
INFO  : 2017-07-15 20:52:51,169 Stage-2 map = 56%,  reduce = 0%, Cumulative CPU 65.78 sec
INFO  : 2017-07-15 20:52:54,235 Stage-2 map = 67%,  reduce = 0%, Cumulative CPU 72.68 sec
INFO  : 2017-07-15 20:52:57,300 Stage-2 map = 78%,  reduce = 0%, Cumulative CPU 79.42 sec
INFO  : 2017-07-15 20:53:02,413 Stage-2 map = 89%,  reduce = 11%, Cumulative CPU 89.38 sec
INFO  : 2017-07-15 20:53:05,476 Stage-2 map = 100%,  reduce = 22%, Cumulative CPU 95.39 sec
INFO  : 2017-07-15 20:53:08,539 Stage-2 map = 100%,  reduce = 62%, Cumulative CPU 105.1 sec
INFO  : 2017-07-15 20:53:11,609 Stage-2 map = 100%,  reduce = 74%, Cumulative CPU 129.21 sec
INFO  : 2017-07-15 20:53:14,683 Stage-2 map = 100%,  reduce = 85%, Cumulative CPU 141.18 sec
INFO  : 2017-07-15 20:53:17,750 Stage-2 map = 100%,  reduce = 97%, Cumulative CPU 153.52 sec
INFO  : 2017-07-15 20:53:18,771 Stage-2 map = 100%,  reduce = 100%, Cumulative CPU 157.06 sec
INFO  : MapReduce Total cumulative CPU time: 2 minutes 37 seconds 60 msec
INFO  : Ended Job = job_1494894036184_3541
INFO  : Number of reduce tasks determined at compile time: 1
INFO  : In order to change the average load for a reducer (in bytes):
INFO  :   set hive.exec.reducers.bytes.per.reducer=<number>
INFO  : In order to limit the maximum number of reducers:
INFO  :   set hive.exec.reducers.max=<number>
INFO  : In order to set a constant number of reducers:
INFO  :   set mapreduce.job.reduces=<number>
INFO  : number of splits:2
INFO  : Submitting tokens for job: job_1494894036184_3542
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036 184_3542/
INFO  : Starting Job = job_1494894036184_3542, Tracking URL = http://iop-bi-master.imdemocloud.com:8088/ proxy/application_1494894036184_3542/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3542
INFO  : Hadoop job information for Stage-3: number of mappers: 2; number of reducers: 1
INFO  : 2017-07-15 20:53:25,159 Stage-3 map = 0%,  reduce = 0%
INFO  : 2017-07-15 20:53:35,389 Stage-3 map = 9%,  reduce = 0%, Cumulative CPU 22.05 sec
INFO  : 2017-07-15 20:53:38,454 Stage-3 map = 22%,  reduce = 0%, Cumulative CPU 29.34 sec
INFO  : 2017-07-15 20:53:41,527 Stage-3 map = 37%,  reduce = 0%, Cumulative CPU 36.44 sec
INFO  : 2017-07-15 20:53:44,590 Stage-3 map = 70%,  reduce = 0%, Cumulative CPU 43.98 sec
INFO  : 2017-07-15 20:53:47,652 Stage-3 map = 100%,  reduce = 0%, Cumulative CPU 47.37 sec
INFO  : 2017-07-15 20:53:49,693 Stage-3 map = 100%,  reduce = 100%, Cumulative CPU 49.77 sec
INFO  : MapReduce Total cumulative CPU time: 49 seconds 770 msec
INFO  : Ended Job = job_1494894036184_3542
</pre>

### Output
<pre>
+-----------------------------+-----------------------------+-----------------------+--+
|        from_airport         |         to_airport          |   minimum_distance    |
+-----------------------------+-----------------------------+-----------------------+--+
| Hilton Head                 | Hilton Head                 | 0.009299350115867511  |
| Sawyer                      | Marquette County Airport    | 0.022554255408101666  |
| Marquette County Airport    | Sawyer                      | 0.022554255408101666  |
| MC Clellan-Palomar Airport  | McClellan-Palomar           | 0.11034224541665748   |
| McClellan-Palomar           | MC Clellan-Palomar Airport  | 0.11034224541665748   |
| University Park             | University Park             | 0.18271236861712067   |
| E 34th St Heliport          | New York Skyports Inc. SPB  | 0.596548041983421     |
| New York Skyports Inc. SPB  | E 34th St Heliport          | 0.596548041983421     |
| Sitka                       | Sitka SPB                   | 0.6736674884017871    |
| Sitka SPB                   | Sitka                       | 0.6736674884017871    |
+-----------------------------+-----------------------------+-----------------------+--+
10 rows selected (146.068 seconds)
</pre>
