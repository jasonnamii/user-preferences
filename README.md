# User Preferences (UP)

**DSL-based instruction set for Claude Cowork / Claude Code.** A compact 15-line configuration that controls how Claude behaves — blind spot flagging, confidence scoring, tone, source hierarchy, and more.

UP is not a prompt template. It's a living document that evolves with use. Each rule is a DSL directive that Claude reads at the start of every session, enforced across all tasks.

### What's Inside

- `UP_user-preferences_v29.5.md` — The active UP file (15 rules, DSL format)
- `UP_stability.md` — Stability map tracking each rule's maturity (frozen / stable / trial)

### Current Rules (v29.5)

| Rule | Purpose |
|---|---|
| **BLIND_SPOT** | Proactively flag gaps, risks, and assumptions in the user's frame |
| **EXECUTOR** | Work within the user's frame with autonomous modification rights |
| **CONFIDENCE** | Attach confidence scores (90/70/50/30) with mandatory verification |
| **INFO_BRANCH** | Stop and ask when decision-info is insufficient |
| **TONE** | Assertive by default; no hedging when confidence ≥70 |
| **NUM_VERIFY** | Python verification mandatory for monetary amounts |
| **SOURCE** | Source hierarchy: official > industry > general |
| **EDIT4** | 5-level edit impact assessment with gates |
| **OBSIDIAN** | MCP read-only hard guard for Obsidian vault |
| **MCP_SPEED** | Default parameters for Desktop Commander, Obsidian, and Cowork tools |

### Design Philosophy

The key insight: **rules that Claude already follows by default should be deleted, not kept for safety.** v29.0 reduced 345 lines to 37, then further compressed to 15 lines by extracting detailed procedures into dedicated skills (deliverable-engine, session-briefing, trigger-dictionary, etc.).

### Usage

These preferences are loaded automatically in Claude Cowork sessions. To use in Claude Code, reference the UP file in your project's `CLAUDE.md`:

```markdown
# User Preferences
See: [UP_user-preferences_v29.5.md](path/to/UP_user-preferences_v29.5.md)
```

## Part of Cowork Skills

This is part of 25+ custom skills. See the full catalog: [github.com/jasonnamii/cowork-skills](https://github.com/jasonnamii/cowork-skills)

## License

MIT License — feel free to use, modify, and share.
