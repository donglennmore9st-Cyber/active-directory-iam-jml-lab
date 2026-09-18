# Active Directory IAM Joiner-Mover-Leaver Lab



## Project Overview



Built a Windows Server 2025 Active Directory lab using VMware Workstation to practice Identity and Access Management (IAM) lifecycle operations.



The project demonstrates a complete Joiner-Mover-Leaver (JML) workflow using Active Directory Users and Computers and PowerShell verification.



## Lab Environment



- VMware Workstation Pro

- Windows Server 2025

- Active Directory Domain Services (AD DS)

- DNS Server

- Domain: gcyber.test

- Domain Controller: DC01

- DC01 IP Address: 192.168.15.10



## Active Directory Structure



Created a structured Organizational Unit (OU) design:



- Users

&#x20; - HR

&#x20; - Finance

&#x20; - Sales

&#x20; - IT

- Groups

- Computers

- Service Accounts

- Disabled Users



![OU Structure](screenshots/01-ou-structure.png)



## Security Groups



Created Global Security Groups for department-based access management:



- GG\_HR\_Users

- GG\_Finance\_Users

- GG\_Sales\_Users

- GG\_IT\_Users



![Security Groups](screenshots/02-security-groups.png)



## Joiner Scenario



Created a fictional employee:



**Sarah Lee**



Initial department:



**Sales**



Actions performed:



1. Created the user account `sarah.lee`

2. Placed the account in the Sales OU

3. Assigned membership to `GG\_Sales\_Users`

4. Configured password change at next logon

5. Verified group membership using PowerShell



## Mover Scenario



Sarah Lee transferred from Sales to Finance.



Actions performed:



1. Moved Sarah from the Sales OU to the Finance OU

2. Removed `GG\_Sales\_Users`

3. Added `GG\_Finance\_Users`

4. Verified the new group membership with PowerShell



This demonstrated the principle of least privilege by removing obsolete access before retaining only the permissions required for the new role.



## Leaver Scenario



Sarah Lee was offboarded.



Actions performed:



1. Disabled the user account

2. Removed Finance security-group access

3. Moved the account to the Disabled Users OU

4. Retained the identity for audit and administrative purposes

5. Verified that the account was disabled

6. Verified that only the default Domain Users membership remained



![Disabled User](screenshots/03-disabled-user.png)



![Leaver Verification](screenshots/04-leaver-verification.png)



## PowerShell Verification



Examples used during the project:



```powershell

Get-ADPrincipalGroupMembership sarah.lee | Select-Object Name



Get-ADUser sarah.lee -Properties Enabled |

Select-Object Name,Enabled,DistinguishedName

```


## AGDLP Role-Based File Access
Implemented department-based access control using the **AGDLP** model:

**Accounts → Global Groups → Domain Local Groups → Permissions**

### Access Design

- Finance: `GG_Finance_Users` → `DL_Finance_RW` → Finance Share
- Sales: `GG_Sales_Users` → `DL_Sales_RW` → Sales Share
- HR: `GG_HR_Users` → `DL_HR_RW` → HR Share
- IT: `GG_IT_Users` → `DL_IT_RW` → IT Share

Users were assigned to department-based Global Groups. These groups were nested into Domain Local groups, and permissions were assigned to the Domain Local groups instead of directly to individual users.

This demonstrates scalable role-based access control and the principle of least privilege.

### Access Validation

- Finn Test → Finance access authorized
- Sally Test → Sales access authorized
- Hannah Test → HR role membership verified
- Ian Test → IT access authorized
- Cross-department access attempts were denied

### Evidence

#### Authorized Finance Access

![Finance Authorized Access](01-finance-authorized-access.png)

#### Finance Share Permission

![Finance Share Gate](02-finance-share-gate.png)

#### Effective Nested Membership

![Finance Effective Membership](03-finance-effective-membership.png)

#### Cross-Department Access Denied

![Finance Access Denied](04-finance-access-denied.png)

#### Authorized Sales Access

![Sales Authorized Access](05-sales-authorized-access.png)

### Verification Commands

```

Get-ADPrincipalGroupMembership username | Select-Object Name

Get-ADGroupMember GroupName -Recursive |
Select-Object Name,SamAccountName,ObjectClass

Get-SmbShareAccess -Name ShareName

Get-SmbShare
```
