# Superstore Sales Analysis  

## Introduction  
Sales analysis helps businesses improve their sales process, achieve sales goals, support operational decision-making, and boost team performance. A sales analysis report includes key metrics such as total revenue, cost of goods sold (COGS), profit, and other performance indicators depending on the industry.  

For this project, I am analyzing the **Superstore Sales dataset** from **Kaggle**. I am using **Power Query and Power BI** to clean, transform, and analyze the data, ensuring my report remains **dynamic and insightful**.  

## Business Problem  
The key business questions I aim to answer in this analysis are:  
1. What is the total sales, quantity sold, and total profit? 
2. Which region is generating the highest sales?
3. In which category do we see the highest number of sales?  
4. Which ship mode is most frequently used by customers? 
5. Who are the top 10 customers based on sales?

---

## 📂 Data Overview  
My dataset consists of a **single CSV file** with **9,994 rows** and **21 columns**, including:  
- **Order Details** – Order ID, Order Date, Ship Date, Order Priority  
- **Customer Details** – Customer ID, Customer Name, Segment  
- **Location Details** – City, State, Country, Region  
- **Product Details** – Product ID, Category, Sub-Category, Product Name  
- **Sales Information** – Sales, Quantity, Discount, Profit  
- **Shipping Information** – Ship Mode  

---
## Analysis Overview

I started with importing the data into power query.

<img width="661" alt="importing_data" src="https://github.com/user-attachments/assets/1b8c6c38-849d-45ec-99d9-dd37e1757c8b" />

## 🧹 Data Cleaning Process  
Before performing the analysis, I cleaned the dataset in **Power Query** by:  
✅ Checking for **incorrect spellings**  
✅ Ensuring **each column has the correct data type**  
✅ Identifying and **removing duplicate records**  
✅ Handling **missing or blank values**  

To **profile the data**, I enabled **Column Profile and Column Quality** under the ‘View’ tab in Power Query. This allowed me to visualize the data quality, ensuring **100% validity, 0% errors, and 0% empty values** before proceeding.  

---

## 🏗 Data Modeling  
To optimize performance and ensure an efficient **data model**, I structured the dataset into **Dimension Tables** and a **Fact Table**, following a **Star Schema approach**.  

### 🔹 Dimension Tables Creation  
Since the dataset is large and contains repeated categorical values, I extracted key **Dimension Tables**:  

#### 1️⃣ **Region Dimension Table**  
- **Steps:**  
  1. Duplicated the **Superstore Table** and renamed it to **Region**.  
  2. Selected the **Region column** and removed all other columns.  
  3. Removed duplicates to get a unique list of regions.  
  4. Added an **Index Column** (starting from 1) and renamed it to **Region_ID**.  
  5. Merged this table with the **Superstore Fact Table**, replacing the Region column with **Region_ID** to reduce redundancy.  

#### 2️⃣ **Other Dimension Tables** (Created using similar steps)  
- **Customers Dimension Table**  
- **Category Dimension Table**  
- **Sub-Category Dimension Table**  
- **Ship Mode Dimension Table**  
- **State Dimension Table**  
- **Segment Dimension Table**  

### 🔹 Fact Table  
- The **Superstore Table** serves as the **Fact Table**, containing transactional data such as **Order ID, Sales, Profit, Quantity, Discount**, and foreign keys linking to the Dimension Tables.  

---

## 🔗 Star Schema Model  
Once all **Dimension Tables** were created, I established **one-to-many relationships** between them and the **Fact Table**, creating a **Star Schema Model** for efficient querying.  

### 📅 Date Table  
Since I need to use **Time Intelligence functions** in Power BI, I created a **Date Table**.  
- The **Order Date** is linked to the Fact Table through an **active relationship**.  
- The **Ship Date** has an **inactive relationship**, which I can activate using the **USERRELATIONSHIP function** in DAX when needed.  

---

## 📈 Insights and Reporting  
With a **cleaned and structured data model**, I am now able to:  
✅ Calculate **total sales, total quantity sold, and total profit**.  
✅ Identify **which region generates the highest sales**.  
✅ Determine **the category with the highest number of sales**.  
✅ Analyze **the most frequently used ship mode**.  
✅ Identify **the top 10 customers based on total sales**.  

I will build **interactive Power BI reports and dashboards** to visualize these insights and support business decision-making. 🚀  

---

## 🎯 Conclusion  
By following this **structured approach**, I ensure that the analysis is **accurate, efficient, and valuable** for deriving business insights. **This project enhances my data modeling, data transformation, and visualization skills using Power BI.**  

🔹 **Next Steps:**  
- Implement **DAX measures** for deeper analysis.  
- Create **dynamic dashboards** to showcase insights effectively.  
- Explore **predictive modeling** for future sales trends.  

🚀 **This project demonstrates my ability to transform raw data into meaningful business insights!**  
