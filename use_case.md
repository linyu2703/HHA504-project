# Cloud Based Integration: Remote Patient Monitoring (RPM) Data Pipeline and Analytics

## Description
This project designs a cloud-based solution for the continuous and secure monitoring of high-risk patients using data from wearable medical devices.

## Problem Statement
Chronic disease management is an important area of healthcare that handles long-term illnesses like diabetes, heart disease, or asthma, aiming to improve patients' quality of life and prevent serious health complications. The current problem is that it relies heavily on infrequent and costly clinic visits. Additionally, clinicians and clinical staffs often lack a scalable, real-time mechanism to monitor patient status outside of the hospital. This leads to delayed intervention for critical events.

## Solution
Automated cloud-based remote patient monitoring pipeline that ingests high-volume sensor data, processes and aggregates the readings, stores the results in a managed SQL database for operational use, and utilizes a data warehouse for complex analytics and reporting.

## Target Users
1. Physicians/Clinicians:
    * They review high-priority alerts and current patient vital signs (e.g. heart rate, blood sugar).
    * They use the summarized data to make timely, data-driven decisions regarding patient care and medication adjustments.
2. Clinical Staffs/Nurses:
    * They monitor the operational dashboard for new alerts and patient risk status changes.
    * They triage and follow up on alerts that require immediate patient outreach or troubleshooting device connectivity issues.
    * They manage patient enrollment and ensure data compliance.
3. Hospital/Program Administrators:
    * They use Tableau dashboards to assess the overall cost-effectiveness and Return on Investment (ROI) of the RPM program.
    * They compare patient adherence rates and clinical outcomes across different departments.
    * They use the analysis and reports to make data-driven decisions relating to program scalability and resource allocation.

## Data Sources
Primary Data: Raw Sensor Readings (JSON) with the following fields:
`timestamp`, `location`, `department`, `patient_id`, `device_id`, `heart_rate_bpm`, `systolic_bp`, `diastolic_bp`, `blood_glucose_mg_dL`, `body_temperature_f`, `activity_level`, `battery_status`

Data source: JSON file transmitted via API endpoints from patient wearable devices.

## Basic Workflow
1.  Data Ingestion (Device -> Cloud Run API)
    * The patient's wearable device securely transmits the raw sensor data (JSON loadout).
    * The external API endpoint, hosted on the Flask App (Cloud Run), receives the high-volume data stream.
2.  Raw Data Storage (Cloud Run -> Cloud Storage)
    * The received raw JSON files are stored in the Cloud Storage.
    * Files are auto-organized based on a predefined structure, such as patient ID and upload date.
3.  Data Processing Trigger (Cloud Storage -> Cloud Functions)
    * The creation of a new file in Cloud Storage automatically triggers a Cloud Function (serverless compute).
    * The Cloud Function reads the raw data.
4.  ETL and Data Cleaning (Cloud Functions -> Cloud SQL)
    * The Cloud Function performs data cleaning and aggregation.
    * The cleaned, aggregated data (operational metrics and alerts) is then loaded into the Cloud SQL (PostgreSQL) database.
5.  Analytics Processing (Tableau)
    * Tableau connects directly to the Cloud SQL database to run complex queries and generate patient trend reports based on the aggregated metrics.
6.  Dashboard Visualization (Cloud Run <- Cloud SQL/Tableau <- Cloud SQL)
    * Clinicians access the Flask App (Cloud Run) for real-time operational alerts from Cloud SQL.
    * Administrators access the Tableau Dashboard (external to GCP) for sophisticated analytical reports and trend analysis, pulling data directly from Cloud SQL.
