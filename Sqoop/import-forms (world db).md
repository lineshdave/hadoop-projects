### Generic argument for Sqoop
<pre>sqoop --connect jdbc:mysql://quickstart.cloudera:3306/world --username root -P</pre>

### Evaluate
<pre>sqoop-eval --connect jdbc:mysql://quickstart.cloudera:3306/world --username root -P --query "show databases"</pre>

### List all databases
<pre>sqoop-list-databases --connect jdbc:mysql://quickstart.cloudera:3306 --username root -P</pre>

### List all tables from <b>world</b> database
<pre>sqoop-list-tables --connect jdbc:mysql://quickstart.cloudera:3306/world --username root -P</pre>
