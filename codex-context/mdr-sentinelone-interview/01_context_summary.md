# Context Summary

## 작업 목적

안랩의 엔드포인트 보안 제품 기술지원 직무 면접을 준비한다. 목표 직무는 고객 대상 엔드포인트 제품 교육·콘텐츠 제작과 국내·글로벌 기술지원 KB 작성이다. SentinelOne EDR을 운영한 MDR 경험을 제품 이해, 고객 문의 분석, 문서화와 연결해 설명한다.

## 배경

- 신입으로 MDR 프로젝트에 투입되어 SentinelOne 콘솔을 반복 사용했다.
- Alert 이름만 확인하지 않고 프로세스, 명령줄, 파일 경로·해시, 부모·자식 프로세스, IP Connect와 연관 이벤트를 Event Search에서 확인했다.
- SentinelOne KB와 Customer Portal Q&A를 참고해 탐지 조건, 정책 동작 방식, 제품 제약사항을 확인했다.
- 분석 결과를 고객이 이해할 수 있는 메일과 주간·월간 보고서로 정리했다.
- MDR은 SentinelOne과 외부 운영 파트너가 협업하는 구조였다. Endpoint 탭과 Full Disk 관련 Alert의 1차 분석·대응은 SK쉴더스가 수행했다.
- Custom 탭 이벤트는 운영 기준에 따라 SK쉴더스가 Resolve 처리했지만, 보고서에 쓸 오탐 사유와 정상 운영 근거를 확보하기 위해 Event Search로 추가 검증했다.

## 현재까지 정리한 주요 경험

### ANAGENT 반복 탐지 개선

Infrabot 기반 SSH/SFTP 자동화 과정에서 `sshd`, `sftp-server`의 파일 생성 행위가 반복 탐지됐다. 4~5월 Alert 9,475건을 자산, Agent UUID, 파일 해시·경로 기준으로 분석하고 IP Connect 이력을 Infrabot 접속 정보와 대조했다. 개별 자산 예외가 어려운 정책 제약을 확인한 뒤 `anagent` 태그와 파일 해시를 함께 조건으로 사용하는 제한적 예외 방안을 제안했고 운영 반영까지 연결했다.

동일 7일 비교에서 주요 `sshd`·`sftp-server` 반복 탐지는 1,094건에서 663건으로 감소했다. 총 431건, 약 39.4% 감소다. 예외로 인한 보안 공백을 보완하기 위해 예외 대상 Agent의 22222 포트로 유입되는 연결 중 허용 IP가 아닌 Source IP를 탐지하는 STAR Custom Rule을 설계·검증·반영했다.

### Linux·Windows 서버 EDR 악성 명령어 탐지 테스트

고객사가 Linux와 Windows에서 탐지 테스트를 진행했으나 일부 테스트에서 Alert가 발생하지 않아 미탐 원인과 테스트 방법의 적절성을 문의했다. Event Search에서 Process Creation 로그와 Behavioral Indicator를 확인하고 SentinelOne의 Linux·Windows Behavioral Detection KB와 대조했다.

분석 결과 특정 명령어 문자열 자체가 항상 Alert를 생성하는 것이 아니라 프로세스 실행 관계, 파일 접근·생성, 네트워크 연결 등 후속 행위와 정책 조건에 따라 Alert 생성 여부가 달라질 수 있음을 확인했다. 로그 수집, Indicator 생성, 최종 Threat Alert 승격을 구분해 분석했고 솔루션 담당 엔지니어의 검토 결과도 분석 방향과 일치했다.

## 현재 상태

- 안랩 지원동기, 유관 경험, 담당업무 요약, 포트폴리오 문구를 작성했다.
- SentinelOne 콘솔의 Alerts, Event Search, Agent management, Detections, Policies and settings, Reports와 Activities를 면접 대비 항목으로 분류했다.
- 다음 작업은 이 맥락을 바탕으로 예상 질문과 1분·3분 답변을 확정하는 것이다.
