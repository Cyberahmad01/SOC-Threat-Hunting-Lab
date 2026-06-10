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


