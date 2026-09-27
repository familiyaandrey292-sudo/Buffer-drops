# SECURITY.md

## Scope

Buffer-drops transfers raw PowerShell diagnostic or test results.

It may contain:

- file paths;
- environment details;
- process information;
- network information;
- configuration fragments;
- logs;
- test output;
- partial or full result payloads.

Therefore, `latest.txt` must be treated as sensitive by default.

## GitHub repository

The Buffer-drops repository should be private unless the result is known to be non-sensitive.

Recommended repository:

```text
https://github.com/familiyaandrey292-sudo/Buffer-drops
```

## Git history risk

Git keeps history.

Even if `latest.txt` is overwritten later, previous contents may remain in commits.

Do not push:

- passwords;
- API keys;
- personal access tokens;
- private keys;
- session tokens;
- browser auth secrets;
- Ed25519 private keys;
- confirmation tokens;
- personal data;
- full sensitive LLM responses;
- raw audit records containing sensitive arguments or payloads.

## Agent_ChatAi boundary

Agent_ChatAi has privacy-conscious audit logging and intentionally excludes action arguments and full result payloads from audit records.

Buffer-drops does not provide the same privacy guarantee automatically.

If Buffer-drops is used for Agent_ChatAi diagnostics, the caller must decide whether the raw result is safe to transfer.

## Safe usage pattern

Use Buffer-drops when:

- result is large;
- result is not secret;
- repository is private or result is non-sensitive;
- history retention is acceptable.

Do not use Buffer-drops when:

- result may contain secrets;
- repository is public;
- user did not explicitly allow remote transfer;
- local-only inspection is sufficient.

## Marker security

The marker:

```text
GitHub Nikname Buffer-drops\\latest.txt
```

is not a secret.

It only signals that `latest.txt` was pushed successfully.

However, anyone with access to the GitHub repository can read `latest.txt`.

Therefore, repository access control is the real security boundary.