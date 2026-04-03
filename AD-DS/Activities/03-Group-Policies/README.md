# 🔐 Activity: Group Policy Objects (GPOs)

## Objective
Configure domain-wide Group Policy Objects (GPOs) using the Group Policy Management Console (GPMC) to lock down the environment and push configurations.

## ITIL 4 Alignment: Service Configuration Management
GPOs are the backbone of centralized endpoint administration, aligning closely with the ITIL 4 **Service Configuration Management** practice. By leveraging GPOs, we ensure that our Configuration Items (CIs)—such as workstations and servers—adhere to a strict, secure baseline. This proactive management significantly reduces the volume of reactive incidents logged with the Service Desk.

- **Scope and Targeting (LSDOU):** We map policies specifically to targeted Organizational Units (e.g., restricted to `OU=_STAFF`) rather than the root domain to prevent unintended lockouts of critical service accounts or Domain Controllers, thereby protecting live services.

## Process Evidence

### GUI Configuration
Creating new GPOs and linking them to specific Organizational Units:
![GPO Configuration GUI](../screenshots/create-new-gpo-gui.png)

*(Note: Documentation for specific GPOs like mapped drives and password policies will be expanded here as they are developed).*
