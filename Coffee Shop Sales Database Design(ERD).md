☕ Coffee Shop Sales Database Design and ERD

**Overview**

For this project, I designed a relational database 🗄️ for a coffee shop sales system, 
focusing on both operational efficiency and analytical capabilities. 

The database supports:
- Customer management 👥
- Sales tracking 🛒
- Product categorization 🏷️
- Staff management 👩‍💼

This project demonstrates my ability to analyze business requirements, design normalized schemas, and implement best practices in relational database design.


**Context Before Normalization**

Originally, the data was captured in a flat file format, which caused several problems:

- Redundant data: Customer, staff, and product details repeated across multiple sales records 🔄
- Data anomalies: Frequent insertion, deletion, and update issues ⚠️
- Limited scalability: Hard to query efficiently for reporting and analytics 📉
- Poor maintainability: Updating product, staff, or customer info required multiple row edits 🔧

**Normalization and ERD Design**


To ensure data integrity and performance, I normalized the database up to 3rd Normal Form (3NF).

**Entities created:**

1. Customer: Unique customer info (name, email, birthdate, gender, loyalty date)

2. Staff: Employee info (name, position, start date, location)

3. Sales Outlet: Store locations with manager info, address, city, and contact

4. Product Type: Categories to reduce redundancy

5. Product: Product details (name, description, type, price)

6. Sales Transaction: Sales events linked to customers, staff, and outlets

7. Sales Detail: Items sold per transaction (quantity + price)



**Key relationships:**

- Customer → Sales Transaction (1:N)
- Staff → Sales Transaction (1:N)
- Sales Outlet → Sales Transaction (1:N)
- Product → Sales Detail (1:N)
- Sales Transaction → Sales Detail (1:N)

This design ensures data consistency, prevents redundancy, and enhances performance.

**Highlights for a Data Architect**

- Normalization: Eliminated redundancy, preserved performance 📏
- Data Integrity: Strong primary + foreign key design 🛡️
- Scalability: Handles large transactions + product growth 📈
- Analytics Ready: Easy to build BI dashboards + materialized views 📊
- Extensible: Flexible for promotions, loyalty programs, multi-outlets 🔮

**ERD Visualization**

Below is the resulting Entity-Relationship Diagram (ERD) after normalization:

![](https://github.com/DonnaDia/aws-data-solutions-architect-portfolio/blob/d857358ce417a81b3f8616bf4dfed77283f70960/cofee_shop_db.png?raw=true)

**Skills Demonstrated**

- Data modeling + normalization (1NF → 3NF)
- Relational database design (PostgreSQL, MySQL)
- ERD creation + visualization
- Referential integrity + foreign keys 🔑
- Database prep for analytics + reporting


✨ This project highlights my expertise in designing robust, scalable, and maintainable relational database architectures, 
vital for roles such as **Data Architect, Database Designer**, and **Analytics Engineer**.
