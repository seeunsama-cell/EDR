# SentinelOne EDR Missed Detection Context Handoff

## Purpose

This folder is a handoff package for continuing the SentinelOne Server EDR missed-detection support work in another PC or a new Codex thread.

It does not attempt to move the original Codex conversation. Instead, it captures the operational context that a new thread needs to continue work immediately:

- customer issue background
- investigation summary
- key technical decisions
- SentinelOne KB references
- Event Search queries
- mail/reply templates
- weekly report wording
- remaining action items

## Start Prompt For New Codex Thread

Use this prompt in a new Codex thread:

```text
이 repo의 codex-context/sentinelone-edr-missed-detection/ 폴더를 읽고 SentinelOne 서버 EDR 악성 명령어 탐지 테스트 미탐 건 업무를 이어가줘.

먼저 00_README.md부터 07_open_items.md까지 순서대로 읽고, 현재 이슈 상태/남은 작업/메일 회신/주간보고 문구를 파악해줘.

중요:
- 기존 작업 파일이나 unrelated changes는 건드리지 말 것.
- 민감정보는 마스킹해서 다룰 것.
- SentinelOne 콘솔/메일/KB 내용을 근거로 고객 대응 문구를 작성할 것.
```

## Reading Order

Read the files in this order:

1. `00_README.md`
2. `01_context_summary.md`
3. `02_key_decisions.md`
4. `03_references.md`
5. `04_queries_or_rules.md`
6. `05_mail_templates.md`
7. `06_weekly_report_notes.md`
8. `07_open_items.md`

## Security Notes

- Do not paste SentinelOne access tokens into prompts, docs, commits, or emails.
- Exact customer tenant URLs, endpoint UUIDs, IP addresses, personal emails, and file hashes should be treated as internal data.
- This handoff uses placeholders such as `<CONSOLE_URL>`, `<LINUX_AGENT_UUID>`, `<WINDOWS_AGENT_UUID>`, `<TEST_IP>`, and `<CASE_ID>`.
- If exact values are needed, retrieve them from the SentinelOne console, Gmail thread, or approved internal artifacts.
- Do not commit downloaded Agent logs, exploit samples, scripts, hashes, or screenshots unless explicitly approved.

