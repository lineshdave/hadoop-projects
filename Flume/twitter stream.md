# Twitter Stream Data to Hadoop HDFS

## Twitter App Creation
<pre>
Go to Twitter Website (https://apps.twitter.com)

Create a new Twitter Application

Generate Access Tokens (if not available for the first time)

Copy Consumer Key (API), Consumer Secret Key, Access Token, and Access Secret Token
</pre>

![twitter apps creation](https://cloud.githubusercontent.com/assets/19809692/26755279/45f14194-4858-11e7-99fb-d5210138551b.JPG)

## Flume command setup
<pre>
hdfs dfs -mkdir -p /user/cloudera/output/handson_train/flume/tweets

flume-ng agent --name LinTwitterAgent --conf-file LinTwitterAgent.properties  --classpath flume-sources-1.0-SNAPSHOT.jar
</pre>

## Flume Configuration File Creation
<pre>
# Naming the components on the current agent.
LinTwitterAgent.sources = twitSource
LinTwitterAgent.channels = fChannel
LinTwitterAgent.sinks = hSink

# Describing/Configuring the source
#LinTwitterAgent.sources.twitSource.type = com.cloudera.flume.source.TwitterSource
LinTwitterAgent.sources.twitSource.type = org.apache.flume.source.twitter.TwitterSource
LinTwitterAgent.sources.twitSource.channels=fChannel
LinTwitterAgent.sources.twitSource.consumerKey= IkBRyuRZ936iOy85VBDIpO7Zr
LinTwitterAgent.sources.twitSource.consumerSecret =n8f0Mi7tIJIlaZHOfNcrz20PMvpbq28iJOk7YG4s1M2bGy1dXT
LinTwitterAgent.sources.twitSource.accessToken =101821869-cKU7awYSotbV4RtKlQENzoP7XBNAgLQXati0EdjS
LinTwitterAgent.sources.twitSource.accessTokenSecret = kTp8Pl5HbRHINzDJm5nePHiiaGEx1LrrQLCmzdbAp6AH8
LinTwitterAgent.sources.twitSource.keywords = narendramodi

LinTwitterAgent.sources.twitSource.interceptors=tmp1
LinTwitterAgent.sources.twitSource.interceptors.tmp1.type=timestamp

# Describing/Configuring the channel
LinTwitterAgent.channels.fChannel.type = file
LinTwitterAgent.channels.fChannel.capacity = 100000000
LinTwitterAgent.channels.fChannel.transactionCapacity = 1000
LinTwitterAgent.channels.fChannel.checkpointDir=/home/cloudera/hadoop-training-projects/flume/twitter-stream/file_channel_dir/chkpint_dir
</pre>

## Flume command execution
<pre>
# Execute the command setup file or individual commands as shown above in the Flume Command Setup
Example:
./lin-flume-command.txt (ensure that execute permission has been provided
</pre>

## Hadoop HDFS Output Snapshot
![flume-twitter-stream output](https://cloud.githubusercontent.com/assets/19809692/26755252/8c3e41ca-4857-11e7-9664-291364a2d227.JPG)

