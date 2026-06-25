# Lab 004 - SharePoint Online + Teams Governance
---

## Objective & Scenario
The objective of this lab is to simulate the deployment of a corporate collaborative platform for a fictitious company named **Contoso Study**. The environment structures the collaborative architecture across four key departments: **IT**, **HR**, **Finance**, and **Directory**. 

The implementation covers:
* The core relationship between Microsoft Teams, SharePoint Online, and Microsoft 365 Groups.
* Internal and external sharing governance.
* Lifecycle management, versioning, data recovery, and creation restrictions via PowerShell.

---

## 1. Architectural Architecture: Teams, SharePoint, and M365 Groups

Before deploying collaborative resources, it is fundamental to document the relationship and inherited permissions between components. Whenever a new Team is created via Microsoft Teams, the following resources are automatically provisioned under a unified identity and lifecycle:

                   ┌─────────────────┐
                   │ Microsoft Teams │
                   └────────┬────────┘
                            │
     ┌──────────────────────┼──────────────────────┐
     ▼                      ▼                      ▼
    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
    │   M365 Group    │    │ SharePoint Site │    │ Shared Mailbox  │
    └────────┬────────┘    └────────┬────────┘    └─────────────────┘
             │                      │
             ├─► Planner            └─► Document Library
             │
             └─► OneNote

* **Microsoft 365 Group:** Manages identity and membership.
* **SharePoint Site:** Provisions a dedicated document library matching the Team's name.
* **Shared Mailbox:** Created automatically but hidden from the standard Exchange Online admin center shared mailbox list and end-user folder views; accessible directly via the **Groups** section in Outlook.
* **Planner & OneNote:** Standard productivity instances tied to the M365 Group identity.

> **Important:** Administrators must distinguish this integrated creation flow from independent group provision pathways within the Microsoft 365 admin center to prevent resource fragmentation and unnecessary over-provisioning.

---

## 2. Deploying Departmental Teams

Three departmental teams were deployed with their privacy configuration strictly set to **Private**.

### 2.1 Team Structure & Membership
The initial provisioning mapping is defined below:

| Team Name | Owners | Members | Privacy |
| :--- | :--- | :--- | :--- |
| **Team RH** | `Cristiane` | `Alana`, `Cristiane` | Private |
| **Team Financeiro** | `Isadora`  | `Rayssa`, `Isadora`  | Private  |
| **Team TI**  | `Diogo` | `Gustavo`, `Diogo` | Private  |

### 2.2 Provisioning Validation
After deploying the teams, the environment was verified with the following results:
* **Microsoft 365 Group:** Successfully provisioned.
* **Teams Instance:** Fully active and functional.
* **SharePoint Site:** Correctly built with matching permissions.
* **Shared Mailbox Traffic:** Confirmed. Group emails can be monitored within Outlook under **Groups**, verifying receipt of the system group welcome emails.

<img width="905" height="546" alt="001-teams" src="https://github.com/user-attachments/assets/ed38e249-58d1-459e-8d4e-e944f8acc929" />
<img width="618" height="385" alt="002-teams" src="https://github.com/user-attachments/assets/be2b200c-f64c-4fcc-9d48-fa7b7d20c0c1" />

---

## 3. Channel Architecture & Governance

Channels were provisioned directly via the Microsoft 365 Groups admin interface using a **Standard** privacy level, meaning all team members inherit access.

### 3.1 Departmental Channel Mapping
* **Team RH:** `General` (Default), `Recrutamento`, `Folha de Pagamento` 
* **Team Financeiro:** `General` (Default), `Contas a Pagar`, `Contas a Receber` 
* **Team TI:** `General` (Default), `Projetos`, `Incidentes` 

### 3.2 Channel Type Classification
1. **Standard Channels:** Available to all members; files are stored within the core team SharePoint document library.
2. **Private Channels:** Used to restrict access to a granular sub-segment of team members. 
   > **Note:** SharePoint automatically provisions a completely separate, isolated site collection for each private channel to safeguard file access.
3. **Shared Channels (Teams Connect):** Designed to facilitate seamless collaboration with internal or external users without forcing them to join the parent team. Like private channels, a shared channel provisions its own dedicated SharePoint site collection with independent permissions.

---

## 4. SharePoint Permissions & Access Validation

Testing was performed on the **Team Financeiro** SharePoint site collection to analyze permission boundaries.

### 4.1 Global Administrator Access Boundaries
> **Critical:** A Global Administrator account does not automatically gain read/write access to individual SharePoint site collections unless explicitly granted site permissions or added as a site collection administrator.

### 4.2 Standard Group Role Mappings
Permissions inside the SharePoint site reflect three distinct default groups:
* **Owners (`Isadora`):** Full Control. Can perform all actions on pages/documents and alter site permission models.
* **Members (`Isadora`, `Rayssa`):** Edit permissions. Can modify pages and documents but cannot change site-wide access permissions.
* **Visitors (Empty):** Read-only permissions.

