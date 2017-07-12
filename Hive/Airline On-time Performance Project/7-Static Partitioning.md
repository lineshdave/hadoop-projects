# Static Partitioning
Hive tables can be partitioned on one or more column values (example - year) in order to reduce the scanning and retrieval of records due to queries from HDFS based systems.

## Example-1 (One Column)
#### Table Creation
<pre>
-- create an external parquet table with partition (by year) on airline timing
create external table pq_airline_timing_part
        (month tinyint,dayofmonth tinyint,dayofweek tinyint,
        deptime smallint, crsdeptime smallint, arrtime smallint, crsarrtime smallint,
        uniquecarrier string, flightnum string, tailnum string, actualelapsedtime smallint,
        crselapsedtime smallint, airtime smallint, arrdelay smallint, depdelay smallint,
        origin string, dest string, distance smallint, taxiin string, taxiout string,
        cancelled string, cancellationcode string, diverted string, carrierdelay smallint,
        weatherdelay smallint, nasdelay smallint, securitydelay smallint, lateaircraftdelay smallint)
partitioned by (year smallint)
stored as parquet
location '/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd';

-- create results in successful creation of the external parquet table and the output is No rows selected

-- manually add a partition to the above external table defined with a partition
alter table pq_airline_timing_part add partition (year=2007);

-- alter results in successful update of the external parquet table and the output is No rows selected

-- insert data into a table partition using static partitioning
insert overwrite table pq_airline_timing_part partition(year=2007)
select
        month,
        dayofmonth,
        dayofweek,
        deptime,
        crsdeptime ,
        arrtime,
        crsarrtime ,
        uniquecarrier  ,
        flightnum ,
        tailnum ,
        actualelapsedtime  ,
        crselapsedtime  ,
        airtime  ,
        arrdelay ,
        depdelay ,
        origin  ,
        dest ,
        distance   ,
        taxiin ,
        taxiout   ,
        cancelled ,
        cancellationcode  ,
        diverted   ,
        carrierdelay   ,
        weatherdelay  ,
        nasdelay ,
        securitydelay    ,
        lateaircraftdelay
from airline_timing where year = 2007;

-- above insert statement results in the following mapreduce task shown in the next section
</pre>

### MapReduce Task Output due to Static Partition
<pre>
INFO  : Number of reduce tasks is set to 0 since there's no reduce operator
INFO  : number of splits:7
INFO  : Submitting tokens for job: job_1494894036184_3266
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894    036184_3266/
INFO  : Starting Job = job_1494894036184_3266, Tracking URL = http://iop-bi-master.imdemocloud.com:80    88/proxy/application_1494894036184_3266/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3266
INFO  : Hadoop job information for Stage-1: number of mappers: 7; number of reducers: 0
INFO  : 2017-07-09 21:06:51,641 Stage-1 map = 0%,  reduce = 0%
INFO  : 2017-07-09 21:06:59,831 Stage-1 map = 14%,  reduce = 0%, Cumulative CPU 7.32 sec
INFO  : 2017-07-09 21:07:00,860 Stage-1 map = 29%,  reduce = 0%, Cumulative CPU 14.76 sec
INFO  : 2017-07-09 21:07:01,884 Stage-1 map = 62%,  reduce = 0%, Cumulative CPU 67.07 sec
INFO  : 2017-07-09 21:07:03,938 Stage-1 map = 64%,  reduce = 0%, Cumulative CPU 69.87 sec
INFO  : 2017-07-09 21:07:14,164 Stage-1 map = 86%,  reduce = 0%, Cumulative CPU 114.6 sec
INFO  : 2017-07-09 21:07:26,437 Stage-1 map = 93%,  reduce = 0%, Cumulative CPU 145.73 sec
INFO  : 2017-07-09 21:07:27,458 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 147.56 sec
INFO  : MapReduce Total cumulative CPU time: 2 minutes 27 seconds 560 msec
INFO  : Ended Job = job_1494894036184_3266
INFO  : Stage-4 is selected by condition resolver.
INFO  : Stage-3 is filtered out by condition resolver.
INFO  : Stage-5 is filtered out by condition resolver.
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_trai    n/airline_performance/flights_parquet_partd/year=2007/.hive-staging_hive_2017-07-09_21-06-46_467_6099    024841256988090-63/-ext-10000 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/h    andson_train/airline_performance/flights_parquet_partd/year=2007/.hive-staging_hive_2017-07-09_21-06-    46_467_6099024841256988090-63/-ext-10002
INFO  : Loading data to table ok_airline_ld.pq_airline_timing_part partition (year=2007) from hdfs://    iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_p    arquet_partd/year=2007/.hive-staging_hive_2017-07-09_21-06-46_467_6099024841256988090-63/-ext-10000
INFO  : Partition ok_airline_ld.pq_airline_timing_part{year=2007} stats: [numFiles=7, numRows=7453215    , totalSize=150029309, rawDataSize=208690020]
No rows affected (42.287 seconds)
</pre>

