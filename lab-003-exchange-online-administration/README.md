# LAB 003 – Exchange Online Administration

## Objective

Implement and administer common Exchange Online workloads in a corporate environment, including mailbox management, mail flow rules, security controls, troubleshooting, and PowerShell administration.

---

## Environment

| Item                 | Details                                                      |
| -------------------- | ------------------------------------------------------------ |
| Platform             | Exchange Online                                              |
| Tenant Type          | Microsoft 365 E5 Trial                                       |
| Identity Source      | Microsoft Entra ID                                           |
| Administration Tools | Exchange Admin Center, Microsoft Defender Portal, PowerShell |

---

## Lab Scope

This lab covers:

* Shared Mailboxes
* Mailbox Delegation
* Mail Flow Rules
* Security and Protection
* Quarantine Management
* Message Trace
* Exchange Online PowerShell

---

# Phase 1 – Shared Mailboxes

## Objective

Create and manage shared mailboxes used by multiple users.

### Shared Mailboxes Created

* compartilhado.financeiro
* compartilhado.rh
* compartilhado.suporte

### Shared Mailbox vs User Mailbox

| User Mailbox              | Shared Mailbox                         |
| ------------------------- | -------------------------------------- |
| Assigned to a single user | Shared among multiple users            |
| Personal mailbox          | Department or team mailbox             |
| Requires user sign-in     | Accessed through delegated permissions |
| Individual ownership      | Shared ownership                       |

### Common Use Cases

Shared mailboxes are commonly used for:

* Finance departments
* Human Resources
* Purchasing teams
* Customer support

They centralize communication while preserving message history independently of employee turnover.

---

## Mailbox Delegation

### Permissions Configured

Financeiro:

* Financeiro01 → Full Access
* FinanceiroManager → Full Access
* FinanceiroManager → Send As

### Permission Types

#### Full Access

Allows users to:

* Read messages
* Move messages
* Delete messages
* Manage mailbox contents

Does not allow sending messages as the mailbox.

#### Send As

Allows users to send messages using the mailbox identity.

Recipients see:

From: [financeiro@company.com](mailto:financeiro@company.com)

#### Send on Behalf

Allows users to send on behalf of another mailbox.

Recipients see:

From: User Name on behalf of [financeiro@company.com](mailto:financeiro@company.com)

### Recommended Scenarios

| Permission     | Typical Use Case                  |
| -------------- | --------------------------------- |
| Full Access    | Mailbox management                |
| Send As        | Shared departmental communication |
| Send on Behalf | Executive assistant scenarios     |

---

# Phase 2 – Mail Flow Rules

## External Sender Identification

### Objective

Add an external sender warning to incoming messages.

### Example

Subject modified with:

[EXTERNAL]

<img width="1058" height="379" alt="001-mail-flow-rule" src="https://github.com/user-attachments/assets/3e03c371-6f13-4195-b166-fb7b7c975b1b" />
<img width="579" height="781" alt="002-mail-flow-rule" src="https://github.com/user-attachments/assets/a9a450a9-9633-4a66-b0cb-228898bfb595" />
<img width="566" height="564" alt="003-mail-flow-rule" src="https://github.com/user-attachments/assets/a1756c00-c38b-486a-96b4-4170fc074774" />
<img width="495" height="358" alt="004-mail-flow-rule" src="https://github.com/user-attachments/assets/342545e8-aac7-4c03-b018-15c7ba34a335" />

### Risks Mitigated

* Phishing
* Business Email Compromise (BEC)
* Identity Spoofing
* Accidental information disclosure

### Observation

Mail flow rules do not replace security controls but provide an additional layer of user awareness.

---

## Corporate Disclaimer

### Objective

Display a warning banner for messages originating from outside the organization.

### Example

"This message originated from outside the organization. Exercise caution when opening links, attachments, or sharing sensitive information."

<img width="527" height="193" alt="005-disclaimer" src="https://github.com/user-attachments/assets/c04334d5-466c-4df1-9c9c-dad2136aaaf2" />
<img width="1293" height="386" alt="006-disclaimer" src="https://github.com/user-attachments/assets/2d8d5e0c-729d-471c-bbf1-6c7f70f07cf1" />

