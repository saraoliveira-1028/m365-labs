# Issues Encountered

## Issue 1 – Newly Created User Did Not Appear as Licensed

### Description

After creating the user **Isis** and assigning a license during the provisioning process, the user did not immediately appear in the licensed users list.

### Evidence

<img width="1305" height="468" alt="009-issue-usuario-licenciado" src="https://github.com/user-attachments/assets/43d6c87d-d3dd-4b65-a900-cb2bce6df212" />
<img width="814" height="256" alt="010-issue-usuario-licenciado" src="https://github.com/user-attachments/assets/6a57f3a9-4c86-4de4-9769-5a6b4658855c" />

### Analysis

The license assignment was completed successfully during user creation. However, the Microsoft 365 Admin Center did not immediately reflect the updated state.

### Resolution

Waited a few seconds and refreshed the portal.

The user appeared correctly after the refresh.

### Lessons Learned

Microsoft 365 administrative portals may experience short synchronization delays after user provisioning and license assignment.

---

## Issue 2 – Conditional Access Appeared Disabled

### Description

When attempting to create Conditional Access policies, the feature appeared unavailable.

### Evidence

<img width="1275" height="417" alt="011-acesso-condicional-desabilitado" src="https://github.com/user-attachments/assets/342e14ec-8ccb-4b60-8afa-d1f805d8b087" />

### Analysis

This behavior is common in newly created Microsoft 365 tenants because **Security Defaults** are enabled by default.

<img width="1575" height="180" alt="012-acesso-condicional-desabilitado" src="https://github.com/user-attachments/assets/c267cb23-e2b2-420a-963b-f0c9e9583f63" />

When Security Defaults are enabled, Conditional Access policies cannot be configured.

### Resolution

Navigated to Microsoft Entra ID settings and disabled Security Defaults.

After disabling Security Defaults, Conditional Access policies became available for configuration.

### Lessons Learned

Always verify whether Security Defaults are enabled before troubleshooting Conditional Access availability in new tenants.

---

## Issue 3 – Unable to Create MFA Conditional Access Policy

### Description

While creating a Conditional Access policy to enforce Multi-Factor Authentication (MFA), an error message was displayed.

### Evidence

<img width="373" height="120" alt="013-issue-politica" src="https://github.com/user-attachments/assets/63669e76-7b38-4357-b6f3-675edc7a6481" />

### Initial Hypothesis

Users targeted by the policy might not have the required licensing.

### Troubleshooting Process

#### Step 1 – Verify Licensing

Checked whether affected users had the required Entra licensing assigned.

**Result:**

* Licensing was correctly assigned.
* No licensing issues were identified.

#### Step 2 – Consider Replication Delay

Since the licensing configuration was correct, a propagation delay was suspected.

### Timeline

| Time  | Action                                    |
| ----- | ----------------------------------------- |
| 14:08 | Final validation performed before waiting |
| 17:06 | New test executed                         |

### Resolution

The policy was created successfully during the second test.

The root cause was determined to be configuration replication time within Microsoft Entra ID.

### Lessons Learned

Recently assigned licenses may require time to propagate across Microsoft cloud services before dependent features become available.

When licensing appears correct but a feature remains unavailable, replication delay should be considered before further troubleshooting.
