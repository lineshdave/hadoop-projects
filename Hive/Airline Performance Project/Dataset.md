### Datasets
The Airline On-time Performance data for multiple years is available in a zipped format at [Statistical Computing Statistical Graphics](http://stat-computing.org/). As mentioned on this web site, the data originally comes from [Bureau of Transportation Statistics - RITA](http://www.transtats.bts.gov/OT_Delay/OT_DelayCause1.asp).

### Variable Descriptions
<pre>
No      Name           Description
1       Year	        1987-2008
2       Month	        1-12
3       DayofMonth      1-31
4       DayOfWeek       1 (Monday) - 7 (Sunday)
5	DepTime	actual departure time (local, hhmm)
6	CRSDepTime	scheduled departure time (local, hhmm)
7	ArrTime	actual arrival time (local, hhmm)
8	CRSArrTime	scheduled arrival time (local, hhmm)
9	UniqueCarrier	unique carrier code
10	FlightNum	flight number
11	TailNum	plane tail number
12	ActualElapsedTime	in minutes
13	CRSElapsedTime	in minutes
14	AirTime	in minutes
15	ArrDelay	arrival delay, in minutes
16	DepDelay	departure delay, in minutes
17	Origin	origin IATA airport code
18	Dest	destination IATA airport code
19	Distance	in miles
20	TaxiIn	taxi in time, in minutes
21	TaxiOut	taxi out time in minutes
22	Cancelled	was the flight cancelled?
23	CancellationCode	reason for cancellation (A = carrier, B = weather, C = NAS, D = security)
24	Diverted	1 = yes, 0 = no
25	CarrierDelay	in minutes
26	WeatherDelay	in minutes
27	NASDelay	in minutes
28	SecurityDelay	in minutes
29	LateAircraftDelay	in minutes
</pre>

### Data Description
The data consists of flight arrival and departure details for all commercial flights within the USA, from October 1987 to April 2008. This is a large dataset: there are nearly 120 million records in total, and takes up 1.6 gigabytes of space compressed and 12 gigabytes when uncompressed. To make sure that you're not overwhelmed by the size of the data, we've provide two brief introductions to some useful tools: linux command line tools and sqlite, a simple sql database.
For this project, three years of data - 2006 through 2008 was used.

### Data Challenge
The aim of the data expo is to provide a graphical summary of important features of the data set. This is intentionally vague in order to allow different entries to focus on different aspects of the data, but here are a few ideas to get you started:
<OL
<LI>When is the best time of day/day of week/time of year to fly to minimise delays?</LI>
<LI>Do older planes suffer more delays?</LI>
<LI>How does the number of people flying between different locations change over time?</LI>
<LI>How well does weather predict plane delays?</LI>
<LI>Can you detect cascading failures as delays in one airport create delays in others? Are there critical links in the system?</LI>
</OL>
