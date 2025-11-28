#  💹 Power BI Financial_Data

## 📝 Description

Step-by-step data analytics project showcasing the full process of transforming raw Excel financial data into an interactive Power BI dashboard. 

Includes **`data cleaning`**, **`creating a model`** and **`creating visualizations`** using the Financial Sample Dataset from Microsoft https://learn.microsoft.com/en-us/power-bi/create-reports/sample-financial-download.

---

## 📊 Data

The dataset contains detailed financial information for a fictional global company operating across multiple markets, product categories, and customer segments. Data is designed to simulate real-world business performance and serves as a foundation for end-to-end data analytics project.

Data is organized in a single table containing 700 observations representing monthly sales records from 2013–2014. Each record includes key business metrics such as revenue, cost, profit, and discounts, allowing for comprehensive profitability analysis across regions and product lines.

Main fields include:
- **`Segment`** - customer group classification (*Government, Midmarket, Channel Partners, Enterprise*)
- **`Country`** - gegraphical market where sales were made,
- **`Product`** - product line (*Paseo, VTT, Amarilla, Montana, Carretera*),
- **`Discount Band`** - applied discount range (*None, Low, Medium, High*),
- **`Units Sold, Sales, COGGS, Profit`** - core financial indicators,
- **`Date, Month, Year`** - time attributes.
---

## 🎯 Goal

The purpose of this dashboard is to highlight the most important financial insights that enable stakeholders to make informed, data-driven decisions. By visualizing key metrics such as sales performance, discount impact, profitability and regional trends, the dashboard supports effective resource allocation, production planning and market strategy optimization.

Its goal is to transfrorm complex financial data into a clear overview, allowing decision-makers to quickly identify opportunities or spot inefficiencies and take appropiate actions.

---

## 🚀 Program Walk-through

### 1. First look into data
*`Financial data`* excel workbook

![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/53281d38ea59c872f158815d4c8b4a0fd547cf76/Screenshots/Financial%20data%20first%20look.png)

One of the first issues visible in the dataset is the presence of fractional values in the *`Units Sold`* column. Since units sold should always be whole numbers, these observations may introduce inconsistencies, affecting further results and interpretations.

---

### 2. Fixing *`Units Sold`*

To ensure data reliability, I created a new column *`[New] Units Sold`* - where all values are rounded down. Based on this corrected field, I recalculated several key indicators to ensure they now use corrected data

![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/7aa13a369c931439469b76a0d62e39fe1871aa23/Screenshots/Data%20with%20new%20columns.png)

- **`[New] Gross Sales`** - recalculated by multiplying **`Sale Price`** by the corrected *`[New] Units Sold`*. AS a result, **`[New] Gross Sales`** is slightly lower for records where fractional units previously existed.
- **`[New] Sales`** - represents net sales after discounts, recalculated using updated gross values.
- **`COGS Formula`** - before recalculating **`[New] COGS`**, I determined the original fodmula: **`Units Sold`** * **`Manufacturing Price`** * **`Coefficient`**
- **`Coefficient`** - identified by reversing the initial COGS calculation **`(COGS/Units Sold) / Manufacturing Price`**
- **`[New] COGS`** - recomputed using the corrected units: **`[New] Units Sold`** * **`Manufacturing Price`** * **`Coeficient`**
- **`[New] Profit`** - final corrected metric computed as: **`[New] Sales`** - **`[New] COGS`**
  
---

### 3. Changing the *`Date`* 

The dates in the report are all set to the first day of each month, which suggests that the values represent data for the previous month. To reflect this correctly, I adjusted each date to the last day of the preceding month.


![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/a5378ee35da3987ce870c8ec6e41534e1f9b4f3c/Screenshots/Date.png)

---

### 4. Uploading the data into Power BI and further data cleaning

After uploading the data into Power BI I corrected misspeled names in *`Segment`* column, changed type of data to fit the actual values and added *`[New] Discount Rate`* column for future visuals.

