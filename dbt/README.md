## Analytics engineering

Source: https://www.udemy.com/course/complete-dbt-data-build-tool-bootcamp-zero-to-hero-learn-dbt/learn/lecture/31028384#overview

Start with theory- build out the design first to understand the value.

Maslow's Pyramid of Data Needs (first three stages are involved here)
1. Data collection: google analytics, an endless number of APIs here
2. Data wrangling
3. Data integration: staging -> target database. "Refresh" or "Update" all data.
4. BI + Analytics
5. Artificial Intelligence


ETL: Extract, Transform, Load
- Cost of storage and computation has come way down
- Traditional approach
- Issue: changing schema, scaling is not easy, hard to test and debug an ETL

Reorganized ELT as things became cheaper to store
- Do it in DB
- Fivetran and Stitch (load) to load in just a few clicks
- T = Transform
- Warehouse: high peformance reporting, execute SQL against it


Data Lake: repo with all kind of data: structured, unstructured
- Amazon S2, Hadoop HDFS
- Point is to store files
- Issues: pay whether used or not. Even if data warehouse storage isn't much.
- "External tables" can store in Amazon S3 unstructured data

Data Lakehouse:
- Combine best features of data lakes + data warehouse
- Sits ontop of local cloud storage 

The Modern Data Stack (horizontally, and fully managed):
- Compute + storage decoupled

Data source: stripe, google analytics
Extract/load: Airbyte, Fivetran, Stitch, Segemnt
Transorm: dbt, Dataiku
Warehouse: Snowflake
ReverseETL: Hightouch, census, grouparoo
Destination: hubspot, zendesk

Slowly changing dimensions - data that changes rarely + unpredictably
- Storing historic data, "SCD" types 0-3
- Type 0: skip updating the data warehouse column
- Type 1 (overwrite): only the new value is imported
- Type 2: Add new row
- Type 3: Add new attribute

dbt: Spotlight, production-grade data pipelines
- SQL statements: when they are done, they are done
- dbt: absolute visibility 
- modularity, portability
- version controlled transformations
- can create different environments
- will craft your model dependency order


Analytics eng: responsible for data flow
- Import into data warehouse
- Clean, do transformations
- Export to a BI tool
- Then make sure pipeline well tested and well documented

### Models : SQL definitions that create materialized views. Stored as .sql files. Can refer to other models 
src_listings
src_reviews
src_hosts

The trend these days is to add SQL support to avoid or minimize programming.