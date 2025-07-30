🧠 Case 1: Daily Sales Ingestion and Reporting

📌 Scenario

A retail company collects daily sales data from multiple small shops.

Each shop uploads a structured `.csv` file once per day.

The company needs a **serverless**, **scalable**, and **low-cost** solution to:

- Ingest and store raw files
- Clean and format data
- Organize files for efficient querying
- Enable analysts to run ad hoc SQL queries

🧱 Architecture Overview

| Stage           | AWS Services Used                          | Purpose                                                  |
|-----------------|--------------------------------------------|----------------------------------------------------------|
| **Acquisition** | Amazon S3 (`/raw`)                         | Receive daily `.csv` files from shops                    |
| **Integration** | AWS Lambda                                 | Clean/transform raw files and write to partitioned S3    |
| **Storage**     | Amazon S3 (`/processed`)                   | Store cleaned files, partitioned by `shop_id` and `date` |
| **Cataloging**  | AWS Glue Crawler + Data Catalog            | Detect schema and register partitioned data              |
| **Consumption** | Amazon Athena                              | Run SQL queries on processed data                        |



🗂️ S3 Partitioning Structure
```

s3://your-bucket/processed/shop\_id=001/date=2025-07-30/data.parquet

````



🧑‍💻 Example Athena Query
```sql
SELECT shop_id, SUM(price * quantity) AS total_sales
FROM sales_processed
WHERE date = '2025-07-30'
GROUP BY shop_id;
````


🧭 Architecture Diagram

![](https://github.com/DonnaDia/aws-data-solutions-architect-portfolio/blob/060b15d999658a283a70c8c39996708bd5eea7dc/simple%20project.jpeg?raw=true)


✅ Design Highlights

* **Serverless stack** using S3, Lambda, Athena
* **Partitioned storage** improves query speed and cost efficiency
* **Glue Catalog** manages schema and partitions for scalable analytics with Apache Parquet format for better performance
