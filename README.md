# Cloud-Native SIEM Engineering & Threat Detection Lab

## 📌 Project Overview
This project demonstrates the end-to-end engineering, configuration, and validation of an enterprise-grade Security Information and Event Management (SIEM) pipeline in a cloud ecosystem. The environment features a centralized Linux-based SIEM manager monitoring a remote Windows production instance. The deployment was successfully validated by simulating real-world adversary tradecraft mapped to the MITRE ATT&CK framework.

---

## 🏗️ Architecture & Component Breakdown

* **SIEM Manager:** Wazuh Server deployed on an Azure Ubuntu Linux VM.
* **Monitored Endpoint:** Microsoft Windows Server VM acting as the production host.
* **Network Infrastructure:** Managed entirely via Azure Network Security Groups (NSGs).
* **Telemetry Agent:** Wazuh Endpoint Agent (`ossec-agent`).

---

## 🛠️ Technical Implementation & Milestones

### Phase 1: Infrastructure Deployment & Secure Networking
* Provisioned independent cloud compute resources within Microsoft Azure.
* Isolated and locked down administrative access by restricting web console visibility exclusively to secure encrypted channels (**HTTPS / Port 443**).
* Architected dedicated, bi-directional network pipelines (**Ports 1514 & 1515**) to safely pass encrypted cryptographic agent registrations and live log streams without exposing vulnerable internal ports to the public internet.

### Phase 2: Endpoint Agent Engineering & Troubleshooting
* Successfully deployed the endpoint monitoring daemon onto the target Windows host utilizing a decoupled, multi-stage PowerShell installation workflow.
* Triaged and resolved a network connection timeout issue (`ERROR: (1208): Unable to connect to enrollment service`) by engineering high-priority ingress rules inside the cloud firewall, successfully stabilizing the secure TLS handshake between the host and the manager.
* Verified real-time connection status, bringing the telemetry pipeline to an **Active** state.

---

## 🚨 Threat Simulation & Incident Detection Validation

To prove the efficacy of the alerting logic, a live adversarial scenario was simulated directly on the Windows endpoint:

### 1. Defense Evasion: Log Wiping (MITRE ATT&CK T1070.001)
* **Action:** Executed a privileged command to purge the local Windows Security log channel (`Clear-EventLog -LogName Security`). This mimics an advanced adversary attempting to remove historical traces of a breach.
* **SIEM Detection:** The pipeline instantly identified the anomalous gap in telemetry and generated a high-severity alert: **"The audit log was cleared."**

### 2. Credential Access: Brute-Force Simulation (MITRE ATT&CK T1110)
* **Action:** Orchestrated a PowerShell automation loop generating rapid, failed authentication requests against the endpoint using unauthorized local accounts (`HackerSteve`).
* **SIEM Detection:** The SIEM successfully aggregated and flagged multiple sequential **Event ID 4625** occurrences, categorizing the behavior as active password guessing.

---

## 📈 Key Technical Skills Demonstrated

* **Cloud Security Operations:** Network Security Group (NSG) engineering, firewall rule prioritization, and secure compute provisioning.
* **SIEM/XDR Pipeline Engineering:** Log ingestion, cryptographic endpoint registration, and telemetry pipeline orchestration.
* **Systems Administration:** Linux service management (`systemctl`), Windows PowerShell scripting, and event log architecture.
* **Threat Hunting:** Event analysis, MITRE ATT&CK tactical mapping, and indicator of compromise (IoC) verification.
