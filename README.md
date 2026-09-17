\# Active Directory IAM Joiner-Mover-Leaver Lab



\## Project Overview



Built a Windows Server 2025 Active Directory lab using VMware Workstation to practice Identity and Access Management (IAM) lifecycle operations.



The project demonstrates a complete Joiner-Mover-Leaver (JML) workflow using Active Directory Users and Computers and PowerShell verification.



\## Lab Environment



\- VMware Workstation Pro

\- Windows Server 2025

\- Active Directory Domain Services (AD DS)

\- DNS Server

\- Domain: gcyber.test

\- Domain Controller: DC01

\- DC01 IP Address: 192.168.15.10



\## Active Directory Structure



Created a structured Organizational Unit (OU) design:



\- Users

&#x20; - HR

&#x20; - Finance

&#x20; - Sales

&#x20; - IT

\- Groups

\- Computers

\- Service Accounts

\- Disabled Users



!\[OU Structure](screenshots/01-ou-structure.png)



\## Security Groups



Created Global Security Groups for department-based access management:



\- GG\_HR\_Users

\- GG\_Finance\_Users

\- GG\_Sales\_Users

\- GG\_IT\_Users



!\[Security Groups](screenshots/02-security-groups.png)



\## Joiner Scenario



Created a fictional employee:



\*\*Sarah Lee\*\*



Initial department:



\*\*Sales\*\*



Actions performed:



1\. Created the user account `sarah.lee`

2\. Placed the account in the Sales OU

3\. Assigned membership to `GG\_Sales\_Users`

4\. Configured password change at next logon

5\. Verified group membership using PowerShell



\## Mover Scenario



Sarah Lee transferred from Sales to Finance.



Actions performed:



1\. Moved Sarah from the Sales OU to the Finance OU

2\. Removed `GG\_Sales\_Users`

3\. Added `GG\_Finance\_Users`

4\. Verified the new group membership with PowerShell



This demonstrated the principle of least privilege by removing obsolete access before retaining only the permissions required for the new role.



\## Leaver Scenario



Sarah Lee was offboarded.



Actions performed:



1\. Disabled the user account

2\. Removed Finance security-group access

3\. Moved the account to the Disabled Users OU

4\. Retained the identity for audit and administrative purposes

5\. Verified that the account was disabled

6\. Verified that only the default Domain Users membership remained



!\[Disabled User](screenshots/03-disabled-user.png)



!\[Leaver Verification](screenshots/04-leaver-verification.png)



\## PowerShell Verification



Examples used during the project:



```powershell

Get-ADPrincipalGroupMembership sarah.lee | Select-Object Name



Get-ADUser sarah.lee -Properties Enabled |

Select-Object Name,Enabled,DistinguishedName

