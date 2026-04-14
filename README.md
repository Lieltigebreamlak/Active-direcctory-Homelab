# Active Directory Homelab

![Active Directory OU Structure](screenshots/ad-ou-structure.png)
*Active Directory structure with departmental OUs (Finance, HR, IT, Sales)*

## 📌 Overview
A hands-on Active Directory homelab built to practice user/group management for help desk roles. This lab simulates a production environment with department-specific OUs, security groups, and user accounts.

**Current Status:** Active Directory configured ✓  
**In Progress:** Group Policy Objects, DHCP setup

## 🏗️ Current Lab Environment
| Component | Details |
|-----------|---------|
| **Hypervisor** | Oracle VirtualBox |
| **Domain Controller** | Windows Server 2025 (WS2026-DC) |
| **Domain** | Homelab.local |
| **Active Directory Role** | Fully installed and configured |

## 📁 Active Directory Structure (Completed)

### Organizational Units Created
Homelab.local
├── Builtin
├── Computers
├── Domain Controllers
├── Finance
│ ├── Computers
│ ├── Groups
│ └── Users
├── ForeignSecurityPrincipals
├── HR
│ ├── Computers
│ ├── Groups
│ └── Users
├── IT
│ ├── Computers
│ ├── Groups
│ └── Users
├── Managed Service Accounts
├── Sales
│ ├── Computers
│ ├── Groups
│ └── Users
└── Users

![Full AD Hierarchy](screenshots/ad-full-hierarchy.png)
*Complete Active Directory Users and Computers view showing all OUs*

### 👥 User Management
Sample users created across all departments including:

**IT Department:**
- Alex Rivera
- Jamie Smith

**Additional Users Across OUs:**
- Mike Chen
- Sarah Johnson
- *(Plus additional users in Finance, HR, and Sales OUs)*

![User Properties](screenshots/ad-user-properties.png)
*User account configuration showing account options and expiration settings*

### 🔐 Security Groups
| Group Name | Members | Purpose |
|------------|---------|---------|
| IT-FullAccess | Alex Rivera, Jamie Smith | Full access to IT resources |
| IT-ReadOnly | *(Configured as needed)* | Read-only access for auditing |

![IT-FullAccess Group Members](screenshots/ad-group-membership.png)
*Security group showing IT department members with full access permissions*

## ✅ Skills Demonstrated (Current)
- Active Directory Domain Services installation
- Organizational Unit (OU) design and hierarchy
- User account creation across multiple departments
- Security group creation and membership management
- Understanding of ADUC navigation and structure
- Department-specific OU organization (Finance, HR, IT, Sales)

## 🚧 In Progress (Coming Soon)
- Group Policy Objects (drive maps, password policies)
- DHCP server configuration
- Additional client VM for testing
- Ticketing system integration

## 🎯 Help Desk Relevance
This AD environment directly supports common help desk tasks:
- Password resets and account unlocks
- New hire user creation
- Department transfers (moving users between OUs)
- Access requests via security group membership
- Account troubleshooting
- Understanding departmental structure for proper user placement

## 📅 Update History
- **March 18, 2026** - Initial AD structure complete with Finance, HR, IT, Sales OUs and users across all departments. Security groups configured for IT access control.
- *Next update* - GPO implementation

  Client Deployment & Troubleshooting Journey
After building the AD structure, I deployed a Windows 11 client and joined it to the domain. Here's the journey:

1. Domain Controller Network Configuration
https://screenshots/dc-network-config.png
Domain Controller with dual NICs: NAT (192.168.1.10) for internet and Internal Network (192.168.100.1) for homelab communication.

2. Active Directory Structure
https://screenshots/ad-ou-structure.png
Complete AD structure with departmental OUs (Finance, HR, IT, Sales) and dedicated Workstations OU for client machines. Initial Domain Join Attempt
https://screenshots/domain-join-error.png
First attempt to join domain failed - Domain Controller could not be contacted despite correct domain name.

4. Network Connectivity Verified
https://screenshots/ping-success.png
Confirmed basic network connectivity: Client successfully pinging DC at 192.168.100.1 with 0% loss.

5. DNS Resolution Confirmed
https://screenshots/network-dns-tests.png
Verified DNS resolution: nslookup homelab.local returns DC IP addresses, confirming DNS is working properly.

6. Success! Domain Login Achieved
https://screenshots/domain-login-success.png
Client successfully joined to domain - domain user Alex Rivera can now log in. This confirms all troubleshooting was successful.

✅ Troubleshooting Lessons Learned

Network connectivity must be verified first (ping)

DNS resolution is critical for domain discovery

Firewall rules can block domain join even when ping works

SRV records must be present for clients to locate DC

Persistence pays off - every error is solvable



- Current Status

✅ Domain Controller configured and running

✅ AD structure with department OUs complete

✅ DNS functioning with proper SRV records

✅ Windows 11 client successfully joined to domain

✅ Domain user login verified

✅ DHCP configuration

⏳ Group Policy Objects (next phase)

## 🌐 DHCP Server Configuration

I installed and configured DHCP on the Domain Controller to automatically assign IP addresses to clients, eliminating the need for static IP configuration.

### DHCP Scope Details
| Setting | Value |
|---------|-------|
| Scope Name | Homelab Internal Network |
| IP Range | 192.168.100.50 - 192.168.100.200 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.100.1 |
| DNS Server | 192.168.100.1 |
| Lease Duration | 8 days |

### Client Receiving IP via DHCP
![DHCP Client Success](screenshots/dhcp-client-success.png)
*Client ipconfig showing DHCP-enabled and IP address automatically assigned from the 192.168.100.50-200 range*

