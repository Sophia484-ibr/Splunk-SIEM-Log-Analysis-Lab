# Splunk-SIEM-Log-Analysis-Lab
# Windows Event Log Analysis & Detection Lab (Splunk SIEM)

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
---

## Phase 2: Detection & Log Analysis
Transitioning to the role of a SOC Analyst, I utilized Splunk's Search Processing Language (SPL) to investigate the endpoint telemetry.

1. Identifying Failed Logons
I queried Windows Event ID 4625 (An account failed to log on) to check for authentication anomalies.
source="WinEventLog:Security" EventCode=4625
| stats count by Target_User_Name, IpAddress, Logon_Type
| sort - count

## Analysis Findings:
Attacker IP Identified: The logs pointed directly to [Insert your Kali IP here].

Targeted Account: Administrator / [Insert your Windows User here].

Volume: Over [Insert number of attempts] failed authentication attempts occurred within a 60-second window, highlighting a clear brute-force signature rather than a standard forgotten password.
## Key Takeaways & Conclusion
Telemetry is King: Without turning on Advanced Audit Policies in Windows, high-fidelity data like the source IP of a failed network logon would go unrecorded.

Proactive Alerting: This query can be converted directly into a real-time Splunk Alert. If Event Code 4625 triggers >10 times in 1 minute from a single IP, an automated alert can notify the security team to block the source IP at the firewall level.
