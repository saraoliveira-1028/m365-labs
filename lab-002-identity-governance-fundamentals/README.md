# LAB 002 – Identity Governance Fundamentals

## Objective

Implement a foundational identity governance model in Microsoft Entra ID using:

* Users
* Security Groups
* Microsoft 365 Groups
* Dynamic Groups
* Group-Based Licensing
* Role-Based Access Control (RBAC)
* Administrative Units (AUs)

---

## Environment

| Item               | Details                |
| ------------------ | ---------------------- |
| Tenant Type        | Microsoft 365 E5 Trial |
| Hybrid Environment | No                     |
| Test Users         | 7 Internal Users       |
| Guest Users        | 2 External Invitations |
| Identity Platform  | Microsoft Entra ID     |

---

## Lab Scope

This lab focuses on the implementation of identity governance concepts commonly used in enterprise environments:

* User lifecycle management
* Organizational structure
* Group management
* Dynamic membership
* Automated license assignment
* Delegated administration
* Administrative boundaries

---

# Phase 1 – Organizational Structure

## User Provisioning

Seven users were created using CSV import.

### Why CSV Import?

The CSV approach allows multiple user attributes to be populated simultaneously, including:

* Display Name
* Department
* Job Title
* User Principal Name

Compared to manual creation, CSV import significantly reduces administrative effort and improves consistency.

### Guest User Testing

Two guest invitations were sent:

* Gmail account
* Hotmail account

### Observation

The Hotmail invitation was received immediately.

The Gmail invitation was delivered approximately two hours later, indicating a possible delay in invitation processing.

---

# Phase 2 – Security Groups

## Design Approach

A shared ownership model was implemented.

Each security group contains:

* One business owner
* One IT owner

### Benefits

* Reduced operational dependency on IT
* Improved continuity during absences
* Distributed administrative responsibility

## Security Groups Created

* SG-TI
* SG-Financeiro
* SG-RH
* SG-Diretoria

---

# Phase 3 – Microsoft 365 Groups

## Microsoft 365 Groups Created

* M365-TI
* M365-Financeiro
* M365-RH

### Resources Automatically Provisioned

When a Microsoft 365 Group is created, Microsoft automatically generates:

* Exchange Online Mailbox
* SharePoint Online Site
* Collaboration Resources

### Key Difference

| Security Group                 | Microsoft 365 Group     |
| ------------------------------ | ----------------------- |
| Security and access management | Collaboration workloads |
| Conditional Access targeting   | Teams integration       |
| Permission assignment          | SharePoint site         |
| Policy assignment              | Shared mailbox          |

---

# Phase 4 – Dynamic Groups

## Objective

Automate membership management based on user attributes.

### Attribute Updates

The following department values were standardized:

| User    | Previous Value | New Value |
| ------- | -------------- | --------- |
| Rayssa  | Financeiro     | Finance   |
| Allan   | RH             | HR        |
| Gustavo | TI             | IT        |

### Dynamic Group Created

**DG-Finance**

Rule:

```text
user.department -eq "Finance"
```

### Validation

A new Finance user was created and automatically added to the group.

Users whose department attribute matched the rule were added without manual intervention.

---

# Phase 5 – Group-Based Licensing

## Objective

Automate Microsoft 365 license assignment.

### Security Group Created

* SG-Licenciamento-E5

### Configuration

Microsoft 365 E5 licenses were assigned directly to the security group.

### Validation Tests

#### Test 1

Removed a user from the group.

Result:

* License removed automatically.

#### Test 2

Added the user back to the group.

Result:

* License restored automatically.

### Benefits

* Reduced manual effort
* Improved scalability
* Consistent license assignment
* Lower risk of human error

### Future Improvement

Use Dynamic Groups together with Group-Based Licensing to fully automate license assignment based on department or role.

---

# Phase 6 – RBAC

## Principle of Least Privilege

The Principle of Least Privilege states that users should receive only the permissions necessary to perform their responsibilities.

### Best Practices

* Assign specialized administrator roles whenever possible.
* Minimize the number of Global Administrators.
* Separate responsibilities by workload.

Examples:

* Exchange Administrator
* SharePoint Administrator
* Teams Administrator

### Security Benefits

* Reduced attack surface
* Improved accountability
* Lower risk of privilege misuse

---

# Phase 7 – Administrative Units

## Administrative Units Created

* AU-Brazil
* AU-Europe

### User Segmentation

#### AU-Brazil

* Finance Department
* Human Resources Department

#### AU-Europe

* Executive Management

### Delegated Administration

The User Administrator role was delegated within Administrative Units.

### Observations

Administrative Unit membership did not update immediately after configuration.

Changes were eventually reflected after replication completed.

### Important Finding

Users delegated as User Administrators can only manage users directly assigned to the Administrative Unit.

Adding Security Groups to the Administrative Unit did not provide the expected delegated administration experience.

### Additional Finding

Delegated User Administrators:

* Can access Microsoft Entra ID
* Cannot access the Microsoft 365 Admin Center
* Can view all users
* Can edit only users inside their delegated Administrative Units

---

# Key Learnings

* Group-Based Licensing is one of the most valuable automation features available in Microsoft Entra ID.
* Dynamic Groups significantly reduce administrative overhead.
* Security Groups and Microsoft 365 Groups serve different purposes and should not be treated interchangeably.
* Administrative Units are useful for delegated administration but require careful understanding of their scope limitations.
* Replication delays should always be considered when validating identity-related changes.

---

## Related Notes

* Questions and Research
