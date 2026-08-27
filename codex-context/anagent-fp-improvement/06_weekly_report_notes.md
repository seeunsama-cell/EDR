# Weekly Report Notes

## anagent Additional Detection Review

Use this wording:

```text
○ anagent 추가 탐지 건 예외처리 방식 검토
  - ap02.fms.com 자산에서 anagent 관련 프로세스의 신규 SHA-256 Hash 3종이 확인되어 예외처리 요청 접수
  - 기존 anagent Tag에 신규 Hash를 추가할 경우 동일 Tag 대상 전체 서버로 예외 범위가 확대될 수 있어, ap02 전용 Tag 생성 방안 유선 문의 진행 (7/21)
  - 쉴더스 검토 결과, 해당 Hash는 단일 자산이 아닌 anagent Tag 대상 여러 자산에서 공통 탐지되는 값이며, 비허용 Source IP 접근 탐지를 위한 Custom Rule이 별도 운영 중이므로 추가 Tag 생성은 불필요한 것으로 확인
  - 기존 anagent Tag 기반 예외 정책에 신규 Hash 추가 적용 및 해당 Tag 그룹에 ap02.fms.com 자산 추가 완료
```

## Full Disk Scan Residual False Positives

Use this wording:

```text
○ Full Disk Scan 잔여 과탐 건 분석 마감
  - 6월 9일 이후 전 부문 대상 Full Disk Scan 진행으로 동일 패턴의 탐지 이벤트가 대량 누적되어 KT 측 처리 일정 문의 접수 (6/24)
  - 쉴더스 측 확인 결과, 타 긴급 대응 건 우선 처리로 인해 후순위 건인 Full Disk Scan 관련 이벤트 분석이 지연된 것으로 확인
  - 7/24까지 처리 완료 예정으로 회신
  - 금일 기준 6/9~7/7 기간 내 미처리 이벤트 0건 확인
```

## Tag-Based Exclusion Management Sheet

Use this wording:

```text
○ Tag 예외처리 자산 현황 관리
  - 태그 기반 예외처리 요청이 메일 스레드 형태로 누적되어 대상 자산, 예외 조건, 적용 현황 확인이 어려운 상태
  - Tag, Exclusion Name, 등록된 Blocklist 값, Hostname, Agent UUID, 추가일자, STAR Rule 반영 여부 기준으로 관리 시트 구성
  - 콘솔 Inventory > All Assets에서 Tags 필터 검색을 통해 태그별 자산 현황 조회 가능하도록 운영 기준 정리
```

## anagent Tag-Based Exception Completed

Short wording:

```text
○ anagent 관련 이벤트 태그 기반 예외처리 적용
  - anagent 관련 반복 오탐 개선을 위해 대상 자산에 anagent Tag 부여 및 검증된 SHA-256 Hash 예외 처리 적용
  - 비허용 Source IP의 22222 포트 접근 탐지를 위한 STAR Custom Rule 운영
  - 적용 후 동일 Alert 발생 여부 모니터링 예정
```

