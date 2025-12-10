# Architecture & Implementation Plan

## Service Mapping

| Layer | Service (Cloud) | Role in Solution | Related Assignment/Module |
| :----- | :----- | :----- | :----- |
| Frotend/API | Cloud Run | Hosts the Flask web app, exposing the secure API endpoint and serving the operational dashboard. | Assignment 2 |
| Compute | Cloud Functions | Serverless functions triggered by new files in Cloud Storage to execute the ETL process. | Assignment 3/Module 5 |
| Storage | Cloud Storage | Stores high-volume, unstructured raw JSON data as the source for the Cloud Function trigger. | Module 6 |
| Database/SQL | Cloud SQL (PostgreSQL) | Stores cleaned, structured, aggregated patient metrics and serves as primary data source for both the Flask app and BigQuery/Tableau BI tool. | Assignment 4/ Module 7 |
| Analytics | Tableau/BigQuery | BI tools that securely connects to Cloud SQL to create visualizations and analytical reports. | Module 7 |
| Security/Identity | Secret Manager | Securely stores and manages application credentials (e.g. database passwords, API keys). | Assignment 4/ Module 7 |

## Dataflow Narrative
1.  Patient wearable device data is transmitted as JSON files and uploaded via the Flask App API endpoint.
2.  The raw JSON file is stored in the Cloud Storage bucket.
3.  The raw JSON file is then organized based on the upload date and patient ID.
4.  A Cloud Function is triggered automatically by the storage update.
5.  The function loads the aggregated data into a Cloud SQL database.
6.  The function performs data cleaning and transformation.
7.  The cleaned data is used to populate tables within the Cloud SQL database.
8.  User goes on the Flask dashboard and accesses the operational data from Cloud SQL.
9.  Tableau/Big Query runs complex queries to generate summary reports/dashboards for the user.

## Security, Identity, and Governance Basics
* Credentials Management: Credentials will be managed securely using Secret Manager and accessed by Cloud Run and Cloud Functions through environment variables.
* Access Controls: The Cloud Run service account will only have permissions to write to Cloud Storage and read from Cloud SQL. The Cloud Function service account will have specific read permissions for Cloud Storage and write permissions for Cloud SQL. Tableau will require a read-only service account or user for connecting to Cloud SQL.
* PHI Protection: To avoid putting real Protected Health Information (PHI) into public environments, all cloud resources will be deployed within a private Virtual Private Cloud (VPC). The Cloud SQL instance will be configured to only accept secure and authorized connections.

## Cost and Operational Considerations
* Highest Cost Components: Cloud SQL remains the highest cost component, due to the need for an always active relational database instance. The cost of Tableau licensing is also a consideration outside the cloud platform budget.
* Serverless Optimization: We utilize Cloud Run and Cloud Functions, scaling to zero instances when the API is not receiving data and billing only per execution, minimizing compute costs compared to always-on VMs.
* Student Budget/Free Tier: The Cloud SQL instance would be provisioned with the smallest machine type and scheduled to shut down during non-working hours to reduce cost.