![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/1d3d64ba8dbd9ee00c06ec971ef78c77ea2ec790/Screenshots/Data%20sheet.png)

---

Next step was to create additional table for the Dates, called *`Calendar`* table, which consists of Date, Report Date and Year, Month, Month Name columns extracted from Report Date and assigning them to right Fiscal Year and Fiscal Quarter which may be usefull in later stages.

![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/35bc496d44fa03cc1093d8521a363ef4c5567408/Screenshots/Calendar.png)

---
```sql
DROP DATABASE IF EXISTS runs_data;
CREATE DATABASE IF NOT EXISTS runs_data
	CHARACTER SET 'utf8mb4' 
	COLLATE 'utf8mb4_unicode_ci';
SHOW DATABASES;
USE runs_data;
```
#### 2.1 Creating *`runs`* table
```sql
CREATE TABLE runs (
	run_id INT AUTO_INCREMENT PRIMARY KEY,
    run_date DATE NOT NULL,
    run_type ENUM('Sprint series', 'Easy run', 'Progressive run') NOT NULL,
    total_distance_km DECIMAL(5,2),
    total_time TIME
	);
```
Filling the data manually into the table.
```sql
INSERT INTO runs (run_date, run_type, total_distance_km, total_time)
VALUES 
	(STR_TO_DATE('26.06.2025', '%d.%m.%Y'), 'Sprint series', 5.400, '00:40:38'),
	(STR_TO_DATE('30.06.2025', '%d.%m.%Y'), 'Easy run', 6.370, '00:40:00'),
	...
	(STR_TO_DATE('12.10.2025', '%d.%m.%Y'), 'Easy run', 21.310, '01:57:09');
```
#### 2.2 Creating *`segments`* table
```sql
CREATE TABLE segments (
	segment_id INT AUTO_INCREMENT PRIMARY KEY,
    run_id INT,
    segment_number INT,
    segment_type ENUM('Cooldown', 'Fast run', 'Rest', 'Run', 'Slow run', 'Sprint', 'Warmup') NOT NULL,
    segment_distance DECIMAL(5,2),
    segment_time TIME,
    avg_pace TIME,
    avg_hr INT,
    max_he INT,
    FOREIGN KEY (run_id) REFERENCES runs(run_id)
	);
```
Filling the data into segments table using Table Data Import Wizard.

