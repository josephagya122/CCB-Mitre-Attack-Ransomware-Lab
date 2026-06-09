# Cash Capital Bank — Incident Response Standard Operating Procedure
## Document Ref: CCB-IR-SOP-005: Ransomware Containment Playbook

### 1. Phase 1: Identification & Triage
* **Trigger Event:** Automated SIEM alert showing mass file modification actions or unexpected EDR service termination alerts across multiple hosts.
* **Triage Action:** The On-Call Security Analyst must verify the alert by checking the host for high processor utilization and the existence of known ransomware extension markers (e.g., `.locked`).

### 2. Phase 2: Isolation & Containment
* **Network Isolation:** Instruct the network infrastructure team to immediately revoke Active Directory replication tokens and isolate infected subnets using internal firewall access control lists (ACLs).
* **Host Isolation:** Trigger host network isolation commands via the EDR central console for all impacted endpoints to stop lateral worm spreading over Port 445.

### 3. Phase 3: Eradication & Recovery
* **Do Not Pay:** In alignment with corporate compliance policy, no ransom keys will be negotiated.
* **Wipe and Reimage:** Impacted nodes will have their partitions wiped completely. Operating systems will be re-deployed from verified golden master images.
* **Data Restoration:** System states will be restored utilizing the most recent offline, immutable transaction logs hosted in the secure backup repository segment.

### 4. Phase 4: Regulatory Notification (GRC Lifecycle)
* **Reporting Timeline:** Under local financial reporting regulations and compliance directives, a formal notice of the security breach and PII exfiltration assessment must be compiled and dispatched to the banking regulator and data protection agency within **72 hours** of identification.
