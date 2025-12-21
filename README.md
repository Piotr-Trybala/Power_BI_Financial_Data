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

### 1. Initial data inspection
*`Financial data`* excel workbook

![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/53281d38ea59c872f158815d4c8b4a0fd547cf76/Screenshots/Financial%20data%20first%20look.png)

During the initial review of the dataset, one major issue becomes apparent - presence of fractional values in the *`Units Sold`* column. Since units sold should always be represented as whole numbers, these values may introduce inconsistencies and negatively impact subsequent calculations and interpretations.

---

### 2. Correcting *`Units Sold`*

To improve data reliability, I created a new column *`[New] Units Sold`* - where all values are rounded down to the nearest whole number. Based on this corrected field, several key financial indicators were recalculated to ensure consistency across the dataset. 

![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/7aa13a369c931439469b76a0d62e39fe1871aa23/Screenshots/Data%20with%20new%20columns.png)

- **`[New] Gross Sales`** - recalculated by multiplying **`Sale Price`** by the corrected *`[New] Units Sold`*. AS a result, **`[New] Gross Sales`** is slightly lower for records where fractional units previously existed.
- **`[New] Sales`** - represents net sales after discounts, recalculated using updated gross values.
- **`COGS Formula`** - before recalculating **`[New] COGS`**, I determined the original fodmula: **`Units Sold`** * **`Manufacturing Price`** * **`Coefficient`**
- **`Coefficient`** - identified by reversing the initial COGS calculation **`(COGS/Units Sold) / Manufacturing Price`**
- **`[New] COGS`** - recomputed using the corrected units: **`[New] Units Sold`** * **`Manufacturing Price`** * **`Coeficient`**
- **`[New] Profit`** - final corrected metric computed as: **`[New] Sales`** - **`[New] COGS`**
  
---

### 3. Adjusting the *`Date`* column 

All dates in the dataset are set to the first day of each month, suggesting that the reported values correspond to the previous reporting period. To reflect this more accurately, each date was adjusted to represent the last day of the preceding month.


![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/a5378ee35da3987ce870c8ec6e41534e1f9b4f3c/Screenshots/Date.png)

---

### 4. Uploading the data into Power BI and further data cleaning

After importing the data into Power BI, further cleaning steps were performed. These included correcting misspelled values in the *`Segment`* column, adjusting data types to match actual values, and creating a new column *`[New] Discount Rate`* to support future visualizations.

![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/1d3d64ba8dbd9ee00c06ec971ef78c77ea2ec790/Screenshots/Data%20sheet.png)

---

As the next step, a dedicated date dimension table —*`Calendar`*— was created. This table includes Date, Report Date, and derived attributes such as Year, Month, and Month Name. Additionally, each record was assigned to the appropriate Fiscal Year and Fiscal Quarter, enabling more advanced time-based analysis in later stages.


![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/35bc496d44fa03cc1093d8521a363ef4c5567408/Screenshots/Calendar.png)

---
### 5. Creating analytical measures

To enhance interpretability and support meaningful insights, a set of DAX measures was created for use in the final dashboard visualizations.

![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/b7f5423f54fa67e5f0b127e560818abad371374c/Screenshots/Measures.png)

---

Basic aggregate measures used as building blocks for more advanced calculations:

![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/973bc693ecd08b76e9b836611df049469067c678/Screenshots/Sum.png)

![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/973bc693ecd08b76e9b836611df049469067c678/Screenshots/Divide.png)

---

More advanced and analytically meaningful measures:

![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/973bc693ecd08b76e9b836611df049469067c678/Screenshots/Calculate.png)

![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/973bc693ecd08b76e9b836611df049469067c678/Screenshots/Summarize.png)

---

Additional measures designed to clearly communicate trends and analytical outcomes:

![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/973bc693ecd08b76e9b836611df049469067c678/Screenshots/Trend.png)

---

### 6. Creating visualisations 

#### 6.1 *`General Overview`* dashboard
This dashboard is designed to provide a clear, high-level overview of the company's financial performence while allowing stakeholders to quickly drill down into the most important dimensions of the data. Each visual was selected to support decision-making related to profitability, resource allocation and market performance.

![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/ad510214ac022f15800c98cdcd3c6455df76f925/Screenshots/General%20overview.png)

At the top of the dashboard, an *`advanced Card visual`* is divided into four sections presenting key financial measures for both the entire selected period and the previous fiscal year. This allows for an immediate comparison of current performance against historical benchmarks.

To complement absolute values, the dashboard includes calculated measures such as:
- Year-over-Year Profit Change,
- Average Monthly Profit Change.

To provide an immediate visual cue, the dashboard includes a *`trend indicator`* represented by an arrow that signals whether performance increased or decreased compared to the previous month.
The arrow is driven by a dedicated Trend measure, allowing users to quickly identify positive or negative month-over-month changes without the need for deeper numerical analysis.

These metrics help stakeholders assess growth dynamics rather than focusing solely on static totals.

A *`bar chart`* visualizes Total Profit by Segment, enabling quick comparison across customer groups.
*`Conditional formatting`* is applied to highlight segments generating losses in red, making underperforming areas immediately visible and actionable.

A map visual displays the Sum of Profit by Country, providing geographic context to financial performance. This helps identify the most profitable markets and supports decisions related to regional expansion, optimization, or cost control.

To enable a more in-depth analysis of specific periods, the dashboard includes a date slicer that allows stakeholders to dynamically filter all visuals by time.
This functionality makes it possible to focus on selected months or years, compare performance across different time ranges, and analyze trends during periods of particular interest, such as peak sales months or underperforming intervals.

---
#### 6.2 *`Time analysis`* dashboard

The *`Time analysis`* dashboard focuses on understanding how profitability evolves over time, combining relative performance metrics with absolute financial results. Its primary goal is to help identify trends, seasonality patterns, and periods of increased or decreased efficiency.

![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/ad510214ac022f15800c98cdcd3c6455df76f925/Screenshots/Time%20analysis.png)

The core visual of the dashboard is *`dual-axis line chart`* that presents:

- Profit margin (%) over time,
- Total Profit (absolute value) over the same reporting periods.

By combining these two measures in a single visual, the dashboard allows users to analyze not only whether profit is increasing or decreasing, but also why. For example, it becomes possible to identify situations where Total Profit grows while Profit Margin declines, indicating rising costs or pricing pressure.

Below the main chart, two supporting visuals provide additional breakdowns: 

- *`Total Profit by Product`*
- *`Total Profit by Segment`*

alowing user to easily filter for each the elements.

#### 6.2 *`Products and Countries`* dashboard

Next dashboard shows only two visuals, one allowing to filter by Country and second using hierarchical distribution 

![image_alt](https://github.com/Piotr-Trybala/Power_BI_Financial_Data/blob/ad510214ac022f15800c98cdcd3c6455df76f925/Screenshots/Products%20and%20Countries%20hierarchy.png)


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
