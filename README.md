# 🛡️ SOC Home Lab — Enterprise Detection & Response Environment

**Author:** Jean Carlos Urbaez Tejada  
**LinkedIn:** [linkedin.com/in/jean-cybersecurity](https://linkedin.com/in/jean-cybersecurity)  
**Status:** 🟢 Active & Operational | March 2026

---

## 📋 Overview

Enterprise-grade Security Operations Center (SOC) home lab built on VMware Fusion, simulating a real corporate security environment with VLAN segmentation, dual-SIEM architecture, Active Directory, and automated threat response.

---

## 🏗️ Lab Architecture

| Component | Details |
|-----------|---------|
| **Hypervisor** | VMware Fusion — iMac (Intel i7, 64GB RAM) |
| **Network** | 6 VLANs — pfSense firewall |
| **Virtual Machines** | 12+ VMs |
| **Domain** | LAB.local — Windows Server 2025 |
| **SIEMs** | Wazuh (XDR) + Splunk (Analytics) |
| **IDS** | Suricata on pfSense |
| **Endpoint Telemetry** | Sysmon (MITRE ATT&CK mapped) |

---

## 🌐 VLAN Segmentation

| VLAN | Name | Subnet | Systems |
|------|------|--------|---------|
| 10 | Management | 10.10.10.0/24 | pfSense, Suricata, Admin WS |
| 20 | Users | 10.10.20.0/24 | USER01, USER02 |
| 30 | Executive | 10.10.30.0/24 | USER03, USER04 |
| 40 | Servers | 10.10.40.0/24 | DC01, Data Central |
| 50 | SOC | 10.10.50.0/24 | Wazuh, Splunk, Kali Blue |
| 60 | Quarantine | 10.10.60.0/24 | Reserved |

---

## 🔧 Security Stack

| Tool | Category | Function |
|------|----------|----------|
| pfSense | Firewall/Router | VLAN segmentation, NAT, traffic control |
| Wazuh | SIEM/XDR | Log centralization, threat detection, alerts |
| Splunk | SIEM/Analytics | SPL search, dashboards, correlation |
| Suricata | Network IDS | Malicious traffic detection |
| Sysmon | Endpoint Telemetry | Process, network, registry monitoring |
| Active Directory | Identity | Centralized auth, GPOs, LAB.local |
| VirusTotal API | Threat Intel | Automatic hash reputation + auto-quarantine |

---

## ⚡ Key Capabilities Demonstrated

- ✅ Dual-SIEM architecture with full endpoint coverage
- ✅ Automated malware detection & quarantine (VirusTotal API + Wazuh)
- ✅ MITRE ATT&CK mapped Sysmon telemetry
- ✅ Hybrid Windows/Linux Active Directory monitoring
- ✅ Real incident resolution and documentation (8+ incidents)
- ✅ Compliance mapping: NIST 800-53, PCI DSS, HIPAA, GDPR
- ✅ Bilingual technical documentation (EN/ES)

---

## 📁 Repository Structure

SOC-Home-Lab/
├── docs/
│   ├── SOC_Lab_Master_v3_EN.pdf
│   └── SOC_Lab_Master_v3_ES.pdf
├── incidents/
│   └── incident-reports.md
├── detection-scenarios/
│   └── scenarios.md
└── README.md


---

## 🔬 Real Evidence — VirusTotal Integration

Live detection validated on March 19, 2026:

- **Rule triggered:** 87105 — Level 12 CRITICAL
- **Detection:** 62/67 antivirus engines
- **Response time:** < 2 seconds
- **Action:** Automatic quarantine via quarantine_file.sh

---

## 📊 Compliance Mapping

| Framework | Control | Coverage |
|-----------|---------|----------|
| NIST 800-53 | SI-7 | System integrity monitoring |
| PCI DSS | 10.6.1 / 11.4 | Security event monitoring |
| HIPAA | § 164.312 | Access control & audit |
| GDPR | IV_35.7.d | Data protection assessment |
| MITRE ATT&CK | Multiple | Sysmon + Wazuh detection rules |

---

## 📄 Documentation

Full lab documentation available in the `/docs` folder:
- Master Architecture & Operations Document (EN/ES)
- Network diagrams
- Incident reports

---

*Built and maintained by Jean Carlos Urbaez Tejada — Cybersecurity Professional*  
*SOC Analyst | Incident Responder | SIEM Engineer*
