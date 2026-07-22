# MDR/SentinelOne 면접 준비 맥락

## 목적

이 폴더는 SentinelOne 기반 MDR 운영 경험과 안랩 엔드포인트 보안 제품 기술지원 면접 준비 맥락을 새 Codex 스레드에서 이어가기 위한 문서 세트다. 원본 분석 파일이나 기존 작업물은 이 폴더에서 수정하지 않는다.

## 새 Codex 스레드 시작 프롬프트

> `codex-context/mdr-sentinelone-interview/`의 문서를 먼저 읽고, SentinelOne 기반 협업형 MDR 운영 경험을 안랩 엔드포인트 보안 제품 기술지원 직무에 맞게 이어서 정리해줘. Endpoint/Full Disk 1차 대응은 SK쉴더스가 담당했고, 나는 Custom 탭 이벤트와 주요 반복 Alert를 Event Search로 재검증해 오탐 사유·고객 문의·주간보고 근거를 만들고 예외정책과 Custom Rule 개선까지 연결했다는 역할 범위를 지켜줘. ANAGENT 태그 기반 예외처리, 22222 포트 Source IP 탐지 Rule, Linux/Windows 악성 명령어 탐지 테스트를 바탕으로 면접 답변·포트폴리오·기술지원 문서를 작성해줘. 문서의 민감정보 마스킹 원칙도 유지해줘.`

## 읽는 순서

1. `01_context_summary.md`
2. `02_key_decisions.md`
3. `03_references.md`
4. `04_queries_or_rules.md`
5. `05_mail_templates.md`
6. `06_weekly_report_notes.md`
7. `07_open_items.md`

## 보안 주의사항

- 고객사명, 계정·사이트 ID, SentinelOne 테넌트 URL, Agent UUID, IP, 파일 해시, 메일 주소는 외부 공유용 문서에서 마스킹한다.
- 이 폴더의 Rule과 쿼리는 실제 값을 제거한 구조·의사코드다.
- 원본 증적 파일을 이 폴더로 복사하지 않는다.
- 공개 저장소에 푸시하기 전 GitHub 저장소 공개 범위와 민감정보 포함 여부를 확인한다.
