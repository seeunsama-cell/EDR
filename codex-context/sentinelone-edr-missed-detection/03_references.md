# References

## Related Mail Subjects

- `Fwd: [EXT] [확인요청] 서버EDR 악성 명령어 탐지 테스트간 미탐 원인 및 테스트 방법 검증 확인 요청드립니다`
- `Re: [EXT] 서버EDR Alert 미발생 관련 문의`
- `Request updated - SentinelOne_ref: 01652963`
- `SentinelOne Request Received: 01653221`
- `Request updated - SentinelOne_ref: 01653221 - Request for Analysis: Missed Detections in Server EDR Windows Command Testing & Method Validation`

## SentinelOne Support Cases

- `01652963`: broad / Linux-related original analysis case
- `01653221`: Windows command testing / missed detection method validation case

## SentinelOne KB Links

These links require SentinelOne Customer Portal access.

- Linux Exploitation Behavioral Detections:
  - https://community.sentinelone.com/s/article/000012084
- Linux Post-Exploitation Behavioral Detections:
  - https://community.sentinelone.com/s/article/000012092
- Windows Exploitation Behavioral Detections:
  - https://community.sentinelone.com/s/article/000012062
- React2Shell / CVE-2025-55182 coverage-related KB:
  - https://community.sentinelone.com/s/article/000011932

## Important Console Objects

Use exact values from the SentinelOne console or approved artifacts. The values below are intentionally masked.

Linux:

- Endpoint name: `ubuntu`
- Agent UUID: `<LINUX_AGENT_UUID>`
- Observed indicators:
  - `ReadPasswdFile`
  - `ReadShadow`
  - `ShellTrapSignal`

Windows:

- Endpoint name: `WIN-GT6PQ6AGTUF`
- Agent UUID: `<WINDOWS_AGENT_UUID>`
- Observed process chain:
  - `w3wp.exe -> cmd.exe -> ncat.exe`
- Observed indicators:
  - `IISWebshell`
  - `ForbiddenProcessFromIISWebshell`
- Observed Alert:
  - `Lateral movement: Forbidden Process Launch From IIS Webshell detected`

## Local Artifacts Mentioned During Work

Do not assume these files are committed. Check local availability before using them.

- `서버EDR_미탐원인분석_메일본문_캡처포함.docx`
- `서버EDR_미탐원인분석_메일본문_캡처포함.html`
- `SentinelOne_Case_01653221_Response.docx`
- `SentinelOne_Linux_React2Shell_Followup_GoogleDocs_sanitized.docx`
- `서버EDR_벤더확인자료_전달메일_초안_GoogleDocs.docx`
- `서버EDR_PDF_내용_전체_전사.txt`

## Screenshots Mentioned During Work

Screenshots were attached through Codex clipboard temp paths. They may not exist on another PC. If needed, recapture from the SentinelOne console.

Key screenshot categories:

- Windows Alert page filtered by `IIS Webshell`
- Windows Event Search for `IISWebshell` / `ForbiddenProcessFromIISWebshell`
- Windows Event Search for `w3wp` / `cmd` / `ncat`
- Linux Event Search showing `indicator.name = null`
- Linux Event Search showing `ReadPasswdFile`, `ReadShadow`, `ShellTrapSignal`
- SentinelOne KB page showing OS-specific Behavioral Detection table

