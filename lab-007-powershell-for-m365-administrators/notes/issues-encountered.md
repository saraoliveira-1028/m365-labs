# Issues Encountered

## Issue 1 – Insufficient Privileges When Running Get-MgUser

### Description

After successfully connecting to Microsoft Graph, the following command returned an authorization error:

```powershell
Get-MgUser
```

### Error

```text
Authorization_RequestDenied

Status: 403 (Forbidden)
```

### Initial Analysis

Authentication completed successfully, indicating that the issue was not related to sign-in.

The error suggested that the access token lacked the required Microsoft Graph delegated permissions.

### Investigation

The default `Connect-MgGraph` session grants only a limited set of delegated permissions.

The `Get-MgUser` cmdlet requires permissions such as:

* User.Read.All
* Directory.Read.All

### Resolution

Disconnected the existing session:

```powershell
Disconnect-MgGraph
```

Reconnected while explicitly requesting the required scopes:

```powershell
Connect-MgGraph -Scopes "User.Read.All","Directory.Read.All"
```

After reconnecting, the command executed successfully.

<img width="973" height="323" alt="013-get-command" src="https://github.com/user-attachments/assets/12849d0d-41be-470a-aba8-277d939284f2" />


### Lessons Learned

* Successfully authenticating to Microsoft Graph does not guarantee permission to execute all cmdlets.
* Always verify the required permission scopes for the cmdlets being used.
* A 403 Forbidden error often indicates insufficient delegated permissions rather than an authentication failure.

