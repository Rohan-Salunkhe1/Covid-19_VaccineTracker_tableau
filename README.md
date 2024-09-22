# Rohan Forklift Company: Data Analytics Project Report

## Project Title:
**Optimizing Sales, Production, and Maintenance Operations Using Data Analytics**

---

## 1. Introduction

### 1.1. Background
Forklift manufacturing companies face challenges in optimizing operations across sales, production, and maintenance. Data analytics can provide actionable insights to improve production quality, reduce downtime, and enhance customer satisfaction by focusing on trends, patterns, and anomalies within business operations.

### 1.2. Objective
The objective of this project is to demonstrate data analytics skills by analyzing sales, production, and maintenance data for a hypothetical forklift manufacturing company using SQL and Google Sheets. The analysis aims to uncover trends and anomalies and provide actionable recommendations to improve business outcomes.

---

## 2. Tools and Technologies Used

### 2.1 SQL
SQL was utilized to manage and manipulate the data stored in the database. SQL queries were used for data cleaning, transformation, and analysis.

### 2.2 Google Sheets
Google Sheets was used for data visualization to display key trends and insights using charts, graphs, and dashboards.

---

## 3. Database Setup and Data Collection

### 3.1 Database Overview
The project data revolves around the following key operational areas: Sales, Production, and Maintenance. Each area has its own table within the database:

- **Sales Table**: Records details about forklift sales transactions.
- **Production Table**: Contains production-related data including units produced, defects, and defect types.
- **Maintenance Table**: Tracks maintenance issues, their resolution times, and costs.

### 3.2 SQL Database Creation
Step 1: A database named `Forklift_Manufacturing` was created to store the tables and manage the data for analysis.

```
CREATE DATABASE Forklift_Manufacturing;
USE Forklift_Manufacturing;
```

### 3.3 SQL Tables Creation
Three tables were created using SQL to hold sales, production, and maintenance data:



- **Sales Table**: Contains sales data with details on sales date, sales amount, region, and customer type.

```
CREATE TABLE Sales (
    Forklift_ID INT,
    Sales_Date DATE,
    Sales_Amount DECIMAL(10, 2),
    Region VARCHAR(50),
    Customer_Type VARCHAR(50)
);

```
- **Production Table**: Captures production details, including units produced, defects, and defect types.
```
CREATE TABLE Production (
    Forklift_ID INT,
    Production_Date DATE,
    Units_Produced INT,
    Defects INT,
    Defect_Type VARCHAR(50)
);
```

- **Maintenance Table**: Records maintenance activities, issue types, resolution times, and costs.
```
CREATE TABLE Maintenance (
    Forklift_ID INT,
    Maintenance_Date DATE,
    Issue_Found VARCHAR(255),
    Resolution_Time DECIMAL(5, 2),
    Maintenance_Cost DECIMAL(10, 2)
);
```

---

## 4. Data Cleaning and Transformation (SQL)

### 4.1 Overview of Data Quality Issues
Raw data is often inconsistent or contains errors, such as missing values, duplicate entries, or outliers that can affect analysis. Data cleaning and transformation steps were taken to ensure data quality before proceeding with analysis.

### 4.2 Data Cleaning Steps

#### Step 1: Identify and Remove Duplicates
Duplicate rows in the Sales, Production, and Maintenance tables were identified and removed to prevent skewed analysis.
```
SELECT Forklift_ID, Sales_Date, COUNT(*)
FROM Sales
GROUP BY Forklift_ID, Sales_Date
HAVING COUNT(*) > 1;
```
To remove duplicates:
```
DELETE FROM Sales
WHERE Forklift_ID IN (
    SELECT Forklift_ID
    FROM Sales
    GROUP BY Forklift_ID, Sales_Date
    HAVING COUNT(*) > 1
);
```

#### Step 2: Handle Missing Values
Missing values in columns, such as defects in the Production table, were handled. For instance, missing defect data was filled with 0 when no defects were reported.
```
UPDATE Production
SET Defects = 0
WHERE Defects IS NULL;
```

#### Step 3: Data Transformation
Standardization of regions, defect types, and date formats was performed to ensure data uniformity. Region names were corrected for consistency, and dates were verified for accuracy.
```
UPDATE Sales
SET Region = 'North'
WHERE Region = 'north';
```
#### Step 4: Ensure Date Consistency
All date values were checked to ensure they follow a standard format and fall within reasonable bounds for the data.
```
SELECT * FROM Sales
WHERE Sales_Date < '2000-01-01' OR Sales_Date > CURRENT_DATE;
```

## 5. Data Analysis and Insights

After cleaning and transforming the data, meaningful insights were generated from the data stored in the Sales, Production, and Maintenance tables. The analysis focused on sales trends, production efficiency, and maintenance cost analysis.

