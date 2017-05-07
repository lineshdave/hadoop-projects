# Import all tables using sqoop

## First create warehouse directories
hdfs dfs -mkdir -p /user/cloudera/output/handson_projects/
hdfs dfs -mkdir -p /user/cloudera/rawdata/handson_projects/

## Next evaluate the query string via sqoop command
sqoop eval --connect jdbc:mysql://quickstart:3306/world --username root --query "select * from city" -P

## Import all tables from <b>world</b> database to the above created parent directories in hadoop
sqoop import-all-tables --connect jdbc:mysql://quickstart:3306/world --username root -p --warehouse-dir /user/cloudera/output/handson_projects/sqoop/world
