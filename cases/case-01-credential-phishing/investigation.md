# Case 01 - Credential Phishing
---

## 1. Initial Triage

### Sender
Microsoft 365 Security <security-alert@example.com>

### Recipient
analyst@company.example

### Subject
Urgent: Verify Your Microsoft 365 Account

### Date/Time
16 Aug 2026 09:42:17 UTC

### Claimed Organization
Microsoft 365

### Initial Suspicion
Urgent account-verification request with a suspicious sender/reply-to combination and login URL.

---

## 2. Email Header Analysis

### From
Microsoft 365 Security <security-alert@example.com>

### Reply-To
microsoft-security@example.net

### Return-Path
<bounce@example.com>

### Message-ID
<20260816094217.12345@example.com>

### Received Headers
- 192.0.2.10 → mail.example.com → mx.company.example
- 198.51.100.25 → mail.example.com

### Sending IP
198.51.100.25

### Mail Servers
- workstation.example.net
- mail.example.com
- mx.company.example

---

## 3. Email Authentication

### SPF
Fail

### DKIM
Fail

### DMARC
Fail

---

## 4. URL Analysis

### Extracted URLs
https://login.microsoftonline.com.security-verify.example/login

### Redirects
None observed

### Domain Investigation
security-verify.example

### Reputation
Not yet investigated


---

## 5. Domain/IP Investigation

### DNS
No live DNS lookup performed. The domain uses the reserved `.example` TLD for safe simulation.

### WHOIS/RDAP
Not applicable. `.example` is reserved for documentation and testing.

### IP
198.51.100.25

### ASN/ISP
Not applicable. `198.51.100.0/24` is reserved for documentation.

### Reputation
No live reputation lookup performed because the indicators are simulated.


---

## 6. IOC Extraction

| IOC Type | Indicator | Verdict | Reason |
|---|---|---|---|
| Domain | security-verify[.]example | Suspicious | Lookalike login domain used in simulated phishing email |
| URL | hxxps://login.microsoftonline.com.security-verify[.]example/login | Suspicious | URL structure attempts to appear associated with Microsoft |
| IP | 198.51.100.25 | Suspicious | Simulated sending IP |
| Email | microsoft-security[@]example[.]net | Suspicious | Reply-To differs from claimed organization |


---

## 7. Risk Assessment

### Verdict
Malicious

### Severity
High

### Evidence
- SPF, DKIM and DMARC all failed.
- Reply-To differs from the claimed organization.
- Login URL uses a deceptive domain structure.
- Email uses urgency to encourage immediate account verification.

---

## 8. Recommended SOC Response

- Block the phishing URL and associated domain where appropriate.
- Search email security logs for other recipients.
- Check whether any users clicked the URL.
- Identify any accounts that submitted credentials.
- Reset compromised credentials if required.
- Escalate confirmed compromise to the incident response team.

---

## 9. Analyst Conclusion

The email is classified as a high-severity malicious phishing attempt targeting Microsoft 365 credentials.