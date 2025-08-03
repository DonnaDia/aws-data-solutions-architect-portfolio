**🧠 Case 2: Streaming Sales Data Ingestion and Reporting**

**📌 Problem Statement**

A growing number of small retail shops submit transaction data daily for centralized analysis. 
The business goal is to detect anomalies (e.g., abnormally high refunds), visualize performance trends in near real-time, 
and make the processed data queryable historically - all without building heavy ETL infrastructure.

**Key constraints:**
- Data arrives frequently and needs to be ingested in real-time
- The data is semi-structured (JSON)
- Retention policy: 30 days
- Need anomaly detection, dashboards, and ad hoc queries


**🧭 Architecture Diagram**

![](https://github.com/DonnaDia/aws-data-solutions-architect-portfolio/blob/03ed509e47d82dad86664427637ecae15deb957f/Streaming%20Daily%20Sales%20Ingestion%20and%20Reporting.jpeg?raw=true)


**🧱Flow Architecture Overview**
1. ***Shops*** send **JSON transactions** via **AWS API Gateway**
2. Data flows through **Kinesis Firehose**, is monitored by **CloudWatch**, and lands in **S3 /raw**
3. Two **Lambda functions** handle:
   * Basic cleaning & validation
   * Conversion to `.parquet`
4. Cleaned `.parquet` data is stored in **S3 /processed**, partitioned by `shop_id` and `date`
5. **S3 Event** triggers a **Step Function**, which runs a **Glue Crawler** to update the **Glue Data Catalog**
6. **AWS Lookout for Metrics** monitors for anomalies and triggers **SNS alerts** if needed
7. Cleaned data is consumed via:

   * **Athena** (for ad hoc querying)
   * **OpenSearch Dashboards** (for near-real-time visualization)



**⚖️ Key Architectural Decisions & Tradeoffs**

🔄 Streaming Method: **Kinesis Firehose** vs. **Kinesis Data Streams**

* **Chosen**: Firehose
* **Why**: Easier setup and native support for S3 delivery and format conversion
* **Tradeoff**: Slightly higher ingestion latency (\~60s) than Kinesis Streams, but acceptable for this use case

🧠 Anomaly Detection: **Lookout for Metrics** vs. **Custom SageMaker Model**

* **Chosen**: AWS Lookout for Metrics
* **Why**: Fully managed, no ML expertise needed, easy SNS integration
* **Tradeoff**: Less customizable than SageMaker-based solutions

📊 Dashboarding: **OpenSearch** vs. **QuickSight**

* **Chosen**: OpenSearch Dashboards
* **Why**: Better support for near-real-time data and search/filter capability
* **Tradeoff**: Higher operational and cost overhead than QuickSight for historical-only queries

🧹 Transformation: **Lambda-based processing**

* **Why**: Stateless, scalable, and easy to maintain
* **Tradeoff**: Limited in processing heavy files; suitable for smaller batch sizes typical in this scenario



**🗂️ S3 Partitioning Structure**
```
s3://<your-bucket>/processed/
  shop_id=shop1/
    date=2025-08-01/
      file1.parquet
    date=2025-08-02/
      file2.parquet
  shop_id=shop2/
    date=2025-08-01/
      file3.parquet
```



**🧑‍💻 Example Athena Query**
```sql
SELECT shop_id, date, SUM(total_sales) as daily_sales
FROM processed_table
WHERE date BETWEEN date '2025-08-01' AND date '2025-08-03'
GROUP BY shop_id, date
ORDER BY date DESC;
```



**✅ Design Highlights**

* **Cost-aware**: S3-based storage with lifecycle rules; serverless compute
* **Partitioned & Queryable**: Efficient queries using Athena + Glue Catalog
* **Real-Time Capable**: Kinesis Firehose + OpenSearch + Lookout for Metrics
* **Operational Simplicity**: Minimal services to manage manually; uses Step Functions and Event Triggers



**❓ When to Use This Architecture**
This architecture suits:

* Teams without a dedicated data engineering team
* Use cases that need a balance between real-time anomaly alerts and analytical reporting
* Projects where scalability and simplicity matter more than millisecond latency



**🏗️ Future Improvements**
* Replace Lambda with AWS Glue ETL jobs for more complex transformations
* Use Amazon QuickSight for lighter historical dashboards if OpenSearch is too costly
* Integrate with Lake Formation for fine-grained data access controls
