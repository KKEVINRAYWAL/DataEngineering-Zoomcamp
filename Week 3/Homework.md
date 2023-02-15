# Week 3 Homework

## creating the external table
```
CREATE OR REPLACE EXTERNAL TABLE `red-tide-376509.week3_homework.external_fhv_tripdata`
OPTIONS (
  format = 'CSV',
  uris = ['gs://dtc-de-zoomcamp/data/fhv/fhv_tripdata_2019-*.csv.gz']
);
```
* Create a table in BG:
```
CREATE OR REPLACE TABLE `red-tide-376509.week3_homework.fhv_nonpartitioned_tripdata`
AS SELECT * FROM `red-tide-376509.week3_homework.external_fhv_tripdata*`;
```

## Question 1:
What is the count for fhv vehicle records for year 2019?
```
SELECT count(*) FROM `red-tide-376509.week3_homework.external_fhv_tripdata*`;
```

## Question 2:

Write a query to count the distinct number of affiliated_base_number for the entire dataset on both the tables.
What is the estimated amount of data that will be read when this query is executed on the External Table and the Table?
```
SELECT count(DISTINCT(affiliated_base_number)) FROM `red-tide-376509.week3_homework.external_fhv_tripdata*`
```

```
SELECT count(*) FROM `red-tide-376509.week3_homework.external_fhv_tripdata`
WHERE PUlocationID is null AND DOlocationID is null
```

## Question 5:

Implement the optimized solution you chose for question 4. Write a query to retrieve the distinct affiliated_base_number between pickup_datetime 2019/03/01 and 2019/03/31 (inclusive).
Use the BQ table you created earlier in your from clause and note the estimated bytes. Now change the table in the from clause to the partitioned table you created for question 4 and note the estimated bytes processed. What are these values? Choose the answer which most closely matches.


```
CREATE OR REPLACE TABLE `red-tide-376509.week3_homework.fhv_tripdata_partitioned` 
PARTITION BY DATE(pickup_datetime)
CLUSTER BY affiliated_base_number AS (
  SELECT * FROM red-tide-376509.week3_homework.fhv_nonpartitioned_tripdata
)
```

```
SELECT DISTINCT(affiliated_base_number) FROM red-tide-376509.week3_homework.fhv_nonpartitioned_tripdata
WHERE pickup_datetime BETWEEN TIMESTAMP("2019-03-01") AND TIMESTAMP("2019-03-31");
```
