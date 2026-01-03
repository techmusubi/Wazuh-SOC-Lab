# SOC Automation & SIEM Lab (Wazuh)

## 🎯 Project Objective
This lab demonstrates the deployment of a centralized Security Information and Event Management (SIEM) solution using Wazuh. The goal is to establish persistent telemetry from a Windows 11 endpoint to a Linux-based manager in an isolated virtual network.

## 🛠️ Phase 1: Network & Infrastructure Configuration

### 1. Persistent Networking (Hyper-V)
To avoid the volatile IP rotation of the Default Switch, I created a custom **Internal Virtual Switch** named `SOC-Internal`. 

**<img width="1109" height="682" alt="IPv4" src="https://github.com/user-attachments/assets/ecb2d520-2b1b-4a04-a4ab-860b2215cb83" />**
*Caption: Configuring the static IP 10.0.0.1 on the Host machine.*

### 2. Static IP Assignment (Ubuntu 22.04)
I configured the Wazuh Manager with a static IP (`10.0.0.2`) using **Netplan**. This ensures that the SIEM dashboard and endpoint agents maintain a consistent connection.

**<img width="1174" height="1038" alt="Result" src="https://github.com/user-attachments/assets/eaf78601-d4a1-434a-9182-6119e7432f8a" />**
*Caption: Netplan YAML configuration for the static lab network.*

### 3. Connectivity Validation
Verified Layer 3 connectivity by whitelisting the host IP in the Ubuntu firewall (`ufw`) and performing a successful ping test.

**<img width="1174" height="1038" alt="Result" src="https://github.com/user-attachments/assets/dbe10776-d90a-49f0-b5be-c5d3269404d9" />**
*Caption: Successful end-to-end connectivity test.*

---
## 🚀 Next Steps
- Deploy Windows 11 Home VM on the `SOC-Internal` segment.
- Install Wazuh Agent and Sysmon for deep endpoint visibility.
