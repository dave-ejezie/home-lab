# KB-006: User Onboarding Procedure

| Field | Value |
|-------|-------|
| **Category** | Identity & Access Management |
| **Priority** | P3 — Standard |
| **Applies To** | helpdesk.lab new starters |
| **Last Updated** | 2026-04-03 |

## Overview

This procedure covers the standard process for onboarding a new employee into the `helpdesk.lab` domain using the automated PowerShell script.

## Prerequisites

- User's First Name, Last Name, and Department
- Information confirmed by HR
- Target OU and Security Groups must exist (e.g., `OU=_STAFF`, `GRP_<Department>`, `GRP_AllStaff`)

## Steps

1. Open PowerShell as Administrator on DC01 or a machine with RSAT installed.
2. Run the onboarding script with the required parameters:
   ```powershell
   .\onboard-user.ps1 -FirstName "John" -LastName "Doe" -Department "IT"
   ```
3. The script will automatically:
   - Generate a `SAMAccountName` and `UPN`
   - Create the user in the corresponding department OU (e.g., `OU=IT,OU=_STAFF,DC=helpdesk,DC=lab`)
   - Assign the user to the department security group (e.g., `GRP_IT`) and `GRP_AllStaff`
   - Set a temporary password

**Important Security Note (Lab vs. Live):**
During lab batch user creation, the script uses a forced hardcoded string password (`Welcome123!`). In a live production environment, this is extremely insecure. Instead, we would generate a complex random password, use Entra ID (Azure AD) Temporary Access Passes (TAP), and communicate credentials securely to the user's personal phone or manager.

## Verification

The script provides color-coded logs in the console:
- **Cyan (INFO):** Action succeeded as expected.
- **Yellow (WARN):** Non-critical failure (e.g., group assignment failed if the group is missing or renamed).
- **Red (ERROR):** Critical failure, account creation may have aborted.
- **Green ([DONE]):** Total success summary.

You can also verify the user's group memberships and target path via Active Directory Users and Computers (ADUC) or by running:
```powershell
Get-ADUser -Identity "jdoe" -Properties MemberOf
```

**Evidence:** See [`AD-DS/Activities/02-User-Creation`](../AD-DS/Activities/02-User-Creation/README.md) for screenshots of all 5 successful onboarding runs and the final verification query.

## Related

- KB-007: User Offboarding Procedure
- 👤 [Activity: Batch User Creation](../AD-DS/Activities/02-User-Creation/README.md)
- 🛡️ [Activity: Security Groups](../AD-DS/Activities/01-Security-Groups/README.md)
