## Copy Local Directory Data to Hadoop HDFS in Compressed (Sequential) File Format

### Create Commands Script/File
<pre>
hdfs dfs -mkdir -p /user/cloudera/output/handson_train/flume/simple_watch_dir (optional if simple dir spool is executed before)

flume-ng agent -f linesh_configured_compressed.properties -n linesh
</pre>

### Create or Identify Local Directory

For this example, a local directory named "watch" is created within the simple_dir_spool directory (where configuration file is placed)

### Create the Flume Configuration File
<pre>
linesh.sources=sc1
linesh.channels=chl ch2
linesh.sinks=snk

#configure your components
linesh.sources.sc1.type=spooldir
linesh.sources.sc1.channels=chl
linesh.sources.sc1.spoolDir=/home/cloudera/hadoop-training-projects/flume/simple_dir_spool/watch


linesh.channels.chl.type=memory


linesh.sinks.snk.type=hdfs
linesh.sinks.snk.channel=chl
linesh.sinks.snk.hdfs.path=/user/cloudera/output/handson_train/flume/simple_watch_dir
linesh.sinks.snk.hdfs.fileType=CompressedStream
linesh.sinks.snk.hdfs.codeC=snappy
linesh.sinks.snk.hdfs.writeFormat=Text
linesh.sinks.snk.hdfs.rollInterval=300
linesh.sinks.snk.hdfs.rollSize=0
linesh.sinks.snk.hdfs.rollCount=100000
linesh.sinks.snk.hdfs.filePrefix=class_ex
linesh.sinks.snk.hdfs.fileSuffix=.tags
</pre>
### Run the Commands Script
<pre>
./linesh-commands.txt (make sure that execute permission on this commands script/file is provided)
</pre>

##### <i>Files can be copied in the local directory while the commands script is running or prior. A file tags.csv is available for copying/moving into the "watch" directory.</i>

### Snapshot of the HDFS Directory Output (using Hue)
![flume-simple-watch-dir snapshot](https://cloud.githubusercontent.com/assets/19809692/26755640/424e6998-485f-11e7-8461-3d48335b2004.JPG)