### 4.3 Validation Matrix

* **Test Case 1 (`Rayssa` - Member):** Logged into the site collection. Verified capability to view all pages, read/write documents, and edit site pages. Verified that she cannot add new members to the site. Access behavior is **Normal/Expected**.
* **Test Case 2 (`rh01` - Non-member):** Attempted access to the Finance site. Access was blocked completely, prompting an **Access Denied / Request Access** interface.

<img width="930" height="340" alt="003-sharepoint" src="https://github.com/user-attachments/assets/6b35160c-190a-4f06-96ed-689784354268" />

---

## 5. External Sharing & B2B Collaboration Governance

### 5.1 Guest Access Lifecycle
To test Business-to-Business (B2B) collaboration, **Guest Access** was enabled at the Teams organizational level. An external account (`sara.oliveira.santos.123@gmail.com`) was invited directly into **Team TI**.

1. The external guest receives a redemption link via email.
2. Upon redemption, a verification code is routed to the guest's email for secure authentication.
3. The user must accept the Entra ID consent prompt, allowing the host tenant (`StudyOnly / m365labsforstudying.onmicrosoft.com`) to receive profile attributes and register activity log states.

<img width="465" height="570" alt="004-external-sharing" src="https://github.com/user-attachments/assets/a7b562ca-933d-4478-8481-d61fcc9ca5bf" />


### 5.2 Guest Permissions Matrix
Once authenticated, the guest user can interact within Teams and SharePoint without requiring a paid license. Within the SharePoint site, the following boundaries apply:
* Can access site pages and documents? **Yes**.
* Can edit documents? **Yes**.
* Can share documents? **Restricted**. The guest can only share files with users who are already existing members of the team. External sharing or sharing with non-team internal corporate members is blocked.

---

## 6. Granular Sharing Policies & Content Lifecycle

### 6.1 Site-Level External Sharing Configuration
Sharing boundaries were evaluated across two different site configurations:

* **Scenario 1 (Finance Site):** External file sharing is strictly prohibited. The sharing capability is limited to **Only People in Your Organization**.
* **Scenario 2 (IT Site):** External file sharing is relaxed, set to allow **New and Existing Guests**.

### 6.2 File-Level Sharing Validation
A spreadsheet titled `Financeiro.xlsx` was created inside the Finance Site to validate these rules:
* **Internal Delegation:** Shared with `Allana` (an HR user) using direct item-level edit permissions. The transaction completed successfully; `Allana` gains access exclusively to this spreadsheet without inheriting rights to any other asset within the Finance site collection.
* **External Delegation:** Attempted to share the file with the external guest user. The application immediately triggered a validation error blocking the request, enforcing the site-level security policy.

<img width="502" height="418" alt="005-file-sharing" src="https://github.com/user-attachments/assets/53ad45f5-7940-4a49-a6f4-7e3807c23798" />

### 6.3 Version Control and Data Recovery
SharePoint Online native data lifecycle management features were validated:

* **Version History:** Changes made across multiple edits (`Version 1`, `Version 2`, `Version 3`) are logged sequentially. Administrators or users can utilize **View** to inspect a specific historical snapshot before selecting **Restore**. When a past version is restored, it does not overwrite history; instead, it commits as the newest major version in the line (e.g., restoring `Version 2.0` after a `Version 4.0` creates a `Version 5.0`).
* **Recycle Bin:** Deleted files can be recovered instantly from the site **Recycle Bin**. Restoring an item returns the file to its exact original location, preserving its full version history and all item-level explicit sharing permissions.

<img width="1029" height="290" alt="006-file-versioning" src="https://github.com/user-attachments/assets/7086cdc2-6fca-4ded-8964-fbec39cc8146" />

---

## 7. Advanced Teams Governance & Creation Restrictions

By default, the global teams policy allows wide-scale creation of private and shared channels by users. In large enterprise environments, it is critical to restrict Microsoft 365 Group and Team creation to prevent "sprawl" (uncontrolled proliferation of teams and underlying SharePoint sites).

Because group provisioning lacks an automated checkbox toggle in the standard administration center UI, this restriction must be enacted programmatically via PowerShell. The script below assigns group creation rights exclusively to a designated Security Group (`SG-Team-Creators`) while disabling it globally for all other users:

```powershell
# Define the Object ID of the Security Group authorized to create M365 Groups
$GroupId = "ObjectID-do-SG-Team-Creators" 

# Retrieve the existing Unified Group directory setting template
$Settings = Get-AzureADDirectorySetting | Where-Object {$_.DisplayName -eq "Group.Unified"} 

# Turn off global group creation capabilities
$Settings["EnableGroupCreation"] = "False" 

# Assign permissions exclusively to the approved Security Group ID
$Settings["GroupCreationAllowedGroupId"] = $GroupId 

# Apply the modified settings instance back to the Entra ID directory
Set-AzureADDirectorySetting -Id $Settings.Id -DirectorySetting $Settings
```
---
