# Questions and Research

## Question

### In a secure Microsoft 365 tenant, what is the recommended approach for administration and access management?

Should administrators primarily use:

- Administrative Units (AUs)
- Security Groups
- Microsoft 365 Groups

---

## Findings

The current industry-recommended approach is based on:

- Role-Based Access Control (RBAC)
- Security Groups
- Conditional Access Policies

Administrative Units should **not** be used as the primary organizational structure of a Microsoft 365 tenant.

---

## Administrative Units (AUs)

Administrative Units are most useful in large-scale environments, such as:

- Multi-region organizations
- Multi-branch organizations
- Large enterprises with delegated administration
- Organizations with separated IT teams

Their primary purpose is to delegate administrative responsibilities to specific scopes of users, groups, or devices.

For small and medium-sized environments, Administrative Units often introduce unnecessary complexity.

---

## Security Groups

Security Groups are primarily used for:

- Access management
- Permission assignment
- Conditional Access targeting
- Security policy assignment

### Examples

- MFA enforcement groups
- Device compliance groups
- Application access groups

---

## Microsoft 365 Groups

Microsoft 365 Groups are designed for collaboration workloads.

They provide shared resources such as:

- Microsoft Teams
- SharePoint Online
- Outlook Group Mailboxes
- Planner
- OneNote

### Examples

- Finance Team
- Human Resources Team
- Project Collaboration Teams

---

## Conclusion

For most Microsoft 365 environments, the recommended design is:

1. Use **Security Groups** for security and policy management.
2. Use **Microsoft 365 Groups** for collaboration workloads.
3. Use **RBAC** for administrative delegation.
4. Use **Conditional Access** to enforce security controls.
5. Implement **Administrative Units** only when there is a clear need for delegated administration at scale.

This approach provides a simpler, more scalable, and easier-to-manage security model.
