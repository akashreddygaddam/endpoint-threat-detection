# System Architecture

## Overview

This project implements an endpoint threat detection and monitoring lab using Sysmon, Wazuh, Splunk, and Atomic Red Team.

The lab demonstrates how endpoint telemetry is collected, forwarded to SIEM platforms, and analyzed to identify suspicious activities.

## Lab Environment

The environment consists of three virtual machines:

| Virtual Machine | Purpose |
|---|---|
| Windows Endpoint | Generates endpoint telemetry |
| Wazuh Server | Centralized log monitoring and detection |
| Splunk Server | Log analysis and threat investigation |

## Technology Stack

- **Sysmon:** Collects detailed Windows system activity.
- **Wazuh:** Monitors endpoint events and generates security alerts.
- **Splunk:** Searches, analyzes, and visualizes security events.
- **Atomic Red Team:** Simulates adversary techniques.
- **MITRE ATT&CK:** Maps simulated activities to adversary techniques.
- **VMware Workstation:** Hosts the virtual lab environment.

## Detection Workflow

1. Atomic Red Team executes controlled attack simulations.
2. Sysmon records relevant Windows endpoint activity.
3. Wazuh Agent forwards telemetry to the Wazuh Manager.
4. Splunk Universal Forwarder forwards telemetry to Splunk Enterprise.
5. Wazuh rules and Splunk searches analyze collected events.
6. Security alerts and investigation results are documented.

## SIEM Architecture

Wazuh and Splunk operate as two independent detection and analysis pipelines.

Both platforms receive endpoint telemetry and provide separate investigation capabilities.

## Security Scope

All attack simulations are intended for an isolated lab environment using systems owned or explicitly authorized for testing.
