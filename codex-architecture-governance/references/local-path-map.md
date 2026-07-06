# Local path map

This reference records the verified local Codex architecture snapshot from
2026-07-06. Re-check live paths before making current claims because counts and
plugin caches drift.

## Core roots

| Purpose | Path |
|---|---|
| Global Codex home | `%USERPROFILE%\.codex` |
| Global open-agent skills | `%USERPROFILE%\.agents\skills` |
| Current project workspace | `C:\Users\admin\Documents\Codex配置` |
| Current project knowledge | `%USERPROFILE%\.codex\knowledge\projects\Codex配置-a3bbcc3fe7` |
| Current architecture snapshot | `C:\Users\admin\Documents\Codex配置\outputs\codex-local-architecture-20260706.md` |

## Verified counts on 2026-07-06

| Area | Count or status |
|---|---:|
| `%USERPROFILE%\.codex\skills` | 6 folders before this skill was added |
| `%USERPROFILE%\.codex\skills\.system` | 5 system skills |
| `%USERPROFILE%\.agents\skills` | 64 folders |
| `%USERPROFILE%\.codex\knowledge\projects` | 12 project stores |
| `%USERPROFILE%\.codex\agents` | 3 agent configs |
| `%USERPROFILE%\.codex\prompts` | 94 ECC prompt templates |
| Trusted projects in `config.toml` | 17 entries |
| Existing trusted project paths | 14 entries |
| Missing or stale trusted project paths | 3 entries |

After adding this skill, the `%USERPROFILE%\.codex\skills` count should increase
by one.

## Known global files

- `%USERPROFILE%\.codex\AGENTS.md`
- `%USERPROFILE%\.codex\config.toml`
- `%USERPROFILE%\.codex\hooks.json`
- `%USERPROFILE%\.codex\agents\explorer.toml`
- `%USERPROFILE%\.codex\agents\reviewer.toml`
- `%USERPROFILE%\.codex\agents\docs-researcher.toml`

## Hook setup

`%USERPROFILE%\.codex\hooks.json` uses:

```text
%USERPROFILE%\.codex\skills\knowledge-context-manager\scripts\context_hook.py
```

for:

- `SessionStart`
- `PreCompact`
- `PostCompact`
- `Stop`

## Plugin cache roots

- `%USERPROFILE%\.codex\plugins\cache\openai-bundled`
- `%USERPROFILE%\.codex\plugins\cache\openai-curated`
- `%USERPROFILE%\.codex\plugins\cache\openai-curated-remote`
- `%USERPROFILE%\.codex\plugins\cache\openai-primary-runtime`

## Safe audit commands

Redact values from `config.toml`; never print secrets.

```powershell
Get-ChildItem -LiteralPath "$env:USERPROFILE\.codex" -Force
Get-ChildItem -LiteralPath "$env:USERPROFILE\.codex\skills" -Directory
Get-ChildItem -LiteralPath "$env:USERPROFILE\.agents\skills" -Directory
Get-Content -LiteralPath "$env:USERPROFILE\.codex\hooks.json"
Select-String -LiteralPath "$env:USERPROFILE\.codex\config.toml" `
  -Pattern '^\s*\[.*\]|^\s*[A-Za-z0-9_.-]+\s*=' |
  ForEach-Object { $_.Line -replace '=.*$','= <redacted>' }
```

## Stale path caution

The 2026-07-06 audit found several trusted project entries that appeared to be
old mojibake or renamed Chinese paths. Treat missing paths as cleanup candidates,
not as evidence that a project has no source files.
