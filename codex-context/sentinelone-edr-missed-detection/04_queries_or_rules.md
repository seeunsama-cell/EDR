# Queries, Rules, And Operating Criteria

## Query Notes

- Replace placeholder UUIDs before running queries.
- Set the time range to the exact test period.
- The known test window used during analysis was around `2026-06-10 09:00` to `2026-06-11 09:00` UTC+9, but verify the final window from the customer mail or console.
- Use CSV export when submitting data to SentinelOne Support.
- Screenshots alone are not enough for vendor analysis.

## Linux: Process Creation Logs

Purpose:

Confirm whether command execution telemetry was collected.

```text
| filter( agent.uuid == "<LINUX_AGENT_UUID>" )
| filter( event.type == "Process Creation" )
| columns event.time, event.type, endpoint.name, indicator.name, indicator.category, src.process.cmdline, tgt.process.cmdline
| sort - event.time
| limit 1000
```

Interpretation:

- If commands are visible here, Event Collection is working.
- If `indicator.name = null`, the process event was collected but no Behavioral Indicator was assigned to that event.
- If no Threat Alert exists, the event likely did not satisfy Alert promotion conditions.

## Linux: Behavioral Indicator Logs

Purpose:

Check which Linux behaviors were recognized as indicators.

```text
| filter( agent.uuid == "<LINUX_AGENT_UUID>" )
| filter( event.type == "Behavioral Indicators" )
| columns event.time, event.type, endpoint.name, indicator.name, indicator.category, indicator.description, src.process.cmdline, tgt.process.cmdline
| sort - event.time
| limit 1000
```

Known observed indicators:

```text
ReadPasswdFile
ReadShadow
ShellTrapSignal
```

Interpretation:

- Indicator exists: the behavior was recognized.
- Alert exists: detection logic promoted the behavior to an Alert.
- These are different states.

## Linux: Storyline-Focused Query

Purpose:

Focus on known storylines from a Linux test run.

```text
| filter( agent.uuid == "<LINUX_AGENT_UUID>" )
| filter( src.process.storyline.id == "<STORYLINE_ID_1>" or tgt.process.storyline.id == "<STORYLINE_ID_1>" or src.process.storyline.id == "<STORYLINE_ID_2>" or tgt.process.storyline.id == "<STORYLINE_ID_2>" )
| columns event.time, event.type, endpoint.name, src.process.cmdline, tgt.process.cmdline, indicator.name, indicator.category, indicator.description
| sort - event.time
| limit 1000
```

## Windows: IIS Webshell Indicator Query

Purpose:

Show the Windows detection baseline where IIS Webshell behavior generated indicators and Alerts.

```text
| filter( agent.uuid == "<WINDOWS_AGENT_UUID>" )
| filter( indicator.name contains "IISWebshell" or indicator.name contains "ForbiddenProcessFromIIS" or indicator.name contains "ForbiddenAliveProcessFromIIS" or indicator.name contains "PossibleIISWebshell" )
| columns event.time, event.type, endpoint.name, agent.uuid, indicator.name, indicator.category, indicator.description, src.process.cmdline, tgt.process.cmdline, src.process.storyline.id, tgt.process.storyline.id
| sort - event.time
| limit 1000
```

Expected result:

- Rows with `indicator.name` such as:
  - `IISWebshell`
  - `ForbiddenProcessFromIISWebshell`
- `indicator.category` commonly observed as `Exploitation`.

## Windows: Process Chain Query

Purpose:

Show the `w3wp.exe -> cmd.exe -> ncat.exe` process chain.

```text
| filter( agent.uuid == "<WINDOWS_AGENT_UUID>" )
| filter( src.process.cmdline contains "w3wp" or src.process.cmdline contains "cmd" or src.process.cmdline contains "ncat" or tgt.process.cmdline contains "w3wp" or tgt.process.cmdline contains "cmd" or tgt.process.cmdline contains "ncat" )
| columns event.time, event.type, endpoint.name, agent.uuid, indicator.name, indicator.category, src.process.name, src.process.cmdline, src.process.parent.name, tgt.process.name, tgt.process.cmdline
| sort - event.time
| limit 1000
```

Interpretation:

- Use this to prove the execution chain.
- Use the indicator query above to prove why the Alert was generated.

## Windows: PowerShell / VBS / PS1 Missed-Detection Query

Purpose:

Find events relevant to SentinelOne Support's Windows case questions.

```text
| filter( agent.uuid == "<WINDOWS_AGENT_UUID>" )
| filter( src.process.cmdline contains "powershell" or tgt.process.cmdline contains "powershell" or src.process.cmdline contains ".ps1" or tgt.process.cmdline contains ".ps1" or src.process.cmdline contains "wscript" or tgt.process.cmdline contains "wscript" or src.process.cmdline contains "cscript" or tgt.process.cmdline contains "cscript" or src.process.cmdline contains ".vbs" or tgt.process.cmdline contains ".vbs" )
| columns event.time, event.type, endpoint.name, agent.uuid, indicator.name, indicator.category, src.process.name, src.process.cmdline, src.process.parent.name, tgt.process.name, tgt.process.cmdline, src.process.storyline.id, tgt.process.storyline.id
| sort - event.time
| limit 1000
```

Data to export:

- CSV export of result rows
- Screenshot showing query and returned rows
- Exact test time range
- Relevant storyline IDs if available

## Operating Criteria Used In Customer Explanation

Basic decision logic:

```text
If Event Search has command/process telemetry:
  collection is working.

If indicator.name is null:
  no Behavioral Indicator was assigned to that event.

If Behavioral Indicator exists but no Alert exists:
  the behavior was recognized but did not meet Alert promotion criteria.

If Alert exists:
  a detection/alert rule or behavioral detection condition was satisfied.
```

## STAR Custom Rule Recommendation

Use STAR Custom Rule review if the customer wants Alerts for simple administrative commands.

Potential rule concept:

```text
Alert when selected commands are executed from suspicious parent processes
such as web server workers, script interpreters, or known webshell paths.
```

Do not recommend alerting on every `whoami`, `hostname`, `id`, `pwd`, or `netstat` execution without context. That will likely create noisy detections in server environments.

