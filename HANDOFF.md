# HANDOFF.md

## Project

Buffer-drops.

## Status

Early-stage mandatory result-transport rule for PowerShell diagnostics and tests.

## Source of truth

This file is the source of truth for the purpose, contract, and boundaries of Buffer-drops.

If README.md, RULES.md, old notes, or AI suggestions contradict this file, this file wins.

## Purpose

Buffer-drops provides a deterministic way to return PowerShell command results that may exceed the safe chat/clipboard transfer size.

It is not an AI agent.  
It is not an AGX implementation.  
It is a local Git-backed result drop mechanism.

## Relation to Agent_ChatAi

Agent_ChatAi currently provides:

- AGX ACTION/RESULT protocol with integrity checks.
- ACTION timestamp freshness and replay protection.
- Local policy: ALLOW / CONFIRM / DENY.
- Persistent SQLite replay protection.
- One-time confirmation tokens with TTL.
- Idempotent pending confirmations.
- Confirm and Cancel browser UI flow.
- Browser Auth with Ed25519 challenge-response.
- Non-extractable browser private key.
- Ed25519 RESULT signing and browser verification.
- Privacy-conscious JSONL audit logging with rotation.
- Browser Bridge for Edge.
- Windows Task Scheduler autostart.
- Doctor diagnostics.
- Gateway: 127.0.0.1:8765.
- Browser test page: http://127.0.0.1:8766/index.html.

Buffer-drops may be used by Agent_ChatAi doctor/test workflows to transfer large diagnostic output without putting the whole result into chat.

Buffer-drops does not change Agent_ChatAi security, policy, replay protection, audit, or authentication guarantees.

## Local repository

Local path:

C:\Proj\Agents\Buffer-drops

Expected GitHub repository:

https://github.com/familiyaandrey292-sudo/Buffer-drops

The repository should be private unless the transferred result is known to be non-sensitive.

## Large-result file

The complete large result is stored as:

C:\Proj\Agents\Buffer-drops\latest.txt

In the GitHub repository, the same file is expected at repository root:

latest.txt

## Mandatory marker

The exact marker is:

```text
GitHub Nikname Buffer-drops\\latest.txt
```

Rules:

- The word `Nikname` is literal text.
- Do not replace `Nikname` with a GitHub username.
- Do not remove backslashes.
- Do not reinterpret the marker.
- The marker means that `latest.txt` was successfully pushed to GitHub.

## Normal-size result contract

When a PowerShell command produces a result that fits safely in chat/clipboard transfer:

1. Show the result in the console.
2. Copy the same complete result to Windows Clipboard.
3. The user can paste that result directly into the chat.

## Large-result contract

When the result is too large to return directly:

1. Do not put the huge result into the chat.
2. Save the complete original result to:

```text
C:\Proj\Agents\Buffer-drops\latest.txt
```

3. Push `latest.txt` through normal local Git:

```text
git add
git commit
git push
```

4. Only after successful `git push`, copy this exact single line to Windows Clipboard:

```text
GitHub Nikname Buffer-drops\\latest.txt
```

5. Do not put any error, diagnostic text, commit message, Git output, or other text in Clipboard after successful push.

## Failure contract

If `git add`, `git commit`, or `git push` fails:

- Never copy the marker to Clipboard.
- Keep the complete result in local `latest.txt`.
- Report the Git failure separately.
- Do not claim that the result is available on GitHub.

## Command construction requirement

When giving a PowerShell command whose result may exceed the safe transfer size, the command itself must implement the decision:

```text
generate/collect result -> measure size -> either Clipboard full result OR Buffer-drops + Git push + exact marker
```

Do not give a separate manual simulation of the GitHub upload when the purpose is to test the actual PowerShell result-handling mechanism.

## Safe size threshold

The exact safe size depends on the chat/clipboard environment.

Default implementation threshold:

```text
32768 bytes of UTF-8 text
```

The threshold may be changed by the operator, but the contract above remains mandatory.

## Security and privacy

`latest.txt` may contain raw diagnostic output.

Treat it as sensitive by default.

Because Git keeps history, previous contents of `latest.txt` may remain in repository history.

Do not use Buffer-drops for secrets, private keys, tokens, passwords, personal data, or full result payloads unless:

- the repository is private;
- history retention is acceptable;
- or the caller explicitly sanitizes the result before handing it to Buffer-drops.

If raw output may contain secrets, the operator must choose a safer local-only mode or sanitize the output. This does not weaken the marker contract for allowed use cases.

## Non-goals

- Not a general file synchronization service.
- Not a secret manager.
- Not a replacement for Agent_ChatAi audit logging.
- Not a public data lake.
- No hard quarterly roadmap.

## Minimum acceptance criteria

A compliant PowerShell result handler must satisfy:

- Small result appears in console and Clipboard completely.
- Large result is written completely to `latest.txt`.
- Large result is pushed with normal Git commands.
- Clipboard receives the exact marker only after successful push.
- Clipboard receives no extra text after successful push.
- Git failure prevents marker copy.
- Git failure is reported separately.
- The marker is never simulated manually when testing the real mechanism.