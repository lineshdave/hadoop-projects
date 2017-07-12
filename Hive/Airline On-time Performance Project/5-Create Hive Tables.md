# Create Hive Tables

## Managed Table
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

## External Tables

#### Text Reference
For the next airline_timing external table, the following pig script has to be run that will create a file in HDFS. This file will then be pointed to or refernced by the Hive table.

###### Pig Script <i>(run with command "pig -f ...")</i>
<pre>
raw_data = LOAD '/user/ldave2001/rawdata/handson_train/airline_performance/flights' USING PigStorage(',') AS
        (year:chararray,month:chararray,dayOfMonth:chararray,dayOfWeek:chararray,DepTime:chararray,
        CRSDepTime:chararray,ArrTime:chararray,CRSArrTime:chararray,UniqueCarrier:chararray,FlightNum:chararray,
        TailNum:chararray,ActualElapsedTime:chararray,CRSElapsedTime:chararray,AirTime:chararray,ArrDelay:chararray,
        DepDelay:chararray,Origin:chararray,Dest:chararray,Distance:chararray,TaxiIn:chararray,TaxiOut:chararray,
        Cancelled:chararray,CancellationCode:chararray,Diverted:chararray,CarrierDelay:chararray,WeatherDelay:chararray,
        NASDelay:chararray,SecurityDelay:chararray,LateAircraftDelay:chararray);

headless_data = FILTER raw_data BY (year != 'Year');

rel_data = FOREACH headless_data GENERATE year, month, dayOfMonth, dayOfWeek,
        (DepTime == 'NA' ? '' : DepTime) AS depTime,
        (CRSDepTime == 'NA' ? '' : CRSDepTime) AS crsDepTime,
        (ArrTime == 'NA' ? '' : ArrTime) AS arrTime,
        (CRSArrTime == 'NA' ? '' : CRSArrTime) AS crsArrTime,
        (UniqueCarrier == 'NA' ? '' : UniqueCarrier) AS UniqueCarrier,
        (FlightNum == 'NA' ? '' : FlightNum) AS FlightNum,
        (TailNum == 'NA' ? '' : TailNum) AS TailNum,
        (ActualElapsedTime == 'NA' ? '' : ActualElapsedTime) AS ActualElapsedTime,
        (CRSElapsedTime == 'NA' ? '' : CRSElapsedTime) AS CRSElapsedTime,
        (AirTime == 'NA' ? '' : AirTime) AS AirTime,
        (ArrDelay == 'NA' ? '' : ArrDelay) AS ArrDelay,
        (DepDelay == 'NA' ? '' : DepDelay) AS DepDelay,
        (Origin == 'NA' ? '' : Origin) AS Origin,
        (Dest == 'NA' ? '' : Dest) AS Dest,
        (Distance == 'NA' ? '' : Distance) AS Distance,
        (TaxiIn == 'NA' ? '' : TaxiIn) AS TaxiIn,
        (TaxiOut == 'NA' ? '' : TaxiOut) AS TaxiOut,
        (Cancelled == 'NA' ? '' : Cancelled) AS Cancelled,
        (CancellationCode == 'NA' ? '' : CancellationCode) AS CancellationCode,
        (Diverted == 'NA' ? '' : Diverted) AS Diverted,
        (CarrierDelay == 'NA' ? '' : CarrierDelay) AS CarrierDelay,
        (WeatherDelay == 'NA' ? '' : WeatherDelay) AS WeatherDelay,
        (NASDelay == 'NA' ? '' : NASDelay) AS NASDelay,
        (SecurityDelay == 'NA' ? '' : SecurityDelay) AS SecurityDelay,
        (LateAircraftDelay == 'NA' ? '' : LateAircraftDelay) AS LateAircraftDelay;

STORE rel_data INTO '/user/ldave2001/rawdata/handson_train/airline_performance/flights_processed' Using PigStorage(',');
</pre>

## Back to Hive Tables Creation
<pre>
-- create an external table on airline timing
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

## Parquet Reference
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
