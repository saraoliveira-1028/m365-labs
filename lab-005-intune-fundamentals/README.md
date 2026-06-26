# Lab 005 - Intune Fundamentals

---

## Objective & Scenario

The objective of this lab is to understand the fundamental concepts of **Microsoft Intune**, including **Endpoint Management**, **Compliance**, **Configuration Profiles**, **Device Enrollment**, and **Application Management**. The focus is to build a solid foundational knowledge of how Intune integrates into the Microsoft 365 ecosystem.

### Scenario
You are an IT Administrator for a company that wants to:
* Manage corporate laptops efficiently.
* Apply standardized security configurations.
* Distribute required corporate applications.
* Control and secure access to corporate resources.

---

## Prerequisites & Key Concepts

### What is Microsoft Intune?
> **Note:** 
> Microsoft Intune is a **Unified Endpoint Management (UEM)** platform used to manage devices, applications, and security configurations across multiple operating systems, including Windows, macOS, Android, and iOS.

---

## Heading Hierarchy & Lab Phases

### 1. Initial Portal Exploration
Access the **Microsoft Intune admin center** and navigate through the core components to understand the product structure:

* **Devices:** Contains details about all enrolled devices in the tenant, configuration options, management tools, and update settings. Used to administer and configure devices.
* **Apps:** Used to configure, provision, and distribute applications across different platforms.
* **Endpoint Security:** Manages security configurations, threat policies, and helps maintain and track the security posture of managed devices.
* **Reports:** Generates various reporting types for device investigation, compliance tracking, and environment auditing.
* **Users:** Provides access to organizational user profiles and management.
* **Groups:** Displays security groups used to assign policies, applications, and expiration configurations.
* **Tenant Administration:** Contains tenant-level configurations, licensing statuses, and specialized features like Microsoft Tunnel Gateway and PIM.

### 2. Device Enrollment Models
Device enrollment is the process of registering a device into a Mobile Device Management (MDM) solution like Intune. 

#### Device Registration Matrix
| Model | Description |
| :--- | :--- |
| **Microsoft Entra Joined** | Corporate-owned device. Joined cloud-only directly to Microsoft Entra ID and managed via Intune. |
| **Microsoft Entra Hybrid Joined** | Hybrid environments. Device is joined to both on-premises Active Directory and Microsoft Entra ID. |
| **Microsoft Entra Registered** | **BYOD (Bring Your Own Device)** / Personal devices. The administrator controls only organizational apps and data rather than full device management. |

> **Important:** 
> **Windows Autopilot** is a provisioning mechanism, not an enrollment method itself. It acts as an orchestrator that enables a zero-touch deployment workflow where the manufacturer ships the machine directly to the user. Upon first boot and corporate login, the machine automatically executes an Entra Join, enrolls in Intune, and receives policies.

### 3. Creating Management Groups
Create the following security groups within the portal, recognizing that almost all Intune targets use group-based assignments:
* `SG-Intune-Devices`
* `SG-Intune-Users`

### 4. Compliance Policies
Navigate to **Devices** > **Compliance** (or **Compliance policies**) and click **Create Policy** for **Windows 10 and later**.

#### Settings to Configure:
* **Name:** `Windows Compliance Policy`
* **System Security > Password:** 
  * Require a password to unlock mobile devices: **Require**
  * Simple passwords: **Block**
  * Password type: **Alphanumeric**
  * Password complexity: **Require digits, lowercase letters, uppercase letters, and special characters**
  * Minimum password length: `8`
  * Password expiration (days): `41`
  * Number of previous passwords to prevent reuse: `5`
* **Encryption:** Require encryption of data storage on device (**Require** / BitLocker)
* **Device Security:** Firewall (**Require**)

<img width="1111" height="485" alt="001-policies" src="https://github.com/user-attachments/assets/53706143-ad6b-4e31-8278-d28f0021de41" />
<img width="804" height="813" alt="002-policies" src="https://github.com/user-attachments/assets/96663c68-cf81-45ee-97dc-b43bb76791a9" />

> **Compliance vs. Configuration:**
> * **Compliance Policies:** Used to *verify* if a device meets specific organizational security guidelines. Devices failing these checks are marked non-compliant, which can trigger Conditional Access blocks.
> * **Configuration Profiles:** Used to *actively apply* and enforce settings onto the device.

### 5. Configuration Profiles
Navigate to **Devices** > **Configuration profiles** and create a profile for **Windows 10 and later** using the **Settings catalog**.

#### Settings to Configure:
* **Name:** `Windows Configuration Policy`
* **Administrative Templates > Control Panel > Personalization:**
  * Force specific screen saver (User): **Enabled**
* **System > Removable Storage Access:**
  * All Removable Storage classes: Deny all access: **Enabled**
