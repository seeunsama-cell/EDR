# STAR Custom Rule - Non-Allowed IP Access Detection

## Purpose

The STAR Custom Rule is the compensating detection for the anagent tag-based exception.

Because repeated anagent-related hashes are excluded on tagged assets, the STAR rule monitors for a higher-risk network condition:

- Target is an anagent exception asset.
- Event type is `IP Connect`.
- Network direction is `INCOMING`.
- Connection status is `SUCCESS`.
- Destination port is `22222`.
- Source IP is not in the KT Ansible/Infrabot allowed IP list.

This allows repeated normal automation-related alerts to be reduced while still surfacing unexpected access to the relevant port.

## Final Rule Logic

The final rule was applied in this form:

```text
(agent.uuid in ('8d59583e-7164-3e3d-fafc-f34c63745291','8d5a16db-d5b5-1b60-3a2e-3daa8cc6aa86','8f44aa59-8ef3-fe77-0678-66495b0dd94a','8f44c88f-5c5b-dd2a-b49c-980a19231304','8e6e949f-67f9-89e1-b076-bc1573a281de','8f246c72-7cd5-7a4b-c4c3-1ed10381eb02','8f12631d-8d2b-6de0-2060-5fc802d73f71','8f125bf7-8bde-77d6-91b9-2d0e9d85ed69','8f124e2b-b5d1-44ed-a249-e28559d5347f','8f124477-7529-3625-2180-a140d29c69dd','8ea023f2-464a-82b0-5e83-fc39ce106709','8e7caf90-9c4d-a97e-40d8-b36de5cf12e7','8d59583e-7164-3e3d-fafc-f34c63745291','8d5a16db-d5b5-1b60-3a2e-3daa8cc6aa86','8e0615d8-679c-01fd-1463-9a2a84362cbd','8e086bc8-5076-b416-f4a0-3083989769e5','8e050a9c-ceae-d015-6916-2ce063ca2059','8e79d939-e2aa-8067-e136-ff7bb3a3e5b1','8e6a5623-8c4e-0665-227f-948325367789') and event.type ="IP Connect" and event.network.direction ='INCOMING' and event.network.connectionStatus = 'SUCCESS' and dst.port.number ="22222") AND NOT(src.ip.address in ('10.217.41.173', '10.217.165.19', '10.217.165.26','10.217.165.33', '10.217.165.17', '10.217.165.34','10.217.183.239', '10.217.183.98', '10.220.200.114','10.220.204.70', '10.220.204.75', '10.220.204.36','10.220.204.72', '10.220.204.49'))
```

## Allowed KT Ansible/Infrabot IP List

```text
10.217.41.173
10.217.165.19
10.217.165.26
10.217.165.33
10.217.165.17
10.217.165.34
10.217.183.239
10.217.183.98
10.220.200.114
10.220.204.70
10.220.204.75
10.220.204.36
10.220.204.72
10.220.204.49
```

## Event Search Query for Validation

Use this query to validate allowed or non-allowed access in Event Search.

For non-allowed Source IP access, use `== false`.

```text
| let AllowedIPs = array(
"10.217.41.173", "10.217.165.19", "10.217.165.26",
"10.217.165.33", "10.217.165.17", "10.217.165.34",
"10.217.183.239", "10.217.183.98", "10.220.200.114",
"10.220.204.70", "10.220.204.75", "10.220.204.36",
"10.220.204.72", "10.220.204.49"
)
| let TargetAgents = array(
"8d59583e-7164-3e3d-fafc-f34c63745291",
"8d5a16db-d5b5-1b60-3a2e-3daa8cc6aa86",
"8f44aa59-8ef3-fe77-0678-66495b0dd94a",
"8f44c88f-5c5b-dd2a-b49c-980a19231304",
"8e6e949f-67f9-89e1-b076-bc1573a281de",
"8f246c72-7cd5-7a4b-c4c3-1ed10381eb02"
)
| filter(
event.type == "IP Connect"
AND array_contains(TargetAgents, agent.uuid)
AND event.network.direction == "INCOMING"
AND event.network.connectionStatus == "SUCCESS"
AND dst.port.number == 22222
AND array_contains(AllowedIPs, src.ip.address) == false
)
| columns event.time, event.id,
agent.name, agent.uuid,
src.ip.address, src.port.number,
dst.ip.address, dst.port.number,
src.process.name, src.process.cmdline,
src.process.image.path,
event.network.direction,
event.network.connectionStatus
| sort - event.time
| limit 1000
```

For allowed Source IP access during verification, change:

```text
array_contains(AllowedIPs, src.ip.address) == false
```

to:

```text
array_contains(AllowedIPs, src.ip.address) == true
```

## Rule Maintenance

When exception target assets are added or removed:

- Update the `agent.uuid in (...)` list in the STAR Custom Rule.
- Update the tag-based exception tracking sheet.

When KT Ansible/Infrabot allowed IPs change:

- Update the `src.ip.address in (...)` allowed list.

When only additional exception hashes are added:

- Update the exception policy and hash tracking.
- STAR Custom Rule does not need hash updates because it is network-condition based.

