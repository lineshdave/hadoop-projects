# Dropping Partitions
A Partitioned table once dropped from a Hive metastore will still be available in the distributed file system because it is an external table. So a hdfs command to drop the file will have to be executed along with altering the table in Hive to remove a Partitioned table. An example is given below.

## Example

### Alter Hive Table
<pre>
alter table pq_airline_timing_part drop partition(year=2007);
alter table pq_airline_timing_part2 drop partition(year=2007, month=1);
</pre>

### Remove HDFS File (Partitioned Hive Table)
<pre>
hdfs dfs -rm -R /user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd/year=2007
hdfs dfs -rm -R /user/ldave2001/rawdata/handson_train/airline_performance/flights_parquet_partd2/year=2007/month=1
</pre>

