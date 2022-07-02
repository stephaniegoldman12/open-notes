# Snowflake

### Snowflake architecture

**Cloud infra** - manage infra, access control, security, optimizer (the brain of the system)

**Query processing** - MMP (massive parallel processing) - virtual warehouses are used as compute resources to process queries

**Data storage** - stored in AWS - saved in blobs (compressed, instead of saving into rows) “hybrid columnar storage”

### Virtual warehouses (”servers” spun up to run the queries) - each one doubles the number of servers

XS = 1

S = 2

XL = 16…

4XL = 128

Instead of increasing size of server if start with Small server with 2. If enterprise version can use multi-cluster to cluster 3 “S” warehouses

### Data sampling

- Don’t need complete 10TB db to test or write a query. Take a random sample of 500GB from original DB, and use that to develop.

### Data sharing

- Since storage sep from compute, it’s easy
- Can even share with non-snowflake through a reader account
- Create a publicly available bucket + copy the data into it
- You are the **producer**, whoever has access is the **consumer**

```sql
CREATE SHARE ORDERS_SHARE;
GRANT USAGE ON DATABASE DATA_S TO SHARE ORDERS_SHARE;

```

### Zero-Copy Cloning - copy all data + metadata easily

Usually, you have to copy all of the table structure, in Snowflake, you can clone with just one command

- It’s a completely independent object the clone
- Purpose: usually, to create backups for example to have a test env

```sql
Create table name;
clone source_table;
before ( timestamp => <time> ) 

-- Creates a copy before a certain timestamp
```

### Create warehouse - can use the button, or create with code

Warehouses are used for reporting and data analysis. For example, it can bring together HR data + sales data from all different formats and sources.

**ETL - process of creating the data warehouse.** 

Different layers:

- Raw data (staging area)
- Data integrations (have relationships between tables from different sources) — data transformation step
- Access layer - in the end, make it available

### Snowpipe (”serverless” feature) — managed by Snowflake

When a new file is added to a bucket, it’s loaded into a given table

- If data needs to be auto available for analysis

### Tasks - can execute 1 SQL statement at a certain time

Understand tasks

Create tasks

Schedule tasks

Create a tree of tasks

Create a task history

### Streams - if there are changes to the HR database, it’ll record and update Snowflake with what changes there were

DML - data manipulation language (Change data capture)

- more storage due to additional metadata
    - metadata action, update, row
    - you can query directly from the stream
    - think of steam object like queue that implements changes

```sql
CREATE STREAM <STREAM NAME> 
ON TABLE <table name>

Select * from <stream name>

One insert additional data, remove "stream object"
Since it's moved over to the target object
```

### Materialized views → automatically updated by Snowflake (8.5s to run a query, 0.5s to query the MV)

View: query you want to run frequently, takes a long time to process

- Bad UX, costs money
- Solution - save result of the query in a materialized vew
- Always updated automatically based on the base table
- You can leverage tasks + streams instead as well. Don’t use MVs if the data changes frequently + keep costs in mind

```sql
CREATE MATERIALIZED VIEW OF ORDERS_MV
AS 
SELECT ...QUERY HERE;

SHOW MATERIALIZED VIEWS

-- Query the view
SELECT * FROM ORDERS_MV ORDER BY YEAR;
```

- Update one value

```sql
UPDATE ORDERS;
SET 0_CLERK='CK'
WHERE 0_ORDERDATE='1992-04'
```

- When you run the MV again, it’ll be updated automatically

Highlight only the part of the query you want to run in snowflake