# 📊 Air Quality and Health Impact Analysis with SQL

This project demonstrates the process of importing a CSV file containing air quality and health data into a new SQLite database and performing advanced SQL queries to extract meaningful insights.

---

## 🗂️ Dataset Overview

The dataset `air_quality_health_impact_data.csv` contains records of air pollution metrics and health-related outcomes, including:

- **Air Quality Indicators**: `AQI`, `PM10`, `PM2_5`, `NO2`, `SO2`, `O3`
- **Weather Metrics**: `Temperature`, `Humidity`, `WindSpeed`
- **Health Metrics**: `RespiratoryCases`, `CardiovascularCases`, `HospitalAdmissions`, `HealthImpactScore`, `HealthImpactClass`

---

## 🔄 CSV to SQL Conversion Process

We used **Python with Pandas and SQLite3** to convert the CSV into a new SQL database.

```python
import pandas as pd
import sqlite3

# Load CSV into a DataFrame
df = pd.read_csv("air_quality_health_impact_data.csv")

# Create a new SQLite database and connect
conn = sqlite3.connect("air_quality.db")

# Write the DataFrame to a SQL table
df.to_sql("air_quality_data", conn, if_exists="replace", index=False)

# Close the connection
conn.close()
```
## 📘 SQL Concepts:
----
#### 1. 🧠 Subqueries
#### Query: Find all records where `HealthImpactScore` is above the average.

```sql
SELECT *
FROM air_quality_data
WHERE HealthImpactScore > (
  SELECT AVG(HealthImpactScore)
  FROM air_quality_data
);
```
---
### 2. 🧱 Common Table Expressions (CTE)
#### Query: Calculate average `AQI` and `HealthImpactScore` for each `HealthImpactClass`.

```sql
WITH ImpactStats AS (
  SELECT HealthImpactClass,
         AVG(AQI) AS avg_aqi,
         AVG(HealthImpactScore) AS avg_score
  FROM air_quality_data
  GROUP BY HealthImpactClass
)
SELECT * FROM ImpactStats;
```
---
### 3. 🎯 Filtering
#### Query: Retrieve records where `HealthImpactClass = 3` and combined respiratory and cardiovascular cases exceed 10.

```sql
SELECT *
FROM air_quality_data
WHERE HealthImpactClass = 3
  AND (RespiratoryCases + CardiovascularCases) > 10;
```
---
### 4. 📊 Group By with CASE
#### Query: Group HospitalAdmissions by Wind Speed Ranges.

```sql
SELECT 
  CASE 
    WHEN WindSpeed < 5 THEN '0-5'
    WHEN WindSpeed BETWEEN 5 AND 10 THEN '5-10'
    WHEN WindSpeed BETWEEN 11 AND 15 THEN '11-15'
    ELSE '15+'
  END AS WindSpeedRange,
  SUM(HospitalAdmissions) AS total_admissions
FROM air_quality_data
GROUP BY WindSpeedRange;
```
---
## ✅ Key Takeaways
- Subqueries allow you to filter based on calculated aggregates.
- CTEs make complex queries readable and reusable.
- Filtering with conditions helps extract precise subsets of the data.
- GROUP BY combined with CASE helps analyze trends within custom ranges.
---
## 🛠 Tools Used
- Python (pandas, sqlite3)
- SQLite