![image_alt](https://github.com/Piotr-Trybala/SQL_Runs_Data/blob/2aee9ed4ffcbaaeb73056ad286c9dae6ccee2ea4/Screenshots/Import%20wizard.png)

---

#### 2.3 Overview of created tables

*`runs`* table

![image_alt](https://github.com/Piotr-Trybala/SQL_Runs_Data/blob/63a8bd1e4d6bf7b994bb1208c366d147381a63eb/Screenshots/runs_table_overview.png)

*`segments`* table

![image_alt](https://github.com/Piotr-Trybala/SQL_Runs_Data/blob/63a8bd1e4d6bf7b994bb1208c366d147381a63eb/Screenshots/segments_table_overview.png)

Testing aggregate functions on time-based columns:

```sql
SELECT segment_type,AVG(avg_pace)																	
FROM segments
GROUP BY segment_type;
```
![image_alt](https://github.com/Piotr-Trybala/SQL_Runs_Data/blob/3d341610b5d2233c0b01f1059c0a37c5750556b9/Screenshots/type_time_avg.png)

There are few issues with data:
- **`total_time`** column - data type is *TIME* which can cause issues with aggregate functions (**`runs`** table)
- **`max_he`** column - typo in column name, max_he instead of max_hr (**`segments`** table)
- **`segment_time`** and **`avg_pace`** columns - wrong *TIME* format, seconds are taken as minutes and minutes as hours (**`segments`** table)
- **`segment_time`** and **`avg_pace`** columns - data type is *TIME* which can cause issues with aggregate functions (**`seconds`** table)

---

#### 2.4 Data cleaning 

##### Add column for total time in seconds (*`runs`* table) 

```sql
ALTER TABLE runs
	ADD COLUMN total_time_sec INT AFTER total_time;
UPDATE runs
SET 
	total_time_sec = TIME_TO_SEC(total_time);
```
---

##### Correct typo in column name (*`segments`* table)

```sql
ALTER TABLE segments 
	RENAME COLUMN max_he TO max_hr;
```

##### Correct wrong TIME format before converting into seconds (*`segments`* table)

The *`segment_time`* and *`avg_pace`* columns were imported in incorrect format - seconds were interpreted as minutes and minutes as hours. To fix this, we create new columns using the *`MAKETIME()`* function with *`HOUR`* and *`MINUTE`* as arguments. But first, we need to check for odd results in incorrectly imported data to make sure that functions are applicable.

```sql
SELECT MAX(segment_time), MAX(avg_pace)
FROM segments;
```
![image_alt](https://github.com/Piotr-Trybala/SQL_Runs_Data/blob/c4edf9b01cda3db862e7bf537f72ec7a2975267b/Screenshots/MAXs.png)

Since no value exceeds 60 hours, we can safely apply the transformation.

```sql
ALTER TABLE segments																				
	ADD COLUMN segment_time_fixed TIME AFTER segment_time,
    ADD COLUMN avg_pace_fixed TIME AFTER avg_pace;
```
Fill the new columns with corrected values using *`MAKETIME`* functions

```sql
UPDATE segments
SET
	segment_time_fixed = MAKETIME(0, HOUR(segment_time), MINUTE(segment_time)),
    avg_pace_fixed = MAKETIME(0, HOUR(avg_pace), MINUTE(avg_pace));
```

##### Convert TIME to seconds (INT)

```sql
ALTER TABLE segments																				
	ADD COLUMN segment_time_sec INT AFTER segment_time_fixed,
    ADD COLUMN avg_pace_sec INT AFTER avg_pace_fixed;
UPDATE segments
SET 
	segment_time_sec = TIME_TO_SEC(segment_time_fixed),
	avg_pace_sec = TIME_TO_SEC(avg_pace_fixed);
```
---
##### Overview of the tables after Data Cleaning

*`runs`* table

![image_alt](https://github.com/Piotr-Trybala/SQL_Runs_Data/blob/7768d21c0555f4f12f081aa0b75fe7c6de6f4693/Screenshots/runs_updated.png)

*`segments`* table

![image_alt](https://github.com/Piotr-Trybala/SQL_Runs_Data/blob/7768d21c0555f4f12f081aa0b75fe7c6de6f4693/Screenshots/segments_updated.png)

Testing aggregate functions on new columns:

```sql
SELECT 																								
	segment_type, 
    ROUND(AVG(avg_pace_sec),2) AS avg_pace_sec, 		
	SEC_TO_TIME(ROUND(AVG(avg_pace_sec))) AS avg_pace_formatted
FROM segments
GROUP BY segment_type;
```
![image_alt](https://github.com/Piotr-Trybala/SQL_Runs_Data/blob/478b5825cc5f526d496304bfd39e0a66802c03de/Screenshots/aggregate_functions_new.png)

---
### 3. SQL Exploratory Analysis

#### 3.1 How many runs of each type were completed during training period?

```sql
SELECT 
	run_type, 
    COUNT(*) AS number_of_runs																		
FROM runs
GROUP BY run_type
ORDER BY number_of_runs DESC;
```
![image_alt](https://github.com/Piotr-Trybala/SQL_Runs_Data/blob/2ea3d6cc3012220287c3ef2491871c8ef85ae553/Screenshots/How%20many%20runs%20of%20each%20type%20were%20completed%20during%20training%20period.png)


#### 3.2 What is the average distance and duration for each run type?

```sql
SELECT			
	run_type,
    ROUND(AVG(total_distance_km),2) AS average_distance,
    SEC_TO_TIME(ROUND(AVG(total_time_sec))) AS average_duration
FROM runs
GROUP BY run_type
ORDER BY average_distance DESC;
```
![image_alt](https://github.com/Piotr-Trybala/SQL_Runs_Data/blob/2ea3d6cc3012220287c3ef2491871c8ef85ae553/Screenshots/What%20is%20the%20average%20distance%20and%20duration%20for%20each%20run%20type.png)


#### 3.3 Which runs were the longest?

```sql
SELECT																								
	run_date,
	run_type,
    total_distance_km,
    total_time
FROM runs
ORDER BY total_distance_km DESC
LIMIT 5; 
```
![image_alt](https://github.com/Piotr-Trybala/SQL_Runs_Data/blob/2ea3d6cc3012220287c3ef2491871c8ef85ae553/Screenshots/Which%20runs%20were%20the%20longest.png)

#### 3.4 What is the average and maximum heart rate for each run type?

```sql
SELECT																								
	r.run_type,
    ROUND(AVG(s.avg_hr)) AS avg_hr,
    MAX(s.max_hr) AS max_hr
FROM runs AS r
JOIN segments AS s
USING (run_id)
GROUP BY run_type;
```
![image_alt](https://github.com/Piotr-Trybala/SQL_Runs_Data/blob/2ea3d6cc3012220287c3ef2491871c8ef85ae553/Screenshots/What%20is%20the%20average%20and%20maximum%20heart%20rate%20for%20each%20run%20type.png)

#### 3.5 Which weeks had the highest total training volume?

```sql
SELECT																								
	1+FLOOR(DATEDIFF(run_date, (SELECT MIN(run_date) FROM runs))/7) AS training_week,
    COUNT(*) AS total_runs
FROM runs
GROUP BY training_week;
```
![image_alt](https://github.com/Piotr-Trybala/SQL_Runs_Data/blob/2ea3d6cc3012220287c3ef2491871c8ef85ae553/Screenshots/Which%20weeks%20had%20the%20highest%20total%20training%20volume.png)

#### 3.6 What is the average pace per segment type and to which run_type it belongs?

```sql
SELECT																								
	r.run_type,
    s.segment_type,
    SEC_TO_TIME(ROUND(AVG(s.avg_pace_sec))) AS average_pace,
    ROUND(AVG(s.avg_hr)) AS average_heart_rate
FROM runs AS r
JOIN segments AS s
USING (run_id)
GROUP BY r.run_type, segment_type;
```
![image_alt](https://github.com/Piotr-Trybala/SQL_Runs_Data/blob/2ea3d6cc3012220287c3ef2491871c8ef85ae553/Screenshots/What%20is%20the%20average%20pace%20per%20segment%20type%20and%20to%20which%20run_type%20it%20belongs.png)

#### 3.7 How average pace of segment type 'Run' evolved during each of Easy runs?

```sql
SELECT 				
	run_id,
    s.segment_type,
    s.segment_distance,
    SEC_TO_TIME(s.avg_pace_sec) AS pace_per_segment,
    SEC_TO_TIME(ROUND(AVG(s.avg_pace_sec) OVER(
		PARTITION BY run_id))) AS avg_segment_pace
FROM runs AS r
JOIN segments AS s
USING (run_id)
WHERE s.segment_type = 'Run'
;
```
![image_alt](https://github.com/Piotr-Trybala/SQL_Runs_Data/blob/2ea3d6cc3012220287c3ef2491871c8ef85ae553/Screenshots/How%20average%20pace%20of%20segment%20type%20'Run'%20evolved%20during%20each%20of%20Easy%20runs.png)

#### 3.8 How many runs in Easy runs were 'Long runs' (over 15 km), 'Short runs' (7-15 km) and 'Recovery runs' (<7 km)?

```sql
SELECT	
	CASE WHEN total_distance_km < 7 THEN 'Recovery run'
		WHEN total_distance_km BETWEEN 7 AND 14 THEN 'Short run'
        ELSE 'Long run' 
	END AS run_lenth_category,
    COUNT(*)
FROM runs
WHERE run_type = 'Easy run'
GROUP BY run_lenth_category
;
```
![image_alt](https://github.com/Piotr-Trybala/SQL_Runs_Data/blob/2ea3d6cc3012220287c3ef2491871c8ef85ae553/Screenshots/How%20many%20runs%20Long%20runs%20Short%20runs%20and%20Recovery%20runs.png)

#### 3.9 Which runs were faster than the overall average?

```sql
SELECT	
	*,
    SEC_TO_TIME(ROUND(total_time_sec/total_distance_km)) AS avg_run_pace,
    SEC_TO_TIME(
		ROUND((SELECT SUM(total_time_sec)/SUM(total_distance_km) 
		FROM runs))) AS overall_average_pace
FROM runs 
WHERE (total_time_sec/total_distance_km) < (
	SELECT SUM(total_time_sec)/SUM(total_distance_km)
    FROM runs
    )
;
```
![image_alt](https://github.com/Piotr-Trybala/SQL_Runs_Data/blob/2ea3d6cc3012220287c3ef2491871c8ef85ae553/Screenshots/Which%20runs%20were%20faster%20than%20the%20overall%20average.png)

---

## Insights 👀

The analysis of the training plan revealed a generally consistent running routine with only one week without any recorded session and four weeks with just a single training unit. Overall, training sessions were performed regularly throughout the program. The highest training frequency was observed during weeks 5 and 6, indicating a mid-plan workload peak.

The most frequently performed session type was **`Easy run`**, which dominated the overall structure of the training plan. The longest sessions occurred in the later stages (weeks 20-32), which aligns with typical strategies, where longer runs are introduced progressively into the training plan.

However, a critical obserwvtion is that the current classification of **`Easy run`** session groups together very different types of workout, including short recovery jogs, regular aerobic runs and long endurance runs. This lack of differentiation makes it difficult to analyze performance trends accurately.

A more meaningful categorization would be to split **`Easy run`** into:

- **`Recovery run`** - short, easy sessions aimed to active recovery,
- **`Short run`** - standard aerobic runs,
- **`Long run`** - extended endurance-building efforts. 

Without this categorization, identifying a potential downward trend in pace (improvement) for **`Easy runs`** is challenging, as faster aerobic sessions are mixed with slower recovery runs.

The overall dataset structure is suitable for further analysis, such as calculating rolling averages of total distance, identifying performance trends and exploring correlations between training volume and pace improvements, but requires previously mentioned changes and longer observation period to produce more meaningful insights.

## 💡 Key Skills Demonstrated
- Relational database design – structured raw training data into a clean, well-normalized SQL schema with appropriate keys and constraints.
- Data cleaning and transformation – identified and corrected issues with time formatting, column naming, and data consistency; converted non-aggregable TIME data into numeric formats suitable for analysis.
- Exploratory data analysis using SQL – performed time-series analysis to uncover training patterns, workload peaks, and session distribution across different run types.
- Analytical thinking – recognized classification limitations within the dataset and proposed a more meaningful categorization strategy to enable deeper performance insights.
- Aggregate functions – SUM(), AVG(), MIN(), MAX(), COUNT().
- String & date/time manipulation – MAKETIME(), TIME_TO_SEC(), SEC_TO_TIME(), DATE_FORMAT(), used for converting and standardizing non-aggregable time data.
- Conditional logic – CASE WHEN expressions to classify runs and handle data irregularities.
- JOIN - to combine run-level and segment-level datasets for analysis.
- Subqueries – applied in both SELECT and FROM clauses to calculate intermediate metrics.
- Window functions - AVG() OVER(PARTITION BY ...) to derive rolling averages and segment rankings.
