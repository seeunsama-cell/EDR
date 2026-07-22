# References

## 관련 SentinelOne 룰

- Rule name: `[SOC][IT] AMOS, 클로드디자인 위장 캠페인_IoC_260424`
- Rule ID: `2464293954636699733`
- Rule source: `User created`
- Scope: `Global\KT\IT부문`
- Severity: `Medium`
- Rule type: `Single event`
- Query language: `2.0`
- Actions:
  - `Treat as threat`: `On`
  - `Network quarantine`: `Off`
- Cool off period: `Disabled`

## 관련 이벤트

- 관련 이벤트 ID: `2525527524466213718`
- Hostname: `KAMAS17`
- Host IP: `121.166.88.220`
- File path: `/home/ktsec/KAMAS2_ANAL/tomcat-9.0.91/instance613/bin/catalina.sh`
- Command line: `/bin/sh /home/ktsec/KAMAS2_ANAL/tomcat-9.0.91/instance613/bin/catalina.sh start`
- Detection time: `2026/07/15 08:55:46`
- 매칭 IOC IP: `172[.]240[.]253[.]132`

## 관련 메일

### 2026-07-15 발송 메일

- 제목: `[EXT] [MZCSOC] [KT_IT부문] SentinelOne 탐지 이벤트 확인 요청 드립니다.`
- 요지:
  - 탐지 정보 확인 및 Custom Rule 예외처리 검토 요청
  - `KAMAS17`에서 `catalina.sh` 실행
  - Custom Rule 등록 IP `172[.]240[.]253[.]132`로의 OUTGOING 네트워크/DNS 로그 확인
  - 대상 호스트는 스미싱 대응 크롤링 서버로 이력 확인
  - 정상 업무 과정 탐지일 경우 일부 호스트 예외처리 검토 필요

### 관련 회신

- 요지:
  - Custom Rule 예외처리는 진행하지 않는 방향으로 검토
  - 해당 시스템이 실제 운영 서버이므로 침해사고 여부 식별 필요
  - 일부 오탐 가능성이 있더라도 보안 관점에서 모니터링 유지가 적절

### 사용자 확인 메모

사용자는 관련 메일을 다음 제목으로 언급했습니다.

- `[EXT] [SK쉴더스][KT] SentinelOne 탐지 이벤트 확인 요청 드립니다. (7/25)`

실제 Gmail 검색에서 확인된 발송 메일 제목은 `[EXT] [MZCSOC] [KT_IT부문] SentinelOne 탐지 이벤트 확인 요청 드립니다.`였으며, 날짜는 2026-07-15입니다. 다음 스레드에서 메일 제목을 다시 사용할 경우 조직명/날짜 표기를 한 번 더 확인하는 것이 좋습니다.

## 관련 로컬 파일

아래 파일들이 대화 중 사용되었거나 참조되었습니다. 새 스레드에서 필요 시 존재 여부를 확인해야 합니다.

- `C:/Users/USER/Downloads/[SOC][IT] AMOS, 클로드디자인 위장 캠페인_IoC_260424 ChangeHistory.csv`
- SentinelOne 화면 캡처 이미지들:
  - `C:/Users/USER/AppData/Local/Temp/codex-clipboard-*.png`

Temp 경로의 이미지 파일은 새 PC/새 스레드에서 사라졌을 수 있습니다. 재확인이 필요하면 SentinelOne 콘솔에서 다시 캡처하거나 Export해야 합니다.

## 관련 공개 참고 자료

대화 중 SentinelOne 공개 자료에서 확인한 개념:

- STAR / Custom Detection Rule은 Deep Visibility Query를 기반으로 자동 탐지/대응을 구성할 수 있음
- Storyline은 관련 endpoint activity를 연계해 알림을 구성하는 개념

공개 참고 링크:

- SentinelOne STAR 소개: `https://www.sentinelone.com/blog/customize-your-edr-to-adapt-to-your-environment-with-sentinelone-storyline-active-response-star/`
- SentinelOne Threat Hunting / Storyline 관련 글: `https://www.sentinelone.com/blog/six-steps-to-successful-and-efficient-threat-hunting/`

SentinelOne Community KB는 로그인/세션 의존성이 있어 대화 중 명확한 문서 원문을 확보하지 못했습니다.

