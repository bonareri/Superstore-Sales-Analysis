# Superstore Sales Analysis  

## Project Overview  
This project focuses on analyzing sales data from a fictional Superstore using **Microsoft SQL Server Management Studio (SSMS)** for data management and **Power BI** for visualization. The goal is to extract insights into sales trends, customer behavior, and product performance.  

## Technologies Used  
- **Microsoft SQL Server Management Studio (SSMS)** – For database management and SQL queries  
- **Power BI** – For creating interactive visualizations and reports  
- **SQL** – For data transformation and analysis  

## Data Processing Steps  

1. **Database Setup**  
   - Created the `SuperstoreDB` database in SQL Server  
   - Imported the raw Superstore dataset as a table  
   - Created normalized tables: `Customers`, `Orders`, `Shipping`, `Products`, and `OrderDetails`  

2. **Data Transformation & Cleaning**  
   - Renamed columns for consistency  
   - Removed duplicates and NULL values  
   - Ensured referential integrity using foreign keys  

3. **Data Analysis Using SQL**  
   - Identified top-performing products and categories  
   - Analyzed regional sales trends  
   - Evaluated customer segments and purchase patterns  
   - Measured shipping performance  

4. **Visualization with Power BI**  
   - Connected Power BI to SQL Server using **Import Mode**  
   - Created dashboards for:  
     - **Sales Performance**: Total sales, top-selling products, and revenue trends  
     - **Customer Insights**: Customer segmentation and purchasing behavior  
     - **Regional Sales**: Geographic distribution of sales  
     - **Order & Shipping Analysis**: Order trends, shipping efficiency, and delivery times  

## Key Insights  
- **Top-selling categories** and products contribute significantly to revenue.  
- **Customer segmentation** helps in understanding high-value customers.  
- **Regional trends** reveal sales concentration in specific locations.  
- **Shipping delays** impact customer satisfaction.  

## Future Improvements  
- Automate data updates with scheduled SQL scripts  
- Enhance Power BI reports with advanced DAX calculations  
- Implement machine learning for sales forecasting  

## How to Use  
1. Clone the repository (if applicable).  
2. Restore the `SuperstoreDB` in SQL Server.  
3. Open Power BI and connect to the database.  
4. Explore the dashboards for insights.  

---

### Author: Melody Bonareri  
