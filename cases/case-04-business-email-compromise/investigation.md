# Case 04 - Business Email Compromise

## 1. Initial Triage

### Email Summary

| Field | Details |
|---|---|
| Sender | Finance Department <finance@acme-supplies.example> |
| Recipient | accounts-payable@company.example |
| Reply-To | payments@acme-supplies-finance.example |
| Subject | URGENT: Updated Bank Details - Payment Required Today |
| Date | 17 August 2026 |
| Email Type | Business Email Compromise (BEC) |
| Initial Classification | Suspicious |                                                                           |

### Initial Indicators

The email contains several indicators associated with Business Email Compromise and financial fraud:

* Urgent request to complete a payment before a specific deadline.
* Request to use new banking information for an existing invoice.
* Instruction to stop using previously known payment details.
* Sender claims to represent the vendor's Finance Department.
* `Reply-To` address differs from the sender's domain.
* The requested action involves a financial transaction.
* The email attempts to create urgency and discourage use of the previous banking information.

The email was therefore classified as **Suspicious** during initial triage and requires further investigation.

---

## 2. Email Header Analysis

### Sender Information

```text
From: Finance Department <finance@acme-supplies.example>
To: accounts-payable@company.example
Reply-To: payments@acme-supplies-finance.example>
```

The visible sender claims to represent the Finance Department of Acme Supplies.

The `From` address uses:

```text
acme-supplies.example
```

However, the `Reply-To` address uses:

```text
acme-supplies-finance.example
```

This difference is suspicious because replies related to the payment request would be redirected to a separate domain.

### Received Header

```text
Received: from mail.acme-supplies.example (192.0.2.50)
    by mail.company.example with ESMTP id 7H92K1
    for <accounts-payable@company.example>;
    Mon, 17 Aug 2026 09:14:25 +0000
```

The simulated sending host is:

```text
mail.acme-supplies.example
```

Simulated sending IP:

```text
192.0.2.50
```

The IP address belongs to the documentation/test range and is intentionally non-routable.

---

## 3. Authentication Analysis

The email contains the following authentication results:

```text
SPF: Pass
DKIM: Pass
DMARC: Pass
```

### SPF

```text
spf=pass smtp.mailfrom=acme-supplies.example
```

SPF passed for the simulated sending domain.

This indicates that the sending system was authorized to send email on behalf of the specified envelope sender domain.

### DKIM

```text
dkim=pass header.d=acme-supplies.example
```

DKIM passed and the message was cryptographically authenticated using the simulated `acme-supplies.example` domain.

### DMARC

```text
dmarc=pass header.from=acme-supplies.example
```

DMARC passed because the authenticated domain aligns with the visible `From` domain.

### Authentication Assessment

Although SPF, DKIM, and DMARC all passed, this does **not** establish that the financial request is legitimate.

The authentication results only demonstrate that the email was authenticated for the sending domain. They do not prove that the sender is authorized to request a bank-account change or payment.

This is an important distinction during phishing and BEC investigations.

---

## 4. Sender and Reply-To Analysis

The sender address is:

```text
finance@acme-supplies.example
```

The `Reply-To` address is:

```text
payments@acme-supplies-finance.example
```

The two domains are different:

```text
From domain:     acme-supplies.example
Reply-To domain: acme-supplies-finance.example
```

The separate `Reply-To` domain is suspicious because the email specifically instructs the recipient to communicate through that address after completing the payment.

This could allow an attacker to control subsequent communication and potentially maintain the deception while handling payment-related discussions.

The `Reply-To` mismatch therefore increases the confidence that the email is part of a potential BEC or payment-fraud attempt.

---

## 5. Social Engineering Analysis

The email uses several social-engineering techniques.

### Urgency

The message states that the payment must be completed before:

```text
5:00 PM today
```

This creates time pressure and reduces the likelihood that the recipient will independently verify the request.

### Financial Pressure

The email requests payment using newly provided banking information.

