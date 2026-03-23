# KB-002: GPO Management Guide

| Field | Value |
|-------|-------|
| **Category** | Group Policy |
| **Priority** | P3 — Standard |
| **Applies To** | helpdesk.lab domain |
| **Last Updated** | — |

## Overview

<!-- Describe the GPOs configured in your lab and their purpose -->

## GPOs Configured

### Password Policy

<!-- Document: min length, complexity, lockout threshold, lockout duration -->

### Mapped Network Drive by Department

<!-- Document: which share, which drive letter, which OU it applies to -->

### Restrict Control Panel Access

<!-- Document: which users/OUs this applies to, how to verify -->

## Verification

```powershell
# Run on CLIENT01 to verify GPO application
gpresult /r
```

## Troubleshooting

<!-- Common GPO issues: replication delay, inheritance blocking, WMI filter problems -->

## Related

- KB-001: Account Lockout (Password Policy GPO)
