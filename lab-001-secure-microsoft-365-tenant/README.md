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

<img width="606" height="647" alt="001-mfa-acesso-condicional" src="https://github.com/user-attachments/assets/f4ec7878-f0ae-4d4a-9f4f-bf2bbd53913b" />
<img width="310" height="358" alt="002-mfa-acesso-condicional" src="https://github.com/user-attachments/assets/9f7b7e5e-0974-4621-bf78-e2a1c0d49d03" />
<img width="606" height="618" alt="003-mfa-acesso-condicional" src="https://github.com/user-attachments/assets/93969b63-8d06-44ea-ad06-0a85cce54983" />

#### [Users] MFA Enabled

Targets standard users and requires MFA.

<img width="1287" height="130" alt="004-mfa-acesso-condicional" src="https://github.com/user-attachments/assets/75ce0a03-c287-4cb9-a756-fc592c91eb9d" />

### Configuration Notes

The Break-Glass account was excluded from MFA enforcement.

---

## Step 4 – Configure Conditional Access

### Policy: Require MFA

Configured Conditional Access policies to require MFA for users and administrators.

### Policy: Block Legacy Authentication

Created a Conditional Access policy to block legacy authentication protocols.

<img width="620" height="370" alt="005-block-autenticacao-legada" src="https://github.com/user-attachments/assets/c9435b0c-9e97-4e13-abfa-2baa9e130508" />
<img width="613" height="519" alt="006-block-autenticacao-legada" src="https://github.com/user-attachments/assets/f4664cd9-4c18-4ed7-b980-c6d6c1ab6a35" />
<img width="1628" height="603" alt="007-block-autenticacao-legada" src="https://github.com/user-attachments/assets/d76f32a7-c9f7-4e84-9918-909d909699a7" />
<img width="1619" height="648" alt="008-block-autenticacao-legada" src="https://github.com/user-attachments/assets/71faf416-9aaf-4c48-8a85-8ad33bc96af0" />

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

## Additional Notes

- [Questions and Research](notes/questions.md)
- [Issues Encountered](notes/issues-encountered.md)
