# PySpark GeoData Labeling

## 📌 Overview

This project demonstrates how to process geospatial request logs with Apache Spark. It cleans suspicious duplicate records from the data and assigns each request to the closest Point of Interest (POI) based on geodesic distance calculations.

## 🚀 Features

- **Data Cleanup** – Filters out request records with identical timestamp and geolocation to avoid suspicious duplicates.
- **POI Labeling** – Assigns each request to the geographically nearest POI.
- **Geodesic Distance Calculation** – Uses a custom Spark UDF with `geopy` to measure distances.
- **Scalable Spark Workflow** – Designed to handle large datasets efficiently.

## 📦 Environment Setup

You can run the project via Docker with Jupyter Notebook:

```bash
docker-compose up
```

Then follow the prompt URL to open the Jupyter Notebook UI (e.g., `http://127.0.0.1:8888/?token=<SOME_TOKEN>`).
The data/ directory is mounted inside the container at `~/data/` for easy access to the CSV files:
```python
import os
from pyspark.sql import SparkSession

spark = SparkSession.builder.master('local').getOrCreate()

df = spark.read.options(
    header='True',
    inferSchema='True',
    delimiter=',',
).csv(os.path.expanduser('~/data/DataSample.csv'))
```

## 🛠 How It Works

1. **Data Loading**  
   The application reads two CSV files: one containing request logs (`DataSample.csv`) and another listing Points of Interest (POIs) (`POIList.csv`). Columns are renamed for clarity.

2. **Cleanup Step**  
   Records in the request logs that share the same timestamp and geographic coordinates are considered suspicious duplicates and are filtered out.

3. **Labeling Step**  
   Each request is paired with every POI using a cross join. The geodesic (earth surface) distance between the request location and each POI is calculated. Each request is then assigned to the closest POI based on the minimum distance.

4. **Duplicate Check**  
   After labeling, the system checks if any request IDs are duplicated. Duplicates occur when multiple POIs share the exact same coordinates, causing ambiguity in closest POI assignment. The code prints a warning if duplicates are found.

5. **Result**  
   The labeled data, with each request matched to its nearest POI, is ready for further analysis or processing.

## 📚 Data Files
- `data/DataSample.csv` — Request logs with timestamps and geocoordinates.
- `data/POIList.csv` — Points of Interest with geographic locations.

## ⚠️ Common Issues
- Duplicate IDs after labeling may occur if multiple POIs share the same location.
- Ensure data paths in notebook or scripts match the mounted volume paths (~/data/).

## ▶️ Usage
Run the notebook cells in order to:

- Load and clean the data
- Label requests with closest POI
- Inspect and handle duplicate entries if needed

## 📚 Technologies Used

- **Python** – Primary programming language.
- **PySpark** – Distributed data processing framework for handling large datasets.
- **Geopy** – Geospatial Python library for calculating distances between latitude/longitude points.
- **Jupyter Notebook** – Interactive development environment for running and testing code.
- **Docker & Docker Compose** – Containerization and environment management for reproducible setups.
