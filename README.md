# 🚲 Cyclistic Bike-Share Capstone Project – Google Data Analytics Certificate

## 📌 Project Summary

This project was developed as part of the **Google Data Analytics Professional Certificate (Capstone Project)**. The main objective was to analyze 12 months of Cyclistic bike-share trip data and draw insights to help the business answer:

> **"How do annual members and casual riders use Cyclistic bikes differently?"**

I worked through the entire data pipeline myself — from data collection and storage to cleaning, transformation, and finally creating a Power BI dashboard for visualization.

---

## 📁 Data Source

I obtained the data from the official Divvy/Cyclistic public dataset repository:  
🔗 [Divvy Trip Data – 12 Months](https://divvy-tripdata.s3.amazonaws.com/index.html)

Each file in the dataset represented one month of ride data in `.csv` format. I downloaded the most recent **12 monthly files**.

---

## 🛠 Tools Used

- **Python** (pandas, sqlalchemy) – for automating data loading into MySQL  
- **MySQL** – for data cleaning and transformations  
- **Power BI** – for dashboard creation and analysis  
- **SQL** – to write custom queries and aggregate the data

---

## 🔄 My Workflow

### ✅ Step 1: Downloading and Organizing Data

I downloaded the 12 most recent `.csv` files from the above link and saved them into a folder on my local system. Each file contains ride-level details like user type, timestamps, station info, etc.

---

### ✅ Step 2: Importing CSVs into MySQL using Python

To efficiently store and manage the data, I decided to import all the `.csv` files into a MySQL database. I used Python for this task. Here’s the script I wrote to automate the process:

```python
import os
import pandas as pd
from sqlalchemy import create_engine

# Folder containing the CSV files
csv_folder = r"C:\Users\DELL\OneDrive\Desktop\Google Data Analytics Certificate\cAPSTONE\case_study-1\Last_12_Months_Data"

# Database connection details
database = 'cyclistic_data'
user = 'root'
password = 'root'
host = 'localhost'
port = 3306

# Creating a connection engine
engine = create_engine(f'mysql+pymysql://{user}:{password}@{host}:{port}/{database}')

# Loop to import each CSV into MySQL
for file in os.listdir(csv_folder):
    if file.endswith('.csv'):
        file_path = os.path.join(csv_folder, file)
        table_name = os.path.splitext(file)[0].lower()
        df = pd.read_csv(file_path)
        df.to_sql(name=table_name, con=engine, index=False, if_exists='replace')
        print(f'Imported {file} into table `{table_name}`')

print("All files imported successfully!")
