# Detection Engineering

## Overview

This document describes the endpoint detection workflow implemented using Sysmon, Wazuh, Splunk, and Atomic Red Team.

## 1. Sysmon – Endpoint Telemetry

Sysmon collects detailed Windows system activity.

Important event categories include:

| Event ID | Description |
|---|---|
| 1 | Process Creation |
| 3 | Network Connection |
| 7 | Image Loaded |
| 11 | File Created |
| 12 | Registry Object Created or Deleted |
| 13 | Registry Value Set |
| 22 | DNS Query |

## 2. Wazuh – Security Monitoring

Wazuh receives endpoint telemetry and analyzes events using its detection rules.

Detection engineering activities include:

- Monitoring Windows security events.
- Creating custom detection rules.
- Investigating suspicious endpoint activity.
- Reviewing alert severity and descriptions.

## 3. Splunk – Log Analysis

Splunk is used to search, filter, correlate, and investigate endpoint telemetry.

Example investigation areas:

- Suspicious PowerShell execution.
- Unusual process creation.
- Registry modifications.
- Suspicious network connections.
- Scheduled task creation.

## 4. Atomic Red Team – Detection Validation

Atomic Red Team is used to simulate selected MITRE ATT&CK techniques in an isolated environment.

The generated telemetry is reviewed to determine whether the monitoring and detection systems identify the activity.

## 5. MITRE ATT&CK Mapping

| Technique | ID | Activity |
|---|---|---|
| PowerShell | T1059.001 | Command execution |
| Windows Command Shell | T1059.003 | Command execution |
| Windows Registry | T1112 | Registry modification |
| Scheduled Task | T1053.005 | Scheduled task creation |

## 6. Detection Workflow

1. Simulate an attack technique.
2. Generate endpoint activity.
3. Collect telemetry through Sysmon.
4. Forward events to Wazuh and Splunk.
5. Review alerts and search results.
6. Map observed activity to MITRE ATT&CK.
7. Document detection results and findings.

## 7. Detection Validation

Each detection should be evaluated based on:

- Whether the expected event was generated.
- Whether telemetry reached the SIEM.
- Whether a detection rule matched.
- Whether an alert was generated.
- Whether the activity could be investigated.

Actual detection results must be documented using supporting evidence.
