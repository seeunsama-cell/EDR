# SentinelOne Custom Rule Context Handoff

## 목적

이 폴더는 기존 Codex 대화 스레드에서 진행한 SentinelOne Custom Rule / Endpoint Alert 분석 맥락을 다른 PC 또는 새 Codex 스레드에서 바로 이어가기 위한 인수인계 문서 세트입니다.

대화 스레드 자체를 옮기는 것이 아니라, 새 스레드가 읽고 업무 배경, 판단 근거, 관련 룰, 메일 문구, 주간보고 문구, 남은 작업을 빠르게 파악할 수 있도록 정리했습니다.

## 새 Codex 스레드에서 사용할 시작 프롬프트

```text
이 repo의 codex-context/sentinelone-custom-rule/ 폴더를 읽고, SentinelOne Custom Rule 기반 Endpoint Alert 분석 업무 맥락을 이어가줘.

먼저 00_README.md부터 07_open_items.md까지 순서대로 읽고, 현재까지 정리된 판단과 남은 작업을 요약한 뒤 다음 작업을 진행해줘.

주의:
- 기존 repo 파일이나 unrelated changes는 건드리지 말 것.
- IOC/IP/URL은 외부 전달 시 반드시 defang 처리할 것.
- SentinelOne Custom alerts 탭과 Endpoint 탭의 차이를 혼동하지 말 것.
```

## 읽는 순서

1. `00_README.md` - 폴더 목적, 시작 프롬프트, 보안 주의사항
2. `01_context_summary.md` - 전체 업무 배경과 현재 상태
3. `02_key_decisions.md` - 주요 판단 및 변경된 판단
4. `03_references.md` - 관련 파일, 메일, 링크, 산출물
5. `04_queries_or_rules.md` - SentinelOne 룰/쿼리/운영 기준
6. `05_mail_templates.md` - 카톡/메일/문의 문구
7. `06_weekly_report_notes.md` - 주간보고용 문구
8. `07_open_items.md` - 남은 작업과 다음 스레드 주의사항

## 보안 주의사항

- 이 문서는 내부 업무 인수인계를 목적으로 작성되었습니다.
- 악성 IOC는 자동 링크 방지를 위해 defang 형식으로 표기합니다.
  - 예: `172[.]240[.]253[.]132`
  - URL 예: `hxxps://example[.]com`
- 개인 이메일 주소, 전체 수신자 목록, 계정 정보, 콘솔 세션 정보는 문서화하지 않습니다.
- SentinelOne 콘솔 URL은 내부 환경 정보가 포함될 수 있으므로 외부 공유 시 제거하거나 마스킹합니다.
- 본 문서의 룰명, 이벤트 ID, 호스트명은 업무 재현과 추적에 필요한 최소 범위로 유지했습니다.

