#  ShopSwift Data Modeling Project

## Project Overview

This project presents a scalable and analytics-ready data model for **ShopSwift**, an online retail company selling electronics, fashion, and home appliances. The goal is to design a data foundation that supports business intelligence (BI) use cases including:

- Daily, Weekly, Monthly Sales Reports  
- Top-Selling Products  
- Inventory Tracking  
- Customer Purchase History  

The data model is implemented using a **star schema** to optimize performance for reporting and OLAP queries.



##  Entity-Relationship Diagram (ERD)

![ERD](https://github.com/Lenzy-nana/shopSwift-data-model/blob/main/ERD%20Diagram.jpg?raw=true)



##  Assumptions

- Each **order** can include multiple products, and each **product** can appear in multiple orders (many-to-many, resolved via `FactSales`).
- Each **product** belongs to one **category**.
- **Inventory** is tracked by product (not by warehouse or location).
- A **customer** is uniquely identified by `CustomerID`.
- No returns, cancellations, or refunds are handled in this version.
- Inventory represents current available stock, not historical inventory levels.
- Orders are placed on a specific date (`OrderDate`).



##  Design Choices

We chose a **star schema** for its simplicity, speed, and suitability for OLAP and BI workloads.

### Why Star Schema?

- **Performance:** Optimized for large-scale analytical queries.
- **Ease of Use:** Flat structure is easy to query for non-technical analysts.
- **Compatibility:** Seamless integration with BI tools like Tableau, Power BI, and Looker.
- **Maintainability:** Clear separation between dimensions (descriptive) and facts (measurable events).

### Fact Table

- `FactSales` contains transactional data:
  - Keys to `Order`, `Product`, `Customer`, `Date`
  - Measures such as `Quantity` and `TotalAmount`

### Dimension Tables

- `DimCustomer`: Name, contact, email, and address details  
- `DimProduct`: Product name, price, and category  
- `DimCategory`: Category name  
- `DimOrder`: Order Date
- `DimDate`: Full date breakdown (day, week, month, year)  
- `Inventory`: Stock quantity per product   



##  Scaling Challenges & Considerations

1. **High Volume of Transactions**
   - *Challenge:* Fact table can grow very large.
   - *Solution:* Use table partitioning, archiving, and indexing strategies.

2. **Inventory Concurrency**
   - *Challenge:* Maintaining up-to-date inventory during high traffic.
   - *Solution:* Implement event-driven inventory updates using message queues or streams.

3. **Real-Time Reporting Needs**
   - *Challenge:* Star schema is optimized for batch, not real-time data.
   - *Solution:* Combine with Change Data Capture (CDC) and streaming tools like Kafka or Spark Streaming.

4. **Slowly Changing Dimensions**
   - *Challenge:* Customer or product attributes may change over time.
   - *Solution:* Apply Type 2 Slowly Changing Dimensions (SCD) to preserve history.

5. **Data Quality & Consistency**
   - *Challenge:* Ensuring accurate and consistent data ingestion.
   - *Solution:* Build robust ETL pipelines with validation, deduplication, and error handling.



##  Features Supported

-  Sales analysis by day, week, or month  
-  Top-selling product and category reporting  
-  Inventory level tracking  
-  Customer purchase history  
-  Payment method performance and trends  
