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

Create `Dashboards > Create New Dashboard` (Classic), open **Source**, and paste the contents of [`dashboard.xml`](dashboard.xml). Export with `Export > Export PDF`.

### 4. Findings

| Question | Answer |
|---|---|
| Gender with most fraud, and category | _to complete_ |
| Age group with most fraud, and merchant | _to complete_ |

### 5. Files

- `dashboard.xml`: dashboard source
- `Fraud_Detection_Dashboard.pdf`: exported dashboard
- `screenshots/`: Interesting Fields view and dashboard panels
