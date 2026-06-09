# LAB 001 — Secure Microsoft 365 Tenant

## Objective

Establish a security baseline for a Microsoft 365 tenant by implementing identity protection, access controls, and self-service recovery mechanisms.

---

## Environment

| Item | Details |
|--------|---------|
| Tenant Type | Microsoft 365 E5 Trial |
| Hybrid Environment | No |
| Test Users | 5 |
| Administrative Account | Global Administrator |

---

## Lab Scope

This lab covers:

- User provisioning
- Security groups
- Microsoft 365 groups
- Multi-Factor Authentication (MFA)
- Conditional Access
- Legacy Authentication Blocking
- Self-Service Password Reset (SSPR)
- Break-Glass Account

---

## Step 1 – Create Users

### Activities

- Created test users manually through the Microsoft 365 Admin Center.
- Assigned Microsoft 365 E5 licenses.

### Notes

Users were created manually rather than through CSV import to better understand the provisioning process.

---

## Step 2 – Create Groups

### Security Groups

Created the following security groups:

- [Admin] MFA Enabled
- [Users] MFA Enabled

### Microsoft 365 Groups

Created the following collaboration groups:

- [Finance] Collaboration
- [HR] Collaboration

---

## Step 3 – Implement Multi-Factor Authentication

### Approach

Following Microsoft's recommendation, MFA was enforced through Conditional Access policies instead of per-user MFA.

### Policies Created

#### [Admin] MFA Enabled

Targets administrative accounts and requires MFA.

#### [Users] MFA Enabled

Targets standard users and requires MFA.

### Configuration Notes

The Break-Glass account was excluded from MFA enforcement.

---

## Step 4 – Configure Conditional Access

### Policy: Require MFA

Configured Conditional Access policies to require MFA for users and administrators.

### Policy: Block Legacy Authentication

Created a Conditional Access policy to block legacy authentication protocols.

### Rationale

Legacy authentication protocols do not support modern security controls such as MFA and represent a significant attack surface.

---

## Step 5 – Create Break-Glass Account

### Purpose

Provide emergency administrative access in situations where:

- MFA services are unavailable
- Conditional Access policies are misconfigured
- Identity services experience outages

### Security Considerations

- Not used for daily administration
- Protected with a strong password
- Excluded from Conditional Access policies
- Should be monitored and audited

---

## Step 6 – Configure Self-Service Password Reset (SSPR)

### Configuration

- Enabled SSPR for all users
- Required two authentication methods for password reset

### Observation

Changes were successfully saved, but there was a delay before the Entra ID portal reflected the updated configuration.

---

## Security Baseline Implemented

### Identity Protection

- MFA enabled
- Microsoft Authenticator configured as primary method

### Conditional Access

- MFA enforcement
- Legacy authentication blocked

### Password Management

- Self-Service Password Reset enabled
- Two verification methods required

### Emergency Access

- Break-Glass account implemented
- Excluded from Conditional Access policies

---

## Lessons Learned

- Conditional Access should be preferred over per-user MFA.
- Emergency access accounts are a critical component of tenant security.
- Portal interfaces may take time to reflect configuration changes.
- Security groups simplify policy assignment and future administration.

---

## References

- Microsoft Entra ID Conditional Access
- Microsoft Self-Service Password Reset
- Microsoft Security Best Practices
