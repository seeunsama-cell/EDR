# Mail Templates

## Customer Reply: Event Collection Vs Alert Generation

```text
안녕하세요.

확인 결과, Linux 테스트 장비에서는 명령 실행 로그가 Event Search에 정상 수집된 것을 확인했습니다.
따라서 본 건은 Event Collection 실패라기보다는, 수집된 프로세스 실행 이벤트가 SentinelOne 기본 Behavioral Detection 또는 Alert 생성 조건에 부합하지 않아 Threat Alert로 승격되지 않은 케이스로 판단됩니다.

SentinelOne에서 Event Search에 수집되는 모든 이벤트가 자동으로 Threat Alert로 생성되는 구조는 아니며, Alert는 기본 Behavioral Detection 조건 또는 별도 탐지 룰 조건에 매칭되는 경우 생성됩니다.

단순 시스템 명령 실행까지 Alert 대상으로 관리해야 하는 경우에는 Event Search 쿼리 기반 STAR Custom Rule 구성 검토가 필요합니다.

감사합니다.
```

## Customer Reply: Linux And Windows Comparison

```text
안녕하세요.

Linux/Windows 테스트 결과를 비교하여 확인한 내용 공유드립니다.

Linux 테스트의 경우 RCE/Webshell을 통해 명령 실행은 발생했으나, Event Search상 대부분의 단순 명령 실행 이벤트는 indicator.name 값이 null로 확인되었습니다.
일부 행위에서는 ReadPasswdFile, ReadShadow, ShellTrapSignal 등의 Behavioral Indicator가 확인되었으나, Threat Alert로 승격되지는 않았습니다.

반면 Windows IIS Webshell 테스트에서는 w3wp.exe 하위에서 cmd.exe 및 ncat.exe가 실행된 프로세스 체인이 확인되었고, 해당 이벤트에 IISWebshell / ForbiddenProcessFromIISWebshell 계열 Indicator가 부여되었습니다.
해당 행위는 SentinelOne 기본 Behavioral Detection 조건에 매칭되어 Alert가 발생한 것으로 판단됩니다.

정리하면 Linux 테스트는 로그 수집은 정상이나 Alert 생성 조건에는 매칭되지 않은 케이스로 보이며, Windows IIS Webshell 테스트는 공식 Behavioral Detection 항목과 매칭되어 Alert가 발생한 케이스로 판단됩니다.

감사합니다.
```

## Internal Request: Ask Analyst To Validate Reasoning

```text
안녕하세요.

'[확인요청] 서버 EDR 악성 명령어 탐지 테스트간 미탐 원인 및 테스트 방법 검증 확인 요청드립니다' 제목으로 메일을 드렸는데요,

Linux 테스트에서 Alert가 발생하지 않은 사유와 관련해, 전달드린 내용이 맞는지 분석가님 측에서 확인 가능하실까요?

바쁘신 와중에 확인 부탁드립니다.
감사합니다.
```

## Internal Request: Include Shield-One Engineer If Needed

```text
혹시 쉴드원 엔지니어분께 추가 확인하신다면 요청 시 참조에 포함해주시면 감사하겠습니다.
```

## SentinelOne Support Reply: Windows Case Clarification

```text
Hello Sourav,

Thank you for your response.

This Windows case includes both a detected IIS Webshell behavior and missed behaviors related to PowerShell/VBS execution.

The IIS Webshell command execution was detected. In the SentinelOne console, we confirmed a process chain similar to:

- w3wp.exe -> cmd.exe -> ncat.exe

We also observed IIS Webshell-related Behavioral Indicators such as:

- IISWebshell
- ForbiddenProcessFromIISWebshell

However, other Windows test activities involving PowerShell/VBS or script-based command execution did not generate separate Alerts, and we would like to confirm the detection conditions and missed-detection reason.

Below is the current status for the information you requested:

1. Application name and execution method
- The test was performed in a Microsoft IIS environment using an uploaded ASPX Webshell.
- Additional details about the PowerShell/VBS execution method are being collected from the test owner.

2. PowerShell commands and VBS script contents
- We are collecting the exact PowerShell commands and VBS script contents from the test owner.
- We will share them as soon as they are available.

3. File hashes
- We are collecting hashes for the ASPX Webshell, VBS, and PS1 files used during the test.

4. Expected result
- We expected SentinelOne to generate a Behavioral Detection or Threat Alert for Webshell-originated CMD/PowerShell execution and VBS/PS1 reverse-shell or malicious command behavior.
- We would like to understand why only the IIS Webshell process creation behavior was alerted, while the PowerShell/VBS behaviors were not.

5. Agent logs
- We are preparing the Windows Agent logs collected during the penetration test period and will attach them to this case.

We will follow up with the requested artifacts once they are collected.

Best regards,
```

## Vendor Follow-Up: React2Shell Coverage Question

```text
Hello Team,

During our KB review, we found that SentinelOne published an out-of-band coverage update for React2Shell / CVE-2025-55182.

The Linux test included React2Shell-based RCE command execution, but the observed test commands mostly remained at simple system command execution level, and no Threat Alert was generated.

Could you please confirm whether the published React2Shell coverage is expected to detect the exploit payload itself, the post-exploitation behavior, or only specific behavior patterns after exploitation?

We would also like to confirm whether Agent version, policy configuration, Live Security Update status, or detection engine settings are required for this coverage to generate an Alert.

Best regards,
```

## Tenant Access Request

```text
제목: [SentinelOne] 테스트용 테넌트 사용 가능 여부 확인 요청의 건

안녕하세요.

SentinelOne 서버 EDR 악성 명령어 탐지 테스트 미탐 건 관련하여, 테스트용 SentinelOne 테넌트 사용 가능 여부 확인을 요청드립니다.

현재 해당 테스트용 테넌트는 IRM 소유로 알고 있으며, HALO 사이트 환경에서 제한된 범위로 테스트 진행이 가능한 것으로 알고 있습니다.
아래 목적의 테스트를 진행할 수 있도록 메가존 테스트용 접근 권한 부여가 가능할지 확인 부탁드립니다.

요청 목적은 다음과 같습니다.
- SentinelOne Support 케이스 검증
- Linux Agent / Policy / Detection 동작 확인
- Event Search 로그 및 Alert 발생 여부 확인

대상자는 다음과 같습니다.
- Cloud Security 안세은 매니저: <EMAIL>
- Managed Security 윤재범 매니저: <EMAIL>

운영 환경에 영향이 없도록 제한된 범위에서 진행하겠습니다.

확인 부탁드립니다.
감사합니다.
```

