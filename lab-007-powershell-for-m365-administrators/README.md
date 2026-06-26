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

<img width="945" height="167" alt="002-graph" src="https://github.com/user-attachments/assets/8860cfa0-3fcc-412e-b3ac-665ce692895f" />

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

<img width="469" height="526" alt="006-graph" src="https://github.com/user-attachments/assets/c26acf8c-f651-4b0f-8510-27c02984cfd5" />
<img width="453" height="593" alt="007-graph" src="https://github.com/user-attachments/assets/e375187e-b234-4d64-821e-cdece72e1dc3" />
<img width="1129" height="188" alt="008-graph" src="https://github.com/user-attachments/assets/dbc83c8c-2ce6-4e6a-81fb-da456f214588" />


### Validation

Confirm the current connection.

```powershell
Get-MgContext
```

<img width="606" height="343" alt="009-mg-command" src="https://github.com/user-attachments/assets/79ce5fa8-87e7-4e1c-b794-14669a8eb33b" />

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

<img width="1879" height="850" alt="012-help" src="https://github.com/user-attachments/assets/bb2f9082-b878-464d-b298-954ad0fc346a" />

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

<img width="1278" height="97" alt="015-get-command" src="https://github.com/user-attachments/assets/d89b2ff8-2ba3-47c7-a4bf-d9cf698c6efc" />


## List Microsoft 365 Groups

```powershell
Get-MgGroup
```

<img width="1644" height="354" alt="016-get-command" src="https://github.com/user-attachments/assets/c3ac94c4-e5a0-4865-a286-c5dfe7f5bc15" />


---

# Phase 7 – Filter and Sort Data

## Select specific properties

```powershell
Get-MgUser |
Select-Object DisplayName, UserPrincipalName
```

<img width="876" height="275" alt="017-get-command" src="https://github.com/user-attachments/assets/cdca42e3-ee55-4284-875a-1bb53fdd6373" />


## Sort users alphabetically

```powershell
Get-MgUser |
Sort-Object DisplayName
```

<img width="1566" height="275" alt="018-get-command" src="https://github.com/user-attachments/assets/04ccc575-55de-4b04-81b3-01e68be65dfd" />


---

# Phase 8 – Export Reports

## Export users to CSV

```powershell
Get-MgUser |
Select-Object DisplayName, UserPrincipalName |
Export-Csv -Path "Users.csv" -NoTypeInformation
```

<img width="659" height="303" alt="019-get-command" src="https://github.com/user-attachments/assets/b88a4917-ab02-46be-b014-2444f0a4643c" />


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

<img width="803" height="56" alt="020-variables" src="https://github.com/user-attachments/assets/dfa458c8-7e71-4a49-be29-386dbaf0e92c" />


Variables make it easier to reuse objects throughout a script.

---

# Phase 11 – Loops

Example:

```powershell
foreach ($user in Get-MgUser) {

    $user.DisplayName

}
```

<img width="467" height="294" alt="021-loops" src="https://github.com/user-attachments/assets/446caeb5-b6b9-4638-955e-9d5ed7c7b433" />


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

```powershell
# =========================
# 1. Conectar no Microsoft Graph
# =========================
Connect-MgGraph -Scopes "User.Read.All","Mail.Send"

# =========================
# 2. Buscar usuários ativos (CORRIGIDO)
# =========================
$users = Get-MgUser -All -Property DisplayName,UserPrincipalName,AccountEnabled |
Where-Object { $_.AccountEnabled -eq $true } |
Select-Object DisplayName, UserPrincipalName, AccountEnabled

# (DEBUG opcional - para confirmar que trouxe dados)
$users.Count

# =========================
# 3. Gerar CSV
# =========================
$path = "$env:USERPROFILE\Desktop\Usuarios_Ativos.csv"
$users | Export-Csv -Path $path -NoTypeInformation -Encoding UTF8

# =========================
# 4. Converter CSV para anexo (Base64)
# =========================
$fileBytes = [System.IO.File]::ReadAllBytes($path)
$fileBase64 = [Convert]::ToBase64String($fileBytes)

# =========================
# 5. Criar corpo do e-mail
# =========================
$emailBody = "Relatório de usuários ativos gerado em $(Get-Date)."

# =========================
# 6. Enviar e-mail com anexo
# =========================
Send-MgUserMail -UserId "admin@m365labsforstudying.onmicrosoft.com" -Message @{
    Subject = "Relatório de Usuários Ativos"
    Body = @{
        ContentType = "Text"
        Content = $emailBody
    }
    ToRecipients = @(
        @{
            EmailAddress = @{
                Address = "admin@m365labsforstudying.onmicrosoft.com"
            }
        }
    )
    Attachments = @(
        @{
            "@odata.type" = "#microsoft.graph.fileAttachment"
            Name = "Usuarios_Ativos.csv"
            ContentType = "text/csv"
            ContentBytes = $fileBase64
        }
    )
}
```

### Outcome

<img width="1279" height="314" alt="022-automation" src="https://github.com/user-attachments/assets/04e949dc-5648-462d-bdd6-422d2192e293" />
<img width="1070" height="470" alt="023-automation" src="https://github.com/user-attachments/assets/99cc92fe-045e-4ec6-9fe1-12b534624b52" />

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

* [Issues Encountered](notes/issues-encountered.md)
