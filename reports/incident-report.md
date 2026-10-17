# Windows Forensics Investigation Report

## 1. Case Information

| Field | Details |
|---|---|
| Case ID | WF-001 |
| Investigation Type | Controlled Windows Forensics Lab |
| Analyst | Neo |
| Hostname | DESKTOP-00HNA81 |
| Operating System | Microsoft Windows 11 Home Single Language |
| PowerShell Version | 5.1.26100.9711 |
| Investigation Date | 17 October 2026 |
| Primary Evidence Source | Windows Security Event Log |

## 2. Summary

This investigation examined Windows process creation activity in a controlled lab environment using native Windows security auditing, Event Viewer, and PowerShell

Process Creation auditing was configured to record successful process creation events, and process command-line logging was enabled. Windows Security Event ID 4688 records were examined, and a chronological process timeline was generated to review recorded activity

The timeline included entries for `whoami.exe`, `HOSTNAME.EXE`, `ipconfig.exe`, and `cmd.exe`, alongside other Windows processes. The inspected event details demonstrated that command-line information was available for at least one process creation event

A test artifact, `case-note.txt`, was examined. Its recorded size was 140 bytes, and its SHA-256 hash was calculated. The investigation demonstrates a practical workflow for process event analysis, timeline reconstruction, and evidence integrity documentation

The findings are limited to the records and screenshots collected during the exercise. The lab used controlled, benign activity and did not establish the presence of malicious activity

## 3. Scope

The investigation covered:

- Recording the host environment and investigation start time
- Configuring Windows process creation auditing
- Enabling process command-line logging
- Generating controlled process activity using standard Windows utilities
- Inspecting Windows Security Event ID 4688
- Examining command-line information in process creation events
- Exporting and reviewing process event records
- Reconstructing a chronological process execution timeline
- Recording test artifact metadata and its SHA-256 hash
- Preparing an evidence hash manifest

The planned collection also included Windows logon Event IDs 4624 and 4625. The available screenshots do not establish the final results of that part of the collection

The investigation did not include full disk imaging, memory acquisition, malware analysis, or examination of a live security incident

## 4. Methodology

### 4.1. Environment Documentation

The lab environment was documented using PowerShell. The recorded information included the hostname, operating system, PowerShell version, user context, and investigation start time

**Observed result:** The host was identified as `DESKTOP-00HNA81`, running Microsoft Windows 11 Home Single Language. The recorded PowerShell version was `5.1.26100.9711`. The lab start time was recorded as `2026-10-17T12:05:59.9038131+02:00`

### 4.2. Audit Configuration

Windows process creation auditing was checked using `auditpol`. The process command-line logging registry property was also inspected

**Observed result:**

- The Process Creation auditing subcategory was configured for successful events
- The registry property `ProcessCreationIncludeCmdLine_Enabled` was set to `1`
- These settings supported the collection of process creation events and command-line information where available

The configuration screenshot provides evidence of the displayed settings at the time of inspection

### 4.3. Controlled Activity

The following benign commands were used to generate process activity:

- `whoami.exe`
- `hostname.exe`
- `ipconfig.exe`
- `cmd.exe /c "echo WF-LAB-001"`
- `notepad.exe` opening the test artifact

