# SOC Automation, Detection Engineering & Threat Hunting Lab

## 📌 Project Overview
This project demonstrates the end-to-end security monitoring lifecycle. I built a virtual Security Operations Center (SOC) home lab to collect endpoint telemetry, simulate real-world cyber attacks, engineer custom Detection Rules using Splunk Search Processing Language (SPL), and document professional Incident Response findings.

## 🛠️ Technology Stack & Lab Specs
* **SIEM:** Splunk Enterprise (Host Machine)
* **Endpoint Telemetry:** Windows 10 VM (Subnet: `192.168.18.0/24`) + Sysmon Installed
* **Adversary Simulation:** Kali Linux VM (Subnet: `192.168.18.0/24`)
* **Log Collection:** Splunk Universal Forwarder

## 🏗️ Project Architecture
```text
[ Kali Linux (Attacker) ] ───(Bridged Network)───► [ Windows 10 (Victim) ]
                                                            │
                                                     (Universal Forwarder)
                                                            │
                                                            ▼
                                                 [ Splunk Host (SIEM) ]



---

## 🛠️ Phase 3 & 4: Adversarial Simulation, Overcoming OS Constraints & Detection Engineering

### 1. Endpoint Auditing & Remote Restriction Bypass
Windows endpoints natively drop remote unauthenticated requests via network pipes prior to hitting the authentication subsystem, suppressing log generation. To capture explicit brute-force metrics, the following runtime and registry overrides were deployed:
- **Auditing Enforcement:** `auditpol /set /category:"Logon/Logoff" /success:enable /failure:enable`
- **Registry Bypass:** Created `LocalAccountTokenFilterPolicy` (DWORD 32-bit) with a value of `1` inside `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System`. This successfully forced inbound external network client attempts to resolve inside the LSASS authentication pipeline.

### 2. Threat Simulation & MITRE ATT&CK Mapping
- **Tactic:** Credential Access (TA0006)
- **Technique:** Brute Force: Password Guessing (T1110.001)
Using the automated SMB credential spraying script (`smb_brute_force.sh`), active password variants were directed toward the target system. This resulted in multiple authentication failures (`NT_STATUS_LOGON_FAILURE`) and triggered an autonomous account lockout condition (`NT_STATUS_ACCOUNT_LOCKED_OUT`).

### 3. Splunk Metrics Dashboard
The raw streams ingested via the Universal Forwarder were translated into an operational SOC dashboard, tracking attack distribution trends, time-bucket density spikes, and continuous event feeds:

![SOC Dynamic Dashboard](YAHAN_APNI_DASHBOARD_VALI_IMAGE_KA_NAME_LIKHO.png)

### 4. Correlation Rule & Real-Time Incident Trigger
A signature-based detection rule was deployed to monitor high-frequency credential tracking:
```splunk
index=main "4625" "vboxuser" | stats count by host | where count > 3
