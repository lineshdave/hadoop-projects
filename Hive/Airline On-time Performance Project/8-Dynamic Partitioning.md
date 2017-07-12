# Dynamic Partitioning
Quite similar to Static Partitioning, Hive tables can also be partitioned dynamically on one or more column values (example - year) in order to reduce the scanning and retrieval of records due to queries from HDFS based systems. This type of partitioning is more efficient and effective in systems and for tables where values within the column being  partitioned grows dynamically. 

## Hive Tables with Dynamic Partitions Creation
<pre>
-- set dynamic partitioning parameters in Hive before re-inserting data in tables
set hive.exec.dynamic.partition=true;
set hive.exec.dynamic.partition.mode=nonstrict;

-- ensure static partitions on following tables are dropped and hdfs files removed before re-inserting data

-- inserting data into a hive table using dynamic nonstrict partition mode
insert into pq_airline_timing_part partition(year)
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
        lateaircraftdelay,
        year
from airline_timing ;

-- inserting data into a hive table using dynamic nonstrict partition mode
insert into pq_airline_timing_part2 partition(year, month)
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
        lateaircraftdelay,
        year,
        month
from airline_timing ;
</pre>


## MapReduce Tasks Execution
<pre>
#### FIRST DYNAMIC PARTITIONED TABLE

INFO  : Number of reduce tasks is set to 0 since there's no reduce operator
INFO  : number of splits:7
INFO  : Submitting tokens for job: job_1494894036184_3301
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_33   01/
INFO  : Starting Job = job_1494894036184_3301, Tracking URL = http://iop-bi-master.imdemocloud.com:8088/proxy/   application_1494894036184_3301/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3301
INFO  : Hadoop job information for Stage-1: number of mappers: 7; number of reducers: 0
INFO  : 2017-07-10 12:26:29,745 Stage-1 map = 0%,  reduce = 0%
INFO  : 2017-07-10 12:26:52,286 Stage-1 map = 36%,  reduce = 0%, Cumulative CPU 184.43 sec
INFO  : 2017-07-10 12:27:04,546 Stage-1 map = 41%,  reduce = 0%, Cumulative CPU 286.09 sec
INFO  : 2017-07-10 12:27:06,594 Stage-1 map = 48%,  reduce = 0%, Cumulative CPU 289.38 sec
INFO  : 2017-07-10 12:27:07,617 Stage-1 map = 82%,  reduce = 0%, Cumulative CPU 312.34 sec
INFO  : 2017-07-10 12:27:35,207 Stage-1 map = 93%,  reduce = 0%, Cumulative CPU 374.52 sec
INFO  : 2017-07-10 12:27:42,363 Stage-1 map = 95%,  reduce = 0%, Cumulative CPU 390.67 sec
INFO  : 2017-07-10 12:28:02,777 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 415.97 sec
INFO  : MapReduce Total cumulative CPU time: 6 minutes 55 seconds 970 msec
INFO  : Ended Job = job_1494894036184_3301
INFO  : Stage-4 is selected by condition resolver.
INFO  : Stage-3 is filtered out by condition resolver.
INFO  : Stage-5 is filtered out by condition resolver.
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline   _performance/flights_parquet_partd/.hive-staging_hive_2017-07-10_12-26-24_591_6129781971509917800-63/-ext-1000   0 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flig   hts_parquet_partd/.hive-staging_hive_2017-07-10_12-26-24_591_6129781971509917800-63/-ext-10002
INFO  : Loading data to table ok_airline_ld.pq_airline_timing_part partition (year=null) from hdfs://iop-bi-ma   ster.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd/.hive   -staging_hive_2017-07-10_12-26-24_591_6129781971509917800-63/-ext-10000
INFO  :          Time taken for load dynamic partitions : 148
INFO  :         Loading partition {year=2008}
INFO  :         Loading partition {year=2006}
INFO  :         Loading partition {year=2007}
INFO  :          Time taken for adding to write entity : 1
INFO  : Partition ok_airline_ld.pq_airline_timing_part{year=2006} stats: [numFiles=3, numRows=7141922, totalSi   ze=143733908, rawDataSize=199973816]
INFO  : Partition ok_airline_ld.pq_airline_timing_part{year=2007} stats: [numFiles=4, numRows=7453215, totalSi   ze=150027650, rawDataSize=208690020]
INFO  : Partition ok_airline_ld.pq_airline_timing_part{year=2008} stats: [numFiles=2, numRows=7009728, totalSi   ze=137906992, rawDataSize=196272384]
No rows affected (99.584 seconds)

