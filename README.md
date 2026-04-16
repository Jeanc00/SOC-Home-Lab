# 🛡️ SOC Home Lab — Enterprise Detection & Response Environment

**Author:** Jean Carlos Urbaez Tejada
**LinkedIn:** [linkedin.com/in/jean-cybersecurity](https://linkedin.com/in/jean-cybersecurity)
**Status:** 🟢 Active & Operational | April 2026 | **v4.0**

> 🔗 **[Live Project Overview →](https://jeanc00.github.io/SOC-Home-Lab)**

-----

## 📋 Overview

Enterprise-grade Security Operations Center (SOC) home lab built on VMware Fusion, simulating a real corporate security environment with VLAN segmentation, dual-SIEM architecture, Active Directory, endpoint telemetry, and automated threat response.

Version 4.0 completes three major milestones: **SOC Security Overview Dashboard** in Splunk with real-time auto-refresh panels, **Suricata → Splunk EVE JSON integration** for full network + endpoint visibility, and **end-to-end email alerting** with automated PDF forensic reports delivered to Gmail — zero human intervention required.

-----

## ⚡ Key Stats

|Metric              |Value             |
|--------------------|------------------|
|Virtual Machines    |12+               |
|Segmented VLANs     |6                 |
|SIEM Platforms      |2 (Wazuh + Splunk)|
|Auto-Quarantine Time|< 2 seconds       |
|Incidents Resolved  |9+                |
|Detection Validated |✅ End-to-end live |

-----

## 🏗️ Architecture

### Physical Host

|Component |Spec                                       |
|----------|-------------------------------------------|
|Model     |iMac 2020 (iMac20,1)                       |
|Processor |Intel Core i7-10700K — 8 cores / 16 threads|
|RAM       |64 GB                                      |
|Storage   |Samsung T7 2TB external SSD                |
|Hypervisor|VMware Fusion                              |

### VLAN Topology

|VLAN|Segment   |Subnet       |Key Systems                              |
|----|----------|-------------|-----------------------------------------|
|10  |Management|10.10.10.0/24|pfSense, Suricata, Admin-WS              |
|20  |Users     |10.10.20.0/24|WIN11-USER01, WIN11-USER02               |
|30  |Executive |10.10.30.0/24|EXEC-USER03, EXEC-USER04                 |
|40  |Servers   |10.10.40.0/24|DC01 (WinServer 2025), Ubuntu Data Server|
|50  |SOC       |10.10.50.0/24|Splunk, Wazuh, Kali Blue                 |
|60  |Quarantine|10.10.60.0/24|Isolated — Malware Containment           |

-----

## 🔧 Security Stack

|Tool                |Category           |Role                                                              |
|--------------------|-------------------|------------------------------------------------------------------|
|**Splunk**          |SIEM / Analytics   |SPL search, dashboards, scheduled email alerting                  |
|**Wazuh**           |SIEM / XDR         |Log centralization, event correlation, FIM, VirusTotal integration|
|**Suricata**        |Network IDS        |EVE JSON pipeline → Splunk (fully integrated April 2026)          |
|**pfSense**         |Firewall / Router  |VLAN segmentation, inter-VLAN routing, NAT                        |
|**Sysmon**          |Endpoint Telemetry |Process, network, file, registry events (all Windows endpoints)   |
|**Active Directory**|Identity Management|LAB.local domain, GPOs, DNS, Kerberos auth                        |
|**VirusTotal API**  |Threat Intelligence|Automatic hash enrichment + auto-quarantine                       |
|**Kali Linux**      |Blue Team          |Attack simulation, detection validation, SSH access               |

-----

## 📊 SOC Security Overview Dashboard

Built in Splunk Dashboard Studio with real-time auto-refresh across all panels.

|Panel                |Data Source                     |Threshold          |
|---------------------|--------------------------------|-------------------|
|Total Events         |index=* | stats count           |—                  |
|Critical Events      |Wazuh critical severity         |🔴 Red if ≥ 1       |
|High Events          |Wazuh high severity             |🟠 Orange if ≥ 1    |
|Failed Login Attempts|EventCode 4625                  |🟡 Yellow if ≥ 1    |
|Scan Tool Executions |nmap/masscan/zmap detections    |🔴 Always red if ≥ 1|
|Timeline / Event Map |All indexes                     |Real-time          |
|Recent Scan Activity |SPL v3 with target_ip extraction|10s delay refresh  |

-----

## 🚨 Validated Detection Scenarios

### ✅ Scan Tool Execution Detection — Full Pipeline (April 3, 2026)

```
Kali Blue (nmap) → auth.log → Splunk UF → SPL Alert (regex target_ip) → Gmail PDF
```

**Live detection log:**

```
timestamp : 2026-04-03 19:42:28
host      : jean-blue
user      : root
command   : /usr/bin/nmap
target_ip : 10.10.20.101 (WIN11-USER01)
action    : Alert fired → PDF generated → Email delivered
human intervention: NONE
```

MITRE ATT&CK: `T1046 — Network Service Scanning`

-----

### ✅ VirusTotal Auto-Quarantine — EICAR Test (March 19, 2026)

Wazuh syscheck detected EICAR file → VirusTotal API query → **62/67 engines positive** → Rule 87105 Level 12 triggered → `quarantine_file.sh` executed → file isolated in **< 2 seconds**.

MITRE ATT&CK: `T1203 — Exploitation for Client Execution`

-----

### ✅ Additional Scenarios Validated

- **Privilege Escalation (Linux)** — sudo usage detected via `linux_secure` sourcetype
- **Windows Auth Monitoring** — EventCodes 4624, 4625, 4648, 4672, 4720 correlated across domain endpoints
- **Sysmon Telemetry** — EventIDs 1, 3, 11, 13 validated on all Windows hosts
- **AD Trust Relationship Failure** — Diagnosed and resolved via Kerberos + secure channel repair
- **Hostname Forensics** — Historical event reconstruction across hostname change (jeanread → jean-blue)

-----

## 📡 Telemetry Pipeline

### Windows Endpoints

```
Windows Event Log + Sysmon → Wazuh Agent → Wazuh Manager (10.10.50.20:1514)
                            → Splunk UF   → Splunk Indexer (10.10.50.10:9997)
```

### Linux / Kali Blue

```
rsyslog (auth.log, syslog, kern.log) → Splunk UF → Splunk Indexer
                                      → Wazuh Agent → Wazuh Manager
```

### Suricata (Network IDS)

```
pfSense traffic → Suricata → EVE JSON → Splunk UF → Splunk Indexer ✅ COMPLETED April 2026
```

-----

## 🔑 Notable Engineering Challenges Solved

**Splunk config precedence — silent email failure**
`apps/search/local/alert_actions.conf` silently overrode `system/local/` credentials. Diagnosed by tracing Splunk’s config merge hierarchy. Key troubleshooting lesson for production Splunk deployments.

**Suricata ET SCAN blind spot on internal traffic**
ET SCAN rules only fire for EXTERNAL_NET → HOME_NET. VLAN-to-VLAN scans are invisible to Suricata. Solution: Sysmon EventID 3 for internal reconnaissance detection.

**SPL regex for target IP from raw sudo log**
`COMMAND` field only contains the binary path — not the target. Built `rex field=_raw` pattern to extract target IP from the full sudo log line, delivering complete forensic context in every alert.

**NT SERVICE\SplunkForwarder permissions (Access Denied errorCode=5)**
Resolved by adding the service account to the `Event Log Readers` group on all Windows endpoints via PowerShell.

-----

## 🗺️ Roadmap

|Item                                           |Status                  |
|-----------------------------------------------|------------------------|
|SOC Security Overview Dashboard (Splunk)       |✅ Completed — April 2026|
|Suricata → Splunk EVE JSON integration         |✅ Completed — April 2026|
|Email alerting with PDF attachment             |✅ Completed — April 2026|
|Scan Tool Detection v3 (target IP extraction)  |✅ Completed — April 2026|
|Ubuntu Data Server — dual SIEM integration     |✅ Completed             |
|VirusTotal + auto-quarantine                   |✅ Completed             |
|3 Documented attack scenarios (full IR reports)|🔄 In Progress           |
|External Red Team Kali (bridge mode)           |📋 Planned               |
|GPO for Event Log Readers (AD policy)          |📋 Planned               |
|Velociraptor — threat hunting & DFIR           |📋 Planned               |
|TheHive / Cortex — SOAR & case management      |📋 Planned               |
|Honeypot in VLAN 60 (Quarantine)               |📋 Planned               |

-----

## 📁 Repository Structure

```
SOC-Home-Lab/
├── README.md
├── docs/
│   ├── index.html          # Live project brief (GitHub Pages)
│   └── ir-reports/         # IronCircle incident response reports
```

-----

## 📜 Compliance Mappings

|Framework   |Control                          |Coverage                        |
|------------|---------------------------------|--------------------------------|
|MITRE ATT&CK|T1046, T1203, T1078, T1021, T1055|Detection validated             |
|PCI DSS     |10.6.1 / 11.4                    |Security event monitoring + IDS |
|NIST 800-53 |SI-7                             |System and information integrity|
|GDPR        |Art. 35.7.d                      |Data protection impact          |

-----

*SOC Detection Lab — v4.0 — April 2026 — Jean Carlos Urbaez Tejada*
