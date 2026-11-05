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

## Task 4: Penetration Testing 
**Goal:** complete 11 beginner labs on PortSwigger Web Security Acqdemy, then write a penetration testing report (executive summary, scope, findings per lab, recommendations)
**Scope:** only the PortSwigger training labs (legal, safe environment). No real websites or third-party systems were tested.
**Tools:** PortSwigger Web Security Academy, a browser, Burp Suite Community (for the authentication labs).
### Labs completed
 
| # | Lab (PortSwigger) | Vulnerability | Severity* | Main fix |
|---|---|---|---|---|
| 1 | SQL injection: login bypass | SQL injection | High | Parameterized queries |
| 2 | SQL injection: WHERE clause, hidden data | SQL injection | High | Prepared statements, least-privilege DB account |
| 3 | Reflected XSS, nothing encoded | Cross-site scripting | Medium | Context-aware output encoding, CSP |
| 4 | Stored XSS, nothing encoded | Cross-site scripting | High | Sanitize and encode stored content |
| 5 | DOM XSS in `document.write` sink | DOM-based XSS | Medium | Safe DOM APIs, no untrusted data in sinks |
| 6 | Unprotected admin functionality | Broken access control | High | Server-side authorization checks |
| 7 | Unprotected admin functionality, unpredictable URL | Broken access control | High | Authorization, not obscurity |
| 8 | Username enumeration via different responses | Authentication flaw | Medium | Same error message, rate limiting |
| 9 | 2FA simple bypass | Authentication flaw | High | Enforce 2FA server-side before granting a session |
| 10 | File path traversal, simple case | Path traversal | High | Allowlist files, canonicalize paths |
| 11 | OS command injection, simple case | Command injection | Critical | Avoid shell calls, strict input validation |
 
\*Severity is my indicative rating for a real application, not an official score. Confirm the exact lab titles against your PortSwigger account.
 
### Quick reference: how each lab was solved
 
```text
1  Username: administrator'--   (comment out the password check)
2  /filter?category=Gifts'+OR+1=1--   (show unreleased products)
3  Search box: <script>alert(1)</script>
4  Comment field: <script>alert(1)</script>
5  Search: "><svg onload=alert(1)>   (breaks out of the document.write HTML)
6  /robots.txt reveals the admin panel path, then delete user carlos
7  View page source: admin URL leaked in JavaScript, then delete carlos
8  Burp Intruder with a username list: one response differs (invalid username vs incorrect password), then brute force that user's password
9  Log in as the victim with known credentials, skip the 2FA page and open /my-account directly
10 /image?filename=../../../etc/passwd
11 Stock check parameter: 1 | whoami   (injected into a shell command)
```
 
All payloads are for the PortSwigger training environment only.
 
### Key takeaways
 
1. **Never trust user input.** Validate, parameterize and encode it (labs 1 to 5, 10, 11).
2. **Enforce access control on the server.** Hidden URLs and client-side checks are not protection (labs 6, 7, 9).
3. **Authentication flows need every step enforced**, with consistent errors and rate limiting (labs 8, 9).
4. **Fix both front end and back end.** DOM XSS shows browser code can be vulnerable too (lab 5).
### Top recommendations
 
- Use parameterized queries and an allowlist for file and command inputs.
- Encode output by context and add a Content Security Policy.
- Check authorization on every request and every admin route.
- Return identical login errors, add rate limiting and lockout, and enforce MFA server-side.
- Run regular penetration tests, code reviews and developer training.
### Report structure
 
1. Executive summary
2. Overview of the application tested
3. Scope of testing
4. Key findings table (lab, vulnerability, description, finding, recommendation)
5. Overall analysis
6. Conclusion
7. Appendix A to K: one completion screenshot per lab
### Files
- `Penetration_Testing_Report.pdf`
### Resources
 
- PortSwigger Web Security Academy: portswigger.net/web-security
- OWASP Top 10 and OWASP Web Security Testing Guide: owasp.org
