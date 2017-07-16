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
