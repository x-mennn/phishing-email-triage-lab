# Case 02 - Malicious Attachment


## 1. Initial Triage

Sender:

`Accounts Payable <accounts-payable@vendor-example.example>`

Recipient:

`analyst@company.example`

Subject:

`Urgent: Outstanding Invoice - Payment Required`

Date:

`16 Aug 2026 10:15:32 UTC`

Claimed organization:

`Vendor Example`

Attachment:

`Invoice_2026_0816.txt`

Initial suspicion:

`The email uses an urgent payment-related request and includes an invoice attachment that requires review.`

Initial assessment:

`Suspicious - further investigation required.`




## 2. Email Header Analysis

### From

`Accounts Payable <accounts-payable@vendor-example.example>`

### To

`analyst@company.example`

### Subject

`Urgent: Outstanding Invoice - Payment Required`

### Date

`16 Aug 2026 10:15:32 UTC`

### Reply-To

`billing@vendor-example.example`

### Return-Path

`<bounce@vendor-example.example>`

### Message-ID

`<20260816101532.67890@vendor-example.example>`

### Received Headers

```text
192.0.2.50 → mail.vendor-example.example
```



## 3. Email Authentication

The simulated email contains the following authentication results:

```text
SPF: Pass
DKIM: Pass
DMARC: Pass
```



## 4. Attachment Analysis

Attachment:

`Invoice_2026_0816.txt`

File type:

`Plain text (.txt)`

File size:

`235 bytes`

File extension:

`.txt`

Creation time:

`16-08-2026 20:39:34`

Last modified:

`16-08-2026 20:40:16`

The attachment is a harmless simulated training artifact created specifically for this lab.

The file contains invoice-related information and does not contain executable code or malicious content.

The attachment was not executed as code.





## 5. File Hash Investigation

SHA-256:

C13CD71FE84A8A0254ECCCE587C31113EFBED91FE33CD30A64DF9794796AF992

The SHA-256 hash was calculated locally using PowerShell.

No malware was executed during the investigation.

Threat-intelligence reputation lookup:

Not yet investigated.




## 6. IOC Extraction

| IOC Type | Indicator | Verdict | Confidence | Reason |
|---|---|---|---|---|
| Email | billing[@]vendor-example[.]example | Suspicious | Medium | Reply-To address associated with the simulated sender |
| IP | 192.0.2.50 | Suspicious | Medium | Simulated sending IP |
| Filename | Invoice_2026_0816.txt | Suspicious | Medium | Invoice-themed attachment delivered through an urgent payment request |
| SHA-256 | C13CD71FE84A8A0254ECCCE587C31113EFBED91FE33CD30A64DF9794796AF992 | Suspicious | Medium | Hash uniquely identifies the analyzed attachment |




## 7. Risk Assessment

Verdict:

`Suspicious`

Severity:

`Medium`

Evidence:

- Urgent payment-related request
- Invoice-themed attachment
- SPF passed
- DKIM passed
- DMARC passed
- Attachment was received through a payment-related request
- SHA-256 hash was collected and verified
- The attachment is a harmless simulated training artifact
- No executable code or malicious content was identified

The email is classified as suspicious because it combines an urgent payment request with an attachment. However, the available evidence does not support classifying the attachment as actual malware.




## 8. Recommended SOC Response

- Do not execute the attachment on an endpoint.
- Preserve the original email and attachment for analysis.
- Record the SHA-256 hash for correlation and future investigations.
- Search email security logs for other recipients of the same message.
- Check whether any users opened or interacted with the attachment.
- Verify the payment request through an independent communication channel if required.
- Escalate for further investigation if additional suspicious indicators are identified.




## 9. Analyst Conclusion

The email is classified as a medium-severity suspicious attachment-based phishing scenario.

The message uses an urgent payment-related request and includes an invoice-themed attachment. SPF, DKIM, and DMARC all passed in the simulated environment, demonstrating that successful email authentication does not by itself prove that an email is safe.

The attachment was confirmed to be a harmless training artifact, and no malware was executed during the investigation.

The available evidence supports a suspicious classification rather than a confirmed malicious verdict.`