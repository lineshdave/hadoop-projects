### Static Partitioning
Hive tables can be partitioned on column values (example - year) in order to reduce the scanning and retrieval of records due to queries from HDFS based systems.

### Example-1 (One Column)
#### Creation
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

-- STATIC PARTITIONING
-- manual add a partition to the partitoned table
alter table pq_airline_timing_part add partition (year=2007);
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

</pre>
#### Setup/Initialization
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

#### HDFS Structure
<pre>

</pre>

### Example-2 (Two Columns)
#### Creation
<pre>

</pre>

#### Setup/Initialization
<pre>

</pre>
