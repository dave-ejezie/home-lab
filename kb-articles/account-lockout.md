# KB-001: Account Lockout — Investigation & Resolution

| Field | Value |
|-------|-------|
| **Category** | Access Control |
| **Priority** | P2 — High |
| **Applies To** | helpdesk.lab domain users |
| **Last Updated** | — |

## Symptoms

- User reports they cannot log in — "The referenced account is currently locked out"
- Multiple failed login attempts recorded in Event Viewer

## Cause

- Account locked after 5 consecutive failed password attempts (enforced by Password Policy GPO)
- Common triggers: cached credentials on a secondary device, user forgetting recently changed password, service account with stale credentials

## Resolution Steps

1. Open PowerShell as Administrator on DC01
2. Identify all locked accounts:
   ```powershell
   Search-ADAccount -LockedOut | Select Name, SamAccountName, LockedOut
   ```
3. Check Event Viewer for the source machine:
   - Event Viewer → Windows Logs → Security
   - Filter for **Event ID 4740** (Account Lockout)
   - Note the **Caller Computer Name** — this is where the bad attempts came from
4. Unlock the account:
   ```powershell
   Unlock-ADAccount -Identity "username"
   ```
5. Verify the unlock:
   ```powershell
   Get-ADUser -Identity "username" -Properties LockedOut | Select Name, LockedOut
   ```
6. Contact the user and confirm they can log in
7. If the lockout came from an unexpected machine, investigate further (possible credential stuffing or stale session)

## Prevention

- Educate users on password best practices
- Check for cached/saved credentials on secondary devices after a password change
- Review lockout threshold policy — 5 attempts is standard but can be adjusted if false positives are frequent
- Consider implementing self-service password reset (e.g. Microsoft SSPR)

## osTicket Logging

- **Ticket Priority:** P2
- **Category:** Access Control
- **Resolution Notes:** Document the locked account, source machine (from Event ID 4740), and unlock action taken
- **Close ticket** with steps followed and user confirmation

## Related

- KB-002: GPO Management Guide (Password Policy GPO)
- Event ID reference: 4740 (lockout), 4625 (failed logon), 4767 (account unlocked)
