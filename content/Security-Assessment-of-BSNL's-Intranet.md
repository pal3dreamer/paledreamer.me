---
title: Security Assessment of BSNL's Intranet
date: 2026-07-25
last edited: 2026-07-25
tags:
  - security
  - responsible-disclosure
  - web-security
  - authentication
---

## Introduction

During routine security research, I discovered multiple critical security vulnerabilities affecting BSNL's employee intranet portal (`intranet.bsnl.co.in`) and parts of its public DNS infrastructure.

The most severe issue allowed complete account takeover of employee accounts through the password reset workflow. While investigating the authentication flow, I also identified the disclosure of internal SMS gateway configuration and multiple publicly accessible recursive DNS resolvers.

The vulnerabilities were responsibly disclosed to CERT-In, who coordinated with BSNL. After remediation was confirmed and verified, I decided to document the findings and the disclosure process.

---

## Background
While reviewing the reset workflow, something caught my attention

Although the application correctly sent the OTP through SMS, it also returned the exact OTP inside the HTTP response before redirecting the browser to the confirmation page.

---

## Finding 1: OTP Disclosure

The password reset workflow generated a valid OTP and delivered it to the registered mobile number.

However, the server also embedded the generated OTP directly inside the HTML response sent back to the client.

Since every HTTP response is fully visible to the requester, an attacker could simply inspect the response and recover the OTP without ever needing access to the victim's phone.

The attack required only knowledge of a valid Personnel Number.

```
Personnel Number
        │
        ▼
Request Password Reset
        │
        ▼
Server Generates OTP
        │
        ├────────► SMS sent
        │
        └────────► OTP returned in HTTP response
                        │
                        ▼
Attacker submits OTP
                        │
                        ▼
Password Reset Successful
```

This effectively reduced SMS verification to a cosmetic security control.

---

## Technical Analysis

The vulnerability was not caused by weak OTP generation or predictable values.

Instead, it was a classic implementation flaw.

The application generated the OTP correctly and successfully delivered it through SMS.

However, before redirecting the browser, the server unnecessarily included the OTP inside the HTML response.

---

## Finding 2: Internal SMS Gateway Disclosure

While examining the password reset response, I also discovered that the application exposed details about the backend SMS infrastructure.

The response revealed:

- Internal gateway endpoint
- Internal network addressing
- Service credentials
- SMS request format

All sensitive values have been redacted from this article.

Although this issue alone did not directly allow remote compromise, exposing internal infrastructure provides valuable reconnaissance information and unnecessarily expands the attack surface.

---


## Impact

The OTP disclosure vulnerability allowed complete compromise of employee accounts without requiring access to the victim's mobile device.

Potential impact included:

- Complete account takeover
- Unauthorized password resets
- Bypass of SMS verification
- Compromise of employee accounts
- Exposure of internal infrastructure

Among the reported issues, the OTP disclosure represented the highest risk due to its simplicity and potential scale.

---

## Responsible Disclosure

The vulnerabilities were reported through CERT-In.

Following the initial report, CERT-In requested a proof-of-concept video demonstrating the issue with active timestamps.

After reviewing the submitted evidence, the report was forwarded to BSNL for remediation.

Later, CERT-In informed me that the issues had been fixed and requested verification.

After confirming the remediation, the disclosure process was considered complete.

---

## Disclosure Timeline

- **9 June 2026** Vulnerabilities discovered.
- **9 June 2026** Report submitted to CERT-In.
- **9 June 2026** CERT-In requested proof-of-concept evidence.
- **9 June 2026** Video PoC submitted.
- **25 June 2026** CERT-In reported that BSNL had fixed the vulnerabilities.
- **2 July 2026** Fixes verified.

---



## Conclusion

The most interesting aspect of this research was its simplicity.

A single implementation mistake completely bypassed the security guarantees provided by SMS-based verification.