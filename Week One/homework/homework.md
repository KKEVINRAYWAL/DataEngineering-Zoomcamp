

## This SQL query will return the day with the largest trip distance in the "yellow_taxi_trips" table
```SQL

WITH daily_trips AS (
    SELECT 
        DATE(tpep_pickup_datetime) AS trip_date, 
        SUM(trip_distance) AS total_distance
    FROM yellow_taxi_data
    GROUP BY trip_date
)
SELECT trip_date, total_distance
FROM daily_trips
ORDER BY total_distance DESC
LIMIT 1;
```
### This SQL query will return the number of trips with 2 and 3 passengers on January 1, 2019:
```SQL
SELECT passenger_count, COUNT(*) AS trip_count
FROM yellow_taxi_trips
WHERE tpep_pickup_datetime >= '2019-01-01 00:00:00' AND
      tpep_pickup_datetime < '2019-01-02 00:00:00'
GROUP BY passenger_count
HAVING passenger_count IN (2, 3);
```
### This SQL query will return the dropoff zone with the largest tip for passengers picked up in the Astoria Zone:
```SQL
SELECT zdo.Zone AS dropoff_zone, MAX(tip_amount) AS max_tip
FROM yellow_taxi_trips t, zones zpu, zones zdo
WHERE t."PULocationID" = zpu."LocationID" AND
      t."DOLocationID" = zdo."LocationID" AND
      zpu.Zone = 'Astoria'
GROUP BY zdo.Zone
ORDER BY max_tip DESC
LIMIT 1;

```

### I also tried this version but was not successful in both, awaiting solution to fact check and benchmark
```SQL
SELECT zdo.Zone AS dropoff_zone, MAX(tip_amount) AS max_tip
FROM yellow_taxi_trips t
JOIN zones zpu ON t."PULocationID" = zpu."LocationID"
JOIN zones zdo ON t."DOLocationID" = zdo."LocationID"
WHERE zpu.Zone = 'Astoria'
GROUP BY zdo.Zone
ORDER BY max_tip DESC
LIMIT 1;

```
