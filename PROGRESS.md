# 📊 Lab Progress

> Tracking completed milestones across each lab phase. Updated as work is finished.

---

## ✅ Phase 0 — Environment Setup & GitHub (17–21 Mar)

- Provisioned DC01 (Windows Server 2022, 8 GB RAM) and CLIENT01 (Windows 11, 4 GB RAM) on UTM
- Installed AD DS and DNS Server roles on DC01
- Promoted DC01 to domain controller — new forest `helpdesk.lab` (Windows Server 2016 functional level)
- Verified domain with `Get-ADDomain`, `Get-ADForest`, and AD services health check
- Created OU structure: `_STAFF` (IT, HR, Finance, Sales), `_COMPUTERS`, `_GROUPS`, `_DISABLED`
- Created department security groups: IT-Staff, HR-Staff, Finance-Staff, Sales-Staff, IT-Admins
- Configured AD Sites & Services: London-HQ (`192.168.100.0/24`) + Manchester-Branch (`192.168.1.0/24`)
- GitHub repo live with README, AD-DS walkthrough, and 13 setup screenshots
- Knowledge base article templates created (7 articles planned)

---

## 🔄 Phase 1 — Active Directory & Identity (21–28 Mar)

*In progress*

- [ ] Daily onboarding drills (5 users across departments)
- [ ] Account lockout investigation (Event ID 4740)
- [ ] GPO configuration (password policy, mapped drives, Control Panel restriction)
- [ ] Offboarding drill
- [ ] KB articles: Account Lockout + GPO Management

---

## 🔜 Phase 2 — Networking (28 Mar–1 Apr)

- [ ] Dual-VLAN in Packet Tracer (IT + HR departments)
- [ ] DHCP troubleshooting (169.x.x.x investigation)
- [ ] DNS troubleshooting
- [ ] KB articles: DHCP + DNS

---

## 🔜 Phase 3 — ITIL 4 Incident Management (1–5 Apr)

- [ ] Daily incident queue (5–8 tickets/day for 5 days)
- [ ] P1 major incident simulation (DC01 unreachable)
- [ ] Shift reports committed to GitHub
- [ ] KB article: P1 Major Incident Procedure

---

## 🔜 Phase 4 — Microsoft 365 & Admin Centre (6–7 Apr)

- [ ] M365 Admin Centre tasks (user creation, licensing, groups, password reset)
- [ ] AD vs Entra ID comparison

---

## 🔜 Phase 5 — MSP Scenarios & Portfolio Polish (7–10 Apr)

- [ ] Multi-client simultaneous support drill
- [ ] Final KB articles: Onboarding + Offboarding
- [ ] Portfolio polish and final README update
