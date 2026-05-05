# RapidRide: Urban Ride-Sharing Fleet Analytics

## Project Structure
* `generate_fleet_data.py`: A Python data-generator that synthesises a realistic, collision-free dataset of 50 vehicles and 500+ time-stamped rides.
* `massive_fleet_dataset.cql`: The generated Cassandra Query Language script containing the keyspace definition, schema, and bulk insertion queries.
* `analysis_queries.cql`: A curated set of queries used to extract business intelligence (High-Value Targeting, Commuter Exhaust, Fleet Segmentation).

## Setup & Execution

### Prerequisites
* [Docker](https://www.docker.com/) (to run the Cassandra cluster)
* Python 3.x (to generate the dataset)

### 1. Spin up the Cassandra Cluster
If you don't already have a Cassandra container running, start one:
```bash
docker run --name cass-lab -d -p 9042:9042 cassandra:latest
```

### 2. Generate the Dataset
Run the Python script to generate the synthetic telemetry data:
```bash
python generate_fleet_data.py
```
*(This will generate a file named `massive_fleet_dataset.cql` in your directory).*

### 3. Load Data into Cassandra
Copy the generated file into your Docker container and execute it using `cqlsh`:
```bash
docker cp massive_fleet_dataset.cql cass-lab:/massive_fleet_dataset.cql
docker exec -it cass-lab cqlsh -f /massive_fleet_dataset.cql
```

## Analytical Insights Extraction
Once the data is loaded, you can drop into the Cassandra shell to perform analytics:
```bash
docker exec -it cass-lab cqlsh
```

**Example 1: Fleet Segmenting (Finding Corporate-Ready SUVs with WiFi)**
```sql
SELECT driver_name, car_model, amenities 
FROM FleetDB.Vehicles 
WHERE amenities CONTAINS 'WiFi' ALLOW FILTERING;
```

**Example 2: Commuter Exhaust Analysis (Long-distance airport drops)**
```sql
SELECT ride_time, distance_km, fare_amount, vehicle_id 
FROM FleetDB.Completed_Rides 
WHERE zone_id = 'Electronic_City' AND distance_km > 20.0 ALLOW FILTERING;
```

**Example 3: Time-Series Burst Analytics**
```sql
SELECT ride_time, distance_km, fare_amount 
FROM FleetDB.Completed_Rides 
WHERE zone_id = 'Koramangala' 
  AND ride_time >= '2026-05-12T00:00:00.000+0000' 
  AND ride_time <= '2026-05-12T23:59:59.000+0000';
```

***
```markdown
# RapidRide: Urban Ride-Sharing Fleet Analytics

![Apache Cassandra](https://img.shields.io/badge/Apache%20Cassandra-1287B1?style=for-the-badge&logo=apache-cassandra&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)

An end-to-end Big Data analytics pipeline built on **Apache Cassandra**. This project simulates the high-velocity telemetry backend of an urban ride-sharing platform (like Uber or Ola), handling massive write throughput, dynamic surge pricing, and concurrent driver statistics.

*This project was originally developed as an Alternate Assessment Tool (AAT) for the Big Data Analytics curriculum at Visvesvaraya Technological University (VTU).*

## Key Features Demonstrated
This project implements several advanced NoSQL wide-column database mechanics:
* **Time-Series Data Modeling:** Optimised partition keys (`zone_id`) and clustering keys (`ride_time DESC`) for O(1) latency traffic burst analytics.
* **Auto-Expiring Data (TTL):** "Surge Pricing" multipliers that automatically vanish server-side after 30–120 minutes without background application jobs.
* **Lock-Free Concurrency (Counters):** Utilisation of CRDTs (Conflict-Free Replicated Data Types) to atomically track driver lifetime ride counts.
* **Dynamic Inventory (Collections):** Utilisation of Cassandra `LIST<TEXT>` collections to map flexible vehicle amenities (WiFi, Dashcam, SUV) without expensive relational `JOIN` operations.
* **Data Portability:** Pipeline for operational data archival via `COPY TO/FROM` CSV operations.

## Project Structure
* `generate_fleet_data.py`: A Python data-generator that synthesises a realistic, collision-free dataset of 50 vehicles and 500+ time-stamped rides.
* `massive_fleet_dataset.cql`: The generated Cassandra Query Language script containing the keyspace definition, schema, and bulk insertion queries.
* `analysis_queries.cql`: A curated set of queries used to extract business intelligence (High-Value Targeting, Commuter Exhaust, Fleet Segmentation).

## Setup & Execution

### Prerequisites
* [Docker](https://www.docker.com/) (to run the Cassandra cluster)
* Python 3.x (to generate the dataset)

### 1. Spin up the Cassandra Cluster
If you don't already have a Cassandra container running, start one:
```bash
docker run --name cass-lab -d -p 9042:9042 cassandra:latest
```

### 2. Generate the Dataset
Run the Python script to generate the synthetic telemetry data:
```bash
python generate_fleet_data.py
```
*(This will generate a file named `massive_fleet_dataset.cql` in your directory).*

### 3. Load Data into Cassandra
Copy the generated file into your Docker container and execute it using `cqlsh`:
```bash
docker cp massive_fleet_dataset.cql cass-lab:/massive_fleet_dataset.cql
docker exec -it cass-lab cqlsh -f /massive_fleet_dataset.cql
```

## Analytical Insights Extraction
Once the data is loaded, you can drop into the Cassandra shell to perform analytics:
```bash
docker exec -it cass-lab cqlsh
```

**Example 1: Fleet Segmenting (Finding Corporate-Ready SUVs with WiFi)**
```sql
SELECT driver_name, car_model, amenities 
FROM FleetDB.Vehicles 
WHERE amenities CONTAINS 'WiFi' ALLOW FILTERING;
```

**Example 2: Commuter Exhaust Analysis (Long-distance airport drops)**
```sql
SELECT ride_time, distance_km, fare_amount, vehicle_id 
FROM FleetDB.Completed_Rides 
WHERE zone_id = 'Electronic_City' AND distance_km > 20.0 ALLOW FILTERING;
```

**Example 3: Time-Series Burst Analytics**
```sql
SELECT ride_time, distance_km, fare_amount 
FROM FleetDB.Completed_Rides 
WHERE zone_id = 'Koramangala' 
  AND ride_time >= '2026-05-12T00:00:00.000+0000' 
  AND ride_time <= '2026-05-12T23:59:59.000+0000';
```

## License
This project is open-source and available under the [MIT License](LICENSE).
```
Use however the F you want I dont care
```
