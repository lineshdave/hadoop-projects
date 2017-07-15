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
<br>

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
