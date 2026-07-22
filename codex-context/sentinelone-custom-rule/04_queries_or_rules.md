# Queries or Rules

## SentinelOne Custom Rule 개념

이 건의 룰은 행위 기반 탐지라기보다 IOC 기반 탐지입니다.

즉, 프로세스 자체의 악성 행위를 분석해서 탐지한 것이 아니라, 이벤트 필드에 포함된 IP/URL/파일 해시가 사전에 등록한 침해지표와 일치하는지 확인하는 방식입니다.

## 룰명

```text
[SOC][IT] AMOS, 클로드디자인 위장 캠페인_IoC_260424
```

## 룰 설명

```text
AMOS, 클로드디자인 위장 캠페인 관련 침해지표 대응
```

## 주요 탐지 조건

룰은 다음 조건 중 하나라도 만족하면 매칭되는 OR 구조입니다.

```text
src.ip.address in (...)
OR dst.ip.address in (...)
OR url.address contains:anycase (...)
OR tgt.file.md5 in (...)
```

## 주요 IOC IP

문서/메신저/메일 공유 시 자동 링크 방지를 위해 defang 형태를 사용합니다.

```text
157[.]90[.]104[.]39
172[.]240[.]253[.]132
51[.]91[.]153[.]29
23[.]109[.]150[.]181
193[.]233[.]132[.]188
46[.]101[.]104[.]172
217[.]119[.]139[.]117
3[.]85[.]252[.]251
34[.]234[.]154[.]208
172[.]240[.]108[.]0/24
172[.]240[.]127[.]0/24
104[.]21[.]0[.]95
```

## 주요 도메인/URL IOC

원문 룰에는 아래와 같은 도메인/URL 문자열이 포함되어 있었습니다. 외부 공유 시 `.`을 `[.]`으로 치환하고, URL scheme은 `hxxp`/`hxxps`로 표기합니다.

```text
claude-design[.]org
www[.]claude-design[.]org
cluade-design[.]org
laislivon[.]com
rvdownloads[.]com
Mac-force[.]squarespace[.]com
arkypc[.]com
lakhov[.]com
wiiv-adguard[.]pro
wmni-protect[.]pro
mjbw-adguard[.]co[.]in
mvmn-defender[.]sbs
mosved[.]com
bdsclk[.]com
srowox[.]com
kettledroopingcontinuation[.]com
realizationnewestfangs[.]com
precheck[.]adsmbdc[.]com
adsmbdc[.]com
4nicebets[.]org
madridkings[.]shop
github[.]com/anthropic-claude-design/claude-design/releases/download/claude-design/ClaudeDesign-Optimized_x64.7z
hobbledispleased[.]com
epsoneventmanager[.]org
multitrk[.]com
dedelk[.]com
greenactiv[.]com
```

## 파일 해시 IOC

```text
MD5: 45029deaf9033802d08b5f82b77978fa
```

## 관련 이벤트 분석 기준

분석 시 확인할 항목:

- 어떤 조건이 매칭됐는지
  - `src.ip.address`
  - `dst.ip.address`
  - `url.address`
  - `tgt.file.md5`
- 대상 자산/호스트가 무엇인지
- 이벤트 타입이 무엇인지
  - 예: TCPv4, File Scan, File Deletion 등
- Endpoint 탭에서는 어떤 프로세스/Storyline으로 표시되는지
- 프로세스 자체가 악성인지, 정상 프로세스가 IOC와 접촉한 것인지
- 호스트의 업무 성격상 IOC 접속이 정상 업무일 수 있는지
- 예외처리 시 보안 가시성 저하가 발생하는지

## 운영 기준

- IOC는 메신저/메일/문서에서 defang 처리합니다.
- `Treat as threat = On`인 Custom Rule은 Endpoint Alert로 연계될 수 있음을 고려합니다.
- Custom alerts 탭의 Alert 상태와 Endpoint 탭의 Alert 상태는 별도 객체로 판단합니다.
- 운영 서버의 경우 단순 오탐 가능성만으로 예외처리하지 않고, 보안 가시성 유지 필요성을 함께 검토합니다.

