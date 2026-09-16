# Attack Simulation Log

This log documents the 5 Atomic Red Team tests executed against `win-victim` during lab testing. Exact timestamps and rule IDs were not recorded at the time of testing — those fields are marked below rather than estimated.

---

## Test 1 — T1059.001: PowerShell (Encoded Command)

- **Command executed:** Manual `powershell.exe -EncodedCommand <base64>` execution
- **Expected behavior:** Sysmon Event ID 1 (Process Creation) logs the encoded command line
- **Result:** ✅ Success
- **Detected in:** Splunk + Wazuh
- **Exact timestamp / rule ID:** *Not recorded at time of testing*

---

## Test 2 — T1547.001: Registry Run Key Persistence

- **Atomic Test:** Test #1 (`Invoke-AtomicTest T1547.001 -TestNumbers 1`)
- **Expected behavior:** Sysmon Event ID 13 (Registry value set) under `HKCU\...\Run`
- **Result:** ✅ Success, exit code 0
- **Detected in:** Splunk + Wazuh
- **Exact timestamp / rule ID:** *Not recorded at time of testing*

---

## Test 3 — T1218.010: Regsvr32 (LOLBin)

- **Atomic Test:** Test #1
- **Expected behavior:** Regsvr32 loading a remote/local COM scriptlet
- **Result:** ❌ Blocked pre-execution by Windows Defender
- **Detection classification:** `HackTool:Win32/PortMon!AMTB`
- **Notes:** Confirmed via `Get-MpThreatDetection` — never reached Sysmon/SIEM layer. Documented as a defense-in-depth finding, not a detection gap.
- **Exact timestamp:** *Not recorded at time of testing*

---

## Test 4 — T1003.001: LSASS Memory Access (Mimikatz)

- **Atomic Test:** Test #1
- **Expected behavior:** Sysmon Event ID 10 (ProcessAccess) targeting `lsass.exe`
- **Result:** ❌ Blocked pre-execution by Windows Defender
- **Detection classification:** `Trojan:Win64/Phave.MK`
- **Notes:** Confirmed via `Get-MpThreatDetection` — same as Test 3, a real-time AV signature match before Sysmon telemetry could capture it.
- **Exact timestamp:** *Not recorded at time of testing*

---

## Test 5 — T1071.001: Malicious User Agent / C2-style Traffic

- **Atomic Test:** Test #1
- **Expected behavior:** Sysmon Event ID 3 (Network Connection) with an anomalous user agent string
- **Result:** ✅ Success, exit code 0
- **Detected in:** Splunk + Wazuh
- **Exact timestamp / rule ID:** *Not recorded at time of testing*

---

## Summary Table

| # | Technique | Result | Detected In |
|---|---|---|---|
| 1 | T1059.001 | Success | Splunk + Wazuh |
| 2 | T1547.001 | Success | Splunk + Wazuh |
| 3 | T1218.010 | Blocked by Defender | N/A |
| 4 | T1003.001 | Blocked by Defender | N/A |
| 5 | T1071.001 | Success | Splunk + Wazuh |

---

**Note on evidence:** Screenshots for these tests were not saved during original testing. If asked about this project in an interview, be prepared to speak to the methodology, the rules/searches you wrote, and the reasoning behind each result — that's what actually demonstrates the skill, more than any single screenshot would.
