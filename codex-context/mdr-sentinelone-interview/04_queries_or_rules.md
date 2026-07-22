# Queries and Rules

## Event Search 분석 순서

실제 고객 값은 넣지 않고 다음 순서로 조회한다.

1. `agent.uuid` 또는 `endpoint.name`으로 대상 자산을 좁힌다.
2. `event.time`으로 Alert 발생 전후 시간대를 제한한다.
3. `event.type`에서 Process Creation, IP Connect, 파일 관련 이벤트를 확인한다.
4. `src.process.name`, `src.process.cmdline`, `src.process.parent.name`으로 프로세스 계보를 확인한다.
5. `src.process.image.path`, `src.process.image.sha256`으로 파일을 식별한다.
6. `src.process.storyline.id`로 같은 실행 흐름을 묶는다.
7. 네트워크 연결 필드와 Source IP, 목적지 포트를 정상 자동화 정보와 대조한다.

## 필드 체크리스트

- 자산: `agent.uuid`, `endpoint.name`, `os.name`, `site.name`
- 프로세스: `src.process.name`, `src.process.cmdline`, `src.process.parent.name`, `src.process.parent.cmdline`
- 파일: `src.process.image.path`, `src.process.image.sha1`, `src.process.image.sha256`
- 행위: Process Creation, 파일 생성·삭제, 네트워크 연결, Indicator, Storyline
- 운영 맥락: 접속 주체, Source IP, 자동화 도구, 발생 시간, 반복 여부

## 태그 기반 예외 조건 의사코드

```text
agent.tag == "anagent"
AND file.sha256 == "<APPROVED_HASH>"
AND event matches the known normal automation behavior
```

해시·자산·IP 값은 실제 문서에 기록하지 않는다. 태그만으로 예외를 적용하지 않고 파일 식별자와 정상 운영 근거를 함께 요구한다.

## STAR Custom Rule 의사코드

```text
target_agent.tag == "anagent"
AND direction == "inbound"
AND destination_port == 22222
AND source_ip NOT IN <APPROVED_AUTOMATION_IP_SET>
=> generate custom alert
```

Rule 적용 전 Event Search에서 대상 Agent, 방향, 포트, Source IP 조건을 검증하고, 고객사·솔루션 담당 부서 검토 후 운영 반영한다.

## Linux·Windows 탐지 테스트 판정 기준

```text
raw event collected?
  -> Behavioral Indicator created?
    -> Threat Alert promoted?
```

명령어 문자열만으로 항상 Alert가 발생한다고 가정하지 않는다. 프로세스 관계, 파일 접근·생성, 스크립트 경로, 네트워크 연결, 정책 조건과 KB 기준을 함께 본다.

## 운영 기준

- Alert 이름만으로 정탐·오탐을 판정하지 않는다.
- `Resolved`, `Analyst Verdict`, `Mitigation Status`를 구분한다.
- 예외를 제안할 때 정상 업무 범위와 공격자 악용 가능성을 함께 검토한다.
- 고객 보고서에는 이벤트 근거, 정상 운영 근거, 적용한 조치, 잔여 위험을 구분해 기록한다.
