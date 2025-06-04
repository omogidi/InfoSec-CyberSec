# 💻 Lab 03 - Domain Preparation & WSUS GPO

This lab guides you through setting up a Windows Server 2008 R2 domain environment, joining client machines, and configuring Group Policy for Windows Server Update Services (WSUS).

---

## 🧰 Lab Requirements

- Virtual Machines (VMs):
  - **Windows Server 2008 R2** (`S2008R2.7z`)
  - **Windows 7** (`W7.7z`)
  - **Windows 10**
- VMWare installed
- 7-Zip or equivalent to extract VM images

---

## 🖥️ VM Setup

1. Extract VM images to:  
   `ISM/INFO6003/`

2. Configure each VM:

| VM         | IP Address   | Hostname               |
|------------|--------------|------------------------|
| Server 2008 R2 | `10.0.0.60` | `2008-username`         |
| Windows 7  | `10.0.0.50` | `W7-username`           |
| Windows 10 | `10.0.0.40` | `Win10-username`        |

3. Set network mode to **host-only**.
4. Confirm VM connectivity using `ping`.

---

## 🏢 Create a Windows Domain

On `S2008R2`:

1. Open **Command Prompt** and run:
   ```bash
   dcpromo
