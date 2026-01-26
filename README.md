# Data Engineering Zoomcamp 2026
# Module 1 Homework: Docker & SQL
## Question 1. Understanding Docker images
```>docker run -it --entrypoint bash python:3.13```  
```# pip --version```
## Question 2. Understanding Docker networking and docker-compose
db:5432
## Question 3. Counting short trips
PostgreSQL Query
```
SELECT COUNT(*) AS short_trips
FROM green_tripdata_2025_11
WHERE lpep_pickup_datetime >= DATE '2025-11-01'
  AND lpep_pickup_datetime <  DATE '2025-12-01'
  AND trip_distance <= 1;
```  
## Question 4. Longest trip for each day
PostgreSQL Query
```
SELECT 
    DATE(lpep_pickup_datetime) AS pickup_day,
    MAX(trip_distance) AS max_trip_distance
FROM green_tripdata_2025_11
WHERE trip_distance < 100
GROUP BY DATE(lpep_pickup_datetime)
ORDER BY max_trip_distance DESC
LIMIT 1;
```  
## Question 5. Biggest pickup zone
PostgreSQL Query
```
SELECT 
    t.Zone,
    SUM(g.total_amount) AS total_revenue
    FROM green_tripdata_2025_11 g
    JOIN taxi_zone_lookup t
    ON g.PULocationID = t.LocationID
    WHERE DATE(g.lpep_pickup_datetime) = '2025-11-18'
    GROUP BY t.Zone
    ORDER BY total_revenue DESC
    LIMIT 1;  
```  
## Question 6. Largest tip
PostgreSQL Query
```
SELECT 
    dz.Zone AS dropoff_zone,
    MAX(g.tip_amount) AS max_tip
FROM green_tripdata_2025_11 g
JOIN taxi_lookup pz
  ON g.PULocationID = pz.LocationID
JOIN taxi_lookup dz
  ON g.DOLocationID = dz.LocationID
WHERE pz.Zone = 'East Harlem North'
  AND g.lpep_pickup_datetime >= TIMESTAMP '2025-11-01'
  AND g.lpep_pickup_datetime <  TIMESTAMP '2025-12-01'
GROUP BY dz.Zone
ORDER BY max_tip DESC
LIMIT 1;
```  

## Question 7. Terraform Workflow
terraform init, terraform apply -auto-approve, terraform destroy