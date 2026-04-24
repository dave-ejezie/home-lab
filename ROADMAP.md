# 🗺️ Home Lab Roadmap

> This roadmap tracks core 1st-line competencies alongside the hands-on lab activities that prove them.

---

## ✅ Phase 0 — Environment Setup & Foundation

- [x] Provisioned DC01 (Windows Server 2022) and CLIENT01 (Windows 11) on UTM
- [x] Promoted DC01 to domain controller (`helpdesk.lab`)
- [x] Created core OU structure (`_STAFF`, `_COMPUTERS`, `_GROUPS`)
- [x] Client Domain Join (`CLIENT01` to `helpdesk.lab`)
- [x] AD Sites & Services: London-HQ + Manchester-Branch

---

## 🔄 Phase 1 — Active Directory — Password Resets & Permissions

**Competencies**
- [x] Diagnose a lockout and find the source machine using Event ID 4740
- [x] Know the difference between unlocking an account and resetting a password *(Proven)*
- [x] Delegate service desk rights without granting Domain Admin
- [x] Understand NTFS permissions vs share permissions *(Proven via Security Groups)*
- [x] Use PowerShell for AD tasks, not just the GUI *(Proven via Automation)*

**Supporting Labs**
- [x] **Lab 1.1** - Password Reset Scenarios
- [x] **Lab 1.2** - Diagnosing Account Lockouts
- [x] **Lab 1.3** - Security Group & Access Management
- [x] **Lab 1.4** - Delegating Password Reset / Least Privilege
- [x] **Lab 1.5** - Automated User Provisioning (`onboard-user.ps1`)
- [x] **Lab 1.6** - Windows Admin Center Deployment (Centralised web management)
- [x] **GPOs** - Configuration (password policy, mapped drives)


---

## 🔜 Phase 2 — M365 & Google Workspace Support

**Competencies**
- [ ] Manage licences — assign, remove, and identify unused ones
- [ ] Reset MFA for a locked-out user securely
- [ ] Create and manage shared mailboxes and distribution lists
- [ ] Understand the difference between Teams and a distribution group
- [ ] Navigate Google Workspace admin basics

**Supporting Labs**
- [ ] **Lab 2.1** - M365 Admin Centre Fundamentals
- [ ] **Lab 2.2** - MFA Reset (Critical NHS Skill)
- [ ] **Lab 2.3** - Shared Mailboxes & Distribution Lists
- [ ] **Lab 2.4** - Conditional Access Troubleshooting
- [ ] **Lab 2.5** - Google Workspace Comparison (Optional)

---

## 🔜 Phase 3 — Ticketing Systems (ServiceNow, Zendesk, Jira)

**Competencies**
- [ ] Know the ITIL difference between an Incident and a Service Request
- [ ] Prioritise using impact × urgency
- [ ] Update tickets clearly so anyone picking it up knows the status
- [ ] Hands-on experience with ServiceNow (or Zendesk/Jira)
- [ ] Write a KB article that prevents repeat calls

**Supporting Labs**
- [ ] **Lab 3.1** - ServiceNow PDI Setup & Core Workflow
- [ ] **Lab 3.2** - Zendesk Free Trial
- [ ] **Lab 3.3** - Jira Service Management Free
- [ ] **Lab 3.4** - End-of-Shift Handover Reports

---

## 🔜 Phase 4 — Windows Troubleshooting & User Support

**Competencies**
- [ ] Read Event Viewer and know which Event IDs matter
- [ ] Have a structured troubleshooting approach
- [ ] Fix the top common 1st-line issues (Teams, Outlook, printer, OneDrive)
- [ ] Use Quick Assist for remote support
- [ ] Document a fix clearly enough for a KB article

**Supporting Labs**
- [ ] **Lab 4.1** - Event Viewer: The Essential Event IDs
- [ ] **Lab 4.2** - The Top 8 Common 1st-Line Fixes
- [ ] **Lab 4.3** - Remote Support with Quick Assist

---

## 🔜 Phase 5 — Network, VPN & Connectivity

**Competencies**
- [ ] Run the diagnostic sequence in order and explain each step
- [ ] Know the difference between DNS and DHCP failures
- [ ] Interpret `ipconfig /all` output and spot anomalies
- [ ] Configure or troubleshoot a VPN connection
- [ ] Explain a network issue to a non-technical user

**Supporting Labs**
- [ ] **Lab 5.1 & 5.2** - The Full Network Diagnostic Sequence / DNS Break/Fix
- [ ] **Lab 5.3** - DHCP Failure Simulation (169.254.x.x)
- [ ] **Lab 5.4** - VPN Configuration (Windows Built-in)
- [ ] **Lab 5.5** - VLAN Routing (Packet Tracer)

---

## 🔜 Phase 6 — Device & Hardware Troubleshooting

**Competencies**
- [ ] Use Device Manager to identify and resolve driver issues
- [ ] Run disk health checks and interpret results
- [ ] Boot to Safe Mode and explain when you'd use it
- [ ] Know the most common BSOD stop codes
- [ ] Retrieve a BitLocker recovery key from Active Directory

**Supporting Labs**
- [ ] **Lab 6.1** - Device Manager & Driver Troubleshooting
- [ ] **Lab 6.2** - Disk Health Checks
- [ ] **Lab 6.3** - Boot Modes and Recovery
- [ ] **Lab 6.4** - Reading BSOD Stop Codes

---

## 🔜 Phase 7 — Professional User Communication & Portfolio Polish

**Competencies**
- [ ] Write clear ticket updates
- [ ] Handle angry or frustrated users calmly
- [ ] Communicate planned outages professionally
- [ ] Escalate without blaming anyone
- [ ] Deliver a powerful STAR interview story

**Supporting Labs**
- [ ] **Lab 7.1** - Ticket Update Templates
- [ ] **Lab 7.2** - Difficult User Scenarios
- [ ] **Lab 7.3** - The Escalation Decision Framework
- [ ] **Section 8** - Formulate Top 5 Interview STAR Stories
- [ ] Finalise all remaining KB articles and clean up repository