* **Microsoft Edge:**
  * Allows users to edit favorites: **Enabled**
  * Allow or deny screen capture: **Enabled**

<img width="1067" height="451" alt="003-configuration-profiles" src="https://github.com/user-attachments/assets/eabe07fd-5c01-4487-96e9-6c421129b6ea" />
<img width="815" height="806" alt="004-configuration-profiles" src="https://github.com/user-attachments/assets/fdcfa2d2-ff23-4776-926d-6153c4958e05" />

### 6. Endpoint Security Policies
Navigate to the **Endpoint security** blade to explore specialized security baselines.

> **Key Takeaway:**
> While both **Configuration Profiles** and **Endpoint Security Policies** apply settings using the same underlying Configuration Service Providers (CSPs) on Windows, Configuration Profiles focus on general operating and operational environment standardization, whereas Endpoint Security Policies focus purely on hardening (Antivirus, Firewall, Attack Surface Reduction, and Disk Encryption).

### 7. Application Distribution
Navigate to **Apps** > **Windows** > **Windows apps** and choose **Add**. Select **Microsoft 365 Apps** for **Windows 10 and later**.

#### App Suite Information:
* **Suite Name:** `Microsoft 365 Apps for Windows 10 and later`
* **Description:** `Microsoft 365 Apps for Windows 10 and later`
* **Category:** `Productivity`
* **Show this as a featured app in the Company Portal:** `Yes`

#### App Suite Settings Configuration:
* **Configuration format:** `Configuration designer`
* **Architecture:** `64-bit`
* **Default file format:** `Office Open XML Format`
* **Update channel:** `Current Channel`
* **Remove other versions:** `Yes`
* **Version to install:** `Latest`
* **Shared computer activation:** `No`
* **Accept the Microsoft Software License Terms on behalf of users:** `Yes`

<img width="849" height="779" alt="005-apps" src="https://github.com/user-attachments/assets/7780deb2-302a-4af3-b91e-c8e808733ddf" />
<img width="841" height="808" alt="006-apps" src="https://github.com/user-attachments/assets/4c299413-3e88-4718-89fb-f80f426f0bc3" />
<img width="1129" height="680" alt="007-apps" src="https://github.com/user-attachments/assets/bed5c645-3d1e-4266-b4ad-34b0340ba34c" />

#### Assignments:
* Under **Required**, assign to: `SG-Financeiro`

> **Note:** 
> * **Required Assignments:** Automatically force-install applications on targeted user or device groups.
> * **Available Assignments:** Make applications optional for users to self-install via the Company Portal app (applicable mainly to registered/BYOD devices).

### 8. Integrating Conditional Access with Compliance
To bridge identity security with device status, navigate to **Microsoft Entra ID** > **Protection** > **Conditional Access** and create a new policy:

* **Name:** `[Compliance] Windows 10 Devices`
* **Target Resources:** Select **Cloud apps** > Include **Office 365**.
* **Access Controls > Grant:** Select **Grant access** and check **Require device to be marked as compliant**.

<img width="655" height="856" alt="008-conditional-access" src="https://github.com/user-attachments/assets/a5f203b7-677b-4c13-9c74-ca7362cca138" />
<img width="308" height="366" alt="009-conditional-access" src="https://github.com/user-attachments/assets/bda339db-425c-44ca-b3f5-31e821b5adde" />

---

## Conceptual Overview: Windows Autopilot

Organization registers device using Hardware Hash
│
▼

Hardware Hash is associated with the Microsoft Tenant
│
▼

A Deployment Profile is assigned to the device
│
▼

Manufacturer (OEM) ships the device directly to the user
│
▼

User powers on the device for the first time
│
▼

Device identifies that it belongs to the organization
│
▼

User signs in using corporate credentials
│
▼

Device performs an Entra Join
│
▼

Device enrolls automatically into Microsoft Intune
│
▼

Apps, configurations, and compliance policies are applied
│
▼

Device is ready for corporate use

### Deployment Modes
* **User-driven mode:** Designed for traditional workflows where the user walks through the customized out-of-box experience (OOBE).
* **Self-deploying mode:** Designed for kiosks, digital signage, or shared devices where no user interaction or user credentials are required.
* **Pre-provisioning (White Glove):** Allows IT professionals or OEMs to pre-stage applications and configurations on the device before delivery, minimizing user waiting times during final deployment.

---

## 9. Environment Reporting
Navigate to **Reports** to audit configurations. Administrators can monitor environmental compliance statuses, Windows updates, software health, and application installation progress across the organization by filtering data using dropdown menus and selecting **Generate report**.

---
## Additional Notes

- [Supplementary Research](notes/supplementary-research.md)
