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

1. Open PowerShell as Administrator on DC01 or launch Windows Admin Center.
2. Identify all locked accounts:
   ```powershell
   Search-ADAccount -LockedOut | Select Name, SamAccountName, LockedOut
   ```
3. **CRITICAL:** Find the source machine of the lockout before unlocking.
   Rather than manually filtering the Event Viewer GUI, run this optimized command to pull **Event ID 4740** directly:
   ```powershell
   Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4740} | Select-Object -First 1 | Format-List
   ```
   *Look for `Caller Computer Name` in the output to identify the rogue device.*
   ![PowerShell Lockout Investigation](../AD-DS/Activities/screenshots/dc01-powershell-investigation.png)

4. Unlock the account:
   ```powershell
   Unlock-ADAccount -Identity "username"
   ```
5. Contact the user, inform them of the source device (e.g. "Do you have your Outlook open on CLIENT01?"), and clear any cached credentials on that device.

## Prevention

- Educate users on password best practices
- Clear stale credentials from Windows Credential Manager or Mobile Mail Apps after a password change.
- Consider implementing self-service password reset (e.g. Microsoft SSPR)

## ServiceNow Logging

- **Ticket Priority:** P3
- **Category:** Account & Access
- **Resolution Notes:** Must document the `Caller Computer Name` discovered via Event ID 4740. 
  *(Example: Investigated Security Logs via DC01. Found account locked out by CLIENT01. Cleared cached credentials on CLIENT01. Unlocked account. Confirmed access restored.)*

## Related

- 🖥️ [Activity: Diagnosing Account Lockouts & Root Cause Analysis](../AD-DS/Activities/06-Account-Lockouts/README.md)
- Event ID reference: **4740 (lockout)**, 4625 (failed logon), 4767 (account unlocked)
