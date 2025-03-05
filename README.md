# Superstore Sales Analysis  

## Introduction  
This project involves analyzing the Superstore sales dataset to gain insights into customer behavior, product performance, and overall sales trends. The data was stored and normalized using Microsoft SQL Server Management Studio (SSMS) before being analyzed in Power BI for visualization.

## Business Problem  
The key business questions I aim to answer in this analysis are:  
1. What is the total sales, quantity sold, and total profit? 
2. Which region is generating the highest sales?
3. In which category do we see the highest number of sales?  
4. Which ship mode is most frequently used by customers? 
5. Who are the top 10 customers based on sales?

---

## Data Overview  
The dataset was sourced from **Kaggle** and consists of  **9,994 rows** and **21 columns**
It includes sales transactions from **2014 to 2017**, covering the following:
- **Orders**: Order details including order date, customer ID, and region.
- **Order Details** – Order ID, Order Date, Ship Date, Order Priority  
- **Customer Details** – Customer ID, Customer Name, Segment  
- **Location Details** – City, State, Country, Region  
- **Product Details** – Product ID, Category, Sub-Category, Product Name  
- **Sales Information** – Sales, Quantity, Discount, Profit  
- **Shipping Information** – Ship Mode  

---

## Data Preparation & Normalization
The dataset originally contained **denormalized** sales records. To ensure **data integrity, minimize redundancy, and improve query efficiency**, I **normalized** the data using **Microsoft SQL Server Management Studio (SSMS)**. The process involved:

1. **Breaking down large tables** into smaller, related tables using **primary and foreign keys**.
2. **Removing redundant data** by organizing attributes into separate entities.
3. **Ensuring referential integrity** by defining appropriate relationships.

### Tables Created:
After normalization, the following tables were created:

- **Customers** (`Customer_ID`, `Customer_Name`, `Segment`, `Country`, `City`, `State`, `Postal_Code`)
- **Orders** (`Order_ID`, `Order_Date`, `Customer_ID`, `Region`)
- **Products** (`Product_ID`, `Product_Name`, `Category`, `SubCategory`)
- **Shipping** (`Ship_ID`, `Order_ID`, `Ship_Mode`, `Ship_Date`)
- **OrderDetails** (`OrderDetail_ID`, `Order_ID`, `Product_ID`, `Quantity`, `Sales`, `Discount`, `Profit`)

These tables were then **populated with data**, ensuring consistency in keys and relationships.

## Data Exploration in SQL
After normalizing and inserting the data, I conducted **exploratory data analysis (EDA) in SQL** using **SSMS**. Some of the key queries performed:

1. **Checking for duplicates:**
   ```sql
   SELECT Customer_ID, COUNT(*)  
   FROM Customers  
   GROUP BY Customer_ID  
   HAVING COUNT(*) > 1;
  ``
  ![image](https://github.com/user-attachments/assets/528a2340-0758-446e-b118-5e35384d5b98)

2. **Removing duplicate records using CTE**
   ```sql
   WITH CTE AS (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY Customer_ID ORDER BY Customer_Name) AS rn
    FROM SuperStore
    )
    DELETE FROM CTE WHERE rn > 1;
   ``
### Data Exploration

1. **Total Sales per Year**
   ```sql
   SELECT YEAR(Order_Date) AS Year, SUM(Sales) AS TotalSales
   FROM Orders o
   JOIN OrderDetails od ON o.Order_ID = od.Order_ID
   GROUP BY YEAR(Order_Date)
   ORDER BY Year;
   ``
![image](https://github.com/user-attachments/assets/6e150f94-0706-4046-81ad-6c588be12f89)

2. **Top 10 Customers by Total Sales**
   ```sql
   SELECT TOP 10 
    C.Customer_Name, 
    SUM(OD.Sales) AS Total_Sales
   FROM OrderDetails OD
   JOIN Orders O ON OD.Order_ID = O.Order_ID
   JOIN Customers C ON O.Customer_ID = C.Customer_ID
   GROUP BY C.Customer_Name
   ORDER BY Total_Sales DESC;
   ``
![image](https://github.com/user-attachments/assets/6c57ecf3-360b-492c-a703-0cbb67612d9a)

3. **Best-Selling Products**
   ```sql
   SELECT TOP 10 
    P.Product_Name, 
    SUM(OD.Quantity) AS Total_Quantity_Sold
   FROM OrderDetails OD
   JOIN Products P ON OD.Product_ID = P.Product_ID
   GROUP BY P.Product_Name
   ORDER BY Total_Quantity_Sold DESC;

![image](https://github.com/user-attachments/assets/e1b865f8-1e7f-4466-943a-cceb717a8f2c)


## Analysis Overview

I started with importing the data into power query.

<img width="661" alt="importing_data" src="https://github.com/user-attachments/assets/1b8c6c38-849d-45ec-99d9-dd37e1757c8b" />

### Data Cleaning Steps in Power Query

#### 1. Removed Unnecessary Columns  
Some columns were removed to keep only relevant data for analysis:  
- **Row ID** – Sequential numbering not needed for analysis.  
- **Postal Code** – Removed if regional analysis is not required.  
- **Product Name** – Dropped if not necessary for aggregated sales analysis.  

#### 2. Cleaned Order Date & Ship Date Columns  
##### Splitting and Reformatting Dates  
- **Split the Date Columns**:  
  - Separated *Order Date* and *Ship Date* into three columns: Year, Month, and Day using "/" as the delimiter.  

- **Converted to Date Format**:  
  - Recombined the split columns using `#date()` function:  

    ```powerquery
    #date(
      Number.FromText([Order Date - Copy.3]), 
      Number.FromText([Order Date - Copy.1]), 
      Number.FromText([Order Date - Copy.2])
    )
    ```

  - Applied the same transformation to *Ship Date*.

These steps ensured that the date columns were correctly formatted for further analysis.

---

## Exploratory Data Analysis (EDA)

## Data Modeling  
To optimize performance and ensure an efficient **data model**, I structured the dataset into **Dimension Tables** and a **Fact Table**, following a **Star Schema approach**.  

### Dimension Tables Creation  
Since the dataset is large and contains repeated categorical values, I extracted key **Dimension Tables**:  

#### 1️⃣ **Region Dimension Table**

- **Steps:**  
  1. Duplicated the **Superstore Table** and renamed it to **Region**.  
  2. Selected the **Region column** and removed all other columns.  
  3. Removed duplicates to get a unique list of regions.  
  4. Added an **Index Column** (starting from 1) and renamed it to **Region_ID**.  
  5. Merged this table with the **Superstore Fact Table**, replacing the Region column with **Region_ID** to reduce redundancy.
 
  <img width="526" alt="merging_data" src="https://github.com/user-attachments/assets/a9419331-3875-465b-a7f4-4be870d81126" />


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

![image](https://github.com/user-attachments/assets/4cfa58f4-ba85-4055-a9de-90d1d4a71261)


###  Date Table  
Since I need to use **Time Intelligence functions** in Power BI, I created a **Date Table**.  
- The **Order Date** is linked to the Fact Table through an **active relationship**.  
- The **Ship Date** has an **inactive relationship**, which I can activate using the **USERRELATIONSHIP function** in DAX when needed.  

<img width="497" alt="Time intelligence" src="https://github.com/user-attachments/assets/9b516f54-7cdc-4742-88a9-b51506e90699" />

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
