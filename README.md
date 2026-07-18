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
-- 1. Select Active Database
USE energy_project;

-- 2. Create Table Structure for Wind & Solar.
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
-- Check data is import succesfully.
SELECT * FROM energy_production LIMIT 10;

-- Analysis 1: Solar vs Wind (Total Performance Analysis)
SELECT 
    Source, 
    SUM(Production) AS Total_Production,
    AVG(Production) AS Avg_Hourly_Production,
    COUNT(*) AS Total_Hours_Recorded
FROM energy_production
GROUP BY Source;

-- Analysis 2: Peak Energy Production Hours Analysis
SELECT 
    Start_Hour,
    End_Hour,
    Source,
    ROUND(AVG(Production), 2) AS Avg_Production
FROM energy_production
GROUP BY Start_Hour, End_Hour, Source
ORDER BY Avg_Production DESC
LIMIT 10;

-- Analysis 3: Seasonal Impact on Energy Generation
SELECT 
    Season,
    Source,
    SUM(Production) AS Total_Production,
    ROUND(AVG(Production), 2) AS Avg_Production
FROM energy_production
GROUP BY Season, Source
ORDER BY Total_Production DESC;

-- Analysis 4: Total Energy production by year and Source.
SELECT 
    RIGHT(Date, 4) AS Production_Year,
    Source,
    SUM(Production) AS Total_Production
FROM energy_production
WHERE Source IN ('Solar', 'Wind')
GROUP BY RIGHT(Date, 4), Source
ORDER BY Production_Year ASC, Source;

-- Analysis 5: Comparing average and total production between weekdays and weekends.
SELECT 
    CASE 
        WHEN Day_Name IN ('Saturday', 'Sunday') THEN 'Weekend'
        ELSE 'Weekday'
    END AS Day_Type,
    Source,
    ROUND(AVG(Production), 2) AS Avg_Production,
    SUM(Production) AS Total_Production
FROM energy_production
GROUP BY 
    CASE 
        WHEN Day_Name IN ('Saturday', 'Sunday') THEN 'Weekend'
        ELSE 'Weekday'
    END, 
    Source
ORDER BY Source, Avg_Production DESC;

-- Analysis 6: Finding which months yield the highest average energy production.
SELECT 
    Month_Name,
    Source,
    ROUND(AVG(Production), 2) AS Avg_Monthly_Production,
    SUM(Production) AS Total_Monthly_Production
FROM energy_production
GROUP BY Month_Name, Source
ORDER BY Avg_Monthly_Production DESC
);
