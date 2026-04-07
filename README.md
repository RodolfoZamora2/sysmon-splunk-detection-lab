## Detection Coverage

This lab was expanded from a basic Splunk log-ingestion setup into a small SOC-style detection and alerting workflow using **Sysmon telemetry** forwarded from a Windows host into **Splunk Enterprise**.

The detections built and validated in this lab were:

### 1. Suspicious PowerShell Bypass Detection

This detection looks for **Sysmon Event ID 1 (Process Create)** events involving `powershell.exe` with suspicious arguments such as:

- `-ExecutionPolicy`
- `Bypass`
- `-NoProfile`

A controlled test was executed with:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -Command "whoami"
```

This validated that Splunk could detect PowerShell executions using bypass-style arguments often associated with attacker tradecraft.

### 2. EncodedCommand PowerShell Detection

This detection looks for PowerShell executions using -EncodedCommand, which is commonly used to obfuscate command-line activity.

A controlled test was executed with:
```powershell
powershell.exe -NoProfile -EncodedCommand VwBoAG8AYQBtAGkA
```
This validated that Splunk could detect encoded PowerShell command execution from Sysmon process creation events.

### 3. Hidden PowerShell Detection

This detection looks for PowerShell launched with hidden window arguments, which can indicate stealthy execution.

A controlled test was executed with:
```powershell
powershell.exe -WindowStyle Hidden -Command "whoami"
```
This validated detection of PowerShell launched in a less visible way from the user’s perspective.

### 4. Registry Persistence Run Key Detection

This detection looks for registry modifications associated with Run key persistence, specifically under:

HKCU\Software\Microsoft\Windows\CurrentVersion\Run

A controlled test was executed with:

```powershell
reg add HKCU\Software\Microsoft\Windows\CurrentVersion\Run /v UpdaterTest /t REG_SZ /d "C:\Windows\System32\notepad.exe" /f
```
This validated that Splunk could detect suspicious persistence-related registry activity from Sysmon telemetry.

## Alerting Workflow

For each detection, a Splunk alert was created and configured to:

- run on a scheduled cron interval of every 15 minutes
- search the last 15 minutes of data
- trigger when the number of results was greater than 0
- add matching detections to Triggered Alerts

The following alerts were created and validated:

- Suspicious PowerShell Bypass Detection
- EncodedCommand PowerShell Detection
- Hidden PowerShell Detection
- Registry Persistence Run Key Detection

This demonstrates a basic SOC workflow of:

1. collect endpoint telemetry
2. search for suspicious behavior
3. convert detections into alerts
4. validate triggered alerts
5. review matching event details for triage

## Validation Summary

The lab was validated end to end through controlled attack simulation and alert verification.

Validation included:

- confirming Sysmon was installed and generating events on the Windows host
- confirming Sysmon Operational logs were visible in Event Viewer
- forwarding Sysmon logs into Splunk through the Splunk Universal Forwarder
- confirming Sysmon events appeared in Splunk with the expected source and sourcetype
- building detections for suspicious PowerShell activity and registry persistence
- converting detections into Splunk alerts
- generating fresh test activity to trigger those alerts
- reviewing triggered alerts and matching event details in Splunk

This moved the project beyond simple log ingestion and into detection engineering + alert validation + basic triage.

## Triage Notes

For each validated alert, matching event details were reviewed in Splunk to inspect:

- event time
- host
- user context
- source and sourcetype
- command line
- parent process
- suspicious arguments or persistence-related artifacts

This helped demonstrate that detections were not only firing, but were also producing enough context for a junior SOC analyst to begin triage.

## Cleanup

After testing registry persistence behavior, the test Run key value was removed with:
```powershell
reg delete HKCU\Software\Microsoft\Windows\CurrentVersion\Run /v UpdaterTest /f
```
This ensured the environment was cleaned up after validation.

## Screenshots

The screenshots below document the project from setup through detection, alerting, validation, and cleanup.

### Sysmon Setup and Verification
- 01-sysmon-installed.png
- 02-sysmon-config-verified.png
- 03-sysmon-operational-log-events.png
- 04-sysmon-inputs-conf-configured.png
- 05-splunk-forwarder-restarted-after-sysmon.png
- 06-sysmon-events-ingested-in-splunk.png
### PowerShell Detection and Alerting
- 07-powershell-process-create-detection.png
- 08-suspicious-powershell-arguments-detection.png
- 09-test-powershell-bypass-detected.png
- 10-suspicious-powershell-alert-configured.png
- 11-suspicious-powershell-alert-triggered.png
### EncodedCommand Detection and Alerting
- 12-encodedcommand-powershell-detection.png
- 13-encodedcommand-alert-configured.png
- 14-encodedcommand-alert-triggered.png
- 15-encodedcommand-alert-triage-details.png
### Hidden PowerShell Detection and Alerting
- 16-hidden-powershell-detection.png
- 17-hidden-powershell-alert-configured.png
- 18-hidden-powershell-alert-triggered.png
- 19-hidden-powershell-alert-triage-details.png
### Registry Persistence Detection and Alerting
- 20-registry-persistence-detection.png
- 21-registry-persistence-alert-configured.png
- 22-registry-persistence-alert-triggered.png
- 23-registry-persistence-alert-triage-details.png
- 24-registry-persistence-cleanup.png
