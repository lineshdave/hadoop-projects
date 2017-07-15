# Custom Built Functions

There are multiple publicly available and/or open-source projects or repositories available for Hive Custom Built Functions. One such example is [HiveSwam](https://github.com/livingsocial/HiveSwarm/blob/master/README.markdown).

## Jar Addition in Hive
The JAR files can be downloaded and built using maven on the local machine and then added to Hive in 3 different ways depending on the machine setup.<br>
If Cloud environment such as imdemocloud is used:<br>
<OL>
<LI>Copy the JAR file to the HDFS file system (hdfs dfs -put HiveSwarm-1.0-SNAPSHOT.jar .)</LI>
<LI>Login to Hive using the beeline command (use appropriate database)</LI>
<LI>Execute the following command "add jar hdfs://iop-bi-master.imdemocloud.com:8020/user/ldave2001/HiveSwarm-1.0-SNAPSHOT.jar"</LI>
</OL>
