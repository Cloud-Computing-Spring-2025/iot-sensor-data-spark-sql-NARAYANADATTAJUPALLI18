# 🚀 IoT Sensor Data Analysis with Apache Spark SQL

This project showcases how to analyze IoT sensor data using **Apache Spark SQL**. You’ll work with a simulated dataset of sensor readings—covering temperature, humidity, location, and type—collected over time. Throughout the project, you will:

- Load and inspect structured data
- Run SQL queries for filtering and aggregation
- Analyze temporal patterns
- Use window functions for ranking
- Build pivot tables for comparative analysis

Each task produces a final CSV output stored in the `output/` directory.

---

## 🛠️ Prerequisites

To run the project locally, make sure you have the following:

### ✅ Tools & Libraries

- **Apache Spark** 3.x
- **Python** 3.8+
- Python packages:
  ```bash
  pip install pyspark faker
  ```

---

## 📦 Dataset Generation

Sensor data is simulated using the `data_generator.py` script. It generates a realistic `sensor_data.csv` file containing timestamped readings.

To create the dataset:

```bash
python data_generator.py
```

> Output: `input/sensor_data.csv`

---

## 📁 Project Structure

```
iot-sensor-data-spark-sql/
├── input/
│   └── sensor_data.csv               # Generated sensor readings
├── output/                           # Output CSVs for each task
├── src/
│   ├── task1_load_explore.py         # Load data and basic exploration
│   ├── task2_filter_aggregate.py     # Filter data and aggregate by location
│   ├── task3_time_analysis.py        # Time-based temperature trends
│   ├── task4_sensor_ranking.py       # Rank sensors using window functions
│   └── task5_pivot_hour_location.py  # Pivot table by hour and location
├── data_generator.py                 # Script to simulate sensor data
└── README.md                         # Project overview and instructions
```

---

## 📊 Task Overview

### 🔍 Task 1: Load & Explore Data

**Objective**: Read the CSV data, register a temp view, and explore key metrics.

- Load `sensor_data.csv` with schema inference
- Register as a temporary SQL view: `sensor_readings`
- Run queries:
  - View first 5 rows
  - Count total records
  - List unique locations
- Output saved to: `output/task1_output`

---

### 📈 Task 2: Filtering & Aggregation

**Objective**: Filter readings by temperature range and compute averages per location.

- Filter: temperatures within 18–30 °C (in-range) and out-of-range
- Group by `location` to calculate:
  - `AVG(temperature)`
  - `AVG(humidity)`
- Sort results by average temperature
- Output saved to: `output/task2_output`

---

### 🕒 Task 3: Hourly Temperature Analysis

**Objective**: Understand temperature patterns across the day.

- Convert string `timestamp` to `TimestampType`
- Extract `HOUR(timestamp)`
- Group by hour, compute `AVG(temperature)`
- Output saved to: `output/task3_output`

---

### 🏆 Task 4: Ranking Sensors

**Objective**: Use window functions to rank sensors by average temperature.

- Group by `sensor_id`
- Compute average temperature
- Apply `DENSE_RANK()` window function (descending order)
- Select top 5 sensors
- Output saved to: `output/task4_output`

---

### 📊 Task 5: Pivot Table by Hour & Location

**Objective**: Visualize how temperature varies by location and time of day.

- Extract `hour_of_day` from timestamps
- Group by `location` and `hour_of_day`, compute average temperature
- Pivot: rows → locations, columns → hour of day
- Identify the (location, hour) combination with the highest temperature
- Output saved to: `output/task5_output`

---

## ⚠️ Troubleshooting & Common Errors

### ❌ CSV Sink Mode Errors

**Issue**: Error due to using `update`/`complete` modes with CSV

**Fix**: Use `foreachBatch()` and write each batch manually.

---

### ❌ Cannot Write `window` Column to CSV

**Issue**: Spark’s `window` column is a struct and not CSV-friendly

**Fix**: Extract and flatten `window.start` and `window.end` into separate columns.

---

### ❌ SQL Pivot Not Working

**Issue**: SQL doesn’t support pivoting directly

**Fix**: Use SQL for aggregation, then switch to the PySpark DataFrame API for pivoting.

---

## ✅ Summary & Takeaways

Through this hands-on assignment, you learned to:

- Load and explore structured IoT data using Spark
- Perform filtering and aggregation with Spark SQL
- Analyze time-series patterns
- Rank data using window functions
- Use pivoting for multidimensional comparisons

By combining SQL queries with the PySpark DataFrame API, you built a powerful and flexible data analysis workflow—ideal for real-world IoT applications. 🎯
