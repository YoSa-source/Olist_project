# 📊 Olist Brazilian E-Commerce Analytics Dashboard

An end-to-end Business Intelligence project built in **Power BI** as the final graduation requirement for the **Data Analysis Diploma at Smart Investment Solution (SIS)**.

---

## 📌 Project Overview
This project provides a comprehensive analysis of Brazilian e-commerce operations using the Olist dataset (over 100,000 real orders between 2016 and 2018). The goal is to evaluate sales performance, monitor operational shipping efficiencies, and identify drivers affecting customer review scores.

---

## 🧱 Data Architecture & Modeling
The data was modeled into an optimized **Star Schema** to ensure fast query processing and clean filter propagation:

* **Fact Tables:**
  * `Fact_Orders`: High-level order details, timestamps, and order statuses.
  * `Fact_Order_Items`: Granular line-item data, product IDs, item prices, and freight values.
  * `Fact_Order_Payments`: Payment types, installments, and payment amounts.
  * `Fact_Order_Reviews`: Review scores (1 to 5) and feedback timestamps.
* **Dimension Tables:**
  * `Dim_Customers`: Customer locations (cities and states).
  * `Dim_Products`: English category names and product specifications.
  * `Dim_Sellers`: Seller locations and regions.
  * `Dim_Date`: Custom calendar generated via DAX for time intelligence.

---

## 🛠️ Data Cleaning Steps (Power Query)
* Standardized text formats (capitalized cities, uppercase states).
* Handled null values logically without altering business transaction integrity.
* Merged category translation mappings to display all product categories in English.
* Converted numerical and date columns to strictly typed formats (`Fixed Decimal Number`, `Date/Time`).
* Aggregated geolocation coordinates to prevent model bloat and improve rendering speed.

---

## 📐 Key DAX Measures
* **Total Revenue:**
  ```dax
  Total Revenue = SUM(Fact_Order_Items[price])
  Average Order Value = DIVIDE([Total Revenue], [Total Orders], 0)
  YoY Revenue Growth % = 
DIVIDE([Total Revenue] - [Revenue SPLY], [Revenue SPLY], 0)
Avg Delivery Days = 
AVERAGEX(
    FILTER(Fact_Orders, NOT(ISBLANK(Fact_Orders[order_delivered_customer_date]))),
    DATEDIFF(Fact_Orders[order_purchase_timestamp], Fact_Orders[order_delivered_customer_date], DAY)
)