### DHCP Server Active Lease
![DHCP Address Lease](screenshots/dhcp-address-lease.png)
*DHCP console showing client with active IP address lease, confirming the server is distributing addresses correctly*

### Skills Demonstrated
- DHCP role installation and authorization in Active Directory
- Scope creation with proper subnet and options
- Router (default gateway) configuration in DHCP
- DNS server configuration in DHCP
- Client-side DHCP configuration
- Lease verification and management

### Why DHCP Matters for Help Desk
DHCP is one of the most common help desk troubleshooting areas. Understanding how to configure, verify, and troubleshoot DHCP prepares me for:
- **"My computer won't get an IP address"** tickets
- **"I can't connect to the internet"** - often a DHCP issue
- **IP conflicts** - understanding lease management
- **New computer setup** - ensuring DHCP is working

- ## ⚙️ Group Policy Objects (GPO) - In Progress

I'm currently implementing Group Policy to manage workstation settings across the domain.

### Working GPO
| GPO Name | Target | Settings | Status |
|----------|--------|----------|--------|
| IT-Desktop Wallpaper | IT Users | Forces company wallpaper on desktop | ✅ Working |

![Desktop Wallpaper Applied](screenshots/gpo-wallpaper-working.png)
*IT user desktop showing forced wallpaper - proof that GPO application is working*

### GPO Currently Troubleshooting
| GPO Name | Target | Desired Settings | Status |
|----------|--------|------------------|--------|
| Screen Lock | Workstations | Screen saver timeout (10 min), password protect | 🔄 In Progress |

### GPO Configuration Verified
| Component | Status | Evidence |
|-----------|--------|----------|
| GPO Linked to Workstations OU | ✅ Correct | Screenshot shows link with Enforced = Yes |
| Security Filtering | ✅ Correct | Authenticated Users included |
| Permissions | ✅ Correct | Read access granted |
| Loopback Processing | ✅ Enabled | Merge mode configured for user settings |

![GPO Linked to Workstations](screenshots/gpo-linked-workstations.png)
*Screen Lock GPO properly linked to Workstations OU*

![GPO Security Filtering](screenshots/gpo-security-filtering.png)
*Security filtering includes Authenticated Users*

### Registry Settings
Even though the GPO isn't fully applying yet, I verified the settings work locally:

```cmd
reg query "HKCU\Control Panel\Desktop" /v ScreenSaveActive


# 🛠️ osTicket Helpdesk Lab (Windows Server + IIS + PHP + MariaDB)

## 📌 Overview

This project demonstrates the deployment of a fully functional **osTicket helpdesk system** in a Windows Server lab environment using IIS, PHP, and MariaDB.

The goal of this lab was to simulate a real-world IT support environment and strengthen skills in:

* System administration
* Web server configuration
* Troubleshooting
* Database setup
* Helpdesk workflows

---

## 🧱 Lab Environment

* **OS:** Windows Server (VM - VirtualBox)
* **Web Server:** IIS
* **Backend:** PHP 8.4 (FastCGI)
* **Database:** MariaDB 10.11
* **Application:** osTicket v1.18.3

---

## ⚙️ Key Steps

### 1. IIS Installation & Configuration

* Installed IIS roles and features
* Enabled CGI for PHP support
* Configured Default Website

### 2. PHP Setup

* Installed PHP manually
* Configured `php.ini`
* Enabled required extensions:

  * mysqli
  * gd
  * xml
  * mbstring
* Integrated PHP with IIS using FastCGI

### 3. Database Setup (MariaDB)

* Installed MariaDB Server
* Created database:

  ```sql
  CREATE DATABASE osticket;
  ```
* Created user and assigned privileges

### 4. osTicket Deployment

* Downloaded and extracted osTicket
* Placed files in:

  ```
  C:\inetpub\wwwroot\osticket
  ```
* Completed web-based installer
* Connected to MariaDB database

---

## 🔧 Troubleshooting Highlights

During this lab, I resolved multiple issues:

* ❌ HTTP 500 (FastCGI crash)
  ✔ Fixed by installing Visual C++ Redistributable and correcting `php.ini`

* ❌ PHP syntax errors
  ✔ Identified and corrected malformed configuration

* ❌ IIS 403 / 404 errors
  ✔ Fixed default document settings and routing

* ❌ MariaDB not recognized
  ✔ Resolved PATH issues and command execution errors

* ❌ File permission issues (osTicket config)
  ✔ Adjusted NTFS permissions for IIS user

---

## 🧪 Testing

* Created test users via web portal
* Submitted tickets from user interface
* Managed and responded to tickets via Admin Panel

---

## 🔐 Post-Install Security

* Removed `/setup` directory
* Restricted write permissions on:

  ```
  include/ost-config.php
  ```

##

---

## 🚀 Skills Demonstrated

* Windows Server administration
* IIS configuration
* PHP deployment and debugging
* Database management (MariaDB/MySQL)
* Web application troubleshooting
* Helpdesk system implementation

---

## 🎯 Outcome

Successfully deployed and configured a working helpdesk system capable of handling real support tickets.

This lab simulates real world IT support environments and reinforces practical troubleshooting and system setup skills.

##


## 📫 Connect With Me
- **GitHub:** [@lielti-gebreamlak](https://github.com/lielti-gebreamlak)
- **LinkedIn:** [Lielti Gebreamlak](https://www.linkedin.com/in/lielti-gebreamlak-253555392/)

## 📚 Resources Used
- Microsoft Learn: Active Directory Domain Services and Group Policy Management
- VirtualBox documentation
- r/sysadmin homelab community guides
