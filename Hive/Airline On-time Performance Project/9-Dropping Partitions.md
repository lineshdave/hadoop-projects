### Dropping Partitions
A Partition once dropped from a hive metastore will still be available on the distributed file system not because it is an external table.<BR>
So there will have to be a hdfs command to drop the file as well along with altering table in HIVE as given in the example below.

### Example

#### Alter Hive Table
<pre>
alter table pq_airline_timing_part drop partition(year=2006);
</pre>

#### Remove HDFS File (Partitioned Hive Table)
<pre>
hdfs dfs -rm -R /user/cloudera or ldave2001/output/handson_train/airline_time_performance/flight_parquet_partd/year=2006
</pre>

