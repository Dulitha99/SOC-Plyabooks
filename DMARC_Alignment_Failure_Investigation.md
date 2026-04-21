# SOC Playbook

## DMARC Alignment Failure Investigation

---

## 1. Purpose

This playbook provides a standardized procedure for Security Operations Center (SOC) analysts to investigate and respond to email alerts involving DMARC failures due to SPF and DKIM alignment issues.

---

## 2. Scope

This playbook applies to:

- Microsoft Defender for Office 365
- Mimecast Email Security Gateway
- Any environment monitoring email authentication failures

---

## 3. Objective

Determine whether a DMARC failure is:

- Legitimate but misconfigured
- Suspicious and requires further investigation
- Malicious (phishing or spoofing attack)

---

## 4. Definitions

- **SPF (Sender Policy Framework):** Validates sending IP against domain DNS records
- **DKIM (DomainKeys Identified Mail):** Validates message integrity via cryptographic signature
- **DMARC (Domain-based Message Authentication, Reporting & Conformance):** Ensures alignment between sender identity and authentication results
- **Alignment:** Matching of SPF or DKIM domain with the visible From domain

---

## 5. Trigger Conditions

This playbook is triggered when:

- DMARC = fail
- Reason includes:
  - SPF not aligned
  - DKIM not aligned

---

## 6. Investigation Procedure

### 6.1 Step 1 - Identify the Email

**Microsoft Defender:**

- Navigate to Threat Explorer
- Search by recipient, subject, sender, or timestamp

**Mimecast:**

- Use Message Tracking
- Filter using recipient or sender details

---

### 6.2 Step 2 - Extract Authentication Results

Collect the following fields from email headers:

- spf=
- dkim=
- dmarc=
- header.from
- smtp.mailfrom (Return-Path)
- dkim d=

Construct a comparison table:

| Field       | Value |
| ----------- | ----- |
| From Domain |       |
| SPF Domain  |       |
| DKIM Domain |       |

---

### 6.3 Step 3 - Analyze Alignment

Evaluate whether alignment exists:

- SPF aligned if Return-Path domain matches From domain
- DKIM aligned if d= domain matches From domain

If neither SPF nor DKIM aligns with the From domain, DMARC fails.

---

### 6.4 Step 4 - Assess Sender Reputation

**Microsoft Defender:**

- Review sender details in Email Entity view
- Check first-seen, sending patterns, reputation

**Mimecast:**

- Check sender IP reputation
- Review authentication history

---

### 6.5 Step 5 - Evaluate User Context

Determine:

- Whether the recipient expects the email
- Whether the sender is a known service (HR, CRM, SaaS)

---

### 6.6 Step 6 - Inspect Email Content

Review for:

- Suspicious links or domains
- Attachments
- Urgency or social engineering language
- Credential harvesting attempts

Use safe preview or sandbox tools where available.

---

## 7. Decision Matrix

### 7.1 Legitimate (Misconfiguration)

Indicators:

- SPF = pass
- DKIM = pass
- Known service provider
- Expected by user

Conclusion:

- Authentication misalignment
- Not malicious

---

### 7.2 Suspicious

Indicators:

- Unknown sender
- Mixed authentication signals
- Unclear intent

Conclusion:

- Requires further investigation

---

### 7.3 Malicious (Phishing/Spoofing)

Indicators:

- Brand impersonation
- Credential harvesting links
- User did not expect email
- Domain mismatch

Conclusion:

- Confirmed phishing or spoofing attempt

---

## 8. Response Actions

### 8.1 Legitimate (Misconfiguration)

Actions:

- Mark as Not Phishing in Defender
- Allow sender (if necessary)
- Notify internal IT/email team

Recommendation:

- Fix SPF/DKIM alignment on sender side

---

### 8.2 Suspicious

Actions:

- Escalate to Tier 2/3
- Perform sandbox analysis
- Monitor for similar activity

---

### 8.3 Malicious

Actions:

1. Block sender/domain
2. Search for similar emails
3. Quarantine or purge emails
4. Investigate user interaction

---

## 9. Threat Hunting

Search for:

- Repeated DMARC failures from same domain
- Same sending IP across multiple emails
- Similar phishing patterns

---

## 10. Documentation

Record:

- Sender domains (From, SPF, DKIM)
- Alignment results
- Verdict
- Actions taken
- User impact

---

## 11. Key Notes

- DMARC failure alone does not indicate malicious activity
- Alignment is critical for identity verification
- SaaS platforms commonly cause misalignment issues
- Always validate against user expectations

---

## 12. Workflow Summary

DMARC Fail -> Check Alignment -> Verify Sender -> Analyze Content -> Confirm User Context -> Classify -> Respond

---

End of Playbook
