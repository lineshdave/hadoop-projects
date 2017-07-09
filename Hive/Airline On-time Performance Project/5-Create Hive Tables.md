### Create Hive Tables

#### Managed Table
<pre>
use ok_airline_ld;

-- create a managed table on airline timing
create table m_airline_timing
        (year smallint,month tinyint,dayofmonth tinyint,dayofweek tinyint,
        deptime smallint, crsdeptime smallint, arrtime smallint, crsarrtime smallint,
        uniquecarrier string, flightnum string, tailnum string, actualelapsedtime smallint,
        crselapsedtime smallint, airtime smallint, arrdelay smallint, depdelay smallint,
        origin string, dest string, distance smallint, taxiin string, taxiout string,
        cancelled string, cancellationcode string, diverted string, carrierdelay smallint,
        weatherdelay smallint, nasdelay smallint, securitydelay smallint, lateaircraftdelay smallint)
row format delimited
fields terminated by ',';
</pre>

#### External Tables

##### Text Reference
For the next airline_timing external table, the following pig script has to be run that will create a file in HDFS. This file will then be pointed to or refernced by the Hive table.

<pre>
-- creates an external table table on airline timing
create external table airline_timing
        (year smallint,month tinyint,dayofmonth tinyint,dayofweek tinyint,
        deptime smallint, crsdeptime smallint, arrtime smallint, crsarrtime smallint,
        uniquecarrier string, flightnum string, tailnum string, actualelapsedtime smallint,
        crselapsedtime smallint, airtime smallint, arrdelay smallint, depdelay smallint,
        origin string, dest string, distance smallint, taxiin string, taxiout string,
        cancelled string, cancellationcode string, diverted string, carrierdelay smallint,
        weatherdelay smallint, nasdelay smallint, securitydelay smallint, lateaircraftdelay smallint)
row format delimited
fields terminated by ','
location '/user/ldave2001/rawdata/handson_train/airline_performance/flights_processed';
</pre>

##### Parquet Reference
<pre>
-- creates an external table table on airline timing using parquet format
create external table pq_airline_timing
        (year smallint,month tinyint,dayofmonth tinyint,dayofweek tinyint,
        deptime smallint, crsdeptime smallint, arrtime smallint, crsarrtime smallint,
        uniquecarrier string, flightnum string, tailnum string, actualelapsedtime smallint,
        crselapsedtime smallint, airtime smallint, arrdelay smallint, depdelay smallint,
        origin string, dest string, distance smallint, taxiin string, taxiout string,
        cancelled string, cancellationcode string, diverted string, carrierdelay smallint,
        weatherdelay smallint, nasdelay smallint, securitydelay smallint, lateaircraftdelay smallint)
stored as parquet
location '/user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet';

-- copy records from airline_timing to pq_airline_timing
insert overwrite table pq_airline_timing select * from airline_timing;
</pre>
