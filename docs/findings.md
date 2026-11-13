# Findings and Analysis

## 1. Overview

This document summarizes the findings from endpoint threat detection experiments using Sysmon, Wazuh, Splunk, and Atomic Red Team.

## 2. Detection Results

| MITRE ATT&CK ID | Technique | Result |
|---|---|---|
| T1059.001 | PowerShell | Detected by Wazuh and Splunk |
| T1547.001 | Registry Run Keys | Detected by Wazuh and Splunk |
| T1218.010 | Regsvr32 | Blocked by Windows Defender |
| T1003.001 | LSASS Memory Access | Blocked by Windows Defender |
| T1071.001 | Application Layer Protocol | Detected by Wazuh and Splunk |

## 3. Key Observations

- Sysmon provides detailed endpoint telemetry.
- Wazuh provides centralized security monitoring.
- Splunk supports event searching and investigation.
- Atomic Red Team helps validate detection capabilities.
- Windows Defender can prevent certain simulated activities before SIEM validation.

## 4. Detection Engineering Lessons

- Endpoint telemetry visibility is essential for threat detection.
- SIEM platforms require properly configured event forwarding.
- Detection rules should be validated against actual event data.
- MITRE ATT&CK provides a useful framework for organizing detection coverage.
- Prevention events and successful SIEM detections must be distinguished.

## 5. Conclusion

The lab demonstrates how endpoint telemetry can be collected, monitored, and investigated through two independent SIEM platforms.

It also highlights the importance of validating detection rules using controlled attack simulations.
