# 🚲 Cyclistic Bike-Share Capstone Project  
**Google Data Analytics Professional Certificate**

---

## 📌 Project Overview

As part of the **Google Data Analytics Professional Certificate**, I completed this end-to-end capstone project to analyze **12 months of ride data** from the Cyclistic bike-share program. The primary goal was to explore usage patterns between **annual members** and **casual riders**, and generate actionable insights to support **marketing strategies** that encourage casual riders to convert to members.

I handled the full data analysis lifecycle — from **data extraction**, **loading**, and **cleaning**, to **transforming**, **analyzing**, and **visualizing** the insights using **MySQL**, **Python**, and **Power BI**.

---

## 📁 Data Source

I utilized publicly available trip data from Cyclistic:  
🔗 [Divvy Trip Data – 12 Months](https://divvy-tripdata.s3.amazonaws.com/index.html)

Each `.csv` file corresponds to one month of ride data, containing details such as timestamps, user types, start/end stations, and bike types. I downloaded the **latest 12 months** and organized them locally for analysis.

---

## 🛠 Tools & Technologies Used

| Tool          | Purpose                                      |
|---------------|----------------------------------------------|
| **Python** (Pandas, SQLAlchemy) | Automated CSV import into MySQL |
| **MySQL**      | Data cleaning, transformation, and queries  |
| **Power BI**   | Dashboard creation and data visualization   |
| **SQL**        | Feature engineering and summary views       |

---

## 🔄 Project Workflow

### ✅ Step 1: Data Collection & Organization
- Downloaded the latest 12 `.csv` files (one per month).
- Stored them in a structured local folder for easy access.

---

### ✅ Step 2: Automated Import to MySQL using Python

I developed a Python script to streamline the import process:

```python
import os
import pandas as pd
from sqlalchemy import create_engine

csv_folder = r"C:\Users\DELL\OneDrive\Desktop\Google Data Analytics Certificate\CAPSTONE\case_study-1\Last_12_Months_Data"

# Database connection
engine = create_engine('mysql+pymysql://user:password@localhost:3306/cyclistic_data')

# Import each CSV into MySQL
for file in os.listdir(csv_folder):
    if file.endswith('.csv'):
        file_path = os.path.join(csv_folder, file)
        table_name = os.path.splitext(file)[0].lower()
        df = pd.read_csv(file_path)
        df.to_sql(name=table_name, con=engine, index=False, if_exists='replace')
        print(f"Imported {file} into table `{table_name}`")
```

✅ **Result:** All CSVs were successfully imported into MySQL tables.

---

### ✅ Step 3: Consolidating All Monthly Tables

To enable analysis across all months, I combined the individual tables into one unified dataset:

```sql
DROP TABLE IF EXISTS cyclistic_data.last_12_months_data_tripdata;

CREATE TABLE cyclistic_data.last_12_months_data_tripdata AS last_12_months_data_tripdata

SELECT * FROM cyclistic_data.`202304-divvy-tripdata`
UNION ALL
SELECT * FROM cyclistic_data.`202305-divvy-tripdata`
UNION ALL
SELECT * FROM cyclistic_data.`202306-divvy-tripdata`
UNION ALL
SELECT * FROM cyclistic_data.`202307-divvy-tripdata`
UNION ALL
SELECT * FROM cyclistic_data.`202308-divvy-tripdata`
UNION ALL
SELECT * FROM cyclistic_data.`202309-divvy-tripdata`
UNION ALL
SELECT * FROM cyclistic_data.`202310-divvy-tripdata`
UNION ALL
SELECT * FROM cyclistic_data.`202311-divvy-tripdata`
UNION ALL 
SELECT * FROM cyclistic_data.`202312-divvy-tripdata`
UNION ALL 
SELECT * FROM cyclistic_data.`202401-divvy-tripdata`
UNION ALL 
SELECT * FROM cyclistic_data.`202402-divvy-tripdata`
UNION ALL 
SELECT * FROM cyclistic_data.`202403-divvy-tripdata`;
```

---

### ✅ Step 4: Data Cleaning & Feature Engineering

To improve quality and extract key insights, I:

- Removed rides longer than 24 hours (likely invalid).
- Created new columns:
  - `duration_in_hours`
  - `day_name` (weekday name)
  - `day_type` (Weekend vs. Weekday)

SQL Transformation:

```sql
SELECT
    ride_id,
    rideable_type,
    started_at,
    ended_at,
    TIMESTAMPDIFF(HOUR, started_at, ended_at) AS duration_in_hours,
    DAYNAME(started_at) AS day_name,
    CASE 
        WHEN DAYOFWEEK(started_at) IN (1, 7) THEN 'Weekend'
        ELSE 'Weekday'
    END AS day_type,
    member_casual,
    start_station_name,
    end_station_name
FROM cyclistic_data.last_12_months_data_tripdata
WHERE TIMESTAMPDIFF(HOUR, started_at, ended_at) <= 24
ORDER BY started_at;
```

---

### ✅ Step 5: Connecting to Power BI

I connected Power BI directly to the cleaned MySQL dataset using:

- **Server:** localhost  
- **Port:** 3306  
- **Database:** `cyclistic_data`  
- **Authentication:** Basic (root user)

This allowed for dynamic and real-time data visualization.

---

## 📊 Key Insights & Dashboards

Using Power BI, I built interactive dashboards to explore:

- 👥 **Rider Type Comparison**: Members vs Casual usage trends  
- 🗓️ **Day/Time Trends**: Peak usage days, weekdays vs weekends  
- ⏱️ **Ride Duration Patterns**: Average trip times by user type  
- 📍 **Station Hotspots**: Most frequently used start/end stations  
- 🔁 **Behavioral Patterns**: Membership tendencies over time  

📈 These insights are valuable for Cyclistic’s marketing team to develop **targeted campaigns** that convert **casual riders into annual members**.

---

## 📂 Project Structure

```
cyclistic-capstone/
├── data/                # Raw CSV files
├── scripts/             # Python scripts for data import
├── sql/                 # SQL scripts for transformation
├── PowerBI_Report.pbx   # Final Power BI dashboard
└── README.md            # Project documentation
```

---

## 👤 About Me

I am an **aspiring Data Analyst** transitioning from a background in **Project Sales Engineering**. With a passion for turning raw data into actionable insights, I aim to leverage my technical and analytical skills to support data-driven decision-making in business environments.

- 🔗 [LinkedIn – Naveen Surla](https://www.linkedin.com/in/naveen-surla-587565242/)  
- 💻 [GitHub – naveensurla](https://github.com/naveensurla)
