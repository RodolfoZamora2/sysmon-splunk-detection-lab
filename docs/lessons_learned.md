# Lessons Learned

## 1. Log ingestion alone is not enough
Forwarding logs into a SIEM is only the starting point. The real value comes from turning telemetry into useful detections and validating that those detections actually trigger under realistic test conditions.

## 2. Sysmon provides much stronger endpoint visibility
Compared with standard Windows logs alone, Sysmon gave more detailed visibility into process execution and registry activity. This made it much easier to build targeted detections for suspicious PowerShell behavior and persistence-related actions.

## 3. Detection logic must be tested with real activity
It is not enough to write a search and assume it works. Each detection in this lab had to be validated by generating fresh activity, confirming the event was ingested, and verifying that the alert triggered as expected.

## 4. Alerting configuration matters
A working detection can still fail operationally if the alert is not configured correctly. Scheduling, time range, trigger conditions, and alert actions all mattered in making the workflow behave like a basic SOC monitoring process.

## 5. Context is critical for triage
A useful alert needs enough surrounding detail for an analyst to begin investigating. Reviewing fields like host, user, process name, parent process, command line, and registry path helped show whether the detection produced actionable context.

## 6. Controlled simulations are valuable for portfolio projects
By generating known PowerShell and registry persistence behaviors in a controlled lab, I was able to validate detection logic safely while also building strong proof for a portfolio project.

## 7. Cleanup should be part of validation
Testing persistence-related activity required cleanup afterward. Removing the test Run key showed that safe rollback and lab hygiene are important parts of hands-on security work.

## 8. This project strengthened detection engineering skills
This lab helped reinforce practical skills in:

- endpoint telemetry collection
- Sysmon log analysis
- Splunk searching and filtering
- detection engineering
- alert creation
- basic alert triage

## Conclusion

This project expanded a basic Splunk ingestion lab into a more realistic SOC-style workflow by combining endpoint telemetry, behavioral detections, alerting, validation, and triage. It was a strong step forward from simple log collection into actual defensive monitoring.
