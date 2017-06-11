### Generic argument for Sqoop
<pre>sqoop --connect jdbc:mysql://quickstart.cloudera:3306/world --username root -P</pre>

### Evaluate
<pre>sqoop-eval --connect jdbc:mysql://quickstart.cloudera:3306/world --username root -P --query "show databases"</pre>

### List all databases
<pre>sqoop-list-databases --connect jdbc:mysql://quickstart.cloudera:3306 --username root -P</pre>

### List all tables from <b>world</b> database
<pre>sqoop-list-tables --connect jdbc:mysql://quickstart.cloudera:3306/world --username root -P</pre>

### Import a table with defaults
<pre>sqoop-import --connect jdbc:mysql://quickstart.cloudera:3306/world --username root --table city --target-dir /user/cloudera/output/handson_train/may/sqoop/import/city-defaults -P</pre>

### Import a table with defaults with a single mapper
<pre>sqoop-import --connect jdbc:mysql://quickstart.cloudera:3306/world --username root --table city --target-dir /user/cloudera/output/handson_train/may/sqoop/import/city-defaults-singlemapper -m 1 -P</pre>

### Import a table with defaults with a single mapper to an existing folder
<pre>sqoop-import --connect jdbc:mysql://quickstart.cloudera:3306/world --username root --table city --target-dir /user/cloudera/output/handson_train/may/sqoop/import/city-defaults-singlemapper -m 1 --append -P</pre>

### Import a table with defaults with a single mapper and with tab-delimited records
<pre>sqoop-import --connect jdbc:mysql://quickstart.cloudera:3306/world --username root  --table city --target-dir /user/cloudera/output/handson_train/may/sqoop/import/city-tab-delimited -m 1 --fields-terminated-by '\t' --append -P</pre>

### Import all countries into 1 file in a true csv format, "nanes are quote", comman, escaped
<pre>sqoop-import --connect jdbc:mysql://quickstart.cloudera:3306/world --username root  --table city --target-dir /user/cloudera/output/handson_train/may/sqoop/import/city-csv-format -m 1 --fields-terminated-by ',' --enclosed-by "\"" --escaped-by "\\"  -P</pre>

### Import table with different format (parquet file) into a single file
<pre>sqoop-import --connect jdbc:mysql://quickstart.cloudera:3306/world --username root  --table city --target-dir /user/cloudera/output/handson_train/may/sqoop/import/city-parquet -m 1 --as-parquetfile -P</pre>

### Import table with different format (avro file) into a single file
<pre>sqoop-import --connect jdbc:mysql://quickstart.cloudera:3306/world --username root  --table city --target-dir /user/cloudera/output/handson_train/may/sqoop/import/city-avro -m 1 --as-avrodatafile  -P</pre>

### Import table with where clause and avro file
<pre>sqoop-import --connect jdbc:mysql://quickstart.cloudera:3306/world --username root  --table city --target-dir /user/cloudera/output/handson_train/may/sqoop/import/city-where-population-avro -m 1 --as-avrodatafile --where 'population > 100000'  -P</pre>

### Import table with where clause and avro file
<pre>sqoop-import --connect jdbc:mysql://quickstart:3306/world --username root --table city --target-dir /user/cloudera/output/handson_train/sqoop/city_na -m 2 --fields-terminated-by \001 --as-avrodatafile --where "countryCode in ('USA', 'CAN', 'MEX')" -P</pre>

### Import table with where clause with avro format using a password file
<pre>
## password file was created with the following command
## echo "cloudera" > passwordfile
## hdfs dfs -moveFromLocal passwordfile /user/cloudera
## hdfs dfs -chmod 600 /user/cloudera/passwordfile
## make sure that there is not carriage return character after the cloudera password in the file
</pre>

### Append the output to the previous result from the where clause exercise
<pre>sqoop-import --connect jdbc:mysql://quickstart:3306/world --username root --table city --target-dir /user/cloudera/output/handson_train/sqoop/city_na -m 2 --fields-terminated-by \001 --as-avrodatafile --where "countryCode in ('USA', 'CAN', 'MEX')" --password-file /user/cloudera/passwordfile --append</pre>

### Import all tables with avro format
<pre>sqoop-import-all-tables --connect jdbc:mysql://quickstart.cloudera:3306/world --username root  --warehouse-dir /user/cloudera/output/handson_train/may/sqoop/importall/world -m 1 --as-avrodatafile --password-file /user/cloudera/passwordfile</pre>
