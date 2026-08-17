# Case 03 - Domain Impersonation

## 1. Initial Triage

Sender:

`Microsoft 365 Support <support@microsoft-security.example>`

Recipient:

`analyst@company.example`

Subject:

`Action Required: Microsoft 365 Security Verification`

Date:

`17 Aug 2026 09:18:42 UTC`

Claimed organization:

`Microsoft 365`

Reply-To:

`support@microsoft-security.example`

URL:

`https://microsoft-security.example/account/verify`

Initial suspicion:

`The email uses Microsoft 365 branding and an urgency-based security verification request, but the sender domain appears inconsistent with Microsoft's legitimate domain infrastructure.`

Initial assessment:

`Suspicious - potential domain impersonation. Further investigation required.`



## 2. Email Header Analysis

### From

`Microsoft 365 Support <support@microsoft-security.example>`

### To

`analyst@company.example`

### Subject

`Action Required: Microsoft 365 Security Verification`

### Date

`17 Aug 2026 09:18:42 UTC`

### Reply-To

`support@microsoft-security.example`

### Return-Path

`<bounce@microsoft-security.example>`

### Message-ID

`<20260817091842.34567@microsoft-security.example>`

### Received Headers

```text
192.0.2.40 → mail.microsoft-security.example → mx.company.example
```
### Sending IP

192.0.2.40

Mail Server

mail.microsoft-security.example

Authentication Results
SPF: Pass
DKIM: Pass
DMARC: Pass

The email authentication results indicate that the simulated message passed SPF, DKIM, and DMARC checks.

However, successful authentication does not establish that the sender domain represents the legitimate organization being impersonated.

Further domain analysis is required.


## 3. Domain Analysis

Claimed organization:

`Microsoft 365`

Sender domain:

`microsoft-security.example`

The sender domain uses Microsoft-related branding but does not match Microsoft's legitimate domain.

The domain appears designed to create trust by combining the organization name "Microsoft" with the term "security".

The use of a lookalike or misleading domain is a strong indicator of potential domain impersonation.

Domain assessment:

`Suspicious - potential impersonation domain.`

The SPF, DKIM, and DMARC results passed for `microsoft-security.example`. This indicates that the email was authenticated for the domain it was sent from, but it does not establish that the domain itself legitimately belongs to Microsoft.


## 4. URL Analysis

URL:

`https://microsoft-security.example/account/verify`

Protocol:

`HTTPS`

Domain:

`microsoft-security.example`

Path:

`/account/verify`

URL assessment:

`Suspicious`

The URL uses the domain `microsoft-security.example`, which incorporates Microsoft-related branding but is not Microsoft's legitimate domain.

The `/account/verify` path is consistent with a security verification or account-management lure and is designed to encourage the recipient to interact with the link.

The use of a Microsoft-branded lookalike domain combined with an account verification request is consistent with a domain impersonation phishing technique.

No URL execution was performed during the investigation.

Conclusion:

`The URL is a phishing indicator because the destination domain does not legitimately represent Microsoft despite using Microsoft-related branding.`


## 5. IOC Extraction

| IOC Type | Indicator | Verdict | Confidence | Reason |
|---|---|---|---|---|
| Domain | microsoft-security.example | Suspicious | High | Uses Microsoft-related branding but does not represent Microsoft's legitimate domain |
| URL | https://microsoft-security.example/account/verify | Suspicious | High | Points to a Microsoft-branded lookalike domain and requests account verification |
| Email | support@microsoft-security.example | Suspicious | High | Sender uses the same misleading Microsoft-branded domain |
| IP | 192.0.2.40 | Suspicious | Medium | Simulated sending IP associated with the suspicious domain |



## 6. Risk Assessment

Verdict:

`Confirmed Phishing - Domain Impersonation`

Severity:

`High`

Evidence:

- Sender claims to represent Microsoft 365.
- Sender domain is `microsoft-security.example`.
- The domain uses Microsoft-related branding but does not represent Microsoft's legitimate domain.
- The email requests immediate security verification.
- The URL points to the same Microsoft-branded lookalike domain.
- SPF passed for the lookalike domain.
- DKIM passed for the lookalike domain.
- DMARC passed for the lookalike domain.
- The successful authentication results do not establish that the domain legitimately belongs to Microsoft.

The email is classified as a high-severity domain impersonation phishing attempt because it uses trusted-brand impersonation and an account verification lure to encourage the recipient to interact with a suspicious domain.

The authentication results indicate that the message was authenticated for the sending domain, but the sending domain itself is not legitimate Microsoft infrastructure.


## 7. Recommended SOC Response

- Quarantine the email from the recipient's mailbox.
- Block the identified sender domain and phishing URL through the appropriate email and web security controls.
- Search the environment for other messages containing the same sender, domain, or URL.
- Determine whether any users interacted with the suspicious URL.
- Review endpoint and authentication logs for signs of account compromise.
- If user interaction occurred, initiate the organization's account-compromise response process.
- Preserve the original email and relevant headers as evidence.
- Add the identified domain and URL to the organization's threat-intelligence and detection records.


## 8. Analyst Conclusion

The email is classified as a high-severity domain impersonation phishing attempt.

The sender claims to represent Microsoft 365 but uses the domain `microsoft-security.example`, which does not represent Microsoft's legitimate domain infrastructure.

The email combines trusted-brand impersonation with an urgent account-security verification request and directs the recipient to a URL hosted on the same suspicious domain.

SPF, DKIM, and DMARC passed for the sending domain. However, these authentication results only demonstrate that the message was authenticated for `microsoft-security.example`; they do not prove that the domain legitimately belongs to Microsoft.

The primary indicators identified during the investigation are the suspicious sender domain, phishing URL, sender address, and simulated sending IP.

No interaction with the suspicious URL was performed during the investigation.