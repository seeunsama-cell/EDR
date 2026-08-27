# Detection Evidence and Counts

## Original Evidence Basis

Main files used during the work:

- `MDR_anagent 탐지 로그.xlsx`
- `anagent_4-5월_Alert_해시별_전수집계_20260621.xlsx`
- `anagent_KT_Ansible_IP Connect_매칭_결과.xlsx`

Related generated/working outputs may exist under:

- `outputs/anagent-ipconnect-verify/`
- `outputs/tag-exclusion-management/`

## Monthly Alert Volume

The working summary used for customer-facing explanation:

- April 2026: `4,875` events
- May 2026: `4,600` events
- Total: `9,475` events

These counts were used to explain the expected false-positive reduction impact if validated anagent-related hashes are excluded under the target tag.

## Initial Top Asset Review

The analysis focused on high-volume assets for each alert name.

For `sftp-server - Creation of a suspicious`:

- `BD-L3-CRDPDB01`
- `BD-L3-CRDPDB02`
- `mypagedb1`
- `mypagedb2`
- `p-safe-pk1-d01`

For `sshd - Creation of a suspicious`:

- `p-recor-pk1-d01`
- `p-gscan-pd1-w01`
- `p-gscan-pd1-w02`
- `p-gscan-pd1-w03`
- `p-gscan-pd1-w04`

After KT Ansible/Infrabot IP Connect matching, the practical first exception target set was:

- `BD-L3-CRDPDB01`
- `BD-L3-CRDPDB02`
- `mypagedb1`
- `mypagedb2`
- `p-safe-pk1-d01`
- `p-recor-pk1-d01`

Later, additional anagent-related assets were added as confirmed.

## Main Repeated Hashes

Initial commonly handled SHA-256 values:

```text
f6295bf5c9b0c0dbf824456c2e1eb1b69bba8550eb765d5f890cdbb15160b398
75104951c97e58d15d7b5b1721661eb1fc48e73df73e862e60f35bfc95995e94
a16d1eeb4d8e01412078f41bb676e5ea85fd46f9e8505fc29e9cae416c97378b
```

Additional hashes later considered/added through follow-up detections:

```text
3b7f2c7ddc8ebb584a3e7bec16923214e98af75a951507edcc04ffbe9d8a0965
47324c31d7fca3928aaa79f5ec3e42cf107ea8d5357ff4756e326583002ffb531
a74052ea1a6f990ac7a232757c9ea38dc7a7a31bd5217906c998750e70d1aef2
```

## 6월 Continuity Check

During June review, the same alert names and same SHA-256 values continued to appear on the target assets.

Customer-facing wording used:

```text
자산별 탐지 내역을 교차 검증한 결과, 동일한 파일 해시가 해당 자산에서 반복적으로 탐지됐으며, 6월 현재까지도 동일 자산에서 해당 해시의 Alert가 지속 발생하고 있음을 확인했습니다.
```

## IP Connect Verification

The purpose of IP Connect verification was to confirm that target assets had communication history with the KT Ansible/Infrabot server IP list.

This supported the conclusion that the repeated `sshd` / `sftp-server` alerts were likely related to normal Ansible/Infrabot automation activity rather than arbitrary suspicious behavior.

Deep Visibility/Event Search availability was limited, so IP Connect raw data was confirmed for the available time range rather than the entire April-May period.

Customer-facing wording used:

```text
IP Connect 이벤트는 SentinelOne Deep Visibility의 데이터 조회 가능 기간 제한에 따라 2026년 5월 9일 ~ 2026년 6월 1일 범위에서 확인했습니다.
해당 통신 이력은 아래 쿼리 구문을 통해 SentinelOne Event Search에서 확인하실 수 있습니다.
```

