# 👤 Activity: Batch User Creation

| Field | Value |
|---|---|
| **Environment** | helpdesk.lab — Windows Server 2022 |
| **Tool Used** | PowerShell 5.1 — `onboard-user.ps1` |
| **Status** | ✅ Complete |
| **Date** | 2026-04-03 |

---

## Objective
Use the automated `onboard-user.ps1` script to create staff accounts across all department OUs and assign users to the correct security groups.

---

## Prerequisites

Before running this activity, the following must be in place:

- [ ] AD DS role installed and DC01 promoted to Domain Controller
- [ ] OU structure exists — `OU=_STAFF` with sub-OUs for each department (`IT`, `HR`, `Finance`, `Sales`)
- [ ] Security groups created in `OU=_GROUPS` (see [Activity: Security Groups](../01-Security-Groups/README.md))
- [ ] `onboard-user.ps1` saved to `C:\Scripts` on DC01
- [ ] PowerShell running **as Administrator** on DC01
- [ ] Execution Policy allows the script to run (`Set-ExecutionPolicy RemoteSigned`)

---

## ITIL 4 Alignment: Service Request Management & Standard Changes

User onboarding is one of the most frequently repeated tasks handled by a Service Desk. By scripting this process, we treat it not as a one-off manual task, but as a **Standard Change** — a pre-approved, repeatable, low-risk procedure with a predictable outcome.

- **Automation reduces errors:** Typing user details manually into ADUC means any typo creates a wrong username or drops an attribute silently. A script enforces the same format every time.
- **Attributes matter downstream:** Passing `-JobTitle` and `-Department` populates AD fields used by other systems — such as Exchange address books, dynamic distribution groups, or Entra ID (Azure AD) syncing.
- **Lab vs. Live password handling:** In this lab we use a forced plain-text password for speed. In a live environment, this would be replaced with a randomly generated password or a Temporary Access Pass (TAP) — a one-time, short-lived credential passed to the user via a secure channel (not email).

---

## Step-by-Step Walkthrough

### Step 1: Run the Script for Each User

All five script executions were run from `C:\Scripts` on DC01:

```powershell
cd C:\Scripts

.\onboard-user.ps1 -FirstName "James"   -LastName "Carter"   -Department "IT"      -JobTitle "IT Support Technician"
.\onboard-user.ps1 -FirstName "Sarah"   -LastName "Mitchell" -Department "HR"      -JobTitle "HR Manager"
.\onboard-user.ps1 -FirstName "Michael" -LastName "Osei"     -Department "Finance" -JobTitle "Finance Analyst"
.\onboard-user.ps1 -FirstName "Emma"    -LastName "Thompson" -Department "Sales"   -JobTitle "Sales Executive"
.\onboard-user.ps1 -FirstName "Priya"   -LastName "Sharma"   -Department "IT"      -JobTitle "IT Support Technician"
```

> **Plain English:** We are running the same script five times, each time passing in a different person's details. The script takes care of everything else automatically — generating a username, finding the right folder in Active Directory, setting a password, and assigning the user to the right groups.

---

### Step 2: Understanding the Script's Log Output

Using **James Carter** as a worked example. After running the script, the terminal prints a timestamped activity log:

```
[INFO] Starting onboarding: James Carter | Dept: IT
[INFO] Generated SAMAccountName: jcarter
[INFO] UPN: jcarter@helpdesk.lab
[INFO] Target OU: OU=IT,OU=_STAFF,DC=helpdesk,DC=lab
[INFO] Target OU verified: OU=IT,OU=_STAFF,DC=helpdesk,DC=lab
[INFO] Account created: jcarter
[INFO] Added to group: GRP_IT
[INFO] Added to group: GRP_AllStaff
[INFO] Onboarding complete for jcarter
[INFO] Temporary password: Welcome123! (must change at first login)

[DONE] Account ready.
  Username:  jcarter
  UPN:       jcarter@helpdesk.lab
  Dept OU:   OU=IT,OU=_STAFF,DC=helpdesk,DC=lab
  Temp PW:   Welcome123! (user must change on first login)

ACTION: Communicate credentials to user via secure channel (not email).
```

**What each line means:**

