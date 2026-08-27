# Open Items and Next Actions

## Immediate Next Actions

- Confirm that `ap02.fms.com` is present in the `anagent` tag group.
- Confirm the new hashes are registered in the existing `anagent 관련 프로세스` exception policy.
- Confirm the `ap02.fms.com` Agent UUID is reflected in the STAR Custom Rule if it is an active exception target.
- Update the tag-based exception management sheet.
- Keep the Confluence documents current:
  - `SentinelOne 태그 기반 예외처리 배경 지식`
  - `E6. SentinelOne 태그 기반 예외처리 운영 방안`

## When a New anagent Host Is Reported

1. Confirm whether the alert is one of the anagent-related repeated alert names.
2. Confirm the file path / command line / process user context.
3. Confirm whether the hash is already known.
4. Check whether the asset had KT Ansible/Infrabot IP Connect history.
5. If valid:
   - Add the asset to the `anagent` tag.
   - Add the Agent UUID to the STAR Custom Rule.
   - Add new hash to the existing exception policy only if it is validated as anagent-related.
6. Update the management sheet and weekly report notes if needed.

## When a New Hash Is Reported

Check:

- Does it appear on multiple `anagent` tag assets?
- Is it tied to the same alert names?
- Does it use expected file paths or command lines?
- Is the related target asset covered by the STAR rule?

Decision:

- If common across verified anagent assets: add to existing anagent tag-based exception policy.
- If unique to one asset or unclear: consider a dedicated tag or hold for further validation.

## When Allowed IPs Change

Update:

- STAR Custom Rule allowed IP list.
- Event Search validation query.
- Management sheet / Confluence documentation.

## Useful New Thread Prompt

```text
Read all files under codex-context/anagent-fp-improvement.
Continue the MDR anagent false-positive improvement work.
Prioritize practical Korean wording for emails, weekly reports, and Confluence.
When discussing SentinelOne, preserve the distinction between Exclusion Scope, exception condition, and Applied on Tags.
```

