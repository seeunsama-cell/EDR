# Open Items

## Immediate Next Steps

1. Confirm exact Windows missed-detection scope.
   - Identify whether the missed behavior was PowerShell, VBS, PS1, reverse shell, or CMD command execution.
   - Confirm whether it was launched via IIS Webshell, local execution, scheduled task, or another path.

2. Collect artifacts requested by SentinelOne Support for case `01653221`.
   - Application name and execution method
   - Exact PowerShell commands
   - VBS script contents
   - PS1 script contents if applicable
   - ASPX Webshell filename and contents if shareable
   - SHA1/SHA256 hashes
   - Expected detection result
   - Windows Agent logs collected during the test

3. Collect Linux support artifacts for the Linux case.
   - Exact React2Shell and WebLogic test commands
   - Test time window and timezone
   - Linux Agent version and policy
   - Live Security Update status if visible
   - Linux Agent logs collected immediately after test

4. Reconcile Windows Agent UUID discrepancy.
   - Some prior notes showed different Windows UUID values.
   - Use the SentinelOne console and exported CSVs as source of truth.
   - Do not mix UUIDs between tenants, scopes, or endpoint reinstall states.

5. Confirm React2Shell coverage expectations with SentinelOne.
   - Does coverage apply to exploit payload, exploit attempt, or post-exploitation behavior?
   - What Agent version / policy / LSU status is required?
   - Should simple post-RCE commands create an Alert?

## Things To Avoid

- Do not claim "Agent missed everything" when Event Search telemetry exists.
- Do not claim "Indicator means Alert" because these are separate states.
- Do not state a detection rule is absent only because a string is not found in KB. KB visibility may not expose every internal condition.
- Do not provide exploit payloads or malicious scripts in broad emails unless approved and required by Support.
- Do not paste SentinelOne tokens, exact tenant tokens, or sensitive sample contents into commits.

## Next Codex Thread Checklist

Start with:

1. Read all files in this folder.
2. Ask whether the user wants:
   - customer reply
   - SentinelOne Support reply
   - weekly report wording
   - resume/self-introduction wording
   - console query guidance
3. If Gmail is mentioned, use Gmail connector to read the latest thread before drafting.
4. If Google Docs export is requested, use Google Docs / Documents workflow and verify output.
5. If SentinelOne console interaction is requested, use the in-app browser or Chrome control skill only if explicitly needed and session is available.

## Current Best One-Line Explanation

```text
Linux는 명령 실행 로그 수집은 정상이나 기본 Behavioral Detection의 Alert 승격 조건에는 대부분 매칭되지 않았고, Windows IIS Webshell은 w3wp.exe 하위 cmd.exe/ncat 실행 체인이 IISWebshell 계열 Indicator와 매칭되어 Alert가 발생한 것으로 정리됩니다.
```

