# Real-Time-Food-Delivery-Analysis
![Real Food Delivery Pipeline drawio](https://github.com/user-attachments/assets/2c733b8c-f40b-47ff-99c7-7ecc4a60b96f)

# Architecture Overview:

## 1.Data Pipeline Workflow:
### Data Ingestion into Redshift:
Dimension Tables: The application includes three dimension tables, which hold metadata about entities like Customers, Drivers, and Restaurants. These are static tables updated periodically.
Fact Table: The fact table contains transactional data, such as real-time updates about deliveries, which are continuously ingested.

## 2. AWS Redshift Setup
The Redshift cluster acts as the central data warehouse where both the dimension and fact data are stored. Redshift is optimized for analytical queries and scales with the application's data size.
Dimension Tables: The data for the dimension tables is uploaded to Amazon S3. AWS Airflow is used to automate the creation and loading of these tables into Redshift.
Fact Table: The fact table is designed to capture real-time transactional data, focusing on high-throughput data inserts.
## 3. AWS S3 for Data Storage
Data Upload: The data for the dimension tables is stored in Amazon S3 in CSV format. The files are uploaded manually or programmatically from a source.
Data Transformation: AWS Airflow DAGs orchestrate the process of creating tables in Redshift and loading the data from S3 into the corresponding dimension tables in Redshift.
## 4. Airflow DAGs
First DAG: This DAG handles the creation of dimension tables in Redshift and the loading of data from S3 into these tables. It automates the ETL (Extract, Transform, Load) process for static data.
Second DAG: This DAG is triggered after the first one. It is responsible for orchestrating the real-time data ingestion process from Kinesis into Redshift. This includes the handling of any schema updates or changes to the streaming process.
## 5. Data Streaming with AWS Kinesis
Kinesis Stream: Real-time data about food delivery transactions is streamed into AWS Kinesis. The data is produced by a mock Python data generator script simulating transaction events.
Pyspark Streaming Application: The streaming data from Kinesis is ingested into Redshift using a Pyspark Streaming Application running on AWS EMR. The application processes the streaming data in near real-time, performs necessary transformations, and loads the data into the fact table in Redshift.
## 6. AWS EMR for Data Processing
EMR Cluster: The Pyspark application is executed on an AWS EMR cluster, which is responsible for the heavy lifting of processing streaming data. The EMR cluster reads the Kinesis stream, applies business logic and transformations, and then pushes the processed data into Redshift.
Pyspark Code: The application is written using Pyspark Streaming, enabling the processing of large-scale streaming data and efficient integration with Redshift.
## 7. Mock Data Generation
Python Data Generator: A Python script simulates real-time transaction data for the food delivery service. The mock generator produces structured data and sends it to the Kinesis stream to mimic the flow of real-time events in the food delivery service.
## 8. Real-Time Data Processing
Continuous Data Flow: The entire pipeline is designed for real-time data ingestion, where events are captured continuously through Kinesis and processed in near real-time through the Pyspark streaming application running on EMR.
Ingestion into Redshift: After processing, the streaming data is ingested directly into the Redshift fact table, allowing for near real-time analytics and reporting.
## 9. Automation and Orchestration
Airflow DAGs: Airflow DAGs automate the entire process. From loading the dimension tables to orchestrating the continuous stream of transactional data into Redshift, Airflow ensures the system runs smoothly and data is updated in real-time.
## 10. Scalability and Fault Tolerance
AWS Services: The use of AWS services like Redshift, Kinesis, and EMR ensures that the pipeline is scalable and fault-tolerant. Kinesis allows the system to handle large amounts of streaming data, while Redshift scales for data storage and querying.
