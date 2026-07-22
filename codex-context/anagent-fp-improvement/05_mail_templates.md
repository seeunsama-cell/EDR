# Mail Templates and Wording

## Customer Mail - anagent Exception Review

```text
안녕하세요, MZCSOC입니다.

anagent 관련 반복 Alert의 오탐 개선을 위해 태그 기반 예외 처리 방안을 공유드리오니 검토 부탁드립니다.

반복 오탐을 줄이기 위해, anagent 작업 및 KT Ansible/Infrabot 통신 이력이 확인된 자산에 한해 제한적으로 예외 처리를 적용하고자 합니다.

검토 대상 Alert는 다음과 같습니다.

- sftp-server - Creation of a suspicious executable file detected
- sshd - Creation of a suspicious executable file detected

각 자산의 SentinelOne Agent UUID를 기준으로 IP Connect 이력을 조회한 후, 사전에 전달받은 KT Ansible/Infrabot 서버 IP가 src.ip.address 또는 dst.ip.address와 일치하는지 교차 검증했습니다.

해당 통신 이력은 SentinelOne Event Search에서 공유드린 쿼리로 확인할 수 있으며, agent.uuid 값은 조회 대상 자산의 Agent UUID로 변경하여 사용합니다.

자산별 탐지 내역을 교차 검증한 결과, 동일한 파일 해시가 해당 자산에서 반복적으로 탐지됐으며, 6월 현재까지도 동일 자산에서 해당 해시의 Alert가 지속 발생하고 있음을 확인했습니다.

이에 따라 대상 자산에 anagent 식별용 Tag를 부여한 후, 반복 탐지된 SHA-256 Hash를 해당 Tag 범위에서만 Exclusion으로 적용하고자 합니다.

또한 비허용 Source IP의 22222 포트 접근 탐지를 위해 STAR Custom Rule을 별도 운영하여 보완 탐지를 병행하고자 합니다.

확인 부탁드립니다.

감사합니다.
```

## Internal/Shields Advisory Request

```text
안녕하세요.

현재 진행 중인 anagent 관련 예외 처리와 관련하여 의견을 구하고자 합니다.

고객사에서 최초 제안한 예외 방식은 아래 3가지 값을 AND 조건으로 결합하는 방식이었습니다.

- 특정 Alert Name
- anagent 대상 자산 Tag
- 검증된 파일 Hash

즉, 해당 Tag가 적용된 자산에서 특정 Alert와 특정 Hash가 동시에 일치할 때만 예외 처리하는 방향이었습니다.

다만 SentinelOne의 예외는 Hash, Command Line, File Path 등 하나의 Parameter를 기준으로 생성되며, Alert Name + Hash + Tag를 하나의 AND 조건으로 결합할 수 없었습니다. 또한 Alert Name은 예외 Parameter 값으로 제공되지 않았습니다.

이에 현재는 KT Ansible/Infrabot 허용 IP와의 IP Connect 이력이 확인된 자산에 anagent Tag를 적용하고, 해당 Tag 자산에 한해 반복 탐지된 SHA-256 Hash를 예외로 적용한 상태입니다.

고객사에서는 Tag와 Hash만으로 예외 처리할 경우, Ansible 작업이 아닌 상황에서 동일 바이너리가 악용되더라도 Alert가 제외될 수 있다는 보안 공백을 우려하고 있습니다.

이에 보완책으로 비허용 Source IP가 대상 서버의 22222 포트로 INCOMING + SUCCESS 접속하는 경우 STAR Custom Rule로 별도 Alert를 생성하는 방식이 적절할지 검토 의견 부탁드립니다.

현재 Event Search 조회 가능 기간 내에서는 상기 조건에 해당하는 비허용 Source IP의 접속 이력이 확인되지 않았습니다.

향후 예외 적용 대상 자산이 추가되거나 제외되는 경우, 룰의 TargetAgents 목록에서 해당 자산의 Agent UUID를 추가 또는 삭제하는 방식으로 운영하는 것이 적절한지 자문을 구하고자 합니다.

상기 STAR Custom Rule 및 보완 통제 방안의 적절성에 대해 검토 의견 부탁드립니다.

감사합니다.
```

## ap02 Hash Addition Review Wording

```text
예원님, anagent 추가 탐지 건 관련하여 확인 부탁드립니다.

ap02.fms.com에서 신규 SHA-256 Hash 3종이 확인되었습니다.

해당 Hash를 기존 anagent 태그 예외 정책에 추가할 경우, 기존 anagent 태그가 부여된 전체 서버에도 신규 Hash 예외가 적용될 것으로 보여 예외 범위가 과도하게 넓어질 우려가 있습니다.

따라서 ap02.fms.com 전용 Tag를 별도로 생성하고, 해당 Tag에만 신규 Hash 3종을 예외 처리하는 방식이 적절할지 검토 부탁드립니다.

STAR Custom Rule에는 ap02.fms.com의 Agent UUID만 추가하여, 비허용 Source IP의 22222 포트 접근 탐지는 유지하고자 합니다.
```

Review conclusion wording:

```text
쉴더스 검토 결과, 해당 Hash는 단일 자산이 아닌 anagent Tag 대상 여러 자산에서 공통 탐지되는 값이며, 비허용 Source IP 접근 탐지를 위한 Custom Rule이 별도 운영 중이므로 추가 Tag 생성은 불필요한 것으로 확인했습니다.
```

## Tag-Based Exclusion Background Mail

```text
안녕하세요, MZCSOC입니다.

유선으로 문의 주신 태그 기반 예외처리 방식 관련하여 공유드립니다.

현재 IT부문에서는 아래와 같은 형식으로 태그 기반 예외처리 요청을 전달해 주시고 있습니다.

해당 방식은 예외 대상 자산에 식별용 태그를 부여한 뒤, 예외 정책에 해당 태그를 지정하여 태그가 부여된 자산에 한해 예외가 적용되도록 제한하는 방식입니다.

즉, Account/Site/Group 전체 범위에 예외를 적용하는 것이 아니라, 사전에 지정된 태그가 부여된 서버에 한해 예외가 적용되도록 예외 적용 범위를 제한할 수 있습니다.

태그는 콘솔의 Agent Management 화면에서 대상 Endpoint를 선택한 후, Endpoint 상세 화면의 Manage Tags 버튼을 통해 추가할 수 있습니다.

관련하여 태그 기반 예외처리 적용 여부를 확인하기 위해 진행한 테스트 내용도 함께 공유드립니다.
```