A test file named `case-note.txt` was created under `C:\ForensicsLab\TestArtifacts\`

**Observed result:** The process timeline contains entries for `whoami.exe`, `HOSTNAME.EXE`, `ipconfig.exe`, and `cmd.exe`. Notepad activity was also recorded. These entries support that the corresponding processes were recorded by Windows, although event records alone do not prove that every command completed successfully

### 4.4. Evidence Collection

Windows Security Event ID 4688 was inspected through Event Viewer and process event data was prepared for analysis. The process timeline was generated using PowerShell

**Observed result:** Event Viewer displayed 91 filtered Event ID 4688 records. The displayed timeline contained timestamps, event IDs, computer names, executable paths, and command-line information where available

The collection workflow included CSV and XML process event exports. The available screenshots do not independently establish the complete contents of both exports or confirm the final results of the planned logon-event collection

### 4.5. Timeline Reconstruction

The process event records were arranged chronologically using their recorded timestamps and process information

**Observed result:** The timeline showed multiple process creation events during the investigation period, including entries for the intended test commands and other Windows processes. The sequence provided a useful overview of recorded system activity

The timeline was interpreted alongside the event details rather than treating every recorded process as part of the controlled test

## 5. Findings

### 5.1. Process Creation Events

**Primary event:** Event ID 4688 — A new process has been created

**Evidence source:** Windows Event Viewer and the displayed process timeline

**Finding:**

Event Viewer displayed 91 filtered Event ID 4688 records. The timeline included the following representative process entries:

- `C:\Windows\System32\whoami.exe`
- `C:\Windows\System32\HOSTNAME.EXE`
- `C:\Windows\System32\ipconfig.exe`
- `C:\Windows\System32\cmd.exe`

Additional Windows processes were also present in the timeline

These records demonstrate that Windows Security auditing captured process creation activity. The complete event records should be used to verify the precise command-line arguments and correlate individual events with the controlled test actions

The presence of a process creation event establishes that Windows recorded the event; it does not independently establish that the command completed successfully or that the activity was malicious

### 5.2. Command-Line Information

**Evidence source:** Event ID 4688 event details

**Finding:**

The inspected event details contained a populated `CommandLine` field. The displayed example showed `mmc.exe` launching `eventvwr.msc`

This confirms that command-line information was available for at least one inspected process creation event. The available evidence does not establish that every event contained command-line information

Command-line fields should be reviewed individually, and missing information should be documented as a limitation rather than inferred

### 5.3. Logon Events

**Relevant event IDs:**

- Event ID 4624 — An account was successfully logged on
- Event ID 4625 — An account failed to log on

**Evidence source:** Planned logon-event export, `logon-events.csv`, if successfully generated

**Finding:**

The available screenshots do not show the results of the logon-event query. Therefore, no conclusion is made about the number of matching successful or failed logon events

If the logon export was generated, it should be examined to record the actual results. Logon events should not be attributed to a particular process without supporting evidence

### 5.4. Test Artifact

The test artifact was examined using PowerShell file metadata and SHA-256 hashing

| Field | Value |
|---|---|
| Filename | `case-note.txt` |
| Case Reference | WF-001 |
| File Path | `C:\ForensicsLab\TestArtifacts\case-note.txt` |
| File Size | 140 bytes |
| Creation Time (UTC) | 17 October 2026, 10:14:42 AM |
| Last Write Time (UTC) | 17 October 2026, 10:14:42 AM |
| Hash Algorithm | SHA-256 |
| SHA-256 Hash | `03C5A776D452527666F24BECD890D9C433041C6141512B8F462D8061E35B7D38` |

The metadata records the file's displayed size and timestamps. The SHA-256 value provides a reference for comparing the file's contents against a later measurement

The hash does not independently establish who created the file, whether its contents are trustworthy, or whether it was unchanged before the initial measurement

### 5.5. Evidence Integrity

**Evidence source:** `evidence-hashes.csv`, if successfully generated

**Finding:**

A PowerShell command was executed to calculate SHA-256 hashes for files found in the lab's `Evidence` and `Reports` directories, excluding `evidence-hashes.csv` itself. The command returned to the PowerShell prompt, but the available screenshot does not display the manifest's contents or a subsequent verification result

The contents of `evidence-hashes.csv` must be inspected to confirm which files were included and whether the manifest was populated correctly

A later matching SHA-256 value supports that the measured file contents have not changed between the two measurements. Hashing alone does not prove the authenticity or provenance of the original evidence

## 6. Timeline Analysis

The process execution timeline was generated from Windows Security Event ID 4688 records. The following representative entries are visible in the captured timeline

| Timestamp (17 October 2026) | Process | Event ID | Interpretation |
|---|---|---|---|
| 12:15:09 PM | `whoami.exe` | 4688 | Records creation of a process commonly used to identify the current user context |
| 12:15:18 PM | `HOSTNAME.EXE` | 4688 | Records creation of a process commonly used to retrieve the computer name |
| 12:15:29 PM | `ipconfig.exe` | 4688 | Records creation of a process commonly used to inspect network configuration |
| 12:15:45 PM | `WmiPrvSE.exe` | 4688 | Records creation of a Windows Management Instrumentation provider process. Its presence alone does not establish a relationship to the test commands |
| 12:15:45 PM | `cmd.exe` | 4688 | Records creation of the Windows command interpreter. The full event should be checked to confirm the exact arguments |
| 12:16:24 PM | `Notepad.exe` | 4688 | Records Notepad process activity during the investigation. Review the event command line to confirm its relationship to the test artifact |

The timeline demonstrates that Windows recorded process creation activity during the collection period. It includes both the intended test commands and other Windows processes

The inspected event details confirm that command-line information was available for at least one process creation event. However, the available records do not prove that every command completed successfully or that every process was directly associated with the controlled test

The timeline should therefore be treated as a reconstruction of recorded process activity, interpreted alongside the full event details and supporting artifact evidence

## 7. Conclusion

The investigation demonstrated a practical Windows forensics workflow using Windows Security auditing, Event Viewer, and PowerShell on a Windows 11 workstation. Process Creation auditing was configured for successful events, and the process command-line logging property was set to `1`

Event Viewer displayed 91 filtered Event ID 4688 records. The generated timeline included entries for `whoami.exe`, `HOSTNAME.EXE`, `ipconfig.exe`, and `cmd.exe`, along with other Windows processes. The inspected event details also demonstrated that command-line information was available for at least one recorded event

The test artifact `case-note.txt` was recorded as 140 bytes, and its SHA-256 hash was calculated. These results provided practical experience in process event analysis, chronological reconstruction, file metadata examination, and integrity documentation

The findings are limited to the evidence observed and collected. The available screenshots do not establish the final logon-event results or confirm a subsequent integrity verification of the complete evidence manifest. The exercise used controlled, benign activity and did not establish the presence of malicious activity

## 8. Limitations

- The investigation covered selected Windows Security event identifiers and a defined collection period
- The findings rely on the event records and screenshots available for analysis
- Event availability and field content depend on the effective audit configuration and operating system behaviour
- The timeline includes background Windows processes as well as intended test activity
- A process creation event does not prove that the process completed successfully
- The available evidence does not confirm the final results of the logon-event collection
- The contents and subsequent verification of the complete evidence hash manifest were not demonstrated in the available screenshots
- The investigation did not include a complete disk image or memory acquisition
- Missing events or fields do not prove that an action did not occur
- SHA-256 hashes support integrity comparison but do not independently establish evidence authenticity

## 9. Evidence Inventory

The following files were part of the lab workflow. Confirm the presence of each file before treating this list as a complete inventory

| File | Purpose |
|---|---|
| `process-events.csv` | Readable export of process creation records |
| `process-events.xml` | Structured export of process creation records |
| `logon-events.csv` | Logon event export, if generated |
| `process-timeline.csv` | Chronological process event analysis |
| `evidence-hashes.csv` | SHA-256 evidence manifest, if generated |
| `lab-notes.txt` | Host and environment information |
| `test-start-time.txt` | Investigation start-time reference |
| `TestArtifacts/case-note.txt` | Controlled test artifact |

Raw event exports and other potentially sensitive forensic data should remain outside the public repository unless reviewed and appropriately sanitised

## 10. Final Assessment

This lab demonstrated the configuration and inspection of Windows process creation auditing, examination of Event ID 4688, review of command-line information, chronological timeline reconstruction, and SHA-256 hashing of a test artifact

The exercise established a foundation for further work in Windows forensics, incident response, and Blue Team operations. Further validation of the logon-event results and evidence hash manifest would improve the completeness of the investigation record

## 11. References

- [Microsoft Learn — 4688(S): A new process has been created](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4688)
- [Microsoft Learn — Audit Process Creation](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/audit-process-creation)
- [Microsoft Learn — Include command line in process creation events](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-admx-auditsettings)
- [Microsoft Learn — Get-WinEvent](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/get-winevent)
- [Microsoft Learn — Get-FileHash](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-filehash)

---

**Case Reference:** WF-001  
**Project:** Windows Forensics Home Lab  
**Author:** [neo-moganedi-tech](https://github.com/neo-moganedi-tech)
