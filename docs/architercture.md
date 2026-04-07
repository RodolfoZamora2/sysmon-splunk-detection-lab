# Architecture

## Lab Overview

This lab was built to simulate a small SOC-style endpoint detection workflow using Sysmon telemetry, Splunk Universal Forwarder, and Splunk Enterprise.

A Windows host generated Sysmon events, the Splunk Universal Forwarder collected those events from the Sysmon Operational log, and Splunk Enterprise ingested and indexed the telemetry for detection and alerting.

## Components

### Windows 11 Host
The Windows host served as the telemetry source. Sysmon was installed to generate high-value endpoint visibility, including process creation and registry modification events.

### Sysmon
Sysmon provided endpoint telemetry used for detection engineering in this lab. The primary events used were:

- Event ID 1: Process Create
- Registry-related events associated with Run key persistence activity

### Splunk Universal Forwarder
The Splunk Universal Forwarder was installed on the Windows host and configured to collect Sysmon Operational logs and send them to Splunk Enterprise.

### Splunk Enterprise
Splunk Enterprise was used to:

- ingest Sysmon telemetry
- search endpoint activity
- build detections
- convert detections into alerts
- review triggered alerts for triage

## Data Flow

1. Sysmon generates endpoint telemetry on the Windows host
2. Sysmon Operational logs are written to Windows Event Logs
3. Splunk Universal Forwarder collects the Sysmon log source
4. Telemetry is forwarded into Splunk Enterprise
5. Splunk searches identify suspicious behavior
6. Scheduled alerts trigger when matching results are found
7. Triggered alerts are reviewed for validation and triage

## Detection Coverage

The lab validated four detections:

1. Suspicious PowerShell Bypass Detection
2. EncodedCommand PowerShell Detection
3. Hidden PowerShell Detection
4. Registry Persistence Run Key Detection

These detections were designed to show how endpoint telemetry can be turned into practical SOC-style monitoring content.

## Validation Approach

Each detection was validated through controlled test activity on the Windows host. Matching events were confirmed in Splunk, converted into alerts, and then reviewed through Triggered Alerts and event details.

This demonstrated end-to-end workflow coverage from telemetry generation to alert triage.

## Security Value

This lab demonstrates:

- endpoint visibility with Sysmon
- centralized log collection into Splunk
- detection engineering from raw telemetry
- alert validation through controlled simulation
- basic triage of suspicious endpoint activity

It moves beyond simple log forwarding by showing how host telemetry can support practical detection and alerting use cases in a SIEM.
