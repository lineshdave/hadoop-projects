# User Defined Functions

## User Defined Functions (UDF)
User Defined Functions are one-to-one based (pass one and get one) and they lead to only mapping tasks, but no reductions. A couple of examples of UDFs are functions like UPPER(), CONCAT(), etc. as shown in the examples below.
They can be used for column mappings, filterings, transformations, encryptions, and projections. 

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
</pre>

## User Defined Aggregate Functions (UDAF)

Many-to-one - pass in many and get one. You must use Group by in UDAFs. They execute both map and reduce tasks.

## User Defined Tabluar Function (UDTF)

One-to-many - pass in one and get many. There are very few such functions. One of the examples of UDTF is "explode". This is useful for exploding fields like arrarys or tables. Basically, it breaks a tabular field and returns it as one single unit field.

A Lateral View is a inline view that is generated from a UDTF that you can use in a select clause of a SQL.
