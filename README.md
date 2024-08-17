# Pizza-Sales-Analysis
This project showcases a comprehensive analysis of a pizza sales dataset to uncover key business insights. The analysis was carried out using SQL for data extraction and manipulation, and Excel was utilized for data cleaning, processing and visualization using a dynamic dashboard.

## Business Requirements
### KPI Requirements
We need to analyse key indicators for our pizza sales data to gain insights into our business performance. Specifically, we want to calculate the following metrics:
1. **Total Revenue:** The sum of the total price of all pizza orders.
2. **Average Order Value:** The average amount spent per order, calculated by dividing the total revenue by the number of orders.
3. **Total Pizzas Sold:** The sum of the quantities of all pizzas sold.
4. **Total Orders:** The total number of orders placed.
5. **Average Pizzas Per Order:** The average number of pizzas sold per order, calculated by dividing the total number of pizzas sold by the total number of orders.

### Chart Requirements
We would like to visualize various aspects of our pizza sales data to gain insights and understand key trends. We have identified the following requirements for creating charts:
1. Daily Trend For Total Orders
2. Hourly Trend For Total Orders
3. Percentage Of Sales By Pizza Category
4. Percentage Of Sales By Pizza Size
5. Total Pizzas Sold By Pizza Category
6. Top 5 Best-Sellers By Total Pizzas Sold
7. Bottom 5 Worst-Sellers By Total Pizzas Sold

## Software Used
- MS SQL Server 2022
- SQL Server Management Studio 20.2: For data extraction and manipulation.
- MS Excel 2021: For data cleaning, processing and visualization.

## Data Source
_pizza_sales.csv_

## Methodology
After the requirements are gathered, the data is analysed using SQL and Excel.

### SQL
Import data in MS SQL Server DB, create DB and write queries to manipulate data in order to uncover actionable insights.
- #### KPI Requirements:
1. **Total Revenue:**
   ```
   SELECT SUM(total_price) AS Total_Revenue
   FROM pizza_sales;
   ```
   <img width="121" alt="image" src="https://github.com/user-attachments/assets/b7264d08-90c9-4743-9c25-cdafabc35520">

   
2. **Average Order Value:**
   ```
   SELECT SUM(total_price)/COUNT(DISTINCT order_id) AS Average_Order_Value
   FROM pizza_sales;
   ```
   <img width="126" alt="image" src="https://github.com/user-attachments/assets/28ca9475-d77d-41cd-927a-f5ab25832c53">


3. **Total Pizzas Sold:**
   ```
   SELECT SUM(quantity) AS Total_Pizza_Sold
   FROM pizza_sales;
   ```
   <img width="121" alt="image" src="https://github.com/user-attachments/assets/7ddb1ebe-9830-4468-8550-73ca93fd6c64">

4. **Total Orders:**
   ```
   SELECT COUNT(DISTINCT order_id) AS Total_Orders
   FROM pizza_sales;
   ```
   <img width="120" alt="image" src="https://github.com/user-attachments/assets/0d74f3d3-2b64-468a-8983-68c6bbc769f2">

5. **Average Pizzas Per Order:**
   ```
   SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2))/
   CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2)) AS Average_Pizzas_Per_Order
   FROM pizza_sales;
   ```
   <img width="142" alt="image" src="https://github.com/user-attachments/assets/cde35d5f-cc9a-472b-826e-8a034a031527">

- #### Chart Requirements:
1. **Daily Trend For Total Orders:**
   ```
   SELECT DATENAME(DW, order_date) AS Order_Day,
   COUNT(DISTINCT order_id) AS Total_Orders
   FROM pizza_sales
   GROUP BY DATENAME(DW, order_date);
   ```
   <img width="153" alt="image" src="https://github.com/user-attachments/assets/f1aa910a-34dc-42b6-8823-042430316686">

2. **Hourly Trend For Total Orders:**
   ```
   SELECT DATEPART(HOUR, order_time) AS Order_Hour,
   COUNT(DISTINCT order_id) AS Total_Orders
   FROM pizza_sales
   GROUP BY DATEPART(HOUR, order_time)
   ORDER BY DATEPART(HOUR, order_time);
   ```
   <img width="150" alt="image" src="https://github.com/user-attachments/assets/e59f50c8-3ad7-4c11-bdbf-611e4c0692a0">

3. **Percentage Of Sales By Pizza Category:**
   ```
   SELECT pizza_category, CAST(SUM(total_price) AS DECIMAL(10,2)) AS Total_Sales, CAST(SUM(total_price)*100/(SELECT 
   SUM(total_price) FROM pizza_sales) AS DECIMAL(10,2)) AS Total_Sales_Percentage
   FROM pizza_sales
   GROUP BY pizza_category;
   ```
   <img width="259" alt="image" src="https://github.com/user-attachments/assets/1f95cc08-396d-43b3-9df6-5024d48f4f71">

4. **Percentage Of Sales By Pizza Size:**
   ```
   SELECT pizza_size, CAST(SUM(total_price) AS DECIMAL(10,2)) AS Total_Sales, CAST(SUM(total_price)*100/(SELECT 
   SUM(total_price) FROM pizza_sales) AS DECIMAL(10,2)) AS Total_Sales_Percentage
   FROM pizza_sales
   GROUP BY pizza_size
   ORDER BY Total_Sales_Percentage DESC;
   ```
   <img width="237" alt="image" src="https://github.com/user-attachments/assets/55576cdf-e158-4442-867e-9216bc2302a1">

