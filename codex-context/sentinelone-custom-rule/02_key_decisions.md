# Key Decisions

## 1. Custom alerts 탭과 Endpoint 탭은 같은 관점이 아니다

### 판단

- `Custom alerts` 탭은 Custom Rule 매칭 결과를 룰 이름 중심으로 보여줍니다.
- `Endpoint` 탭은 엔드포인트 위협/프로세스/Storyline 중심의 Alert 객체를 보여줍니다.

### 근거

동일한 흐름이 다음처럼 서로 다른 이름으로 표시될 수 있었습니다.

- Custom alerts 탭: `[SOC][IT] AMOS, 클로드디자인 위장 캠페인_IoC_260424`
- Endpoint 탭: `gunicorn - Modification Of ld Variables detected`, `catalina.sh detected as Manual` 등

즉, Custom alerts 탭은 “어떤 룰이 매칭됐는지”, Endpoint 탭은 “어떤 자산/프로세스/Storyline 기준으로 위협 알림이 구성됐는지”를 보여줍니다.

## 2. `Initiated By = Custom Rule`은 기본 탐지 엔진 탐지가 아니다

### 판단

Endpoint 탭에 있더라도 `Initiated By = Custom Rule`이면 해당 Endpoint Alert의 기점은 Custom Rule 매칭입니다.

### 근거

Endpoint 탭의 필터에서 `Initiated By = Custom Rule`을 적용했을 때, `catalina.sh detected as Manual` 같은 프로세스 기반 Alert가 표시되었습니다. 이는 기본 엔진이 `catalina.sh` 자체를 악성으로 판단했다는 의미가 아니라, Custom Rule 매칭 결과가 Endpoint Alert 형태로 표현된 것입니다.

## 3. `Treat as threat`는 Custom Rule 매칭을 위협 알림처럼 취급하게 하는 설정이다

### 판단

`Treat as threat = On`이면 Custom Rule 매칭 이벤트가 단순 Custom Alert에서 끝나지 않고 Endpoint Alert / Threat 관점으로 연계될 수 있습니다.

### 주의

모든 Custom Alert가 반드시 Endpoint 탭에 표시되는 것은 아닙니다. Endpoint 탭은 엔드포인트 위협 객체 중심이므로, SentinelOne이 이벤트를 프로세스, 자산, Storyline 등과 연결해 Alert로 구성할 수 있는 경우에 표시됩니다.

## 4. Custom Alert 종결과 Endpoint Alert 종결은 자동 동기화된다고 단정하면 안 된다

### 판단

Custom alerts 탭의 Alert 상태와 Endpoint 탭의 Alert 상태는 별도 객체/라이프사이클로 보는 것이 안전합니다.

### 근거

화면상 Custom alerts 탭에서는 모든 Custom Alert가 `Resolved`로 보였으나, Endpoint 탭의 `Initiated By = Custom Rule` Alert 중 일부는 `In progress`로 남아 있었습니다.

따라서 Custom Alert를 종결해도 매핑된 Endpoint Alert까지 자동 종결된다고 판단하면 안 됩니다.

## 5. KAMAS17 / catalina.sh 건은 정상 업무 기반 오탐 가능성이 있다

### 판단

`catalina.sh`는 Apache Tomcat 구동용 정상 관리 스크립트이며, `KAMAS17`은 스미싱 대응 크롤링 서버로 확인되었습니다.

### 근거

2026-07-15 관련 메일에서 다음 내용이 확인되었습니다.

- `catalina.sh`는 Apache Tomcat에서 서버 시작/중지/재시작 등에 사용하는 정상 스크립트
- 스크립트 실행 시 Custom Rule에 등록된 IOC IP `172[.]240[.]253[.]132` 관련 OUTGOING 네트워크/DNS 로그 확인
- 해당 호스트는 스미싱 대응 크롤링 서버 이력 존재

## 6. 예외처리는 미적용하고 지속 모니터링한다

### 판단

오탐 가능성이 있더라도 운영 서버의 보안 가시성 유지를 위해 Custom Rule 예외처리는 적용하지 않는 방향으로 정리되었습니다.

### 근거

관련 회신에서 운영 서버 특성상 향후 유사 행위가 다른 경로로 발생할 경우 침해 여부 식별이 필요하므로, 일부 오탐 가능성이 있어도 모니터링을 유지하는 것이 적절하다고 판단했습니다.

## 보류/변경된 판단

초기에는 “TCPv4 이벤트는 Endpoint Alert로 잘 안 올라갈 수 있다”고 추정했으나, 실제 화면에서 TCPv4 Custom Rule 매칭 건이 Endpoint 탭의 프로세스 기반 Alert로 표시되는 사례가 확인되어 판단을 수정했습니다.

