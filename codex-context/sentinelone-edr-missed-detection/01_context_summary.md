# Context Summary

## Work Objective

Handle a customer inquiry about SentinelOne Server EDR missed detections during malicious command execution tests, and prepare technically defensible customer replies, vendor case replies, weekly report notes, and internal handoff material.

The core customer question was:

> Linux/Windows server EDR malicious command tests were performed, but several commands did not generate Alerts. Is this a collection failure, a detection logic limitation, or a test methodology issue?

## Background

The customer performed server-side tests using Webshell/RCE/Reverse Shell style scenarios.

Known scenarios from the original mail:

- Linux:
  - React2Shell / React Server Components RCE test, CVE-2025-55182
  - Oracle WebLogic FileUpload Webshell test, CVE-2018-2894
- Windows:
  - IIS ASPX Webshell upload and command execution
  - VBS/PS1 or PowerShell/CMD-based malicious command execution

The original customer mail stated that only one Windows behavior was detected:

- Windows IIS Webshell causing `cmd.exe` process creation
- Alert name seen: `Lateral movement: Forbidden Process Launch From IIS Webshell detected`

Other Linux and Windows test behaviors were reported as missed.

## What Has Been Done

- Reviewed the original email thread and clarified that Windows also had missed-detection scope, not only Linux.
- Used SentinelOne Event Search to compare Linux and Windows test behavior.
- Distinguished three different concepts:
  - Event Collection / telemetry collection
  - Behavioral Indicator assignment
  - Threat Alert creation / promotion
- Checked Linux Event Search results:
  - Process Creation logs were collected.
  - Most simple system commands had `indicator.name = null`.
  - Some sensitive-file or shell behaviors received Behavioral Indicators, for example:
    - `ReadPasswdFile`
    - `ReadShadow`
    - `ShellTrapSignal`
  - These did not necessarily become Threat Alerts.
- Checked Windows Event Search and Alert results:
  - IIS worker process chain was confirmed:
    - `w3wp.exe -> cmd.exe -> ncat.exe`
  - Indicators confirmed:
    - `IISWebshell`
    - `ForbiddenProcessFromIISWebshell`
  - Alert confirmed for IIS Webshell behavior.
- Reviewed SentinelOne KB pages for OS-specific Behavioral Detection references.
- Drafted customer replies explaining that log collection was normal, but Alert creation depends on detection conditions.
- Prepared weekly report language summarizing status and vendor case progress.
- Opened / tracked SentinelOne Support cases for OS-specific analysis.

## Current Status

The most accurate current position is:

- Linux command execution logs were collected normally.
- Most simple command executions did not match default Linux Behavioral Detection conditions strongly enough to become Threat Alerts.
- Some Linux behaviors had Behavioral Indicators, but Indicator presence alone does not guarantee Alert creation.
- Windows IIS Webshell behavior matched a documented Windows IIS Webshell-related detection path and generated an Alert.
- Windows PowerShell/VBS missed-detection scope remains open and requires detailed test artifacts.
- SentinelOne Support requested additional information for the Windows case:
  - application name and execution method
  - PowerShell command and VBS script contents
  - file hashes
  - expected result
  - Agent logs collected during penetration testing

