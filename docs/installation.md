# Installation and Configuration

## 1. Lab Environment

The project uses VMware Workstation with three virtual machines.

| System | Role |
|---|---|
| Windows VM | Endpoint monitoring and attack simulation |
| Wazuh VM | Security monitoring and alert generation |
| Splunk VM | Log analysis and investigation |

## 2. Required Technologies

### Sysmon
Microsoft Sysinternals Sysmon provides detailed Windows system activity monitoring.

### Wazuh
Wazuh provides centralized endpoint monitoring, log analysis, and security detection.

### Splunk
Splunk Enterprise provides log indexing, searching, and security investigation capabilities.

### Atomic Red Team
Atomic Red Team provides controlled adversary technique simulations for detection validation.

## 3. Configuration Overview

### Step 1 – Windows Endpoint
- Install Sysmon.
- Configure Sysmon event collection.
- Verify Windows event generation.
- Install and configure Wazuh Agent.
- Configure Splunk Universal Forwarder.

### Step 2 – Wazuh Server
- Install Wazuh Manager.
- Enroll the Windows endpoint.
- Configure custom detection rules.
- Verify incoming endpoint telemetry.
- Review generated security alerts.

### Step 3 – Splunk Server
- Install Splunk Enterprise.
- Configure the Universal Forwarder connection.
- Verify incoming Windows telemetry.
- Create and execute SPL detection searches.

### Step 4 – Attack Simulation
- Install Atomic Red Team.
- Review the selected ATT&CK test.
- Create a VM snapshot.
- Execute authorized simulations.
- Review Sysmon events.
- Validate Wazuh and Splunk results.
- Document findings.

## 4. Validation

Confirm that:

- Sysmon generates endpoint events.
- Wazuh receives the expected telemetry.
- Splunk indexes the expected events.
- Detection rules identify relevant activity.
- Simulation outcomes are documented accurately.

## 5. Safety

Perform all simulations in an isolated lab environment using authorized systems.

Keep VM snapshots before testing and clean up test artifacts afterward.

Do not publish credentials, private IP configurations, or sensitive logs.
