# Commonwealth Bank Cybersecurity Job Simulation (Forage)

Acting as a cyber security generalist supporting the bank's fraud team. The simulation has 4 tasks: data analysis, incident response, security awareness and penetration testing.

## Task 1: Data Analysis

**Goal:** build a Splunk dashboard on 200 simulated transactions to surface fraud patterns by category, age group, gender, month and merchant.

**Tools:** Splunk Enterprise (local, 60-day trial)
**Dataset:** 200 transactions (step/month, customer, age, gender, postcode, merchant, category, amount, fraud flag)

### 1. Import

1. Make sure `prepared_data` is a real CSV (not `.xlsx`).
2. Splunk: `Settings > Add Data > Upload`, select the file.
3. Timestamp: **Current time**. Save the source type as `fraud_detection.csv`.
4. Check the import (expect 200 events):

```spl
sourcetype="fraud_detection.csv"
```

Note: values contain stray quotes (`'F'`, `'es_food'`). If `fraud=1` returns nothing, use `fraud="'1'"`.

### 2. Searches

Counts:

```spl
sourcetype="fraud_detection.csv" | stats count by category
sourcetype="fraud_detection.csv" | stats count by fraud
sourcetype="fraud_detection.csv" | stats count by age
sourcetype="fraud_detection.csv" | stats count by merchant
```

Fraud by age, category, step, gender:

```spl
sourcetype="fraud_detection.csv" fraud=1 | stats count by age
sourcetype="fraud_detection.csv" fraud=1 | stats count by category
sourcetype="fraud_detection.csv" fraud=1 | stats count by step
sourcetype="fraud_detection.csv" fraud=1 | stats count by gender
```

Top gender and category:

```spl
sourcetype="fraud_detection.csv" fraud=1 | stats count by gender, category | sort - count
```

Top age group and merchant:

```spl
sourcetype="fraud_detection.csv" fraud=1 | stats count by age, merchant | sort - count
```

### 3. Dashboard

Create `Dashboards > Create New Dashboard` (Classic), open **Source**, and paste the contents of [`Dashboard.xml`](Dashboard.xml). Export with `Export > Export PDF`.

### 4. Findings

| Question | Answer |
|---|---|
| Gender with most fraud, and category | female;49 vs 35 for male almost all in `es_transportation`|
| Age group with most fraud, and merchant | 1; 36 cases , `M348934600` |

### 5. Files

- `dashboard.xml`: dashboard source
- `Fraud_Detection_Dashboard.pdf`: exported dashboard
- `screenshots/`: Interesting Fields view and dashboard panels

## Task 2:Responding to a Cyber Incident 
**Goal:** act as cybersecurity analyst; identify the attack and lead the response: contain, resolve, recover and learn.
**Scenario Summary:** staff received emails that appeared to come from the bank (HR) asking them to  download a file. Some users who opened the Word document could not read it, which points to malware.
**Files:** 
`response example`
## Task 3: Security Awareness 
**Goal:** design an infographic for fellow employees on password security, based on advise from the Australian Cyber Security Center (ACSC) [https://www.cyber.gov.au/ ](https://www.cyber.gov.au/)
**Tools:** Canva
**Files:** [`password_security_infographic.pdf`](password_securityçinfographic.pdf)

