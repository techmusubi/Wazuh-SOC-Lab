# SOC Automation Lab: Wazuh SIEM Deployment

## Phase 1: Network & Manager Setup
To establish a controlled environment for security monitoring, I configured a private virtual network and deployed the Wazuh Manager on an Ubuntu server.

### 1. Network Configuration
* **Internal Switch:** Created a Hyper-V Virtual Switch named `SOC-Internal` set to "Internal" to isolate lab traffic.
* **Static Addressing:** Configured the Ubuntu Manager with a static IPv4 address of `10.0.0.2`.

![Static IP Configuration](assets/UbuntuIP.png)

### 2. Wazuh Manager Deployment
* **Docker Orchestration:** Utilized a custom `docker-compose.yml` to provision the Wazuh indexer, manager, and dashboard.

![Docker Compose YAML](assets/DockerYAML.png)

* **Service Verification:** Successfully initialized the containers and verified all management services are in a "Running" state.

![Manager Deployment Result](assets/ManagerResult.png)

---

## Phase 2: Target Endpoint Provisioning (Windows 11)
I deployed a Windows 11 Home VM to serve as the target endpoint for telemetry collection. This required manual environment manipulation to bypass modern hardware requirements.

### 1. Hardware Specification & Configuration
The VM was configured to mirror a high-performance endpoint for stable security auditing:
* **Memory:** Allocated **8GB of RAM** (8192 MB) to prevent performance bottlenecks.
* **Processor:** Assigned **8 Virtual Processors** for smooth OS operation.
* **Network Integrity:** Connected to the **`SOC-Internal`** private virtual switch to ensure isolation.

![VM Resource Allocation](assets/Memory.png)

![Isolated Network Configuration](assets/Network.png)

* **Security Layer:** Enabled **Secure Boot** and the **Trusted Platform Module (TPM)** within Hyper-V settings to meet Windows 11 security standards.

![Hyper-V Security Settings](assets/Security.png)

### 2. Troubleshooting: Bypassing System Requirements
During the initial boot, the installer triggered a "This PC doesn't meet system requirements" error.

![Installation Error](assets/FailedInstall.png)

**Resolution via Registry Injection:**
Instead of restarting, I intercepted the installation at the language selection screen to perform a manual bypass:
1. **Registry Access:** Launched `regedit` via the `Shift + F10` command prompt.
2. **Manual Bypass:** Created the `LabConfig` key in `HKLM\SYSTEM\Setup` and injected **DWORD** values for `BypassTPMCheck` and `BypassSecureBootCheck` set to **1**.

![Registry Bypass Success](assets/Regedit.png)

This allowed the installer to successfully initialize the virtual disk and proceed with the OS deployment.

---

## Phase 3: Connectivity & Endpoint Hardening
With the Windows 11 endpoint provisioned, I established a static networking scheme and implemented host-based security controls to ensure reliable communication with the Wazuh Manager.

### 1. Static IPv4 Configuration
To prevent communication drops in the isolated lab environment, I manually assigned a static IP address to the Windows endpoint.
* **IP Address:** `10.0.0.3`
* **Subnet Mask:** `255.255.255.0`
* **Default Gateway:** Left blank to ensure isolation within the `SOC-Internal` switch.

![Windows IPv4 Configuration](assets/W11_IP.jpg)

### 2. Firewall Whitelisting (Principle of Least Privilege)
Instead of disabling the Windows Firewall, I created a targeted **Inbound Rule** named `Wazuh Manager Whitelist`.
* **Rule Logic:** The rule is set to **Allow** the connection specifically for the Wazuh agent services.
* **Scope Control:** Restricted the "Remote IP Address" field strictly to the Wazuh Manager's IP (`10.0.0.2`), preventing unauthorized access from other potential local sources.

![Firewall Inbound Rule List](assets/Inbound_Rules.png)

![Firewall Scope Settings](assets/Firewall_Rule.png)