| Log Line | What It Means |
|---|---|
| `Generated SAMAccountName: jcarter` | Username built as first initial + surname, all lowercase. This is the login name. |
| `UPN: jcarter@helpdesk.lab` | The full email-style login address — used when signing into domain machines. |
| `Target OU: OU=IT,OU=_STAFF,...` | The exact AD folder the account will be created in. |
| `Target OU verified` | Pre-flight check passed — the folder exists before we try to put anything in it. |
| `Account created: jcarter` | The user object was successfully written to Active Directory. |
| `Added to group: GRP_IT` | User placed in their department security group. |
| `Added to group: GRP_AllStaff` | User also placed in the company-wide group. |
| `[DONE] Account ready.` | Everything completed successfully. |

> **Log Colours explained:**
> - **Cyan** = INFO — something worked as expected.
> - **Yellow** = WARN — something non-critical failed (e.g., group not found). The account was still created.
> - **Red** = ERROR — something critical failed. The script stopped and the account may not have been created.
> - **Green** = [DONE] — full success.

---

## All 5 Users Created

| Full Name | Username | UPN | Department OU | Groups Assigned |
|---|---|---|---|---|
| James Carter | `jcarter` | jcarter@helpdesk.lab | `OU=IT,OU=_STAFF` | GRP_IT, GRP_AllStaff |
| Sarah Mitchell | `smitchell` | smitchell@helpdesk.lab | `OU=HR,OU=_STAFF` | GRP_HR, GRP_AllStaff |
| Michael Osei | `mosei` | mosei@helpdesk.lab | `OU=Finance,OU=_STAFF` | GRP_Finance, GRP_AllStaff |
| Emma Thompson | `ethompson` | ethompson@helpdesk.lab | `OU=Sales,OU=_STAFF` | GRP_Sales, GRP_AllStaff |
| Priya Sharma | `psharma` | psharma@helpdesk.lab | `OU=IT,OU=_STAFF` | GRP_IT, GRP_AllStaff |

---

## Final Verification

After all accounts were created, we ran a query to confirm every user exists in the correct OU with the correct attributes:

```powershell
Get-ADUser -Filter * -SearchBase "OU=_STAFF,DC=helpdesk,DC=lab" -Properties Department, Title, Enabled |
  Select-Object Name, SamAccountName, Department, Title, Enabled, DistinguishedName |
  Sort-Object Department
```

> **Plain English:** We are asking Active Directory to list every user inside the `_STAFF` folder, and show us their name, username, department, job title, whether the account is active, and their precise location in AD — sorted by department. This is our final sign-off that the onboarding worked as expected.

---

## Process Evidence

### Script Execution Logs
| Executions 1–2 (James Carter, Sarah Mitchell) | Executions 3–5 (Michael Osei, Emma Thompson, Priya Sharma) |
| :---: | :---: |
| ![Ad Users Terminal 1](../screenshots/Ad-Users-1.png) | ![Ad Users Terminal 2](../screenshots/Ad-Users-2.png) |

### Final Verification Query
All 5 users confirmed in correct OUs with correct attributes:

![AD Users Confirmation](../screenshots/Ad-Users-confirmation.png)

---

## Troubleshooting

| Symptom | Likely Cause | Fix |
|---|---|---|
| Yellow WARN on group assignment | Group name doesn't match exactly | Run `Get-ADGroup -Filter *` to confirm exact group names |
| Red ERROR — account not created | OU does not exist | Verify OU with `Get-ADOrganizationalUnit -Identity "OU=IT,OU=_STAFF,DC=helpdesk,DC=lab"` |
| `execution of scripts is disabled` | PowerShell Execution Policy blocking the script | Run `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser` |
| `Unblock-File` needed | Script downloaded from internet has a Zone Identifier | Run `Unblock-File -Path C:\Scripts\onboard-user.ps1` |
| User created but not in correct group | Group existed under a different name | Check group names are `GRP_IT`, `GRP_HR` etc. — exact match required |

---

## Related

- 🛡️ [Activity: Security Groups](../01-Security-Groups/README.md)
- 🔐 [Activity: Group Policy Objects](../03-Group-Policies/README.md)
- 📋 [KB-006: User Onboarding Procedure](../../../kb-articles/onboarding.md)
