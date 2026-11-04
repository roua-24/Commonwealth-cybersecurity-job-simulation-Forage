# Commonwealth Bank Introduction to Cybersecurity Job Simulation Forage
. Act as a cyber security generalist supporting the bank's fraud team. 
. Build a Splunk dashboard to analyze 200 simulated customer transactions and surface fraud patterns across category, age group, gender, month, and merchant — helping the team identify suspicious activity patterns for further investigation all through 4 tasks; **data analysis, incident response, security awareness and penetration testing.**
## Task 1 -- Data Analysis: Analyse and visualise cyber data to uncover trends and patterns
Mission: Analyze a provided fraud-related dataset and develop a dashboard to identify and vusualize patterns in the data.
**Tools and Setup:** 
. Splunk Entreprise: (local install 60-day free trial), Data Analysis, Data Visualization.
. Dataset: 200 simulated transactions with fieldsfor step(month), customer, age group, gender, postcode, merchant, category, amount and fraud flag.

**1. Importing the data into Splunk**
Confirm that prepared_data is a real CSV file (not .xlsx). If it is an Excel file, open it and use Save As > CSV UTF-8 (comma delimited).
In Splunk: Settings > Add Data > Upload > Select File, then choose prepared_data.csv.
In Set Source Type:
Timestamp: choose Current time (or "No timestamp (use index time)") so events are indexed correctly.
Click Save As and name the source type fraud_detection.csv.
Click Next until "File has been uploaded successfully", then Start Searching.
Basic search to check the import (200 events expected):
sourcetype="fraud_detection.csv" 
**2. Studying the data (Interesting Fields)**
In the left sidebar of the Events tab, the Interesting Fields section lists the fields detected by Splunk: age, amount, category, customer, fraud, gender, merchant, step, and others. Clicking a field shows its values and counts.
Note: the raw values contain stray quote characters (for example 'F', 'es_transportation'), and amount uses a decimal comma, so it is read as text. If fraud=1 returns nothing, check the exact value of the field and use fraud="'1'" instead.
**3. Searches used for the dashboard**

3.1 Counts by category, fraud, age and merchant

sourcetype="fraud_detection.csv" | stats count by category
sourcetype="fraud_detection.csv" | stats count by fraud
sourcetype="fraud_detection.csv" | stats count by age
sourcetype="fraud_detection.csv" | stats count by merchant

3.2 Fraud detected by age, category, step (month) and gender


sourcetype="fraud_detection.csv" fraud=1 | stats count by age
sourcetype="fraud_detection.csv" fraud=1 | stats count by category
sourcetype="fraud_detection.csv" fraud=1 | stats count by step
sourcetype="fraud_detection.csv" fraud=1 | stats count by gender

3.3 Which gender performed the most fraudulent activities, and in which category


sourcetype="fraud_detection.csv" fraud=1 | stats count by gender, category | sort - count

3.4 Which age group performed the most fraudulent activities, and at which merchant


sourcetype="fraud_detection.csv" fraud=1 | stats count by age, merchant | sort - count


**4. Building the dashboard**

Dashboards > Create New Dashboard > title Fraud Detection Dashboard > Classic Dashboards > Create. 
