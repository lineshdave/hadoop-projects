### Dropping Partitions
A Partitioned table once dropped from a Hive metastore will still be available in the distributed file system because it is an external table. So a hdfs command to drop the file will have to be executed along with altering the table in Hive to remove a Partitioned table. An example is given below.

### Example

#### Alter Hive Table
<pre>
alter table pq_airline_timing_part drop partition(year=2006);
</pre>

#### Remove HDFS File (Partitioned Hive Table)
<pre>
hdfs dfs -rm -R /user/cloudera or ldave2001/output/handson_train/airline_time_performance/flight_parquet_partd/year=2006
</pre>

