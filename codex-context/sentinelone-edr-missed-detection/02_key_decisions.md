# Key Decisions

## 1. Treat Log Collection And Alert Creation Separately

Decision:

Do not describe the Linux issue as an Event Collection failure if Event Search shows Process Creation telemetry for the test time window.

Reasoning:

- SentinelOne Event Search showed Linux process execution logs.
- Process command line, parent/child process information, and endpoint telemetry were present.
- The missing part was Alert promotion, not raw telemetry collection.

Customer-facing wording:

> The command execution logs were collected, so this is not considered an Event Collection failure. The issue is whether the collected event matched SentinelOne default Behavioral Detection or another Alert rule condition.

## 2. Do Not Equate Behavioral Indicator With Threat Alert

Decision:

Explain clearly that a Behavioral Indicator is not the same as a Threat Alert.

Reasoning:

- Linux logs showed indicators such as `ReadPasswdFile`, `ReadShadow`, and `ShellTrapSignal`.
- These events proved that the Agent observed some suspicious behavior.
- However, not every indicator is promoted to a Threat Alert by default.

Operational distinction:

- Event Collection: telemetry exists in Event Search.
- Behavioral Indicator: behavior was recognized as an indicator.
- Threat Alert: detection condition and confidence/severity logic caused alert creation.

## 3. Windows IIS Webshell Was Detected Because Its Indicator Matched A Windows Detection Path

Decision:

Use Windows IIS Webshell as the confirmed detected baseline.

Reasoning:

- Alert page showed `Forbidden Process Launch From IIS Webshell detected`.
- Event Search showed `IISWebshell` and `ForbiddenProcessFromIISWebshell` indicators.
- Process chain was consistent with IIS webshell behavior:
  - `w3wp.exe -> cmd.exe -> ncat.exe`
- SentinelOne Windows Behavioral Detection KB includes IIS Webshell-related entries.

## 4. Linux Webshell/RCE Test Did Not Prove Collection Failure

Decision:

Position the Linux result as "behavior did not meet default Alert conditions", not "Agent failed."

Reasoning:

- RCE/Webshell access can result in many kinds of post-exploitation behavior.
- Simple commands such as `whoami`, `hostname`, `id`, `pwd`, `netstat`, and `ifconfig` are commonly used by administrators.
- Default detection usually requires stronger behavior patterns, correlation, exploit chain context, reverse shell behavior, suspicious file access, persistence, credential access, or documented detection logic.

Important nuance:

React2Shell CVE-2025-55182 coverage may exist, but the existence of coverage does not automatically mean every post-RCE simple command generates a Threat Alert.

## 5. Windows Case Still Includes Missed Detection Scope

Decision:

Do not tell SentinelOne Support that PowerShell/VBS is out of scope.

Reasoning:

- The original customer email included Windows VBS/PS1/PowerShell command testing.
- The IIS Webshell `cmd.exe` creation was detected, but other Windows behaviors were reported as missed.
- Support's questions about PowerShell commands, VBS script contents, file hashes, and expected results are relevant.

## 6. STAR Custom Rule Is The Recommended Path For Alerting On Simple Commands

Decision:

If the customer wants simple system command execution itself to create Alerts, recommend reviewing STAR Custom Rules instead of relying only on default Behavioral Detection.

Reasoning:

- Commands such as `whoami`, `hostname`, `id`, `pwd`, and `netstat` are not inherently malicious in all server contexts.
- Default EDR behavioral detection avoids alerting on every benign admin command.
- STAR Custom Rules can turn customer-specific combinations into alert conditions.

## Changed Or Corrected Judgments

- Earlier interpretation focused heavily on Linux missed detection and Windows detected behavior.
- After rereading the original customer mail, Windows also has missed-detection scope for VBS/PS1/PowerShell behaviors.
- Current handling should keep OS-specific cases separated:
  - Linux missed detection / React2Shell / WebLogic Webshell analysis
  - Windows IIS Webshell detected baseline plus VBS/PS1/PowerShell missed analysis