### HDFS Structure Snapshot [imdemocloud](https://iop-bi-master.imdemocloud.com:8443/gateway/default/hdfs/explorer.html#/user/ldave2001)
![image](https://user-images.githubusercontent.com/19809692/28000435-c0170a28-64f3-11e7-9e3e-abffd728eab8.png)
![image](https://user-images.githubusercontent.com/19809692/28000475-e6f57986-64f3-11e7-94ba-85d993b95771.png)

## Example-2 (Two Columns)
#### Table Creation
<pre>
-- create an external parquet table with partition (by year, month) on airline timing
create external table pq_airline_timing_part2
        (dayofmonth tinyint,dayofweek tinyint,
        deptime smallint, crsdeptime smallint, arrtime smallint, crsarrtime smallint,
        uniquecarrier string, flightnum string, tailnum string, actualelapsedtime smallint,
        crselapsedtime smallint, airtime smallint, arrdelay smallint, depdelay smallint,
        origin string, dest string, distance smallint, taxiin string, taxiout string,
        cancelled string, cancellationcode string, diverted string, carrierdelay smallint,
        weatherdelay smallint, nasdelay smallint, securitydelay smallint, lateaircraftdelay smallint)
partitioned by (year smallint, month tinyint)
stored as parquet
location '/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2';

-- create results in successful creation of the external parquet table and the output is No rows selected

-- manually add a partition to the above external table defined with a partition
alter table pq_airline_timing_part2 add partition (year=2007, month=1);

-- alter results in successful update of the external parquet table and the output is No rows selected

-- insert data into a table partition using static partitioning
insert overwrite table pq_airline_timing_part2 partition(year=2007, month=1)
select
        dayofmonth,
        dayofweek,
        deptime,
        crsdeptime ,
        arrtime,
        crsarrtime ,
        uniquecarrier  ,
        flightnum ,
        tailnum ,
        actualelapsedtime  ,
        crselapsedtime  ,
        airtime  ,
        arrdelay ,
        depdelay ,
        origin  ,
        dest ,
        distance   ,
        taxiin ,
        taxiout   ,
        cancelled ,
        cancellationcode  ,
        diverted   ,
        carrierdelay   ,
        weatherdelay  ,
        nasdelay ,
        securitydelay    ,
        lateaircraftdelay
from airline_timing where year = 2007 and month = 1;

-- above insert statement results in the following mapreduce task shown in the next section
</pre>

### MapReduce Task Output due to Static Partition
<pre>
INFO  : Number of reduce tasks is set to 0 since there's no reduce operator
INFO  : number of splits:7
INFO  : Submitting tokens for job: job_1494894036184_3267
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_14948940361    84_3267/
INFO  : Starting Job = job_1494894036184_3267, Tracking URL = http://iop-bi-master.imdemocloud.com:8088/p    roxy/application_1494894036184_3267/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3267
INFO  : Hadoop job information for Stage-1: number of mappers: 7; number of reducers: 0
INFO  : 2017-07-09 21:26:54,340 Stage-1 map = 0%,  reduce = 0%
INFO  : 2017-07-09 21:27:02,535 Stage-1 map = 43%,  reduce = 0%, Cumulative CPU 23.99 sec
INFO  : 2017-07-09 21:27:03,558 Stage-1 map = 86%,  reduce = 0%, Cumulative CPU 49.39 sec
INFO  : 2017-07-09 21:27:04,580 Stage-1 map = 93%,  reduce = 0%, Cumulative CPU 60.62 sec
INFO  : 2017-07-09 21:27:08,669 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 66.31 sec
INFO  : MapReduce Total cumulative CPU time: 1 minutes 6 seconds 310 msec
INFO  : Ended Job = job_1494894036184_3267
INFO  : Stage-4 is filtered out by condition resolver.
INFO  : Stage-3 is selected by condition resolver.
INFO  : Stage-5 is filtered out by condition resolver.
INFO  : Number of reduce tasks is set to 0 since there's no reduce operator
INFO  : number of splits:1
INFO  : Submitting tokens for job: job_1494894036184_3268
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_14948940361    84_3268/
INFO  : Starting Job = job_1494894036184_3268, Tracking URL = http://iop-bi-master.imdemocloud.com:8088/p    roxy/application_1494894036184_3268/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3268
INFO  : Hadoop job information for Stage-3: number of mappers: 1; number of reducers: 0
INFO  : 2017-07-09 21:27:14,766 Stage-3 map = 0%,  reduce = 0%
INFO  : 2017-07-09 21:27:26,028 Stage-3 map = 100%,  reduce = 0%, Cumulative CPU 12.78 sec
INFO  : MapReduce Total cumulative CPU time: 12 seconds 780 msec
INFO  : Ended Job = job_1494894036184_3268
INFO  : Loading data to table ok_airline_ld.pq_airline_timing_part2 partition (year=2007, month=1) from h    dfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights    _parquet_partd2/year=2007/month=1/.hive-staging_hive_2017-07-09_21-26-49_183_5992017600256698540-63/-ext-    10000
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2007, month=1} stats: [numFiles=1, numRows=6    21559, totalSize=12436691, rawDataSize=16782093]
No rows affected (38.132 seconds)
</pre>

### HDFS Structure Snapshot [imdemocloud](https://iop-bi-master.imdemocloud.com:8443/gateway/default/hdfs/explorer.html#/user/ldave2001)
![image](https://user-images.githubusercontent.com/19809692/28000846-8082b828-64f6-11e7-896d-2b12d6907920.png)
![image](https://user-images.githubusercontent.com/19809692/28000856-9a6dbc24-64f6-11e7-8193-69ba219d4c72.png)
![image](https://user-images.githubusercontent.com/19809692/28000877-ba9acaa0-64f6-11e7-87f2-95d0bb548269.png)
