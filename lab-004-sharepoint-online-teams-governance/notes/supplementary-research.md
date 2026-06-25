# Lab - Microsoft 365 Advanced Governance Notes
---

## Objective
This document outlines the governance research and architectural analysis of Microsoft Teams, SharePoint Online, and Microsoft 365 Groups integration. It focuses on the behavioral differences between collaboration mechanisms (Guest Access vs. Teams Connect) and the segregation of permission layers between Teams and their underlying SharePoint sites.

---

## 1. Collaboration Models: Guest Access vs. Teams Connect (Shared Channels)

During the Microsoft Teams governance study, the **Shared Channels (Teams Connect)** feature was evaluated. This feature enables seamless collaboration between internal and external users without requiring them to be added as Guest users within the host tenant.


### 1.1 Structural Comparison
The table below details the operational differences between the traditional B2B collaboration model and the modern Shared Channels approach:

| Feature / Dynamic | Guest Access | Teams Connect (Shared Channels) |
| :--- | :--- | :--- |
| **User Identity** | User account is provisioned as a Guest in the host tenant. | User remains entirely within their home/origin tenant. |
| **Tenant Switching** | Users must manually switch organizations within the Teams UI. | No tenant switching required; channels appear natively. |
| **Access Scope** | Provides access to the entire Team structure. | Restricted strictly to the specific shared channel. |
| **Model Type** | Traditional B2B collaboration model. | Modern collaboration model based on Shared Channels. |

### 1.2 Common Use Cases
Shared Channels and Teams Connect are highly applicable in the following scenarios:
* **Cross-departmental projects** spanning internal business units.
* **Inter-company collaboration** with verified partner organizations.
* **Mergers and acquisitions (M&A)** onboarding phases.
* **Global distributed teams** requiring friction-free communication.

> **Note:** Although Teams Connect has been available for several years, its adoption is predominantly seen in enterprise and multinational environments. In smaller-scale infrastructures, the classic **Guest Access** model remains widely used.

---

## 2. Permission Architectures in Team-Associated SharePoint Sites

An architectural review of the SharePoint Online sites automatically provisioned by Microsoft Teams revealed complex, multi-layered permission categories. While some terminology overlaps, these categories reside in entirely different administrative planes within the Microsoft 365 ecosystem.

### 2.1 The Core Ecosystem Relationship
When a new Microsoft Team is provisioned, the system automatically spins up an interconnected web of Microsoft 365 components:

[Microsoft Team] ──> [Microsoft 365 Group] ──> [SharePoint Site]
──> [Shared Mailbox]
──> [Planner]
──> [OneNote]

*Permissions established at the Teams level are inherited from the underlying Microsoft 365 Group and synchronized directly down to SharePoint.*

### 2.2 Microsoft 365 Group Layer (Teams Owners & Members)
These roles represent the identity layer managed at the Microsoft 365 Group level:

* **Owners:** Act as the administrators of both the Team interface and the underlying group.
  * *Responsibilities:* Managing group membership, altering Team settings, creating/deleting channels, and establishing administrative permissions.
* **Members:** Standard collaborative users within the Team.
  * *Capabilities:* Chatting in channels, uploading/sharing files, and participating in scheduled meetings.

### 2.3 SharePoint Native Layer (Site Owners, Members, and Visitors)
These roles live natively inside the SharePoint security model. Upon Team creation, SharePoint automatically provisions local security groups (e.g., `Finance Site Owners`, `Finance Site Members`, `Finance Site Visitors`) to handle discrete file and document library permissions.

* **Site Owners (`Full Control`):** Can modify site permissions, provision or delete document libraries, design lists, alter pages, and govern overall site content.
* **Site Members (`Edit`):** Can create, edit, and delete documents, folders, and custom lists.
* **Site Visitors (`Read`):** Restricted entirely to viewing published content and downloading documentation.

---

## 3. Advanced Governance: Site Owners vs. Site Administrators

A critical distinction exists between **Site Owners** and **Site Administrators (Site Collection Administrators)**, despite the naming similarities.

| Feature / Capability | Site Owner | Site Admin (Site Collection Admin) |
| :--- | :--- | :--- |
| **Full Control Permission** | Yes | Yes |
| **Manages Site Permissions** | Yes | Yes |
| **Bypasses/Ignores Local Site Restrictions** | No | Yes |

* **Site Owners:** Their absolute level of access depends entirely on the live permissions explicitly mapped to the site boundaries.
* **Site Admins:** Possess top-tier, un-restrictable clearance across the entire site collection. They do not need to belong to the local Site Owners or Members groups to exercise administrative rights, making this role a critical vector for central IT infrastructure support and data recovery pipelines.

---

## 4. Architectural Loose Coupling and Security Risks

A major takeaway from this study is that while Microsoft Teams and SharePoint Online security mappings are tightly integrated by default, they are **not completely dependent** on one another.

While Team members automatically gain corresponding SharePoint access through the Microsoft 365 Group sync, a SharePoint Administrator can bypass the Team entirely and assign permissions directly inside SharePoint's native groups (`Site Owners/Members/Visitors`).

### 4.1 Drift Scenario Example
Consider a user named `rh01` and a Team named `Finance`:

[User: rh01] ──(Direct Assignment)──> [SharePoint Group: Finance Site Members]

* **Result:** The user `rh01` can successfully access, read, and modify files directly inside the underlying SharePoint site and its document libraries, even though they are **not** a member of the `Finance` Team in the Microsoft Teams application.

### 4.2 Corporate Governance Recommendation
> **Important:** Allowing out-of-band permission assignments directly inside SharePoint creates a compliance drift where the Teams membership no longer reflects actual data access rights. In enterprise topologies, this practice must be heavily restricted, as it complicates access audits, weakens data classification policies, and risks exposing unintended sensitive data pools to organizational AI tools like **Microsoft 365 Copilot**.
