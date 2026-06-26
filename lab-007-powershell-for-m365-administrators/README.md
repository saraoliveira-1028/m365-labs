# LAB 007 – PowerShell for Microsoft 365 Administrators

## Objective

Learn the fundamentals of PowerShell administration for Microsoft 365 by exploring the Microsoft Graph PowerShell SDK, object manipulation, data filtering, reporting, and automation.

---

## Environment

| Item               | Details                        |
| ------------------ | ------------------------------ |
| Platform           | Microsoft Graph PowerShell SDK |
| Tenant             | Microsoft 365 E5 Trial         |
| Authentication     | Delegated Permissions          |
| PowerShell Version | PowerShell 7                   |

---

## Lab Scope

This lab covers:

* Installing PowerShell modules
* Connecting to Microsoft Graph
* Discovering cmdlets
* Using PowerShell Help
* Understanding PowerShell objects
* Querying Microsoft Graph
* Filtering and sorting data
* Exporting reports
* Using the pipeline
* Variables and loops
* Basic automation

---

# Phase 1 – Install the Microsoft Graph PowerShell SDK

## Objective

Install the Microsoft Graph PowerShell module required to administer Microsoft 365.

```powershell
Install-Module Microsoft.Graph
```

### Validation

Verify that the module has been successfully installed.

```powershell
Get-Module Microsoft.Graph -ListAvailable
```

---

# Phase 2 – Connect to Microsoft Graph

## Objective

Authenticate to Microsoft Graph using delegated permissions.

```powershell
Connect-MgGraph
```

### Validation

Confirm the current connection.

```powershell
Get-MgContext
```

---

# Phase 3 – Discover Available Commands

## Objective

Explore available cmdlets within the installed modules.

### List all commands

```powershell
Get-Command
```

### Search for Microsoft Graph user cmdlets

```powershell
Get-Command *MgUser*
```

### Search for mailbox-related cmdlets

```powershell
Get-Command *Mailbox*
```

---

# Phase 4 – Use PowerShell Help

## Objective

Learn how to access documentation directly from PowerShell.

### Display local help

```powershell
Get-Help Get-MgUser
```

### Open Microsoft documentation

```powershell
Get-Help Get-MgUser -Online
```

### Observation

Using the **-Online** parameter opens the official Microsoft Learn documentation for the selected cmdlet.

---

# Phase 5 – Explore Objects

## Objective

Understand how Microsoft Graph returns PowerShell objects.

### Retrieve users

```powershell
Get-MgUser
```

### Inspect object members

```powershell
Get-MgUser | Get-Member
```

### Why this matters

Understanding object properties and methods is essential for building scripts and extracting meaningful information.

---

# Phase 6 – Query Microsoft Graph

## List all users

```powershell
Get-MgUser
```

## Retrieve a specific user

```powershell
Get-MgUser -UserId user@company.com
```

## List Microsoft 365 Groups

```powershell
Get-MgGroup
```

---

# Phase 7 – Filter and Sort Data

## Select specific properties

```powershell
Get-MgUser |
Select-Object DisplayName, UserPrincipalName
```

## Sort users alphabetically

```powershell
Get-MgUser |
Sort-Object DisplayName
```

---

# Phase 8 – Export Reports

## Export users to CSV

```powershell
Get-MgUser |
Select-Object DisplayName, UserPrincipalName |
Export-Csv -Path "Users.csv" -NoTypeInformation
```

### Observation

The exported file is saved in the current PowerShell working directory.

To identify the current location:

```powershell
Get-Location
```

---

# Phase 9 – PowerShell Pipeline

## Objective

Understand how the pipeline passes objects from one command to another.

Example:

```powershell
Get-MgUser |
Where-Object {$_.AccountEnabled -eq $true} |
Sort-Object DisplayName |
Select-Object DisplayName
```

### Benefits

* Cleaner scripts
* Less reliance on variables
* Better readability
* Improved performance

---

# Phase 10 – Variables

```powershell
$user = Get-MgUser -UserId user@company.com

$user.DisplayName
```

Variables make it easier to reuse objects throughout a script.

---

# Phase 11 – Loops

Example:

```powershell
foreach ($user in Get-MgUser) {

    $user.DisplayName

}
```

Loops simplify repetitive administrative tasks across multiple objects.

---

# Phase 12 – Basic Automation

## Objective

Automate the generation of an active users report.

### Workflow

1. Connect to Microsoft Graph.
2. Retrieve active users.
3. Export the report to CSV.
4. Convert the file to Base64.
5. Send the report via email using Microsoft Graph.

### Outcome

A fully automated report containing all active Microsoft 365 users was generated and delivered by email.

---

# Key Learnings

* Microsoft Graph PowerShell is object-oriented.
* PowerShell Help should be the first reference when learning a cmdlet.
* Understanding object properties is essential for automation.
* The pipeline is one of the most powerful features of PowerShell.
* Reports can be generated and delivered automatically using Microsoft Graph.

---

## Related Notes

* Issues Encountered
