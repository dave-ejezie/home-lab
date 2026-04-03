# 🛡️ Activity: Security Groups Configuration

## Objective
Create organizational security groups across core departments (`IT`, `HR`, `Finance`, `Sales`) as well as the overarching `AllStaff` group to manage role-based access control (RBAC).

## ITIL 4 Alignment: Information Security Management
In alignment with the ITIL 4 **Information Security Management** practice, proper RBAC is critical for maintaining the confidentiality, integrity, and availability of our simulated enterprise infrastructure. 
- **Group Scope (AGDLP Methodology):** `Global` (the default). We chose Global scope because these are standard departmental role groups. Users from the domain are placed into these Global groups, which can later be nested into Domain Local groups to assign specific permissions (following Microsoft's AGDLP best practices). This layered approach streamlines access management and reduces operational risk.
- **Group Type:** Security. Unlike Distribution groups (which are used purely for communication streams), Security groups allow us to enforce access control lists (ACLs) on Configuration Items (CIs).

## Process Evidence

### 1. Terminal Creation
Groups were created efficiently via the active directory PowerShell terminal:
| Script Execution 1 | Script Execution 2 |
| :---: | :---: |
| ![Creating Security Group in Terminal](../screenshots/sg-creation-terminal.png) | ![Terminal and GUI Feedback](../screenshots/sg-creation-terminal+gui.png) |

### 2. GUI Confirmation
Verified the groups generated correctly with proper scopes in Active Directory Users and Computers (ADUC):
![Security Groups Confirmed](../screenshots/user-groups-cinfirmation.png)
