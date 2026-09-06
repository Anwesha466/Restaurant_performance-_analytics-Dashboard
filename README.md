# 🍽️ Restaurant Performance Analytics Dashboard

> An end-to-end data analytics project analyzing 8,680+ restaurant records across 9 Indian cities — built with Excel, Python, MySQL, and Power BI.

---

## 📌 Project Description

This project simulates a real-world business analytics workflow for Indian restaurant data sourced from Swiggy. Starting from a raw, messy dataset, the project covers the full pipeline — data cleaning, database design, ETL scripting, and interactive dashboard development — to deliver actionable insights for stakeholders across marketing, operations, and strategy teams.

The final deliverable is a **3-page interactive Power BI dashboard** with dynamic slicers, KPI cards, and charts tracking cuisine popularity, pricing trends, customer ratings, delivery efficiency, and restaurant performance across Indian cities.

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| **Microsoft Excel + Power Query** | Data cleaning, format standardization, duplicate flagging, cuisine extraction |
| **Python (Pandas + SQLAlchemy)** | ETL pipeline — reading cleaned CSVs and loading into MySQL |
| **MySQL** | Relational database storage and querying |
| **Power BI Desktop** | Interactive dashboard development, DAX measures, data modeling |

---

## 📂 Project Structure

```
restaurant_performance_analytics_dashboard/
├── data/
│   ├── swiggy.csv                  # Raw dataset
│   ├── restaurants_cleaned.csv     # Cleaned restaurant-level data
│   └── cuisine_exploded.csv        # Normalized cuisine data (one row per cuisine)
├── scripts/
│   └── load_to_mysql.py            # Python ETL script
├── dashboard/
│   ├── restaurant_performance_analytics_dashboard.pbix   # Power BI file
│   
└── README.md
```

---

## 📊 Dataset

- **Source:** Public Swiggy restaurant dataset (Kaggle)
- **Size:** 8,680 restaurant records
- **Coverage:** 9 Indian cities — Bangalore, Mumbai, Chennai, Kolkata, Hyderabad, Delhi, Pune, Ahmedabad, Surat
- **Columns:** Restaurant ID, Name, Area, City, Cuisine, Price, Average Rating, Total Ratings, Delivery Time, Address

---

## 🔄 Pipeline Overview

### Step 1: Data Cleaning (Excel + Power Query)
- Identified and fixed encoding issue in `Price` column (`? 1,000.00` → numeric)
- Trimmed whitespace from `Area`, `Restaurant`, and `Address` columns
- Flagged 7 duplicate listings using `COUNTIFS` (same Restaurant + Area + City)
- Extracted `Primary Cuisine` from multi-valued `Food type` field
- Created separate `cuisine_exploded` table (one row per cuisine per restaurant) for accurate cuisine-level analysis
- Exported cleaned tables as CSVs for Python ingestion

### Step 2: ETL Pipeline (Python)
- Read cleaned CSVs using `pandas`
- Connected to MySQL using `SQLAlchemy`
- Loaded `restaurants_cleaned` and `cuisine_exploded` as tables into `restaurant_db`
- Verified row counts and data types post-load

```python
from sqlalchemy import create_engine
import pandas as pd

engine = create_engine("mysql+mysqlconnector://root:password@localhost/swiggy_db")
df = pd.read_csv("restaurants_cleaned.csv")
df.to_sql("restaurants", con=engine, if_exists="replace", index=False)
```

### Step 3: Database (MySQL)
- Created `restaurant_db` database in MySQL Workbench
- Stored two normalized tables: `restaurant_cleaned` and `cuisine_exploded`
- Validated data integrity with SQL queries (`COUNT`, `GROUP BY`, `AVG`)

### Step 4: Dashboard Development (Power BI)
- Connected Power BI to MySQL via MySQL Connector/NET
- Built data model with relationships between `restaurant_cleaned` and `cuisine_exploded`
- Created DAX measures:
  - `Rating Band` for distribution analysis
  - `Performance Flag` to identify underperforming restaurants
  - `Restaurant Label` combining name + area for unique identification
- Built 3-page interactive dashboard with City and Cuisine slicers

---

## 📈 Dashboard Pages

### Page 1: Overview
- KPI Cards — Total Restaurants (8.675K), Average Cost (₹348.65), Average Rating (3.66)
- Restaurant Count by City
- Average Price by City
- Average Ratings by City
- Average Delivery Time by City

### Page 2: Deep Dive
- Cuisine Popularity (stacked bar chart by city)
- Top 10 Restaurants by Rating (with price comparison)
- Price vs Rating Scatter Plot
- City and Cuisine slicers for interactive filtering

### Page 3: Market Insights
- Average Price by Cuisine
- Rating Distribution (Donut chart — 5 rating bands)
- Underperforming Restaurants (high price, low rating flag)

---

## 🔍 Key Findings

1. **Mumbai has the highest average restaurant price** (~₹400) — significantly above the dataset average of ₹348
2. **South Indian metros lead in customer satisfaction** — Chennai, Bangalore, and Hyderabad consistently rank highest in average ratings
3. **Chinese and North Indian cuisines dominate** across all 9 cities — highest restaurant count by cuisine type
4. **Price and rating show no strong correlation** — expensive restaurants do not reliably score higher ratings
5. **Kolkata has the highest restaurant density** — 1,400+ listings, making it the most competitive market in the dataset
6. **Delivery times vary significantly by city** — operational efficiency differs across geographies
7. **Underperforming restaurants identified** — restaurants charging above-average prices (>₹348) while rating below average (<3.66) flagged for quality improvement

---

## 💡 Business Recommendations

1. **Focus marketing spend** on Chinese and North Indian cuisines — highest demand across all cities
2. **Quality intervention needed** in Delhi and Pune — both rank lower on average customer ratings
3. **Investigate Surat** as a potential expansion market — fewer listings but decent ratings suggest underserved demand
4. **Prioritize delivery optimization** in Kolkata and Mumbai — highest restaurant density requires efficient logistics
5. **Flag underperforming restaurants** for partner quality programs — high price + low rating is a churn risk for customers

---

## 🚀 How to Run

1. Clone the repository
2. Install Python dependencies:
   ```bash
   pip install pandas sqlalchemy mysql-connector-python openpyxl
   ```
3. Set up MySQL — create database:
   ```sql
   CREATE DATABASE swiggy_db;
   ```
4. Run the ETL script:
   ```bash
   python scripts/load_to_mysql.py
   ```
5. Open `restaurant_performance_analytics_dashboard.pbix` in Power BI Desktop
6. Refresh the data connection

---

## 👩‍💻 Author

**Anwesha** — B.Tech Computer Science, ITER SOA University (2023–2027)  
Targeting Data Analyst roles | Excel • Python • SQL • Power BI

---

*Dataset sourced from Kaggle — publicly available swiggy restaurant listings.*
