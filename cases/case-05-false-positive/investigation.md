# Case 05 - False Positive / Benign Suspicious Email

## 1. Initial Triage

### Email Summary

| Field                  | Details                                                                                        |
| ---------------------- | ---------------------------------------------------------------------------------------------- |
| Sender                 | Billing Team <[billing@northstar-services.example](mailto:billing@northstar-services.example)> |
| Recipient              | [accounts-payable@company.example](mailto:accounts-payable@company.example)                    |
| Reply-To               | [billing@northstar-services.example](mailto:billing@northstar-services.example)                |
| Subject                | Action Required: Invoice Available for Review                                                  |
| Date                   | 17 August 2026                                                                                 |
| Email Type             | Invoice / Billing Notification                                                                 |
| Initial Classification | Suspicious                                                                                     |

### Initial Indicators

The email may initially attract attention because it contains several characteristics commonly observed in phishing emails:

* The subject contains the phrase `Action Required`.
* The email concerns an invoice and payment.
* The recipient is instructed to review an invoice.
* The message originates from an external vendor domain.

Because the email involves a financial process, it should initially be reviewed rather than automatically trusted.

The message was therefore classified as **Suspicious** during initial triage pending further investigation.

---

## 2. Email Header Analysis

### Sender Information

```text
From: Billing Team <billing@northstar-services.example>
To: accounts-payable@company.example
Reply-To: billing@northstar-services.example
```

The sender and `Reply-To` addresses use the same domain:

```text
northstar-services.example
```

There is no `Reply-To` mismatch or redirection to an unrelated domain.

### Received Header

```text
Received: from mail.northstar-services.example (192.0.2.80)
    by mail.company.example with ESMTP id 9K42M7
    for <accounts-payable@company.example>;
    Mon, 17 Aug 2026 10:32:21 +0000
```

The simulated sending host is:

```text
mail.northstar-services.example
```

Simulated sending IP:

```text
192.0.2.80
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
spf=pass smtp.mailfrom=northstar-services.example
```

SPF passed for the simulated sending domain.

### DKIM

```text
dkim=pass header.d=northstar-services.example
```

DKIM passed and the message was authenticated using the same domain as the sender.

### DMARC

```text
dmarc=pass header.from=northstar-services.example
```

DMARC passed and the authenticated domain aligns with the visible `From` domain.

### Authentication Assessment

All three email authentication mechanisms passed and are consistent with the sender domain.

Authentication alone does not prove that an email is harmless, but in this case there are no additional authentication anomalies requiring escalation.

---

## 4. Sender and Reply-To Analysis

The sender address is:

```text
billing@northstar-services.example
```

The `Reply-To` address is:

```text
billing@northstar-services.example
```

Both addresses use the same domain.

There is no evidence of a suspicious redirection to a separate domain.

This reduces the likelihood of sender impersonation or an attempt to redirect communication to an attacker-controlled mailbox.

---

## 5. Social Engineering Analysis

The email contains limited social-engineering pressure.

The subject uses:

```text
Action Required
```

This wording can appear in both legitimate business communication and phishing emails.

However, the body does not contain strong urgency or coercion.

There is:

* No threat of account suspension.
* No demand for immediate payment.
* No request to bypass normal procedures.
* No request to change existing banking information.
* No request for credentials.
* No suspicious external link.
* No unusual payment instructions.

The recipient is specifically instructed to process the invoice according to normal accounts-payable procedures.

This is consistent with a legitimate business billing notification.

---

## 6. Domain Analysis

The sender domain is:

```text
northstar-services.example
```

The `Reply-To` address uses the same domain:

```text
northstar-services.example
```

There is no lookalike-domain variation between the sender and Reply-To addresses.

The domain is therefore not considered suspicious based on the evidence available in this simulated investigation.

Because this is a training artifact, the domain does not represent a real-world organization.

---

## 7. IOC Extraction

| IOC Type | Indicator                                                                       | Confidence | Description                                |
| -------- | ------------------------------------------------------------------------------- | ---------- | ------------------------------------------ |
| Email    | [billing@northstar-services.example](mailto:billing@northstar-services.example) | Low        | Legitimate-looking vendor billing address  |
| Domain   | northstar-services.example                                                      | Low        | Sender and Reply-To domain                 |
| IP       | 192.0.2.80                                                                      | Low        | Simulated sending IP                       |
| Invoice  | NS-2026-0817                                                                    | Low        | Invoice identifier referenced by the email |

No malicious URL, suspicious domain variation, credential-harvesting address, or other high-confidence malicious indicator was identified.

The indicators are retained for investigation context but should **not** automatically be treated as malicious IOCs.

---

## 8. Risk Assessment

### Classification

**Benign / False Positive**

### Severity

**Low**

### Risk

The email initially appeared potentially suspicious because it involved an invoice and used the phrase `Action Required`.

Further analysis did not identify sufficient evidence of phishing or malicious activity.

The sender and Reply-To addresses match, SPF/DKIM/DMARC all pass, and the message does not contain suspicious payment redirection, credential requests, threatening language, or unusual communication instructions.

The email should therefore be closed as a **False Positive / Benign Email**.

### Key Findings

* SPF passed.
* DKIM passed.
* DMARC passed.
* Sender and Reply-To domains match.
* No suspicious URL identified.
* No request to change banking information.
* No credential request.
* No significant urgency or coercion.
* Payment should follow normal accounts-payable procedures.

---

## 9. Recommended SOC Response

1. Close the alert as **Benign / False Positive**.
2. Do not block the sender or domain based solely on this message.
3. Allow the recipient to process the invoice through normal accounts-payable procedures.
4. Retain the investigation evidence for audit and future alert-tuning purposes.
5. If the sender later exhibits suspicious behavior, reassess the domain and historical messages.
6. Consider tuning detection rules so legitimate invoice notifications are not repeatedly escalated without additional suspicious indicators.

---

## 10. Analyst Conclusion

The email was initially considered suspicious because it contained an invoice-related payment context and used the phrase `Action Required`.

Further investigation found no significant indicators of phishing.

SPF, DKIM, and DMARC all passed, and the sender and `Reply-To` addresses use the same domain. The message does not request credentials, attempt to change banking information, contain a suspicious URL, or apply significant social-engineering pressure.

The email is therefore classified as:

**Benign / False Positive - Low Severity**

This case demonstrates that SOC analysts should use multiple pieces of evidence when classifying suspicious emails rather than treating a single suspicious-looking characteristic as proof of malicious activity.

The appropriate response is to close the alert while preserving the evidence and avoiding unnecessary blocking or escalation.
