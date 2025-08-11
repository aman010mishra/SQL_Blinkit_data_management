<h1 align="center">📊 Blinkit Grocery Data Analysis (SQL Project)</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Tool-SQL%20Server-blue" alt="SQL Server Badge">
  <img src="https://img.shields.io/badge/Database%20Size-Sample%20Dataset-green" alt="Dataset Badge">
  <img src="https://img.shields.io/badge/Focus-Data%20Cleaning%20%26%20KPI%20Analysis-orange" alt="Focus Badge">
</p>

<hr>

<h2>📌 Project Overview</h2>
<p>
This project analyzes Blinkit Grocery sales data using SQL. It involves:
</p>
<ul>
  <li>Data cleaning for consistency</li>
  <li>Key Performance Indicators (KPIs) calculations</li>
  <li>Detailed sales analysis by categories, outlet size, and location</li>
  <li>Pivot transformations for better reporting</li>
</ul>

---

<h2>📂 Dataset</h2>
<p>
The dataset (<code>blinkit_data</code>) contains information on:
</p>
<ul>
  <li>Item details (type, fat content, visibility, rating)</li>
  <li>Outlet details (type, location, establishment year, size)</li>
  <li>Sales figures</li>
</ul>

---

<h2>🧹 Data Cleaning</h2>
<pre>
UPDATE blinkit_data
SET Item_Fat_Content = 
    CASE 
        WHEN Item_Fat_Content IN ('LF', 'low fat') THEN 'Low Fat'
        WHEN Item_Fat_Content = 'reg' THEN 'Regular'
        ELSE Item_Fat_Content
    END;
</pre>
<p>
We standardized the <code>Item_Fat_Content</code> field to ensure accurate reporting and avoid mismatched categories.
</p>

---

<h2>📊 Key Metrics (KPIs)</h2>

<ul>
  <li><b>Total Sales:</b> <pre>SELECT CAST(SUM(Total_Sales) / 1000000.0 AS DECIMAL(10,2)) AS Total_Sales_Million FROM blinkit_data;</pre></li>
  <li><b>Average Sales:</b> <pre>SELECT CAST(AVG(Total_Sales) AS INT) AS Avg_Sales FROM blinkit_data;</pre></li>
  <li><b>Number of Items:</b> <pre>SELECT COUNT(*) AS No_of_Orders FROM blinkit_data;</pre></li>
  <li><b>Average Rating:</b> <pre>SELECT CAST(AVG(Rating) AS DECIMAL(10,1)) AS Avg_Rating FROM blinkit_data;</pre></li>
</ul>

---

<h2>📈 Analysis Queries</h2>

<h3>1️⃣ Total Sales by Fat Content</h3>
<pre>
SELECT Item_Fat_Content, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Item_Fat_Content;
</pre>

<h3>2️⃣ Total Sales by Item Type</h3>
<pre>
SELECT Item_Type, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Item_Type
ORDER BY Total_Sales DESC;
</pre>

<h3>3️⃣ Fat Content by Outlet (Pivot)</h3>
<pre>
SELECT Outlet_Location_Type, 
       ISNULL([Low Fat], 0) AS Low_Fat, 
       ISNULL([Regular], 0) AS Regular
FROM (
    SELECT Outlet_Location_Type, Item_Fat_Content, 
           CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
    FROM blinkit_data
    GROUP BY Outlet_Location_Type, Item_Fat_Content
) AS SourceTable
PIVOT (
    SUM(Total_Sales) 
    FOR Item_Fat_Content IN ([Low Fat], [Regular])
) AS PivotTable
ORDER BY Outlet_Location_Type;
</pre>

---

<h3>4️⃣ Total Sales by Outlet Establishment Year</h3>
<pre>
SELECT Outlet_Establishment_Year, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Outlet_Establishment_Year
ORDER BY Outlet_Establishment_Year;
</pre>

<h3>5️⃣ Percentage of Sales by Outlet Size</h3>
<pre>
SELECT Outlet_Size, 
       CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales,
       CAST((SUM(Total_Sales) * 100.0 / SUM(SUM(Total_Sales)) OVER()) AS DECIMAL(10,2)) AS Sales_Percentage
FROM blinkit_data
GROUP BY Outlet_Size
ORDER BY Total_Sales DESC;
</pre>

<h3>6️⃣ Sales by Outlet Location</h3>
<pre>
SELECT Outlet_Location_Type, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Outlet_Location_Type
ORDER BY Total_Sales DESC;
</pre>

<h3>7️⃣ All Metrics by Outlet Type</h3>
<pre>
SELECT Outlet_Type, 
       CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales,
       CAST(AVG(Total_Sales) AS DECIMAL(10,0)) AS Avg_Sales,
       COUNT(*) AS No_Of_Items,
       CAST(AVG(Rating) AS DECIMAL(10,2)) AS Avg_Rating,
       CAST(AVG(Item_Visibility) AS DECIMAL(10,2)) AS Item_Visibility
FROM blinkit_data
GROUP BY Outlet_Type
ORDER BY Total_Sales DESC;
</pre>

---

<h2>📎 How to Use</h2>
<ol>
  <li>Import the dataset (<code>BlinkIT Grocery Data.csv</code>) into SQL Server / MySQL.</li>
  <li>Run the data cleaning query.</li>
  <li>Execute KPI queries to get quick insights.</li>
  <li>Run analysis queries for detailed breakdowns.</li>
</ol>

---

<h2>📌 Conclusion</h2>
<p>
Through this project, we gained insights into Blinkit's sales patterns, product categories, and outlet performance. The combination of data cleaning, aggregation, and pivoting ensures high-quality and actionable business intelligence.
</p>

<hr>
<p align="center">🚀 Developed by <b>Aman Mishra</b> | 💡 sql server microsoft studio</p>
