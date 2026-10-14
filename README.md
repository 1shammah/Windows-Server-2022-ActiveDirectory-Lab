# Windows-Server-2022-ActiveDirectory-Lab
Hands-on Enterprise IT Helpdesk Lab built with Oracle VirtualBox, Windows Server 2022, Active Directory Domain Services, and Windows 11. Demonstrates 1st Line Service Desk ticket troubleshooting, user onboarding, and GPO administration.

## 📋 Table of Contents
* [Overview & Prerequisites](#overview--prerequisites)
* [Part 1: Environment & Windows Server 2022 Installation](#part-1-environment--windows-server-2022-installation)
* [Part 2: Active Directory & Domain Controller Setup](#part-2-active-directory-and-domain-controller-setup)
* [Part 3: VirtualBox Guest Additions, User Onboarding & Active Directory Administration (upcoming)](#part-3-guest-and-user-onboarding)
 

---

## 📌 Overview & Prerequisites
This project documents a hands-on IT Helpdesk practice lab modeled after enterprise infrastructure. It demonstrates foundational 1st Line Service Desk capabilities—including OS deployment, Active Directory administration, user onboarding, and endpoint troubleshooting.

### Core Technologies
* **Hypervisor:** Oracle VirtualBox (v7.1.4) 
* **Server OS:** Windows Server 2022 (64-bit ISO)
* **Client OS:** Windows 11 Enterprise (ISO via Media Creation Tool)

---

<details>
<summary><b>🛠️ Part 1: Environment & Windows Server 2022 Installation (Click to Expand)</b></summary>

<br>

### 1. Software Downloads
1. Downloaded and installed **Oracle VirtualBox 7.1.4**.
2. Acquired the **Windows Server 2022 64-bit ISO** evaluation file from Microsoft.
3. Generated the **Windows 11 ISO** using the Windows 11 Media Creation Tool (`Create ISO file` option).

![Software Downloads](server_2022_images/downloads.png)

---

### 2. Virtual Machine Allocation
1. Opened VirtualBox Manager and selected **New**.
2. Configured VM properties:
   * **Name:** `Server 2022` 
   * **OS Type:** `Windows 2022` 
3. Allocated virtual hardware resources:
   * **Base Memory (RAM):** 8192 MB (8 GB)
   * **Processors:** 2 vCPUs 
   * **Storage:** Created standard virtual disk image

![Virtual Machine Setup](server_2022_images/setup.png)

---

### 3. Windows Server 2022 OS Deployment
1. Attached the **Windows Server 2022 ISO** image to the VM optical drive.
2. Booted the VM and selected **Windows Server 2022 Datacenter (Desktop Experience)** to enable the graphical user interface (GUI).
3. Performed custom installation and allowed the OS installer to copy files and finish updates.
4. Following the system reboot, removed/unmounted the ISO installation disk from the virtual drive.

![ISO Selection](server_2022_images/version_install.png)
![Windows Desktop Experience](server_2022_images/winver.png)

---

### 4. Post-Installation Configuration & Verification
1. Configured the primary local **Administrator account password** and signed in via `Ctrl + Alt + Delete`.
2. Opened **Server Manager** and updated the system **Time Zone** from Pacific Time to local Eastern Time.
3. Adjusted VirtualBox display scaling under `Virtual Screen 1 -> 125%` for optimal screen visibility.
4. Tested administrative Command Prompt CLI utilities for remote system management:
   * `shutdown /i` — Opened the Graphical Remote Shutdown interface
   * `shutdown /s` — Standard full system shutdown
   * `shutdown /r` — Full system reboot
   * `shutdown /?` — Displayed full command parameter list

![Command Line](server_2022_images/shutdown.png)
![Shutdown Confirmation](server_2022_images/shutdown_confirmed.png)

</details>

---

<details>
<summary><b>🎯 Part 2: Active Directory Domain Services (AD DS) & Domain Controller Setup (Click to Expand)</b></summary>

<br>

### 1. Hostname Customization & Server Reboot
1. Signed into Windows Server 2022 as the local Administrator.
2. Opened **Server Manager -> Local Server** and selected the default generated computer name (e.g., `WIN-0907...`).
3. Renamed the computer to **`MYDC-01`** (representing Domain Controller 01 in New York) to align with enterprise naming standards.
4. Restarted the virtual machine to apply the new system hostname.

![Server Rename](server_2022_images/rename-server.png)

---

### 2. Active Directory Domain Services (AD DS) Role Installation
1. Opened **Server Manager**, selected **Manage -> Add Roles and Features**.
2. Selected **Role-based or feature-based installation** and designated `MYDC-01` as the destination server.
3. Selected **Active Directory Domain Services (AD DS)** and accepted all required default dependency features.
4. Completed the role installation wizard.

![AD DS Role Installation](server_2022_images/add-ad-ds-role-01.png)
[AD DS Feature Installation](server_2022_images/add-ad-ds-feature-01.png)

---

### 3. Domain Controller Promotion (PowerShell Scripting Method)
1. Selected the **Notification flag** in Server Manager and clicked **Promote this server to a domain controller**.
2. Selected Deployment Operation: **Add a new forest** and set the Root Domain Name to **`shammahdube.com`**.
3. Set the Directory Services Restore Mode (DSRM) password.
4. On the review screen, clicked **View Script** to export the automatically generated deployment script.
5. Closed the wizard, launched **PowerShell (Run as Administrator)**, pasted the script, entered the Safe Mode password, and executed the automated CLI deployment.
6. Allowed the system to complete the promotion process and perform an automatic reboot .

![PowerShell AD DS Deployment Script](server_2022_images/powershell-promotion.png)

---

### 4. Post-Promotion & Active Directory Verification
1. Signed into the newly established domain as **`SHAMMAHDUBE\Administrator`** .
2. Executed Command Prompt CLI utilities to verify domain controller status and domain health:
   * `whoami` — Confirmed current domain admin identity (`SHAMMAHDUBE\Administrator`)
   * `hostname` — Verified server hostname (`MYDC-01`)
   * `systeminfo` — Verified Domain (`shammahdube.com`), OS edition, and network parameters
   * `net user administrator /domain` — Queried domain user account parameters directly from Active Directory
3. Opened **Server Manager -> Tools -> Active Directory Users and Computers (ADUC)** to confirm AD administration tools are operational.

![Active Directory Verification CLI](server_2022_images/verify-ad-cli-01.png)
![Active Directory Verification CLI](server_2022_images/verify-ad-cli-02.png)
![Active Directory Users and Computers GUI](server_2022_images/aduc-verification.png)

</details>
