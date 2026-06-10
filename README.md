Ahmad jaani, bilkul chill karo! Yeh lo poora ka poora data ek hi single code block ke andar.

Tumne bas upar right side par **`Copy code`** par click karna hai aur GitHub par ja kar paste kar dena hai. Saara purana aur naya data sequence mein proper alignment ke sath isme mojood hai:

```markdown
# SOC Automation, Detection Engineering & Threat Hunting Lab

## 📌 Project Overview
This project demonstrates the end-to-end security monitoring lifecycle. I built a virtual Security Operations Center (SOC) home lab to collect endpoint telemetry, simulate real-world cyber attacks, engineer custom Detection Rules using Splunk Search Processing Language (SPL), and document professional Incident Response findings.

## 🛠️ Technology Stack & Lab Specs
* **SIEM:** Splunk Enterprise (Host Machine)
* **Endpoint Telemetry:** Windows 10 VM (Subnet: `192.168.18.0/24`) + Sysmon Installed
* **Adversarial Simulation:** Kali Linux VM (Subnet: `192.168.18.0/24`)
* **Log Collection:** Splunk Universal Forwarder

## 🗺️ Project Architecture
```text
 [ Kali Linux (Attacker) ] ———(Bridged Network)———> [ Windows 10 (Victim) ]
                                                            |
                                                   (Universal Forwarder)
                                                            |
                                                            ▼
                                                    [ Splunk Host (SIEM) ]

```

---

## 🎯 MITRE ATT&CK Framework Mapping

To align operational metrics with global security standards, simulated adversarial behaviors within this engineering lab are mapped directly to the MITRE ATT&CK framework matrix:

* **Tactic:** Credential Access (TA0006) & Defense Evasion (TA0005)
* **Technique:** Brute Force: Password Guessing (T1110.001) — Executed via automated programmatic network connection loops.
* **Technique:** Valid Accounts (T1078) — Specifically targeted localized endpoint administrative aliases (`vboxuser`).
* **Mitigation/Consequence:** Account Lockout Policies triggered natively under brute-force duress.

---

## 🛠️ Infrastructure Configuration & Deep Troubleshooting

### 1. Endpoint Auditing Activation

By default, target endpoint platforms suppress failed authentication metadata storage. Local endpoint security runtime parameters were manually provisioned via the administrative shell to force global failure tracking:

```cmd
auditpol /set /category:"Logon/Logoff" /success:enable /failure:enable

```

### 2. Streamlined Forwarder Pipe Mapping (`inputs.conf`)

To map custom security channels directly to the central logging infrastructure, targeted configuration matrices were declared within the local forwarder system parameters:

```ini
[WinEventLog://Security]
disabled = 0
index = main
renderXml = true

```

### 3. Core Registry Protection Bypass (Bypassing Remote Restrictions)

Windows operating systems natively drop unauthenticated remote network requests via SMB channels before transferring data to the LSASS local authentication subsystem, suppressing log generation. To capture explicit inbound adversarial metrics, a critical core policy override was implemented:

* **Registry Path:** `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System`
* **Value Generated:** `LocalAccountTokenFilterPolicy` (DWORD 32-bit) set to `1`
* **Impact:** Neutralized network-level authentication drop filters, enabling the external Kali Linux node to force raw data streams into the endpoint's log subsystem.

---

## 🚀 Adversarial Simulation & Dynamic Data Capture

### Command Execution (Kali Linux Client)

An automated network connection loop targeted the endpoint's SMB architecture (`Port 445`) across iterative password blocks.

*Telemetry Results:* Triggered high-density authentication faults resulting in explicit system rejections (`NT_STATUS_LOGON_FAILURE`) and an autonomous endpoint freeze (`NT_STATUS_ACCOUNT_LOCKED_OUT`).

### Ingested SIEM Logs Schema

The generated security streams were successfully parsed inside Splunk, highlighting critical indicators of compromise (IoCs) including **EventID 4625** (Windows Audit Failure) and **EventID 4740** (Endpoint Lockout).

---

## 📊 Detection Engineering & Operational Dashboards

### Real Real-Time Correlation Rule Engine

To turn raw data arrays into actionable security alerts, a high-fidelity monitoring alert was deployed within the Splunk core engine:

```splunk
index=main "4625" "vboxuser" | stats count by host | where count > 3

```

### SOC Real-Time Alert Trigger

The alert rule continuously processes incoming streams. Once the simulation breached the defined threshold, Splunk successfully fired a **Critical Severity Alert** in the SOC management terminal.

### Security Analytics Dashboard Panels

A fully operational, visual multi-panel dashboard was constructed to provide centralized visibility into continuous credential anomalies:

1. **Brute-Force Timeline Trend (Line Chart):** Capturing chronologically distributed attack frequencies.
2. **Top Targeted Endpoints (Bar Chart):** Isolating compromised system parameters across active subnets.
3. **Attack Density Time Buckets (Column Chart):** Evaluating velocity peaks of incoming automated scripts.
4. **Live Incident Feed (Data Matrix Table):** Providing raw investigative tracking for security operations center analysts.

```

```
