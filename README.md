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
   
  ![image](https://github.com/user-attachments/assets/528a2340-0758-446e-b118-5e35384d5b98)

2. **Removing duplicate records using CTE**
   ```sql
   WITH CTE AS (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY Customer_ID ORDER BY Customer_Name) AS rn
    FROM SuperStore
    )
    DELETE FROM CTE WHERE rn > 1;
   
### Data Exploration

1. **Total Sales per Year**
   ```sql
   SELECT YEAR(Order_Date) AS Year, SUM(Sales) AS TotalSales
   FROM Orders o
   JOIN OrderDetails od ON o.Order_ID = od.Order_ID
   GROUP BY YEAR(Order_Date)
   ORDER BY Year;
   
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

4. **Most Profitable Product Categories**
   ```sql
   SELECT 
    P.Category, 
    SUM(OD.Profit) AS Total_Profit
   FROM OrderDetails OD
   JOIN Products P ON OD.Product_ID = P.Product_ID
   GROUP BY P.Category
   ORDER BY Total_Profit DESC;

![image](https://github.com/user-attachments/assets/b67fd5da-4671-4602-9945-e18824783425)

5. **Monthly Sales Trend**
   ```sql
   SELECT 
    YEAR(O.Order_Date) AS Year, 
    MONTH(O.Order_Date) AS Month, 
    SUM(OD.Sales) AS Total_Sales
   FROM OrderDetails OD
   JOIN Orders O ON OD.Order_ID = O.Order_ID
   GROUP BY YEAR(O.Order_Date), MONTH(O.Order_Date)
   ORDER BY Year, Month;

![image](https://github.com/user-attachments/assets/76e65744-1aa1-46ad-be06-9f98debaa3bb)

6. **Region-Wise Sales Performance**
   ```sql
   SELECT 
    O.Region, 
    SUM(OD.Sales) AS Total_Sales
   FROM Orders O
   JOIN OrderDetails OD ON O.Order_ID = OD.Order_ID
   GROUP BY O.Region
   ORDER BY Total_Sales DESC;

![image](https://github.com/user-attachments/assets/5f330117-ff81-4634-a2aa-2606e376dfef)

## Importing Data into Power BI  

After cleaning and normalizing the data in **SQL Server Management Studio (SSMS)**, I imported it into **Power BI** for visualization using **SQL Server as the data source**. 

![image](https://github.com/user-attachments/assets/8b83d7c0-588b-403c-8bbd-52a0dd9ce7d2)

## Data Cleaning and Transformation Using Power Query  

Once the data was imported, I utilized **Power Query** to:  

- Remove unnecessary columns  
- Change data types  
- Rename columns for better readability  
- Filter out null values  
- Create calculated columns  
- Merge tables where necessary
    
---

## Data Modeling  
 
To ensure efficient querying and visualization, I structured the **Power BI data model** using a **Star Schema** approach.  

### Star Schema Design  

The model consists of:  

- **Fact Table**: `OrderDetails` – Contains sales transactions, including `Order_ID`, `Product_ID`, `Customer_ID`, `Sales`, `Quantity`, `Discount`, and `Profit`.  
- **Dimension Tables**:  
  - `Customers` – Stores customer details (`Customer_ID`, `Customer_Name`, `Segment`, `Region`).  
  - `Products` – Stores product details (`Product_ID`, `Product_Name`, `Sub-Category`, `Category`).  
  - `Orders` – Stores order details (`Order_ID`, `Order_Date`, `Ship_Date`, `Ship_Mode`).  
  - `Shipping` – Shipping details: (`Ship_ID`, `Order_ID`, `Ship_Mode`, `Ship_Date`).  

### Relationships in Power BI  

- `OrderDetails` is links to:  
  - `Customers` via `Customer_ID`  
  - `Products` via `Product_ID`  
  - `Orders` via `Order_ID`  
  - `Shipping` via `Order_ID`  

### Data Model Diagram  

![image](https://github.com/user-attachments/assets/c4855664-bccd-4583-9682-655eb150c39a)
  
---

With this model, I was able to create **efficient DAX calculations and visualizations**, ensuring fast performance in Power BI.  

## visualization and dashboard design.

![image](https://github.com/user-attachments/assets/44206177-2825-4041-bec1-e6dcea5b867c)

### Superstore Sales Report  

The report provides a detailed analysis of sales, profit, quantity sold, and customer behavior. 

**Key insights include:**

- Total sales amount to **$2M**, with a profit of **$287K** and **38K** units sold.
- Sales and profit show seasonal trends, peaking in November and December.
- Technology leads in sales, followed by Furniture and Office Supplies.   
- Top Performing Sub-Categories are Phones & Chairs
- Standard Class dominates, handling $1.36M in sales.
- Same Day shipping generates the least sales.
- Top 10 customers contribute significantly to total revenue, with **Todd Sumrall leading**.
- The Consumer Segment emerges as Superstore’s largest, while Home Office represents the smallest segment. 
 
![image](https://github.com/user-attachments/assets/bdde2cc5-a7a2-4f10-8b2e-46a384d295ce)

- Utilizing the Region Slicer, it is evident that the West region contributes the highest sales, amounting to $725.46. Further exploration through the Monthly Sales Table allows us to analyze yearly, quarterly, and weekly sales. 
---

## 🎯 Conclusion  
By following this **structured approach**, I ensure that the analysis is **accurate, efficient, and valuable** for deriving business insights. **This project enhances my data modeling, data transformation, and visualization skills using Power BI.**  

🔹 **Next Steps:**  
 - Explore **predictive modeling** for future sales trends.  

🚀 **This project demonstrates my ability to transform raw data into meaningful business insights!**  
