# Data Engineering Zoomcamp 2026
# Module 3 Homework: Data Warehousing & BigQuery
## Question 1. Counting records
```
SELECT COUNT(*)
FROM `my-de-zoomcamp-2026.ny_taxi.yellow_taxi`;
```

## Question 2. Data read estimation
Query External Table
```
SELECT COUNT(DISTINCT PULocationID)
FROM `my-de-zoomcamp-2026.ny_taxi.yellow_taxi_external`;
```

Query Materialized Table
```
SELECT COUNT(DISTINCT PULocationID)
FROM `my-de-zoomcamp-2026.ny_taxi.yellow_taxi`;
```

## Question 3. Understanding columnar storage
```
SELECT PULocationID
FROM `my-de-zoomcamp-2026.ny_taxi.yellow_taxi`;
```
This query will process 155.12 MB when run.


```
SELECT PULocationID, DOLocationID
FROM `my-de-zoomcamp-2026.ny_taxi.yellow_taxi`;
```
This query will process 310.24 MB when run.

## Question 4. Counting zero fare trips
```
SELECT COUNT(*) 
FROM `my-de-zoomcamp-2026.ny_taxi.yellow_taxi`
WHERE fare_amount = 0;
```

## Question 5. Partitioning and clustering
Partition by the filtering column (the one used in WHERE clause) and Cluster by the column used in sorting (or grouping).

```
CREATE OR REPLACE TABLE
  `my-de-zoomcamp-2026.ny_taxi.yellow_taxi_partitioned`
PARTITION BY DATE(tpep_dropoff_datetime)
CLUSTER BY VendorID AS
SELECT *
FROM `my-de-zoomcamp-2026.ny_taxi.yellow_taxi`;
```

## Question 6. Partition benefits
```
SELECT DISTINCT VendorID
FROM `my-de-zoomcamp-2026.ny_taxi.yellow_taxi`
WHERE tpep_dropoff_datetime >= '2024-03-01'
  AND tpep_dropoff_datetime <  '2024-03-16';
```

```
SELECT DISTINCT VendorID
FROM `my-de-zoomcamp-2026.ny_taxi.yellow_taxi_partitioned`
WHERE tpep_dropoff_datetime >= '2024-03-01'
  AND tpep_dropoff_datetime <  '2024-03-16';
```

## Question 7. External table storage
External Table data is stored in GCP Bucket. BigQuery stores only metadata for external tables.

## Question 8. Clustering best practices
Small tables often gain nothing from clustering, and it adds maintenance overhead. So it is not a good practice to always cluster but only when query patterns benefit.

## Question 9. Understanding table scans
```
SELECT COUNT(*)
FROM `my-de-zoomcamp-2026.ny_taxi.yellow_taxi`;
```
This query will process 0 B when run.

BigQuery can answer COUNT(*) using table metadata only, so it does not need to scan any data blocks. That’s why the estimate is 0 bytes.

