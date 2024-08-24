# Logistics Incremental Data Ingestion ETL Pipeline

## <ins>Overview</ins>
The Logistics Incremental Data Ingestion project is a robust data processing and integration solution leveraging Apache Spark within the Databricks . Its primary objective is to efficiently manage and process logistics order tracking data while incorporating incremental updates into a designated target table. This process entails retrieving, processing, and integrating data from a Google Cloud Storage (GCS) bucket into Delta tables within the Databricks environment.

## <ins>Architectural Diagram:</ins>
![Architectural_diagram](https://github.com/KiranParihar/Logistics-Incremental-Data-Ingestion-ETL-Pipeline/blob/main/Architecture_diagram_Logistics_etl.png)


## <ins>Tools and Technologies used:</ins>
- Google Cloud Platform(GCP)
- Apache Spark
- Databricks 


## <ins>Prerequisites</ins>

1) In GCP bucket, using service account which is connected to databricks, create two folder which are STAGING_ZONE and ARCHIVE.

2) Create storage credentials and external loaction in databricks which will enable us to access file stored on Google Cloud Storage (GCS) into databricks.


## <ins>Steps performed </ins>


**Step 1:** Daily logistics order tracking data file is received at Google Cloud Storage (GCS) bucket.

**Step 2:** As soon as file is received in Google Cloud Storage (GCS) bucket, the job created in Databricks Workflow will get triggered.

**Step 3:** Creation of databricks workflow job and scheduling it to run on daily basis.

I) There are two task within the databricks workflow job:  
a) Stage_data_load:  
--> It will create a delta stage table if doesn't exist and will insert the data from the daily order tracking file which is received at GCS bucket.Also, it take care of archival process of daily files.


b) Target_data_load:  
--> It will create a delta target table if doesn't exist and insert records from stage table into it for the first time and from the second run, it checks for common records and when found it deletes the common record from target_table and load the latest records of into target table.

II) The job within databricks workflow is scheduled to run everytime we receive a new file in source GCS bucket. To schedule this functionality, I have created a storage credential and external location with help of Unity Catalog in databricks and have successfully connected it to my GCP service account.

**Step 4:** Added job notifications which will notify the status of job(START, SUCCESS, FAILURE) on email. If needed, the job notification can also be easily integrated on Slack, Microsoft teams, Webhook and PagerDuty.
