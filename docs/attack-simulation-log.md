\# Attack Simulation Log



All simulations executed via Atomic Red Team on win-victim (Wazuh agent name: DESKTOP-5BMPUDK, IP 192.168.50.128). Detections validated independently in both Wazuh and Splunk, which receive telemetry from the same Sysmon source via two separate pipelines (Wazuh agent, and Splunk Universal Forwarder).



| # | Technique | Test | Timestamp | Result | Splunk Detection | Wazuh Detection |

|---|---|---|---|---|---|---|

| 1 | T1059.001 - PowerShell | Manual base64 -EncodedCommand execution | 2026-09-01 \~16:xx | Success | index=sysmon EventCode=1 CommandLine=\*EncodedCommand\* | Matched encoded-PowerShell custom rule |

| 2 | T1547.001 - Registry Run Keys | "Reg Key Run" Test #1 | 2026-09-01 17:05:24 | Success, exit code 0 | index=sysmon EventCode=13 TargetObject=\*CurrentVersion\\Run\* - 20 events | 21 hits in window, registry persistence rule category |

| 3 | T1218.010 - Regsvr32 LOLBin | "Regsvr32 local COM scriptlet execution" Test #1 | 2026-09-01 17:36 | Blocked by Windows Defender - exit code 5. Confirmed via Get-MpThreatDetection: multiple atomics payloads flagged as real malware | N/A | N/A |

| 4 | T1003.001 - LSASS Memory | Mimikatz + Invoke-Mimikatz -DumpCreds Test #1 | 2026-09-01 16:37 | Blocked by Windows Defender - Access denied on process start | N/A | N/A |

| 5 | T1071.001 - Web Protocols | "Malicious User Agents - Powershell" Test #1 | 2026-09-01 18:32:32 | Success, exit code 0 | index=sysmon EventCode=3 - 154 events | 11 hits in 30-min window |



\## Key findings



Successful detections (3/5): Encoded PowerShell, registry-based persistence, and suspicious outbound connections were all cleanly executed and independently confirmed in both Wazuh and Splunk - validating the end-to-end pipeline from Sysmon to forwarder/agent to SIEM to alert.



AV-blocked techniques (2/5): Windows Defender's signature-based detection intercepted both the LSASS credential-dumping attempt (Mimikatz) and the Regsvr32 LOLBin execution before Sysmon or either SIEM ever observed the activity. This demonstrates defense-in-depth, where a properly-configured endpoint AV catches known-malicious tooling pre-execution, upstream of the SIEM layer entirely. It also highlights a blind spot worth noting: if Defender is ever misconfigured, disabled, or bypassed by an attacker, these techniques would need to rely entirely on the Sysmon/Wazuh/Splunk detection layer, which was not exercised for these two techniques in this test run.



\## Troubleshooting notes worth preserving



\- Naming mismatch: The Wazuh agent registers under the actual Windows hostname (DESKTOP-5BMPUDK), not the VMware Workstation VM label (win-victim). Always verify the registered agent name via the Wazuh Endpoints page.

\- Clock drift: win-victim and splunk-server clocks drifted apart during the lab session, causing indexed events to be invisible under default relative time ranges. Always confirm host clocks are synchronized, prefer "All time" when validating data exists at all.

\- PowerShell session state does not persist: Set-ExecutionPolicy -Scope Process and imported modules only apply to the session they were run in. Every new PowerShell window requires re-running the execution policy bypass and module import.

