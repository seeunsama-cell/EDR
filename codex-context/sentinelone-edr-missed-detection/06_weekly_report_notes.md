# Weekly Report Notes

## Compact Weekly Report Entry

```text
• 주요 이슈 사항
  ◦ SentinelOne 서버EDR 악성 명령어 탐지 테스트 미탐 원인 검토
    ▪ 고객 측 Linux/Windows 서버 대상 RCE/Webshell 기반 악성 명령어 탐지 테스트 수행 후 일부 행위에 대한 Alert 미발생 확인 요청
    ▪ Linux 테스트에서는 명령 실행 로그가 Event Search에 정상 수집되었으나, 대부분 indicator.name 값이 null로 확인되어 Alert 미발생
    ▪ 일부 Linux 행위에서 ReadPasswdFile, ReadShadow, ShellTrapSignal 등 Behavioral Indicator는 확인되었으나 Threat Alert로 승격되지는 않음
    ▪ Windows IIS Webshell 테스트에서는 w3wp.exe -> cmd.exe -> ncat 실행 체인과 IISWebshell / ForbiddenProcessFromIISWebshell Indicator가 확인되어 Alert 발생
    ▪ SentinelOne KB 기준으로 OS별 Behavioral Detection 항목 및 Event Collection / Behavioral Indicator / Threat Alert 차이 정리
    ▪ SentinelOne Support 케이스 오픈 후 Agent 로그, 실행 스크립트/명령어, 파일 해시, 기대 탐지 결과 등 추가 자료 제출 준비 중
```

## Status-Based Version

```text
[진행 중]
SentinelOne 서버EDR 악성 명령어 탐지 테스트 미탐 원인 검토

[내용]
고객 측에서 Linux/Windows 서버 대상 Webshell/RCE/Reverse Shell 기반 악성 명령어 탐지 테스트를 수행하였으나, 일부 행위에 대해 Alert가 발생하지 않아 미탐 여부 및 테스트 방법 검증을 요청함.

[확인 결과]
Linux 테스트 장비에서는 명령 실행 로그가 Event Search에 정상 수집되었으나, 대부분 indicator.name 값이 null로 확인되어 기본 Behavioral Detection 조건에는 매칭되지 않은 것으로 판단됨.
일부 Linux 행위에서 ReadPasswdFile, ReadShadow, ShellTrapSignal 등의 Behavioral Indicator는 확인되었으나 Threat Alert로 승격되지는 않음.
Windows IIS Webshell 테스트에서는 w3wp.exe -> cmd.exe -> ncat.exe 실행 체인과 IISWebshell / ForbiddenProcessFromIISWebshell Indicator가 확인되어 Alert가 발생함.

[조치 사항]
SentinelOne 공식 KB 기준으로 Linux/Windows Behavioral Detection 항목을 비교하고, Event Collection과 Threat Alert 생성 기준 차이를 정리하여 고객 안내자료 작성.
SentinelOne Support 케이스 오픈 후 OS별 분석 진행 중이며, 벤더 요청에 따라 Agent 로그, 테스트 명령어, 스크립트 원문, 파일 해시 등 추가 자료 취합 중.
```

## Customer Request Tracking Entry

```text
관련 메일: Fwd: [EXT] [확인요청] 서버EDR 악성 명령어 탐지 테스트간 미탐 원인 및 테스트 방법 검증 확인 요청드립니다

고객 측에서 Linux/Windows 서버 대상 악성 명령어 탐지 테스트 수행 후 일부 행위에 대해 SentinelOne Alert가 발생하지 않아 미탐 원인 및 테스트 방법 검증 요청.

[확인 내용]
Linux 환경에서는 명령 실행 이벤트가 Event Search에 정상 수집되었으나, 대부분 indicator.name 값이 null로 확인되어 Alert로 승격되지 않은 것으로 판단.
일부 민감 파일 접근 및 Shell 관련 행위에서 ReadPasswdFile, ReadShadow, ShellTrapSignal 등의 Behavioral Indicator는 확인되었으나 Threat Alert 생성 조건까지 충족되지는 않음.
Windows IIS Webshell 환경에서는 w3wp.exe 하위 cmd.exe/ncat 실행 체인과 IISWebshell / ForbiddenProcessFromIISWebshell Indicator가 확인되어 Alert 발생.

[조치 내용]
SentinelOne 공식 KB 기준으로 OS별 Behavioral Detection 기준 검토 및 고객 회신 자료 작성.
SentinelOne Support 케이스 오픈 후 OS별 분석 진행 중이며, 벤더 요청에 따라 Agent 로그 및 테스트 상세 정보 추가 제출 예정.
```

## Related Separate Issue: Server EDR Alert Not Generated After Specific Time

Use this only for the separate customer question about Alert absence after 15:35.

```text
관련 메일: Re: [EXT] 서버EDR Alert 미발생 관련 문의

고객 측에서 15:35 이후 서버 EDR Alert가 발생하지 않아, 단순 미발생 상황인지 또는 이전과 동일한 이벤트 수집/Alert 발생 이슈인지 확인 요청.

[확인 내용]
SentinelOne 콘솔에서 Scope 단위로 Alert 및 Event Search 유입 현황 확인.
IT부문 외 미디어/네트워크 부문에서는 15:35 이후에도 이벤트 및 Alert가 정상 발생함을 확인.
IT부문의 경우 Log 및 Event Search 내 텔레메트리 정보는 정상 유입되고 있었으나, Alert로 전환될 만한 탐지 이벤트가 확인되지 않아 Alert 화면에 노출되지 않은 것으로 확인.

[조치/안내]
본 건은 이벤트 수집 중단 또는 콘솔 장애가 아니라, 해당 시간대 IT부문 내 Alert 생성 조건에 부합하는 이벤트가 발생하지 않은 케이스로 판단됨.
이전 서버EDR 이벤트 수집/Alert 발생 이슈와는 다른 건으로 보이며, 침해위협 티켓도 정상 발송 중임을 확인하여 고객에게 안내 완료.
```

## Resume / Career Bullet Version

```text
- SentinelOne EDR 기반 Linux/Windows 서버 악성 명령어 탐지 테스트 미탐 원인 분석 수행
- Event Search를 활용하여 Endpoint UUID, 프로세스 실행 체인, command line, Behavioral Indicator, Alert 발생 여부 확인
- Linux/Windows OS별 Behavioral Detection 차이를 SentinelOne 공식 KB 기준으로 분석
- Event Collection 로그와 Threat Alert 생성 조건의 차이를 고객 설명자료로 정리
- SentinelOne Support 케이스 대응을 위해 Agent 로그, 테스트 시나리오, 파일/프로세스 정보, 기대 탐지 결과 등 분석 자료 취합
- KB 기반 기술 분석 내용을 고객 회신 메일 및 내부 보고 자료로 문서화
```

