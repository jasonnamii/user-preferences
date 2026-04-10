# User Preferences (UP)

> 🇰🇷 [한국어 README](./README.ko.md)

**A DSL-based behavioral instruction set for Claude Cowork and Claude Code.**

## Prerequisites

- **Claude Cowork or Claude Code** environment
- **[up-manager](https://github.com/jasonnamii/up-manager)** skill — for editing and version management

## Goal

Large language models behave generically by default. User Preferences (UP) is a compact 15-line DSL configuration that shapes Claude's behavior at the session level — enforcing blind spot detection, confidence scoring, edit protocols, tone rules, and source hierarchies. It turns a general-purpose assistant into a calibrated operating partner.

UP is not a prompt template. It is a living specification that evolves through use, managed by a dedicated skill ([up-manager](https://github.com/jasonnamii/up-manager)) with version control, stability tracking, and propagation.

## When & How to Use

UP loads automatically at the start of every Claude Cowork and Claude Code session. You don't invoke it — it runs in the background as a persistent behavioral contract. To modify rules, use the `up-manager` skill. To sync changes to GitHub, use `git-sync`.

## What's Inside

`UP_user-preferences_v29.5.md` — The active configuration (15 rules, pure DSL syntax):

| Rule | What It Does |
|---|---|
| **BLIND_SPOT** | Proactively flags gaps, risks, and assumptions the user hasn't considered |
| **EXECUTOR** | Works within the user's frame with autonomous modification rights (add, reorder, rephrase) |
| **CONFIDENCE** | Attaches confidence scores (90/70/50/30) with mandatory verification at ≥70 |
| **INFO_BRANCH** | Stops and asks when decision-critical information is insufficient |
| **TONE** | Assertive by default; no hedging when confidence ≥70 || **NUM_VERIFY** | Python verification mandatory for monetary amounts |
| **SOURCE** | Source hierarchy: official > industry > general; tertiary alone not allowed |
| **EDIT4** | 5-level edit impact assessment (L0–L4) with escalating gates |
| **OBSIDIAN** | MCP read-only hard guard for Obsidian vault — no write via MCP under any circumstance |
| **MCP_SPEED** | Default parameters for Desktop Commander, Obsidian, and Cowork tool calls |

## Design Philosophy

Rules that Claude already follows by default are deleted, not kept for safety. v29.0 reduced 345 lines to 37, then further compressed to 15 lines by extracting detailed procedures into dedicated skills (deliverable-engine, session-briefing, trigger-dictionary, etc.). Every remaining rule exists because Claude would not do it without explicit instruction.

## Key Features

- **Pure DSL syntax** — no natural language prose; uses `::=`, `→`, `|`, `ENUM`, `GATE:`, `IF` operators for zero-ambiguity parsing
- **Version-controlled** — semantic versioning with changelog tracking; managed by up-manager skill
- **Stability-tracked** — each rule has a maturity status (frozen → stable → trial) with promotion criteria
- **Skill-delegated** — complex procedures live in dedicated skills, not in the UP file itself

## Works With

- **[up-manager](https://github.com/jasonnamii/up-manager)** — edits, version-bumps, and propagates UP changes
- **[git-sync](https://github.com/jasonnamii/git-sync)** — syncs UP to this GitHub repo after modifications

<!-- 🥚 v1부터 v29까지, 가장 많이 수정된 규칙은 "수정 규칙" 자체였다. — N.C. -->

- **[session-briefing](https://github.com/jasonnamii/session-briefing)** — references UP rules when generating session context
## Usage

These preferences load automatically in Claude Cowork sessions. To use in Claude Code, reference the UP file in your project's `CLAUDE.md`:

```markdown
# User Preferences
See: [UP_user-preferences_v29.5.md](path/to/UP_user-preferences_v29.5.md)
```

## Part of Cowork Skills

This is part of 25+ custom skills. See the full catalog: [github.com/jasonnamii/cowork-skills](https://github.com/jasonnamii/cowork-skills)

## License

MIT License — feel free to use, modify, and share.