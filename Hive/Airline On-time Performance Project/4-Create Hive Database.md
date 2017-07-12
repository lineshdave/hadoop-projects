# Create Hive Database

## Beeline Command
<pre>
-- from OS command prompt
beeline -u jdbc:hive2://iop-bi-master.imdemocloud.com:10000/ -n ldave2001
</pre>

## Hive Create DB Command
<pre>
-- within beeline/Hive 
create database ok_airline_ld location '/user/ldave2001/hive/warehouse/airline.db';
</pre>
