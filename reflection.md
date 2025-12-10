# Reflection

## Confidence and Uncertainty
I am most confident in the data ingestion and storage layers using Cloud Run and Cloud Storage. These services are highly scalable, simple to integrate with the Flask application, and the scenario-based trigger from Cloud Storage to Cloud Functions is a reliable pattern that only runs when data is retrieved in JSON file. The integration of Secret Manager for secure credential handling also makes the security much stronger. I am least sure about the performance and security implications of connecting the external Tableau tool directly to Cloud SQL. Supporting heavy, complex analytical queries from Tableau might impact the latency for the Flask application's operational queries.

## Alternative Architecture
I considered using a message service, like a Cloud Pub/Sub, instead of Cloud Storage as the primary ingestion point. While Pub/Sub is excellent for high-velocity streams and decoupling, I rejected it for this project because using Cloud Storage with a file-upload trigger (Cloud Function) is simpler. This approach satisfies the storage requirement while still allowing for event-driven processing, making the overall design more straightforward.

## Next Steps
1. Enhance Monitoring and Alerting:
  - Implement a dedicated monitoring service (e.g. Cloud Monitoring) to track key performance indicators (KPIs).
  - Integrate an external service to send real-time SMS/Email notifications for critical patient alerts, transforming the system into a more proactive monitoring tool.
2. Enhance User Experience:
  - Develop a frontend framework (e.g. React) within the Flask application to allow users to build custom dashboards by rearranging data visualization widgets and filtering tabs.
  - Integrate a Generative AI service (e.g. Gemini API) to create a built-in AI that can answer user questions about patient trends, alert definitions, and data compliance policies.
  - Implement an autoamtic virtual scheduler to generate reports (PDF/CSV) from Cloud SQL and electronically send them to the user via email.
3. ML Implementation:
  - Implement a time-series model to analyze patient data in Cloud SQL and automatically detect and flag abnormal data or trends to predict a critical health events.
  - Develop a predictive model to calculate a patient's risk score and integrating this score as a new column in the Cloud SQL operational tables.
4. Enhance Data Handling:
  - Replace the current file uploading process by developing a dedicated secure API gateway or using an integration service to connect directly to hospital EHR/billing systems for automated data transfer, which will eliminate manual steps.
