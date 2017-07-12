# Total Movies Per Genre

## Problem Statement
Find the total number of movies listed against each genre.

## Algorithm
-- find all genres and the number of movies

-- movieId,title,genres
-- 1,Toy Story (1995),Adventure|Animation|Children|Comedy|Fantasy
-- 2,Jumanji (1995),Adventure|Children|Fantasy
-- 3,Grumpier Old Men (1995),Comedy|Romance
-- 4,"Waiting, to Exhale (1995)",Comedy|Drama|Romance
-- 5,Father of the Bride Part II (1995),Comedy

-- load the data from source
-- remove the header
-- project one field genres
-- split the genres by a pipe   - (Adventure,Children,Fantasy)
-- flatten the tuple of splitted genres (Adventure)
--                                     (Children)
--                                     (Fantasy)
-- group the flattened tuple by name
-- get the count on each group

## Pig Script
<pre>
## First Script - genre-count
-- register new piggbank package jar
register /home/cloudera/hadoop-training-projects/pig/movielens/piggybank-0.16.0.jar;

-- define new classes from piggbank jar
DEFINE myCSVLoader org.apache.pig.piggybank.storage.CSVLoader();
DEFINE myXLSStorage org.apache.pig.piggybank.storage.CSVExcelStorage();

-- load data
data =
      LOAD '/user/cloudera/rawdata/handson_train/movielens/latest/movies'
      USING myCSVLoader()
      AS (movieId:chararray, title:chararray, genres:chararray);

-- remove header row
headless = FILTER data BY movieId != 'movieId';

-- gather genres and flatten to avoid multiple rows
flattened = FOREACH headless GENERATE FLATTEN(STRSPLIT(genres, '\\|', 0)) as f;

-- group each flattened genre record
grouped = GROUP flattened BY f;

-- aggregate each genre record
aggr = FOREACH grouped GENERATE (chararray)group as genre, COUNT(flattened) as num;

-- sort each aggregate genre record
sorted  = ORDER aggr BY genre;

-- store output in different storage formats
STORE sorted INTO '/user/cloudera/output/handson_train/pig/movielens/genre_count/text-file'
      USING  PigStorage();
STORE sorted INTO '/user/cloudera/output/handson_train/pig/movielens/genre_count/avro-file'
      USING  AvroStorage();
STORE sorted INTO '/user/cloudera/output/handson_train/pig/movielens/genre_count/json-file'
      USING  JsonStorage();
STORE sorted INTO '/user/cloudera/output/handson_train/pig/movielens/genre_count/excel-file'
      USING  myXLSStorage();
      
 #### Second Script - genre-movie-count
 -- Find total movies under each genre

-- genre stats
-- output
-- genre, totalNumberofMovies

-- register new piggybank package jar
register /home/cloudera/hadoop-training-projects/pig/movielens/piggybank-0.16.0.jar;

-- define new classes from the piggybank package
DEFINE myCSVLoader org.apache.pig.piggybank.storage.CSVLoader();
DEFINE myXLSStorage org.apache.pig.piggybank.storage.CSVExcelStorage();

-- load the movie data
raw_movie_full =
      LOAD '/user/cloudera/rawdata/handson_train/movielens/latest/movies'
      USING org.apache.pig.piggybank.storage.CSVLoader()
      AS (movieId:chararray, title:chararray, genres:chararray);

-- remove the header
raw_movie =
      FILTER raw_movie_full BY (movieId != 'movieId');

-- project the movieId and genre
movie_genre =
      FOREACH raw_movie
      GENERATE (long)movieId as movieId, FLATTEN(TOKENIZE(genres, '|')) as genre;

-- group the data by genres
grp_movie_genre =
      GROUP movie_genre BY genre;

-- aggregate the grouped data
agg_data =
      FOREACH grp_movie_genre GENERATE group as genre, COUNT(movie_genre) as num_movies;

--sorting
sorted = ORDER agg_data BY genre;

STORE sorted INTO '/user/cloudera/output/handson_train/pig/movielens/genre_movie_count/default-file';
STORE sorted INTO '/user/cloudera/output/handson_train/pig/movielens/genre_movie_count/text-file'
      USING PigStorage(',');
STORE sorted INTO '/user/cloudera/output/handson_train/pig/movielens/genre_movie_count/avro-file'
      USING AvroStorage();
STORE sorted INTO '/user/cloudera/output/handson_train/pig/movielens/genre_movie_count/json-file'
      USING JsonStorage();
STORE sorted INTO '/user/cloudera/output/handson_train/pig/movielens/genre_movie_count/excel'
      USING myXLSStorage;
</pre>

## Output
An output file part-r-00000 is created in the following HDFS directories - <i>/user/cloudera/output/handson_train/pig/movielens/genre_count</i> and <i>/user/cloudera/output/handson_train/pig/movielens/genre_movie_count</i>. It also contains a <i>_SUCCESS</i> file to mark the successful completion and running of the above script. This script can be run in the GRUNT interactive Pig environment or directly from the command using the syntax "pig -f <i>filename</i>" (without the quotes and <i>filename</i> to be replaced with the actual file name).

## HDFS Snapshot - First Script
![image](https://user-images.githubusercontent.com/19809692/27765086-f7039d68-5e76-11e7-8f81-ea5e9b6db47a.png)

## HDFS Snapshot - Second Script
![image](https://user-images.githubusercontent.com/19809692/27765440-d86247b2-5e7e-11e7-804a-d9826a4a8fe1.png)


