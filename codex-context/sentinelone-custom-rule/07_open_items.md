# Open Items

## 남은 작업

1. SentinelOne 이벤트 ID `2525527524466213718`에 실제 노트가 작성되었는지 확인
2. 외주업체 또는 담당자가 제안 문구로 노트를 작성했는지 확인
3. 관련 이벤트의 최종 Alert Status / Analyst Verdict / Mitigation Status 확인
4. 주간보고에 반영할 경우 주차와 고객사 표기 확인
5. 메일 제목의 조직명/날짜 표기 확인
   - 사용자 언급: `[EXT] [SK쉴더스][KT] SentinelOne 탐지 이벤트 확인 요청 드립니다. (7/25)`
   - Gmail에서 확인된 제목: `[EXT] [MZCSOC] [KT_IT부문] SentinelOne 탐지 이벤트 확인 요청 드립니다.`
   - Gmail에서 확인된 발송일: 2026-07-15

## 확인 필요 사항

- SentinelOne 콘솔에서 해당 이벤트의 Raw data에 매칭된 Rule ID 또는 Rule Name이 직접 표시되는지 확인
- Endpoint Alert와 Custom Alert 간 정확한 ID 매핑이 필요한지 확인
- `Treat as threat` 설정이 켜진 다른 Custom Rule의 Endpoint 탭 노출 사례가 동일한지 확인
- 예외처리 미적용 결정이 모든 유사 이벤트에 적용되는 운영 기준인지, 해당 서버/룰에 한정된 판단인지 확인

## 다음 스레드에서 바로 할 일

1. `00_README.md`부터 읽고 전체 맥락 파악
2. 필요한 경우 Gmail에서 관련 메일 제목과 날짜 재검색
3. SentinelOne 콘솔에서 이벤트 ID `2525527524466213718` 검색
4. 노트 작성 여부 확인
5. 미작성 상태이면 `05_mail_templates.md`의 카카오톡 문구를 사용해 담당자에게 작성 요청
6. 주간보고가 필요하면 `06_weekly_report_notes.md`에서 상태별 문구 선택

## 주의사항

- 기존 repo의 삭제 파일/미추적 파일은 건드리지 말 것
- `git reset`, `git checkout --`, `git clean` 사용 금지
- 새 context 문서 외 파일은 stage하지 말 것
- IOC는 항상 defang 처리할 것
- “정탐”과 “오탐 가능성” 표현이 충돌할 수 있으므로 문맥에 따라 `정탐 처리 건`, `오탐 가능성 존재`, `모니터링 유지`로 분리해 표현할 것
- Custom Alert와 Endpoint Alert의 상태값은 자동 동기화된다고 단정하지 말 것

