# 🚲 Cyclistic Bike-Share Capstone Project – Google Data Analytics Certificate

## 📌 Project Summary

As part of the **Google Data Analytics Professional Certificate**, I completed this capstone project to analyze 12 months of trip data from the Cyclistic bike-share program. The goal was to understand the usage patterns of **annual members** versus **casual riders** and provide insights that could help the company improve its marketing strategies.

I independently handled the end-to-end process — from data extraction and transformation to storing, cleaning, and analyzing the data — and finally visualized the insights using Power BI.

---

## 📁 Data Source

I collected the data from Cyclistic’s public dataset repository:  
🔗 [Divvy Trip Data – 12 Months](https://divvy-tripdata.s3.amazonaws.com/index.html)

Each `.csv` file contains one month of ride details including timestamps, user types, station info, and rideable types. I downloaded the **most recent 12 months of data** and organized them locally.

---

## 🛠 Tools & Technologies

- **Python** (with pandas & SQLAlchemy): To automate the process of importing `.csv` files into MySQL  
- **MySQL**: For data cleaning, transformation, and querying  
- **Power BI**: To build interactive dashboards and generate insights  
- **SQL**: To create summary views and engineer custom fields

---

## 🔄 Project Workflow

### ✅ Step 1: Data Collection & Organization

I downloaded the latest 12 months of Cyclistic ride data and saved all `.csv` files into a designated local folder. Each file represented one month of trip-level data.

---

### ✅ Step 2: Importing CSVs into MySQL Using Python

To manage the dataset efficiently, I wrote a Python script to import all the `.csv` files directly into MySQL tables. Below is the script I created:

python
import os
import pandas as pd
from sqlalchemy import create_engine

# Path to the folder containing CSV files
csv_folder = r"C:\Users\DELL\OneDrive\Desktop\Google Data Analytics Certificate\cAPSTONE\case_study-1\Last_12_Months_Data"

# MySQL database connection setup
database = 'cyclistic_data'
user = 'user'
password = 'password'
host = 'localhost'
port = port

# Create the SQLAlchemy engine
engine = create_engine(f'mysql+pymysql://{user}:{password}@{host}:{port}/{database}')

# Import each CSV file into a MySQL table
for file in os.listdir(csv_folder):
    if file.endswith('.csv'):
        file_path = os.path.join(csv_folder, file)
        table_name = os.path.splitext(file)[0].lower()
        df = pd.read_csv(file_path)
        df.to_sql(name=table_name, con=engine, index=False, if_exists='replace')
        print(f"Imported {file} into table `{table_name}`")

print("✅ All files imported successfully!")


---

### ✅ Step 3: Combining All Monthly Tables into One

After loading the files into MySQL, I created a unified table to consolidate all 12 months of data. Here’s the SQL query I used:

DROP TABLE IF EXISTS cyclistic_data.last_12_months_data_tripdata;

CREATE TABLE cyclistic_data.last_12_months_data_tripdata AS
SELECT * FROM cyclistic_data.`202304-divvy-tripdata`
UNION ALL SELECT * FROM cyclistic_data.`202305-divvy-tripdata`
UNION ALL SELECT * FROM cyclistic_data.`202306-divvy-tripdata`
UNION ALL SELECT * FROM cyclistic_data.`202307-divvy-tripdata`
UNION ALL SELECT * FROM cyclistic_data.`202308-divvy-tripdata`
UNION ALL SELECT * FROM cyclistic_data.`202309-divvy-tripdata`
UNION ALL SELECT * FROM cyclistic_data.`202310-divvy-tripdata`
UNION ALL SELECT * FROM cyclistic_data.`202311-divvy-tripdata`
UNION ALL SELECT * FROM cyclistic_data.`202312-divvy-tripdata`
UNION ALL SELECT * FROM cyclistic_data.`202401-divvy-tripdata`
UNION ALL SELECT * FROM cyclistic_data.`202402-divvy-tripdata`
UNION ALL SELECT * FROM cyclistic_data.`202403-divvy-tripdata`;


---

### ✅ Step 4: Data Cleaning & Feature Engineering

To improve data quality and enable effective analysis, I filtered out invalid entries (e.g. trips longer than 24 hours) and added new features like ride duration, weekday names, and weekend vs. weekday labels.

Here’s the SQL transformation I applied:

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
    start_station_id,
    end_station_name,
    end_station_id
FROM cyclistic_data.last_12_months_data_tripdata
WHERE TIMESTAMPDIFF(HOUR, started_at, ended_at) <= 24
ORDER BY started_at;

---

### ✅ Step 5: Connecting MySQL to Power BI

Once the cleaned data was ready, I connected Power BI to the MySQL database using the following settings:

Server: localhost

Port: 3306

Database: cyclistic_data

Authentication: Basic (root user)

I imported the cleaned dataset directly into Power BI for visualization.


---

### 📊 Insights & Analysis

Using Power BI, I created interactive visuals and dashboards to explore trends such as:

🧍‍♂️ Differences in usage between casual and member riders

📆 Popular days and times for each rider type

🚲 Ride duration comparison

📍 Most used start/end stations

🔁 Weekday vs weekend behavior

These insights can be used by Cyclistic to develop targeted marketing campaigns aimed at converting casual users into loyal annual members.


---

### 📁 Project Folder Structure

📦 cyclistic-capstone
┣ 📁 data/                 ← Raw CSV files
┣ 📁 scripts/              ← Python import scripts
┣ 📁 sql/                  ← SQL scripts for transformation
┣ 📊 PowerBI_Report.pbx    ← Power BI Dashboard
┗ 📄 README.md             ← Project summary & documentation

---## **About Me**
I am an aspiring Data Analyst looking for a career transition, with a background as an Engineer in Project Sales. Passionate about transforming raw data into meaningful insights, I aim to leverage analytical skills to drive data-driven decision-making.  

Connect with me on [LinkedIn](https://www.linkedin.com/in/naveen-surla-587565242/) or explore more projects on [GitHub](https://github.com/naveensurla).

