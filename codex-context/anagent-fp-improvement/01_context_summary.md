# anagent False Positive Improvement - Context Summary

## Objective

Reduce repeated SentinelOne false-positive alerts related to anagent operations while keeping compensating detection for suspicious access paths.

The practical goal is:

- Suppress repeated `sshd` and `sftp-server` anagent-related alerts on verified target assets.
- Limit the exception scope with SentinelOne tags.
- Keep a STAR Custom Rule to alert on non-allowed source IP access to TCP/22222.
- Maintain enough written context for another Codex thread to continue without the original conversation history.

## Main Alert Names

The repeated alerts handled in this work are:

- `sftp-server - Creation of a suspicious executable file detected`
- `sshd - Creation of a suspicious executable file detected`

Short forms used in earlier working notes:

- `sftp-server - Creation of a suspicious`
- `sshd - Creation of a suspicious`

## Why Tag-Based Exception Was Used

SentinelOne Exclusion policies are created at a Scope such as Global, Account, Site, or Group.

If an exception is applied too broadly, a File Path, Hash, or Command Line exception can affect many unrelated assets in the same Scope. That creates detection blind spots.

Tag-based exclusion was selected because it allows:

- Create or reuse an Exclusion policy at the required SentinelOne Scope.
- Add `Applied on Tags`.
- Limit actual exception application to assets with the target tag.

Important limitation:

- SentinelOne Exclusion policy does not cleanly support combining multiple exception parameters like `Alert Name + Hash + Tag` as one AND condition.
- The exception condition is usually one supported parameter such as Hash, File Path, or Command Line.
- The tag is used to limit the affected assets, not to create a multi-condition detection expression.

## Final Operating Direction

For anagent:

- Use the existing `anagent` tag.
- Apply validated SHA-256 hash exceptions under the tag-based `anagent` exception policy.
- Add additional hosts to the `anagent` tag group when they are verified as anagent-related.
- Add their Agent UUIDs to the STAR Custom Rule if they should be monitored for non-allowed IP access.
- Keep the STAR Custom Rule as compensating control for non-allowed Source IP access to destination port `22222`.

## Key Decision About ap02.fms.com

An additional host `ap02.fms.com` had newly confirmed anagent-related SHA-256 hashes.

Initial concern:

- If the new hashes are added to the existing `anagent` tag-based exception policy, all assets with the `anagent` tag receive the same hash exception.
- This might widen the exception scope.

Internal/Shields review conclusion:

- The hashes were not unique to a single asset.
- They appeared across multiple assets included in the `anagent` tag group.
- A separate STAR Custom Rule already monitors non-allowed Source IP access.
- Therefore a new dedicated `ap02` tag was not necessary.

Final action:

- Add the new hashes to the existing `anagent` tag-based exception policy.
- Add `ap02.fms.com` to the `anagent` tag group.
- Add or confirm `ap02.fms.com` Agent UUID in the STAR Custom Rule when applicable.

## Related Workstreams

Related but separate workstreams in this project:

- SentinelOne tag-based exclusion background knowledge.
- KAMAS tag-based exclusion case.
- Full Disk Scan residual false-positive handling.
- Weekly report wording.
- Customer and internal email drafts.

