# Splunk-SIEM-Log-Analysis-Lab# Windows Event Log Analysis & Detection Lab (Splunk SIEM)

## Objective
This hands-on project demonstrates how to set up a SIEM pipeline using Splunk Enterprise to monitor, analyze, and detect adversarial activity. I simulated a brute-force attack from a Kali Linux machine against a Windows target, ingested the security logs via a Splunk Universal Forwarder, and built search queries to isolate and identify the malicious actor.

### Tools & Technologies Used:
* **SIEM:** Splunk Enterprise (Indexer)
* **Endpoint Agent:** Splunk Universal Forwarder
* **Attacking Machine:** Kali Linux (Hydra, Nmap)
* **Target Machine:** Windows 10/11 (Advanced Security Auditing Enabled)

---

## Lab Architecture & Topology
1. **Windows Endpoint:** Configured via Local Security Policy (`secpol.msc`) to log granular logon and process creation events.
2. **Splunk Universal Forwarder:** Installed locally on Windows; configured via `inputs.conf` to stream `WinEventLog:Security` to the indexer over port `9997`.
3. **Splunk Enterprise:** Acting as the central SIEM console to ingest, index, and analyze incoming log data.

---

## Phase 1: The Attack (Adversarial Simulation)
Using Kali Linux, I conducted a network brute-force attack against the Windows target via Remote Desktop Protocol (RDP) using **Hydra** to simulate an online password-guessing campaign.

```bash
hydra -l Administrator -P custom_wordlist.txt -vV <Target_IP> rdp