#### SECOND DYNAMIC PARTITIONED TABLE

INFO  : Number of reduce tasks is set to 0 since there's no reduce operator
INFO  : number of splits:7
INFO  : Submitting tokens for job: job_1494894036184_3302
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_33   02/
INFO  : Starting Job = job_1494894036184_3302, Tracking URL = http://iop-bi-master.imdemocloud.com:8088/proxy/   application_1494894036184_3302/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3302
INFO  : Hadoop job information for Stage-1: number of mappers: 7; number of reducers: 0
INFO  : 2017-07-10 12:28:09,341 Stage-1 map = 0%,  reduce = 0%
INFO  : 2017-07-10 12:28:31,829 Stage-1 map = 36%,  reduce = 0%, Cumulative CPU 193.32 sec
INFO  : 2017-07-10 12:28:47,160 Stage-1 map = 68%,  reduce = 0%, Cumulative CPU 329.48 sec
INFO  : 2017-07-10 12:28:49,215 Stage-1 map = 75%,  reduce = 0%, Cumulative CPU 336.6 sec
INFO  : 2017-07-10 12:28:50,239 Stage-1 map = 82%,  reduce = 0%, Cumulative CPU 346.45 sec
INFO  : 2017-07-10 12:29:17,808 Stage-1 map = 93%,  reduce = 0%, Cumulative CPU 408.97 sec
INFO  : 2017-07-10 12:29:23,944 Stage-1 map = 95%,  reduce = 0%, Cumulative CPU 424.21 sec
INFO  : 2017-07-10 12:29:45,415 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 452.07 sec
INFO  : MapReduce Total cumulative CPU time: 7 minutes 32 seconds 70 msec
INFO  : Ended Job = job_1494894036184_3302
INFO  : Stage-4 is filtered out by condition resolver.
INFO  : Stage-3 is filtered out by condition resolver.
INFO  : Stage-5 is selected by condition resolver.
INFO  : Number of reduce tasks is set to 0 since there's no reduce operator
INFO  : number of splits:7
INFO  : Submitting tokens for job: job_1494894036184_3303
INFO  : The url to track the job: http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3303/
INFO  : Starting Job = job_1494894036184_3303, Tracking URL = http://iop-bi-master.imdemocloud.com:8088/proxy/application_1494894036184_3303/
INFO  : Kill Command = /usr/iop/4.1.0.0/hadoop/bin/hadoop job  -kill job_1494894036184_3303
INFO  : Hadoop job information for Stage-5: number of mappers: 7; number of reducers: 0
INFO  : 2017-07-10 12:29:52,597 Stage-5 map = 0%,  reduce = 0%
INFO  : 2017-07-10 12:30:02,840 Stage-5 map = 18%,  reduce = 0%, Cumulative CPU 36.55 sec
INFO  : 2017-07-10 12:30:03,864 Stage-5 map = 27%,  reduce = 0%, Cumulative CPU 75.49 sec
INFO  : 2017-07-10 12:30:04,887 Stage-5 map = 42%,  reduce = 0%, Cumulative CPU 91.01 sec
INFO  : 2017-07-10 12:30:05,909 Stage-5 map = 93%,  reduce = 0%, Cumulative CPU 109.3 sec
INFO  : 2017-07-10 12:30:06,930 Stage-5 map = 100%,  reduce = 0%, Cumulative CPU 112.77 sec
INFO  : MapReduce Total cumulative CPU time: 1 minutes 52 seconds 770 msec
INFO  : Ended Job = job_1494894036184_3303
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2006/month=1 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2006/month=1
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2006/month=11 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2006/month=11
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2006/month=12 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2006/month=12
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2006/month=2 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2006/month=2
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2006/month=4 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2006/month=4
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2006/month=5 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2006/month=5
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2006/month=6 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2006/month=6
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2006/month=7 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2006/month=7
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2006/month=9 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2006/month=9
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2007/month=1 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2007/month=1
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2007/month=10 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2007/month=10
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2007/month=11 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2007/month=11
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2007/month=2 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2007/month=2
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2007/month=4 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2007/month=4
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2007/month=5 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2007/month=5
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2007/month=6 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2007/month=6
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2007/month=8 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2007/month=8
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2007/month=9 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2007/month=9
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2008/month=1 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2008/month=1
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2008/month=10 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2008/month=10
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2008/month=11 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2008/month=11
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2008/month=12 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2008/month=12
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2008/month=2 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2008/month=2
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2008/month=3 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2008/month=3
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2008/month=4 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2008/month=4
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2008/month=5 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2008/month=5
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2008/month=6 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2008/month=6
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2008/month=8 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2008/month=8
INFO  : Moving data to: hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000/year=2008/month=9 from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10002/year=2008/month=9
INFO  : Loading data to table ok_airline_ld.pq_airline_timing_part2 partition (year=null, month=null) from hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/.hive-staging_hive_2017-07-10_12-28-04_208_731647475778143988-63/-ext-10000
INFO  :          Time taken for load dynamic partitions : 1481
INFO  :         Loading partition {year=2007, month=9}
INFO  :         Loading partition {year=2008, month=6}
INFO  :         Loading partition {year=2007, month=5}
INFO  :         Loading partition {year=2006, month=4}
INFO  :         Loading partition {year=2008, month=1}
INFO  :         Loading partition {year=2006, month=2}
INFO  :         Loading partition {year=2006, month=9}
INFO  :         Loading partition {year=2006, month=3}
INFO  :         Loading partition {year=2008, month=5}
INFO  :         Loading partition {year=2006, month=8}
INFO  :         Loading partition {year=2007, month=4}
INFO  :         Loading partition {year=2006, month=10}
INFO  :         Loading partition {year=2006, month=6}
INFO  :         Loading partition {year=2007, month=10}
INFO  :         Loading partition {year=2008, month=10}
INFO  :         Loading partition {year=2007, month=7}
INFO  :         Loading partition {year=2008, month=3}
INFO  :         Loading partition {year=2006, month=1}
INFO  :         Loading partition {year=2007, month=2}
INFO  :         Loading partition {year=2007, month=3}
INFO  :         Loading partition {year=2008, month=7}
INFO  :         Loading partition {year=2008, month=4}
INFO  :         Loading partition {year=2006, month=7}
INFO  :         Loading partition {year=2007, month=1}
INFO  :         Loading partition {year=2007, month=8}
INFO  :         Loading partition {year=2008, month=2}
INFO  :         Loading partition {year=2006, month=11}
INFO  :         Loading partition {year=2007, month=11}
INFO  :         Loading partition {year=2008, month=9}
INFO  :         Loading partition {year=2007, month=6}
INFO  :         Loading partition {year=2006, month=5}
INFO  :         Loading partition {year=2008, month=11}
INFO  :         Loading partition {year=2006, month=12}
INFO  :         Loading partition {year=2008, month=12}
INFO  :         Loading partition {year=2007, month=12}
INFO  :         Loading partition {year=2008, month=8}
INFO  :          Time taken for adding to write entity : 5
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2006, month=1} stats: [numFiles=1, numRows=581287, totalSize=11350008, rawDataSize=15694749]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2006, month=10} stats: [numFiles=1, numRows=611718, totalSize=12270084, rawDataSize=16516386]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2006, month=11} stats: [numFiles=1, numRows=586197, totalSize=11348386, rawDataSize=15827319]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2006, month=12} stats: [numFiles=1, numRows=604758, totalSize=12048677, rawDataSize=16328466]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2006, month=2} stats: [numFiles=1, numRows=531247, totalSize=10388797, rawDataSize=14343669]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2006, month=3} stats: [numFiles=1, numRows=605217, totalSize=11964886, rawDataSize=16340859]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2006, month=4} stats: [numFiles=1, numRows=585351, totalSize=11401813, rawDataSize=15804477]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2006, month=5} stats: [numFiles=1, numRows=602919, totalSize=11683471, rawDataSize=16278813]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2006, month=6} stats: [numFiles=1, numRows=598315, totalSize=12009819, rawDataSize=16154505]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2006, month=7} stats: [numFiles=1, numRows=621244, totalSize=12315396, rawDataSize=16773588]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2006, month=8} stats: [numFiles=1, numRows=628732, totalSize=12307325, rawDataSize=16975764]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2006, month=9} stats: [numFiles=1, numRows=584937, totalSize=11590331, rawDataSize=15793299]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2007, month=1} stats: [numFiles=1, numRows=621559, totalSize=12436691, rawDataSize=16782093]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2007, month=10} stats: [numFiles=1, numRows=629992, totalSize=11931084, rawDataSize=17009784]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2007, month=11} stats: [numFiles=1, numRows=605149, totalSize=11673077, rawDataSize=16339023]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2007, month=12} stats: [numFiles=1, numRows=614139, totalSize=12645790, rawDataSize=16581753]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2007, month=2} stats: [numFiles=1, numRows=565604, totalSize=11483334, rawDataSize=15271308]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2007, month=3} stats: [numFiles=1, numRows=639209, totalSize=12590898, rawDataSize=17258643]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2007, month=4} stats: [numFiles=1, numRows=614648, totalSize=12124095, rawDataSize=16595496]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2007, month=5} stats: [numFiles=1, numRows=631609, totalSize=12264868, rawDataSize=17053443]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2007, month=6} stats: [numFiles=1, numRows=629280, totalSize=13030392, rawDataSize=16990560]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2007, month=7} stats: [numFiles=1, numRows=648560, totalSize=13109171, rawDataSize=17511120]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2007, month=8} stats: [numFiles=1, numRows=653279, totalSize=12862725, rawDataSize=17638533]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2007, month=9} stats: [numFiles=1, numRows=600187, totalSize=11481159, rawDataSize=16205049]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2008, month=1} stats: [numFiles=1, numRows=605765, totalSize=11889862, rawDataSize=16355655]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2008, month=10} stats: [numFiles=1, numRows=556205, totalSize=10120071, rawDataSize=15017535]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2008, month=11} stats: [numFiles=1, numRows=523272, totalSize=9768547, rawDataSize=14128344]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2008, month=12} stats: [numFiles=1, numRows=544958, totalSize=10711377, rawDataSize=14713866]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2008, month=2} stats: [numFiles=1, numRows=569236, totalSize=11137166, rawDataSize=15369372]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2008, month=3} stats: [numFiles=1, numRows=616090, totalSize=11908461, rawDataSize=16634430]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2008, month=4} stats: [numFiles=1, numRows=598126, totalSize=11594075, rawDataSize=16149402]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2008, month=5} stats: [numFiles=1, numRows=606293, totalSize=11534283, rawDataSize=16369911]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2008, month=6} stats: [numFiles=1, numRows=608665, totalSize=11976527, rawDataSize=16433955]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2008, month=7} stats: [numFiles=1, numRows=627931, totalSize=12249450, rawDataSize=16954137]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2008, month=8} stats: [numFiles=1, numRows=612279, totalSize=11788280, rawDataSize=16531533]
INFO  : Partition ok_airline_ld.pq_airline_timing_part2{year=2008, month=9} stats: [numFiles=1, numRows=540908, totalSize=10391485, rawDataSize=14604516]
No rows affected (127.018 seconds)
</pre>

## HDFS Snapshot
### HDFS Setup Due to First Hive Table
![image](https://user-images.githubusercontent.com/19809692/28031668-0532fdc4-6576-11e7-8632-729a385ab32b.png)
![image](https://user-images.githubusercontent.com/19809692/28031697-1fdc38b6-6576-11e7-9eea-2e420b0e1b64.png)
### HDFS Setup Due to Second Hive Table
![image](https://user-images.githubusercontent.com/19809692/28031725-3c2a74f6-6576-11e7-95ca-42bbc16a32ce.png)
![image](https://user-images.githubusercontent.com/19809692/28031768-68206660-6576-11e7-8a8d-2da21df08856.png)
