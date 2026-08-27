# anagent Tag-Based Exception Policy

## Core Policy Concept

The anagent exception policy is operated as:

- Target assets receive the `anagent` tag.
- Validated anagent-related SHA-256 hashes are registered in the tag-based exception policy.
- The exception is limited to assets with the `anagent` tag.

Important:

- The tag limits the target assets.
- The exception condition is the hash.
- SentinelOne Exclusion does not support the intended `Alert Name + Hash + Tag` AND expression as one combined exception policy.

## Why Existing anagent Tag Was Reused

When additional host `ap02.fms.com` and additional hashes were found, there was concern that adding hashes to the existing `anagent` tag would expand the exception to all `anagent`-tagged assets.

Review conclusion:

- The newly confirmed hashes were not unique to `ap02.fms.com`.
- They appeared across multiple assets in the anagent tag group.
- STAR Custom Rule compensating detection is already in place for non-allowed Source IP access.
- Therefore creating a separate `ap02`-only tag was not necessary.

Final direction:

- Continue using the existing `anagent` tag.
- Add the new hashes to the existing anagent tag-based exception policy.
- Add `ap02.fms.com` to the `anagent` tag group.

## Current Hash Management Notes

Initial main SHA-256 values:

```text
f6295bf5c9b0c0dbf824456c2e1eb1b69bba8550eb765d5f890cdbb15160b398
75104951c97e58d15d7b5b1721661eb1fc48e73df73e862e60f35bfc95995e94
a16d1eeb4d8e01412078f41bb676e5ea85fd46f9e8505fc29e9cae416c97378b
```

Additional values from later requests/reviews:

```text
3b7f2c7ddc8ebb584a3e7bec16923214e98af75a951507edcc04ffbe9d8a0965
47324c31d7fca3928aaa79f5ec3e42cf107ea8d5357ff4756e326583002ffb531
a74052ea1a6f990ac7a232757c9ea38dc7a7a31bd5217906c998750e70d1aef2
```

Before adding new hashes:

- Check whether the hash is observed on more than one anagent-tagged asset.
- Check whether the related host is already in the STAR Custom Rule target list.
- If the hash is only for one exceptional host and has different risk profile, consider a dedicated tag.
- If the hash is common to anagent-tagged assets and STAR monitoring is active, reuse the existing `anagent` tag.

## Asset/Tag Management Notes

The tag-based exception asset mapping is maintained in the Google Sheet / Excel-style management table.

Suggested fields:

- `Tag`
- `적용된 Exclusion Name`
- `등록된 Blocklist 값`
- `Hostname`
- `Agent UUID`
- `추가일자`
- `STAR Rule 반영`

Console lookup note:

```text
콘솔의 Inventory > All Assets에서 Tags 필터 검색으로 태그별 자산 현황 조회 가능
```

