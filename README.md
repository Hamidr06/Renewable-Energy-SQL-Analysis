# ⚡ Renewable Energy Grid Production Analysis (SQL)

## 📌 Project Overview
This project focuses on evaluating grid energy production from renewable sources (Solar and Wind) using historical time-series data. The primary objective was to model raw data in MySQL, handle dynamic date parsing challenges, and extract data-driven insights on seasonal performance, hourly peak loads, and annual growth to optimize grid distribution and infrastructure planning.

## 🛠️ Tech Stack & SQL Skills Exhibited
* **Database:** MySQL Workbench
* **SQL Mechanics:** Advanced Aggregations (`SUM`, `AVG`, `COUNT`), Conditional Logic (`CASE WHEN`), String Manipulation (`RIGHT`), and Multi-level Sorting (`ORDER BY`).

---

## 📂 Database Architecture & Setup
The project involves ingesting large-scale hourly generation records into a structured relational schema. String format storage was implemented for dates to bypass structural conflicts during external CSV ingestion.

### 1. Database & Table Initialization
```sql
USE energy_project;

CREATE TABLE energy_production (
    Date VARCHAR(50),      
    Start_Hour INT,
    End_Hour INT,
    Source VARCHAR(50),
    Day_of_Year INT,
    Day_Name VARCHAR(20),
    Month_Name VARCHAR(20),
    Season VARCHAR(20),
    Production INT
);
📊 Key Data Insights & Analytical Queries
1. Overall Asset Contribution and Efficiency
SQL
SELECT Source, 
       SUM(Production) AS Total_Production, 
       AVG(Production) AS Avg_Hourly_Production, 
       COUNT(*) AS Total_Hours_Recorded 
FROM energy_production 
GROUP BY Source;
2. Peak Energy Production Hours Analysis
SQL
SELECT Start_Hour,
       End_Hour,
       Source,
       ROUND(AVG(Production), 2) AS Avg_Production 
FROM energy_production 
GROUP BY Start_Hour, End_Hour, Source 
ORDER BY Avg_Production DESC 
LIMIT 10;
3. Seasonal Infrastructure Impact
SQL
SELECT Season,
       Source, 
       SUM(Production) AS Total_Production,
       ROUND(AVG(Production), 2) AS Avg_Production 
FROM energy_production 
GROUP BY Season, Source 
ORDER BY Total_Production DESC;
4. Annual Energy Production Summary (YoY Trend)
SQL
SELECT RIGHT(Date, 4) AS Production_Year,
       Source, 
       SUM(Production) AS Total_Production 
FROM energy_production 
WHERE Source IN ('Solar', 'Wind') 
GROUP BY RIGHT(Date, 4), Source 
ORDER BY Production_Year ASC, Source;
5. Day-Type Variance Analysis (Weekdays vs Weekends)
SQL
SELECT CASE 
        WHEN Day_Name IN ('Saturday', 'Sunday') THEN 'Weekend' 
        ELSE 'Weekday' 
       END AS Day_Type,
       Source,
       ROUND(AVG(Production), 2) AS Avg_Production, 
       SUM(Production) AS Total_Production 
FROM energy_production 
GROUP BY CASE 
        WHEN Day_Name IN ('Saturday', 'Sunday') THEN 'Weekend' 
        ELSE 'Weekday' 
       END, 
       Source 
ORDER BY Source, Avg_Production DESC;
6. High-Performing Months Analysis
SQL
SELECT Month_Name,
       Source,
       ROUND(AVG(Production), 2) AS Avg_Monthly_Production, 
       SUM(Production) AS Total_Monthly_Production 
FROM energy_production 
GROUP BY Month_Name, Source 
ORDER BY Avg_Monthly_Production DESC;
🚀 Next Steps
Connect this verified MySQL data warehouse into Power BI to deploy an interactive dashboard tracking operational health and asset production peaks.


Ise paste karke jaise hi aap green **"Commit changes..."** button par click karenge, aapka `README.md` alag-alag headings aur dark-themed code blocks ke sath ekdam industry standard ka lagne lagega! 

Iske baad aapka SQL repository part poori tarah se ready hai. Jab aapka dashboard ka plan ho, tab hum aage Power BI par kaam shuru karenge.
