A data lake is a centralized database that collects and stores massive amounts of structured and unstructured data from any number of places. A data lake can store all your data  as is. It doesn’t have to be structured, cleaned, or deduplicated. You can then search, analyze, visualize, and correlate your data on the fly

AWS Lake Formation lets you create a data lake from all of your data regardless of  whether it’s stored on AWS or on-premise

It uses a separate service called AWS Glue that  performs what database nerds call extract, transform, and load (ETL) operations. AWS Glue  is based on the Apache Spark big data framework, so in addition to performing ETL operations, AWS Glue can be used to query massively large data sets.



Connecting to the SQL database using the JDBC connector and importing the data  
directly into the data lake is the most efficient solution.

allow to import any amount of data in real time.

Data Warehouse vs Data Lake
A data warehouse is a database optimized to analyze relational data coming from transactional systems and line of business applications
Data is cleaned, enriched, and transformed so it can act as the “single source of truth” that users can trust.

It doesn’t have to be structured, cleaned, or deduplicated. You can then search, analyze,  
visualize, and correlate your data on the fly.
AWS Lake Formation lets you create a data lake from all of your data regardless of  
whether it’s stored on AWS or on-premises.

It uses a separate service called AWS Glue that  
performs what database nerds call extract, transform, and load (ETL) operations
AWS Glue == Apache Spark framework
AWS Glue, AWS Lake Formation can import data from S3, RDS, AWS  
CloudFront, AWS CloudTrail, AWS Billing, and AWS Elastic Load Balancing (ELB)

Transformation
normalize data 
improve data consistency 
reformat data such as datetime to UTC
remove duplicate from multiple source FindMatches ML


Transformation includes formatting, combining, or eliminating duplicate, corrupted, or otherwise undesirable data (cleaning). Ingesting data from a variety of sources can result in  
some problems with search and analysis. For example, data from different sources may present the same data in different formats. This is especially true with timestamps, where one  
database may represent time in Coordinated Universal Time (UTC), whereas another may  
use a local time zon
