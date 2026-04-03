# KB-002: GPO Management Guide

| Field | Value |
|-------|-------|
| **Category** | Group Policy |
| **Priority** | P3 — Standard |
| **Applies To** | helpdesk.lab domain |
| **Last Updated** | 2026-04-03 |

## Overview

Group Policy Objects (GPOs) allow centralised control of settings across all computers and users in the `helpdesk.lab` domain. Instead of configuring each machine individually, policies are defined once in the Group Policy Management Console (GPMC) and applied automatically based on where a user or computer object sits in the Active Directory OU structure.

> **Plain English:** GPOs are like a rulebook that gets handed to every machine and user when they log in. You write the rules once, and Active Directory enforces them automatically, every time.

## GPOs Configured (Planned)

### Password Policy — *Planned*
> To be applied at the Domain level to cover all accounts.

| Setting | Value |
|---|---|
| Minimum password length | 10 characters |
| Complexity required | Yes |
| Maximum password age | 90 days |
| Account lockout threshold | 5 attempts |
| Lockout duration | 30 minutes |

### Mapped Network Drive by Department — *Planned*
> To be linked to `OU=_STAFF` to apply to all staff users.

| Department | Drive Letter | Share Path |
|---|---|---|
| IT | `Z:` | `\\DC01\IT-Share` |
| HR | `Z:` | `\\DC01\HR-Share` |
| Finance | `Z:` | `\\DC01\Finance-Share` |
| Sales | `Z:` | `\\DC01\Sales-Share` |

### Restrict Control Panel Access — *Planned*
> To be linked to `OU=_STAFF`. Prevents standard users from modifying system settings, reducing misconfiguration risk.

## Verification

```powershell
# Run on CLIENT01 after login to confirm GPO is being applied
gpresult /r
```

> **Plain English:** `gpresult /r` generates a summary of all policies currently applied to the user and machine. Look for your GPO name in the **Applied Group Policy Objects** section.

## Troubleshooting

| Symptom | Likely Cause | Fix |
|---|---|---|
| GPO not applying | Replication delay | Wait 15 minutes or run `repadmin /syncall` on DC01 |
| Policy not taking effect after change | Client has cached old policy | Run `gpupdate /force` on the client |
| GPO applies to wrong users | Linked at wrong level | Check link in GPMC — move to specific OU |
| WMI filter excluding users unexpectedly | Incorrect WMI query | Test the filter in GPMC → WMI Filters |

## Related

- KB-001: Account Lockout (password policy interaction)
- 🔐 [Activity: Group Policy Objects](../AD-DS/Activities/03-Group-Policies/README.md)