5. **Total Pizzas Sold By Pizza Category:**
   ```
   SELECT pizza_category, SUM(quantity) AS Total_Pizzas_Sold
   FROM pizza_sales
   GROUP BY pizza_category
   ORDER BY Total_Pizzas_Sold DESC;
   ```
   <img width="176" alt="image" src="https://github.com/user-attachments/assets/1c36ca4e-9277-4062-ab85-ead67cc2db13">

6. **Top 5 Best-Sellers By Total Pizzas Sold:**
   ```
   SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizzas_Sold
   FROM pizza_sales
   GROUP BY pizza_name
   ORDER BY Total_Pizzas_Sold DESC;
   ```
   <img width="228" alt="image" src="https://github.com/user-attachments/assets/46e89df1-8c4e-459a-8e7a-2c981185a352">

7. **Bottom 5 Worst-Sellers By Total Pizzas Sold:**
   ```
   SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizzas_Sold
   FROM pizza_sales
   GROUP BY pizza_name
   ORDER BY Total_Pizzas_Sold ASC;
   ```
   <img width="225" alt="image" src="https://github.com/user-attachments/assets/0bd749cf-d4d8-445b-91a8-f903f81e771d">


**_NOTE:_**
The Month, Quarter and Week filters can be applied to the above queries using the WHERE clause. For example:
```
SELECT DATENAME(DW, order_date) AS order_day, COUNT(DISTINCT order_id) AS total_orders 
FROM pizza_sales
WHERE MONTH(order_date) = 1
GROUP BY DATENAME(DW, order_date);
```
_Here MONTH(order_date) = 1 indicates that the output is for the month of January._


### MS Excel
Load the data from MS SQL Server database. It automatically gets loaded as a table.

- #### Data Cleaning:
  1. Pizza_size - For ease of understanding, we’ll transform the column using ‘Find and Replace’ as following:
     S-Regular
     M-Medium
     L-Large
     XL: X-Large
     XXL: XX-Large

- #### Data Processing:
  1. Create a new column ‘order_day’ for the day of the week by extracting it from the ‘order_date’ column using the 
     formula =TEXT([@[order_date]],"dddd").
  2. Create a new column ‘total_orders’ where each cell value corresponds to the ratio 1 to the number of times the 
     corrersponding ‘order_id’ value appears in the ‘order_id’ column. This is done so that when the sum of the column 
     ‘total_orders’ is taken, it returns the total number of distinct orders. Formula used: =1/ COUNTIF(B:B, 
     [@[order_id]]).
  3. Create column ‘order_hour’ that states the hour of the order. It is extracted from ‘order_time’ time using 
     =LEFT([@[order_time]],2) as the first 2 characters signify the hour.

- #### Data Analysis:
Pivot tables are used to analyse the data.

##### KPI Requirements:
1. Total Revenue = Sum of total_price
2. Average Order Value = Sum of total_price/Sum of total_orders
3. Total Pizzas Sold = Sum of quantity
4. Total Orders = Sum of total_orders
5. Average Pizzas Per Order = Sum of quantity/Sum of total_orders

<img width="468" alt="image" src="https://github.com/user-attachments/assets/9fa2072a-e2af-498b-96d9-c5808a0b6325">

##### Chart Requirements:
1. _Daily Trend For Total Orders:_ Create a column chart that displays the daily trend of total orders over a specific time period. This chart will help us identify any patterns or fluctuations in order volumes on a daily basis.
2. _Hourly Trend For Total Orders:_ Create a line chart that illustrates the hourly trend of total orders throughout the day. This chart will allow us to identify peak hours or periods of high order activity.
3. _Percentage Of Sales By Pizza Category:_ Create a doughnut chart that shows the distribution of sales across different pizza categories. This chart will provide insights into the popularity of various pizza categories andc their contribution to overall sales.
4. _Percentage Of Sales By Pizza Size:_ Generate a pie chart that represents the percentage of sales attributed to different pizza sizes. This chart will help us understand customer preferences for pizza sizes and their impact on sales.
5. _Total Pizzas Sold By Pizza Category:_ Create a funnel chart that presents the total number of pizzas sold for each pizza category. This chart will allow us to compare the sales performance of different pizza categories.
6. _Top 5 Best-Sellers By Total Pizzas Sold:_ Create a bar chart highlighting the top 5 best-selling pizzas based on the number of pizzas sold. This chart will help us identify the most popular pizza options.
7. _Bottom 5 Worst-Sellers By Total Pizzas Sold:_ Create a bar chart showcasing the bottom 5 worst-selling pizzas based on the number of pizzas sold. This chart will enable us to identify the least popular pizza options.

 - #### Data Visualisation and Dashboard:
All the charts, KPI requirements and the dynamic dashboard can be found in the following workbook:
_pizza_sales_analysis.xlsx_

## Insights
- Orders are highest on weekends, Friday/Saturday evenings.
- There are maximum orders from 12PM-1PM and 5PM-8PM, that is, lunch and dinner times.
- Classic category contributes to maximum sales and total orders.
- Large size pizzas contribute to maximum sales. X-Large and XX-Large size pizzas are the least sold sizes.
- Classic Deluxe and Barbecue Chicken pizzas are the best sellers and revenue generators whereas the Brie Carre is at the 
  bottom in both orders and revenue.






