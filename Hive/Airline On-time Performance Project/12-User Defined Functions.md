# User Defined Functions

User Defined Functions are one-to-one based and they lead to only mapping tasks, but no reductions. A couple of examples of UDFs are functions like UPPER(), CONCAT(), etc. as shown in the examples below.

### Examples:
<pre>
0: jdbc:hive2://iop-bi-master.imdemocloud.com> select upper(city) from airports limit 5;
+-------------------+--+
|        _c0        |
+-------------------+--+
| CITY              |
| BAY SPRINGS       |
| LIVINGSTON        |
| COLORADO SPRINGS  |
| PERRY             |
+-------------------+--+
5 rows selected (0.045 seconds)

0: jdbc:hive2://iop-bi-master.imdemocloud.com> select concat(airport, ', ', city, ', ', state) as address from airports limit 5;
+---------------------------------------+--+
|                address                |
+---------------------------------------+--+
| airport, city, state                  |
| Thigpen , Bay Springs, MS             |
| Livingston Municipal, Livingston, TX  |
| Meadow Lake, Colorado Springs, CO     |
| Perry-Warsaw, Perry, NY               |
+---------------------------------------+--+
5 rows selected (0.035 seconds)

</pre
