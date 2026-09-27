# Buffer-drops

Buffer-drops is a local Git-backed result drop mechanism for PowerShell commands whose output may exceed safe chat/clipboard transfer size.

## Source of truth

Read `HANDOFF.md` first.

`HANDOFF.md` defines the mandatory contract.  
If other files disagree with `HANDOFF.md`, `HANDOFF.md` wins.

## Main idea

Small PowerShell result:

```text
console + full Clipboard copy
```

Large PowerShell result:

```text
local latest.txt -> git add/commit/push -> exact Clipboard marker
```

The exact marker is:

```text
GitHub Nikname Buffer-drops\\latest.txt
```

Important:

- `Nikname` is literal text.
- Do not replace it.
- Do not remove backslashes.
- The marker is copied to Clipboard only after successful `git push`.

## Relation to Agent_ChatAi

Buffer-drops can be used by Agent_ChatAi doctor/test workflows to transfer large diagnostic output.

Buffer-drops is not part of the AGX protocol.  
Buffer-drops does not implement policy, replay protection, browser auth, audit logging, or gateway behavior.

## Files

```text
HANDOFF.md      Source of truth.
README.md       Entry point.
RULES.md        Mandatory rules for AI assistants and PowerShell commands.
SECURITY.md     Privacy and secret-handling warnings.
LICENSE         The Unlicense.
CHANGELOG.md    Change history.
.gitignore      Local ignores, while keeping latest.txt trackable.
```

## Local path

```text
C:\Proj\Agents\Buffer-drops
```

## Expected GitHub repository

```text
https://github.com/familiyaandrey292-sudo/Buffer-drops
```

The repository should be private unless the transferred result is known to be non-sensitive.

## Warning

`latest.txt` may contain raw diagnostic output.

Do not push secrets, tokens, private keys, passwords, personal data, or sensitive full result payloads unless the repository is private and history retention is acceptable.