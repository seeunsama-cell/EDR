# Context Summary

## 작업 목적

SentinelOne에서 `Initiated By = Custom Rule`로 표시되는 Endpoint Alert의 의미를 이해하고, 특정 Custom Rule 탐지 건을 분석해 노트/메일/주간보고에 사용할 수 있는 문구로 정리하는 것이 목적입니다.

특히 다음 질문을 정리했습니다.

- Custom alerts 탭과 Endpoint 탭의 차이는 무엇인가?
- Custom Rule로 탐지된 이벤트가 왜 Endpoint 탭에도 보이는가?
- `Treat as threat` 설정은 어떤 의미인가?
- Custom Alert를 종결하면 Endpoint Alert도 같이 종결되는가?
- 특정 AMOS/Claude Design 위장 캠페인 IOC 룰이 어떤 원리로 탐지했는가?
- 외주업체에게 SentinelOne 노트 작성을 요청할 때 어떤 문구가 적절한가?

## 배경

SentinelOne 콘솔에서 다음과 같은 Endpoint Alert가 확인되었습니다.

- `gunicorn - Modification Of ld Variables detected`
- `bash - Modification Of Shell History detected`
- `catalina.sh detected as Manual`
- `java detected as Manual`

이 중 일부는 Endpoint 탭에서 보이지만 `Initiated By = Custom Rule`로 표시되었습니다. 사용자는 처음에 “기본 탐지 엔진 탐지인지, Custom Rule 탐지가 Endpoint로 승격된 것인지”를 혼동했습니다.

분석 결과, 해당 건들은 SentinelOne 기본 탐지 엔진이 독립적으로 탐지한 것이라기보다, Custom Rule 매칭 결과가 Endpoint Alert / Threat 관점으로 표시된 케이스로 이해하는 것이 적절하다고 정리했습니다.

## 현재까지 진행한 내용

1. SentinelOne 화면 캡처를 기반으로 Custom alerts 탭과 Endpoint 탭의 차이를 정리했습니다.
2. AMOS/Claude Design 위장 캠페인 Custom Rule의 조건식을 확인했습니다.
3. 관련 Gmail thread를 조회해 2026-07-15 발송 메일과 회신 내용을 확인했습니다.
4. `KAMAS17` / `catalina.sh` 이벤트가 Custom Rule IOC IP와 매칭되어 탐지된 건임을 정리했습니다.
5. 외주업체에게 SentinelOne 노트 작성을 요청하는 카카오톡/메일 문구를 작성했습니다.
6. 악성 IP 자동 링크 방지를 위해 IOC는 defang 처리해야 한다고 정리했습니다.

## 현재 상태

최종 판단은 다음과 같습니다.

> Custom Rule `[SOC][IT] AMOS, 클로드디자인 위장 캠페인_IoC_260424`에 등록된 IOC IP `172[.]240[.]253[.]132`와 `KAMAS17` 서버의 OUTGOING 네트워크/DNS 로그가 매칭되어 탐지된 건으로 확인됨. 대상 프로세스 `catalina.sh`는 Apache Tomcat 구동용 정상 관리 스크립트이며, 해당 호스트는 스미싱 대응 크롤링 서버로 확인되어 정상 업무 과정에서 발생한 오탐 가능성이 있음. 다만 침해 여부 식별 및 보안 가시성 유지를 위해 예외처리는 미적용하고 지속 모니터링하는 것으로 전달받음.

