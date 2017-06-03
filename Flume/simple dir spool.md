## Copy Local Directory Data to Hadoop HDFS

### Create Commands Script/File
<pre>
hdfs dfs -mkdir -p /user/cloudera/output/handson_train/flume/simple_watch_dir

flume-ng agent -f linesh_configured.properties -n linesh
</pre>

### Create or Identify Local Directory

For this example, a local directory named "dir" is created within the simple_dir_spool directory (where configuration file is placed)

### Create the Flume Configuration File
<pre>
linesh.sources = source1
linesh.channels = channel1
linesh.sinks = sink1 

#configure source
linesh.sources.source1.type =spooldir
linesh.sources.source1.channels = channel1
linesh.sources.source1.spoolDir = /home/cloudera/hadoop-training-projects/flume/simple_dir_spool/dir

#configure channel
linesh.channels.channel1.type=memory

#sinks
linesh.sinks.sink1.type=hdfs
linesh.sinks.sink1.channel=channel1
linesh.sinks.sink1.hdfs.path=/user/cloudera/rawdata/handson_train/flume/simple_watch_dir
linesh.sinks.sink1.hdfs.filePrefix=tag_parts_
linesh.sinks.sink1.hdfs.fileSuffix=.txt
linesh.sinks.sink1.hdfs.rollInterval=120
linesh.sinks.sink1.hdfs.rollSize=20971520
linesh.sinks.sink1.hdfs.rollCount=0
linesh.sinks.sink1.hdfs.writeFormat=Text
linesh.sinks.sink1.hdfs.fileType=DataStream
</pre>
### Run the Commands Script
<pre>
./linesh-commands.txt (make sure that execute permission on this commands script/file is provided)
</pre>

##### <i>Files can be copied in the local directory while the commands script is running or prior. A file tags.csv is available for copying/moving into the "dir" directory.</i>

### Snapshot of the HDFS Directory Output (using Hue)
![flume-simple-watch-dir snapshot](https://cloud.githubusercontent.com/assets/19809692/26755640/424e6998-485f-11e7-8461-3d48335b2004.JPG)

