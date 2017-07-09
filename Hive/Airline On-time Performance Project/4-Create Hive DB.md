### Create Hive Database/Schema
<pre>
-- from OS command prompt
beeline -u jdbc:hive2://iop-bi-master.imdemocloud.com:10000/ -n ldave2001

-- within beeline/Hive 
create database ok_airline_ld location '/user/ldave2001/hive/warehouse/airline.db';
</pre>
