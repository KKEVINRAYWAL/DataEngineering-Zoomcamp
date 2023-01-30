```SQL
<p> This SQL query will return the day with the largest trip distance in the "yellow_taxi_trips" table</>
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
```SQL
<>This SQL query will return the number of trips with 2 and 3 passengers on January 1, 2019:</>
SELECT passenger_count, COUNT(*) AS trip_count
FROM yellow_taxi_data
WHERE tpep_pickup_datetime >= '2019-01-01 00:00:00' AND
      tpep_pickup_datetime < '2019-01-02 00:00:00'
GROUP BY passenger_count
HAVING passenger_count IN (2, 3);
```
