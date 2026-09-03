\# Endpoint Threat Detection Lab (Sysmon + Wazuh + Splunk)



A home-lab SOC environment built to practice endpoint detection engineering: instrumenting a Windows victim host, forwarding telemetry to two independent detection stacks (Wazuh and Splunk), writing custom detection content, and validating it against real MITRE ATT\&CK-mapped attack simulations from Atomic Red Team.



\## Architecture



Three VMware Workstation VMs on an isolated host-only network (VMnet2, subnet 192.168.50.0/24), with internet egress via a gateway for tooling/updates.



| VM | OS | IP | Role |

|---|---|---|---|

| win-victim | Windows 10 Pro | 192.168.50.128 | Instrumented endpoint (Sysmon, Wazuh agent, Splunk Universal Forwarder) |

| wazuh-manager | Ubuntu 24.04 | 192.168.50.11 | Wazuh manager, indexer, dashboard |

| splunk-server | Ubuntu 24.04 | 192.168.50.10 | Splunk Enterprise indexer + search head |



Note: the Wazuh agent registers under the Windows hostname DESKTOP-5BMPUDK, not the VM label win-victim.



\## Components



\- Sysmon (SwiftOnSecurity config) — endpoint telemetry

\- Wazuh 4.9.0 — detection engine with custom rules in wazuh-rules/local\_rules.xml

\- Splunk Enterprise — second detection pipeline with saved searches in splunk-searches/detection-searches.spl

\- Atomic Red Team — used to simulate five MITRE ATT\&CK techniques



\## Setup summary



1\. Static IP networking across all three VMs, host-only network with internet egress

2\. Sysmon installed on win-victim with SwiftOnSecurity's config

3\. Wazuh manager/indexer/dashboard on wazuh-manager, agent connected on win-victim

4\. Custom Wazuh rules covering encoded PowerShell, LSASS access, registry persistence, LOLBins, suspicious outbound connections

5\. Splunk Enterprise on splunk-server, running as splunk user

6\. Splunk Universal Forwarder on win-victim forwarding Sysmon data to splunk-server:9997

7\. Five MITRE ATT\&CK techniques simulated and validated in both tools



\## Notable troubleshooting



\- Splunk forwarding appeared broken (connected, but zero events indexed). Root cause was Access Denied (errorCode=5) on the Sysmon WinEventLog subscription due to forwarder service permissions. Fixed by running the service as LocalSystem.

\- Clock drift between VMs caused indexed events to be invisible under default search time ranges.

\- Windows Defender blocked two attack simulations (Mimikatz, Regsvr32 LOLBin) with real malware signature detections — a legitimate defense-in-depth finding.



\## Repo structure



README.md

wazuh-rules/local\_rules.xml

splunk-searches/detection-searches.spl

docs/attack-simulation-log.md



\## Screenshots



\- screenshots/01-troubleshooting-splunk-forwarding/ - full debugging arc for the Splunk forwarding issue, from initial port check through root cause (Access Denied on Sysmon WinEventLog subscription) to the fix and final successful ingestion of 14,405 events

\- screenshots/02-technique-t1059-encoded-powershell/ - encoded PowerShell execution

\- screenshots/03-technique-t1547-registry-persistence/ - registry Run key persistence test and Splunk detection

\- screenshots/04-technique-t1218-t1003-defender-blocked/ - Windows Defender blocking the LOLBin and LSASS credential dumping attempts

\- screenshots/05-technique-t1071-outbound-connection/ - suspicious outbound connection test with Splunk and Wazuh detection