### Compliance Benefits

* Security awareness
* Regulatory compliance support
* Reduced social engineering risks

### Limitations

Users may become accustomed to warnings over time and ignore them.

---

## Attachment Blocking

### Blocked Extensions

* .exe
* .bat
* .cmd

<img width="777" height="621" alt="007-block-attachments" src="https://github.com/user-attachments/assets/1ac1e57d-92ae-4040-8e1a-c771399bb736" />

### Security Benefits

Reduces the risk of malware distribution through email attachments.

### Modern Alternatives

Microsoft Defender for Office 365 Safe Attachments provides dynamic analysis of suspicious files before delivery.

### Recommendation

Use OneDrive and SharePoint for secure file sharing whenever possible.

---

# Phase 3 – Protection and Security

## Anti-Spam Review

### Inbound Protection

Reviewed default Exchange Online Protection settings.

### Observations

Exchange Online Protection uses:

* Reputation-based filtering
* Machine learning
* Microsoft threat intelligence

Additional protection options may be evaluated based on organizational requirements.

### Outbound Protection

Outbound policies help:

* Protect domain reputation
* Detect compromised accounts
* Prevent mass spam campaigns
* Restrict unauthorized forwarding

---

## Quarantine Review

### Item Types

The following items can be quarantined:

* Email messages
* Files
* Microsoft Teams messages

### Administrator Actions

Administrators can:

* Review headers
* Preview messages
* Submit content to Microsoft
* Release items
* Block items
* Block senders

---

# Phase 4 – Troubleshooting

## Message Trace

### Purpose

Track email delivery through Exchange Online.

### Information Available

* Sender
* Recipient
* Subject
* Delivery status
* Message events
* Mail flow rule processing

### Common Scenarios

* Missing messages
* Delivery failures
* Transport rule validation
* Mail flow investigations

---

## Delivery Failure Simulation

A transport rule was created to block messages sent to a target mailbox.

### Investigation Process

1. Send test message.
2. Open Message Trace.
3. Locate the affected message.
4. Review Message Events.

### Result

The transport rule action was visible in the message processing events.

---

# Phase 5 – Exchange Online PowerShell

## Connect to Exchange Online

```powershell
Install-Module ExchangeOnlineManagement

Connect-ExchangeOnline
```

## List Mailboxes

```powershell
Get-Mailbox
```

<img width="1105" height="338" alt="008-powershell" src="https://github.com/user-attachments/assets/b08b0fc5-03bd-4146-9bfb-6b80b4e03e0c" />

## List Shared Mailboxes

```powershell
Get-Mailbox -RecipientTypeDetails SharedMailbox
```

<img width="1084" height="154" alt="009-powershell" src="https://github.com/user-attachments/assets/eb4ef447-4e02-4d13-905f-6409293f03a0" />

## Export Mailbox Report

```powershell
Get-EXOMailbox -ResultSize Unlimited |
Select-Object DisplayName, PrimarySmtpAddress |
Export-Csv -Path "C:\Temp\Mailboxes.csv" -NoTypeInformation -Encoding UTF8
```

<img width="789" height="71" alt="010-powershell" src="https://github.com/user-attachments/assets/de1bf48d-c252-4e82-baa5-90f152f620d4" />
<img width="705" height="161" alt="011-powershell" src="https://github.com/user-attachments/assets/382eb096-85c0-419e-9c22-b9149de772e1" />
<img width="863" height="287" alt="012-powershell" src="https://github.com/user-attachments/assets/c2653443-d504-4180-97b7-fd4c8d950e4b" />

### Benefits

* Automation
* Bulk administration
* Reporting
* Auditing

---

# Key Learnings

* Shared Mailboxes provide centralized communication while preserving organizational history.
* Mail Flow Rules are powerful administrative and security tools.
* Message Trace is one of the most important troubleshooting features in Exchange Online.
* Quarantine analysis helps validate security controls and investigate suspicious messages.
* PowerShell remains essential for scalable Exchange administration.

---

## Related Notes

* Issues Encountered