Changing payment instructions is a high-risk business action because fraudulent bank-account changes can redirect legitimate payments to an attacker-controlled account.

### Authority

The sender presents themselves as:

```text
Sarah Mitchell
Finance Department
Acme Supplies
```

This establishes an appearance of authority and legitimacy.

### Change in Normal Process

The recipient is instructed to stop using the previous banking information and use new payment details instead.

A request to change previously established payment information should be independently verified through a trusted communication channel.

---

## 6. Domain Analysis

The primary sender domain is:

```text
acme-supplies.example
```

The `Reply-To` domain is:

```text
acme-supplies-finance.example
```

The `Reply-To` domain introduces an additional domain that is not identical to the sender's domain.

The domain difference is suspicious in the context of an urgent financial request.

However, domain difference alone is not sufficient to prove malicious activity. The primary concern is the combination of:

* Different `Reply-To` domain
* Urgent payment request
* New banking information
* Request to abandon previous payment details
* Financial consequences if the request is fraudulent

The domains are therefore treated as suspicious indicators within the overall BEC investigation.

---


## 7. IOC Extraction

| IOC Type | Indicator | Verdict | Confidence | Reason |
|---|---|---|---|---|
| Email | finance[@]acme-supplies[.]example | Suspicious | Medium | Simulated sender used in BEC payment request |
| Email | payments[@]acme-supplies-finance[.]example | Suspicious | High | Suspicious Reply-To address used for payment communication |
| Domain | acme-supplies[.]example | Suspicious | Medium | Sender domain associated with simulated BEC email |
| Domain | acme-supplies-finance[.]example | Suspicious | High | Separate Reply-To domain associated with payment request |
| IP | 192.0.2.50 | Suspicious | Medium | Simulated sending IP |
| Invoice | INV-88421 | Suspicious | Medium | Invoice identifier associated with the simulated payment-fraud request |

All indicators are part of a simulated cybersecurity training environment.

---

## 8. Risk Assessment

### Classification

**Confirmed Phishing - Business Email Compromise**

### Severity

**High**

### Risk

The primary risk is financial fraud through unauthorized payment redirection.

If the recipient follows the instructions without verification, a legitimate business payment could potentially be sent to fraudulent banking details.

The email also attempts to bypass normal verification by creating urgency and instructing the recipient to stop using previously established payment information.

### Key Risk Factors

* Financial transaction requested
* Bank-account information changed
* Urgent deadline
* Sender claims financial authority
* Suspicious `Reply-To` domain
* Social-engineering pressure
* Potential payment redirection

---

## 9. Recommended SOC Response

1. Quarantine the email and prevent further delivery if the message has reached multiple users.
2. Do not approve or process the requested payment.
3. Contact the legitimate vendor using a previously known and trusted communication channel.
4. Independently verify whether the bank-account change is legitimate.
5. Search the mail environment for additional messages from the identified sender and `Reply-To` address.
6. Search for the suspicious domains and simulated sending IP across available email-security and network telemetry.
7. Identify whether any users replied to the suspicious address.
8. If financial information was already transferred, immediately escalate the incident to the appropriate finance, security, and incident-response teams.
9. Block or monitor the identified suspicious indicators where appropriate.
10. Document the incident and preserve the original email and relevant headers for further investigation.

---

## 10. Analyst Conclusion

The email is classified as **Confirmed Phishing - Business Email Compromise** with **High** severity.

The primary indicators are the urgent request to change payment information, the financial nature of the request, the attempt to create time pressure, and the mismatch between the `From` and `Reply-To` domains.

SPF, DKIM, and DMARC all passed, but these results do not establish that the financial request itself is legitimate. They only indicate that the message was authenticated for the sending domain.

The combination of authentication context, sender identity, `Reply-To` discrepancy, financial request, and social-engineering techniques indicates a potential Business Email Compromise scenario designed to redirect a legitimate payment.

No malware or malicious attachment was identified in this case. The primary threat is **social engineering and financial fraud**.
