# RULES.md

These rules are mandatory for AI assistants and PowerShell command generation related to Buffer-drops.

## Primary rule

Always read `HANDOFF.md` first.

`HANDOFF.md` is the source of truth.

## For AI assistants

Before answering or generating code for Buffer-drops:

1. Read `HANDOFF.md`.
2. Follow the normal-size and large-result contracts.
3. Do not invent functionality that contradicts `HANDOFF.md`.
4. Do not replace the literal marker text.
5. Do not simulate successful GitHub push.
6. Do not copy the marker to Clipboard after failed Git operations.
7. Keep explanations short, direct, and practical.

## Exact marker

The exact marker is:

```text
GitHub Nikname Buffer-drops\\latest.txt
```

Rules:

- `Nikname` is literal text.
- Do not replace `Nikname` with a GitHub username.
- Do not remove backslashes.
- Do not translate the marker.
- Do not wrap it in extra text when copying to Clipboard.

## PowerShell command rule

When generating a PowerShell command whose result may be large, the command itself must implement:

```text
generate/collect result
measure size
if small:
    show in console
    copy full result to Clipboard
else:
    write full result to C:\Proj\Agents\Buffer-drops\latest.txt
    git add latest.txt
    git commit
    git push
    if push succeeded:
        copy exact marker to Clipboard
    else:
        do not copy marker
        report Git failure separately
```

Do not provide a separate manual simulation of GitHub upload when the purpose is to test the real PowerShell result-handling mechanism.

## Clipboard rule

After successful large-result push, Clipboard must contain exactly one line:

```text
GitHub Nikname Buffer-drops\\latest.txt
```

No extra text.

No diagnostic output.

No commit message.

No Git output.

No error text.

## Failure rule

If `git add`, `git commit`, or `git push` fails:

- do not copy the marker;
- keep `latest.txt` locally;
- report the failure separately;
- do not claim that the result is available on GitHub.

## Privacy rule

Treat `latest.txt` as potentially sensitive.

Do not use Buffer-drops for secrets unless:

- repository is private;
- history retention is acceptable;
- or output is sanitized.

## Development style

- Iterative.
- Minimal.
- No premature abstraction.
- No hard quarterly roadmap.
- Documentation must match `HANDOFF.md`.