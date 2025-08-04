# Databricks SQL Activity: Querying NYC Taxi Data (No Visualizations)

## Overview

This activity introduces you to Databricks SQL through querying the `samples.nyctaxi.trips` table. You will write a series of SQL queries to explore taxi ride data in New York City.

## Prerequisites

- Access to Databricks workspace with **Databricks SQL** enabled
- A **SQL Warehouse** running (or permission to start one)
- Access to the `samples` catalog

## Getting Started

1. Go to the **SQL** tab in Databricks.
2. Open the **SQL Editor**.
3. Select your running SQL Warehouse.
4. In the catalog explorer on the left, expand:
   - `samples`
   - `nyctaxi`
   - Click the `trips` table to see its columns.

---

## Part 1: Explore the Table

### Task 1.1: Preview the Data

```sql
SELECT * FROM samples.nyctaxi.trips
LIMIT 10;
```

---

## Part 2: Simple Aggregations

### Task 2.1: Count Total Rides

```sql
SELECT COUNT(*) AS total_trips
FROM samples.nyctaxi.trips;
```

---

### Task 2.2: Average Fare Amount

```sql
SELECT AVG(fare_amount) AS avg_fare
FROM samples.nyctaxi.trips;
```

---

## Part 3: Filtering & Grouping

### Task 3.1: Number of Trips by Zip Code

```sql
SELECT pickup_zip, COUNT(*) AS trip_count
FROM samples.nyctaxi.trips
GROUP BY pickup_zip
ORDER BY trip_count DESC;
```

---

### Task 3.2: Filter for Short Trips (under 2 miles)

```sql
SELECT *
FROM samples.nyctaxi.trips
WHERE trip_distance < 2
LIMIT 20;
```

---

## Part 4: Time-Based Analysis

### Task 4.1: Count of Trips per Year

```sql
SELECT YEAR(tpep_pickup_datetime) AS pickup_year, COUNT(*) AS trip_count
FROM samples.nyctaxi.trips
GROUP BY pickup_year
ORDER BY pickup_year;
```

---

### Task 4.2: Average Fare by Year

```sql
SELECT YEAR(tpep_pickup_datetime) AS pickup_year, AVG(fare_amount) AS avg_fare
FROM samples.nyctaxi.trips
GROUP BY pickup_year
ORDER BY pickup_year;
```

---

## Wrap-Up

Once complete, review the results of each query. You’ve now used Databricks SQL to:
- Explore real-world data
- Use filters and grouping
- Perform simple time-based analysis

Try expanding these queries on your own by joining with other sample tables or adding conditions!
