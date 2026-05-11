# 🔍 Activity: Lab 2.3 — MFA Reset (The "Lost Phone" Scenario)

| Field | Value |
|---|---|
| **Environment** | `helpdesk.lab` — M365 Admin Center / Entra Admin Center |
| **Tool Used** | Entra Admin Center, Azure AD Connect |
| **Status** | ✅ Complete |
| **Date** | 11 May 2026 |

---

## Objective
To securely revoke a compromised/lost Multi-Factor Authentication (MFA) session for a locked-out employee, forcing a re-registration on their new corporate device, while navigating a known Hybrid Identity password synchronization blocker.

---

## ITIL Alignment & The "Why"
In a zero-trust enterprise environment, MFA is the primary barrier against compromised credentials. When an employee loses their mobile device, the Helpdesk must perform a secure identity verification check before manually severing the trust between Microsoft Entra ID and the lost device. 

This activity simulates a **High-Priority Incident (P2)**. Arbitrarily bypassing MFA for a caller without verification is the most common social engineering vector used by threat actors to compromise corporate networks. By forcing a "re-register" rather than disabling MFA, we ensure the user remains protected under conditional access policies.

---

## Execution: Root Cause Analysis (The Hybrid Identity Block)

Before Jane could reach the MFA prompt, her login attempt was immediately blocked with an "Incorrect Password" error, despite using the correct generated password.

### Step 1: Investigating the Sync Block
Because Jane was provisioned via our local PowerShell script on `DC01`, her account was flagged with the `ChangePasswordAtLogon` attribute. Entra ID detected this flag and blocked access because our lab environment is not configured with **Password Writeback**. The cloud has no mechanism to write a new password back down to the local Domain Controller.

### Step 2: The Resolution (Local Password Reset)
To clear the flag and allow Entra ID to authenticate Jane:
1. Jane was instructed to log into a local, domain-joined machine (`CLIENT01`).
2. Windows forced the password change locally.
3. The new password hash synced to the cloud during the next Delta cycle (2-5 minutes).
4. Jane was successfully granted access to `portal.office.com` and prompted to register her new device for MFA via Security Defaults.

*(Note: Jane also required a **Microsoft 365 E5 Developer** license assigned in the M365 Admin Center before she could see any applications like Outlook or Teams).*

---

## Execution: MFA Session Revocation

With Jane's new phone successfully registered, the simulated "lost phone" ticket was initiated by the Admin to revoke the device.

### Step 1: Locating the Authentication Methods
Navigated to **[Microsoft Entra Admin Center](https://entra.microsoft.com)** -> **Identity** -> **Users** -> **Jane Carter** -> **Authentication methods**.

> **Proof of Execution 1:** Jane's actively registered Authenticator app on her Pixel 7 Pro.
> 
> ![Jane Auth Method](../screenshots/M365-jane-auth-method.png)

### Step 2: Forcing Re-Registration
To sever the trust with the device, the **Require re-register MFA** command was executed.

> **Proof of Execution 2:** The Admin confirmation pop-up. Clicking this instantly invalidates the current session token on the device.
> 
> ![MFA Reset Pop-up](../screenshots/M365-mfa-reset.png)

### Step 3: Verification
The old authentication method was wiped from Entra ID. The very next time Jane attempts to authenticate, the system will challenge her to scan a brand new QR code.

> **Proof of Execution 3:** The authentication method is successfully removed and the account is secure.
> 
> ![MFA Reset Confirmation](../screenshots/M365-mfa-reset-confirmation.png)

---

## Final Incident Resolution Report

> **ServiceNow Incident:** INC0012948  
> **Category:** Identity & Access | **Subcategory:** MFA / Authenticator  
> **Priority:** P2 — High (User Locked Out)  
>   
> **Resolution Notes:**  
> User Jane Carter reported a lost mobile device and inability to pass MFA prompts. Identity was securely verified via line manager approval. Investigated initial login failure and determined a Hybrid Identity Password Writeback block was active; guided user to change password on local domain-joined machine. Once password hash synced to Entra ID, user successfully registered new device. Executed 'Require re-register MFA' command in Entra Admin Center to revoke the lost device session. Issue resolved, user has regained access to M365 applications.

---

## Related
- 🖥️ [Lab 2.1 - M365 Admin Centre Fundamentals](../01-M365-Admin-Centre/README.md)