### 5.1 Sales Analysis

#### Step 1: Total Sales by Region
This analysis helps identify which geographical regions contribute the most to the company's sales.
```
SELECT Region, SUM(Sales_Amount) AS Total_Sales
FROM Sales
GROUP BY Region
ORDER BY Total_Sales DESC;
```

**Insight**: The "North" region contributed the highest total sales, followed by "South." This indicates that marketing and sales efforts should focus more on these regions to maximize revenue. Additionally, the "East" region showed relatively lower sales, suggesting a potential opportunity for expansion or further investigation into barriers in that area.

#### Step 2: Customer Type Analysis
Analyzing sales by customer type provides insight into the company's primary customer base.
```
SELECT Customer_Type, SUM(Sales_Amount) AS Total_Sales
FROM Sales
GROUP BY Customer_Type;
```

**Insight**: Industrial customers account for 65% of total sales, making them the company's primary customer type. However, there is a growing segment of retail customers, indicating potential for diversification in customer segments.

### 5.2 Production Analysis

#### Step 1: Production Efficiency
Analyzing the number of units produced vs. defects helps assess the company’s production efficiency.
```
SELECT Forklift_ID, SUM(Units_Produced) AS Total_Units, SUM(Defects) AS Total_Defects
FROM Production
GROUP BY Forklift_ID;
```

**Insight**: Forklift models 101 and 105 showed higher defect rates (4% and 5% respectively) compared to other models, indicating that these models may need design improvements or better quality control in production.

#### Step 2: Defect Type Distribution
Understanding which types of defects are most common helps focus efforts on improving production processes.
```
SELECT Defect_Type, COUNT(*) AS Defect_Count
FROM Production
GROUP BY Defect_Type;
```

**Insight**: "Battery" defects accounted for 40% of all defects, while "Motor" issues made up 35%. Focusing on improving battery design and testing could lead to significant reductions in defect rates and production costs.

### 5.3 Maintenance Analysis

#### Step 1: Maintenance Cost per Issue
This analysis highlights the most expensive maintenance issues and the forklifts that incur the highest costs.
```
SELECT Issue_Found, AVG(Maintenance_Cost) AS Avg_Cost
FROM Maintenance
GROUP BY Issue_Found;
```

**Insight**: Motor issues were the most costly to fix, with an average maintenance cost of $700 per instance, compared to $500 for battery issues. This suggests that further improvements in motor quality could lead to significant cost savings.

#### Step 2: Forklifts with Highest Maintenance Costs
Identifying which forklifts incur the highest maintenance costs can help prioritize improvements.
```
SELECT Forklift_ID, SUM(Maintenance_Cost) AS Total_Cost
FROM Maintenance
GROUP BY Forklift_ID
ORDER BY Total_Cost DESC;
```

**Insight**: Forklift 102 had the highest total maintenance cost, primarily due to recurring motor issues. This model should be prioritized for redesign or enhanced maintenance protocols.

## 6. Visualization in Google Sheets
To present the insights in a more accessible way, the cleaned data was exported to Google Sheets for visualization.

- **Sales by Region**: A bar chart displayed total sales for each region, highlighting the dominance of the "North" region.

  ![Total Sales by Region](https://github.com/user-attachments/assets/447d7667-f550-4205-937b-b636026c98d5)

  
- **Defect Type Distribution**: A pie chart showed the distribution of defect types, with battery issues representing the largest proportion.

  ![Defect Type Distribution](https://github.com/user-attachments/assets/c163b7ec-dddf-4a75-8141-25b57c6bfe39)

- **Maintenance Cost Trends**: A line chart illustrated the rising costs associated with motor-related maintenance issues.

  ![Maintenance Cost Trends](https://github.com/user-attachments/assets/da22d6b8-77da-4340-8cf3-e938e1a9aa56)

- **Defect Count Trends**: This scatter plot help us understand whether there is any relationship between the number of units produced and the number of defects recorded.
  
  ![Defect Count](https://github.com/user-attachments/assets/008e8d17-d557-476c-9c41-c0b99602ea50)


## 7. Conclusion
This project highlights how data analytics is used in optimizing forklift manufacturing operations. By using data cleaning, transformation, and analysis, key operational insights were drawn. Sales performance varied significantly by region, while production defects were concentrated in specific areas (e.g., battery and motor defects). Maintenance costs were higher for motor-related issues, providing clear areas for potential cost savings through product improvements.

### Key Recommendations:
- **Sales Focus**: Increase marketing efforts in high-performing regions (e.g., "North") and address underperforming regions (e.g., "East").
- **Production Improvements**: Prioritize quality control and design enhancements for battery and motor components.
- **Maintenance Efficiency**: Implement preventative measures for motor issues to reduce maintenance costs.
