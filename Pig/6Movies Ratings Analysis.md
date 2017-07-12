## Movies Ratings Analysis

## Problem Statement
Provided a dataset of movies with their title and genre information, and another dataset with ratings information, generate a complete movie rating analysis. The desired output of this analysis will be a list of movies with each row displayed in the following format: movieId, title, totalRating, ratingCount, averageRating

## Algorithm
<pre>
-- find all movies and their titles

-- movieId,title,genres
-- 1,Toy Story (1995),Adventure|Animation|Children|Comedy|Fantasy
-- 2,Jumanji (1995),Adventure|Children|Fantasy
-- 3,Grumpier Old Men (1995),Comedy|Romance
-- 4,"Waiting, to Exhale (1995)",Comedy|Drama|Romance
-- 5,Father of the Bride Part II (1995),Comedy

-- find all movies and their ratings

-- movieId, ratings,datetime
-- 1,2.5,1234567
-- 1,3.5,5567899
-- 2,4.5,5589879

-- load the data from source
-- remove the header
-- summarize counts on each group (movie)
</pre>

## Pig Script
<pre>
-- register the new packages for loading CSV file and variance function
register '/home/cloudera/hadoop-training-projects/pig/movielens/datafu-pig-incubating-1.3.1.jar';
register '/home/cloudera/hadoop-training-projects/pig/movielens/piggybank-0.15.0.jar';

-- define classes to be used from the above packages
DEFINE VAR datafu.pig.stats.VAR();
DEFINE CSVLoader org.apache.pig.piggybank.storage.CSVLoader();

-- load the movie data
raw_movie_full =
  LOAD '/user/cloudera/rawdata/handson_train/movielens/latest/movies'
  USING CSVLoader()
  AS (movieId:chararray, title:chararray, genres:chararray);

--remove the header
raw_movie =
  FILTER raw_movie_full BY (movieId != 'movieId');

-- project the movieId and title
movie =
  FOREACH raw_movie GENERATE (long)movieId AS movieId, title;

-- load the rating data
raw_ratings_full =
  LOAD '/user/cloudera/rawdata/handson_train/movielens/latest/ratings'
  USING PigStorage(',')
  AS (userId:chararray, movieId:chararray, rating:chararray, timestamp:chararray);

--remove the header
raw_ratings =
  FILTER raw_ratings_full BY (userId != 'userId');

-- project the movieId and rating
ratings =
  FOREACH raw_ratings
  GENERATE (long)movieId AS movieId, (float)rating AS rating;

-- group the ratings by movieId
grouped_ratings =
  GROUP ratings BY movieId;

-- for each group, aggregate the rating
movieId_ratings =
  FOREACH grouped_ratings
  GENERATE group AS movieId, (int)COUNT(ratings.movieId) AS num_rating,
  (float)SUM(ratings.rating) AS tot_rating, (float)AVG(ratings.rating) AS avg_rating,
  VAR(ratings.rating) AS rating_variance;

-- join the collections
movie_rating_jn =
  JOIN movieId_ratings BY movieId RIGHT, movie BY movieId;

-- generate fields from the join
movie_rating =
  FOREACH movie_rating_jn
  GENERATE movie::movieId AS movieId, movie::title AS movieTitle,
  movieId_ratings::tot_rating AS total_rating, movieId_ratings::num_rating AS num_ratings,
  movieId_ratings::avg_rating AS avg_rating, movieId_ratings::rating_variance AS var_rating;

-- sort the movie ratings by number of ratings and average ratings in desc order
sorted_ratings =
  ORDER movie_rating BY num_ratings DESC, avg_rating DESC;

-- store the movie ratings
STORE sorted_ratings INTO '/user/cloudera/output/handson_train/pig/movielens/movies_rating_analysis/text-file';
</pre>

## Output
An output file part-r-00000 is created in the text-file directory within the following HDFS directory - <i>/user/cloudera/output/handson_train/pig/movielens/movies_ratings_analysis</i>. This directory also contains a <i>_SUCCESS</i> file to mark the successful completion and running of the above script. This script can be run in the GRUNT interactive Pig environment or directly from the command using the syntax "pig -f <i>filename</i>" (without the quotes and <i>filename</i> to be replaced with the actual file name).

## HDFS Snapshot
![image](https://user-images.githubusercontent.com/19809692/27765744-4cd5aa22-5e88-11e7-9e61-e68de200ded3.png)

![image](https://user-images.githubusercontent.com/19809692/27765754-9e155a4a-5e88-11e7-9a2e-816389ab9bd4.png)

