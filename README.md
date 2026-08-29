# my_skills

Personal collection of reusable Agent Skills. The copies in this repository are the versions used with SGLang Router.

## Included skills

| Skill | Purpose |
| --- | --- |
| [`shuorenhua`](skills/shuorenhua/) | Rewrites Chinese and English text to remove formulaic AI phrasing while preserving facts, terminology, scope, and attribution. |
| [`no-negative-echo`](skills/no-negative-echo/) | Finalizes user-facing artifacts without carrying rejected, session-only alternatives into filenames, metadata, commits, or handoffs. |
| [`cyclomatic-complexity`](skills/cyclomatic-complexity/) | Measures cyclomatic complexity and guides behavior-preserving refactors of branching code. |

Each skill is self-contained under `skills/<name>/`. Supporting references, scripts, assets, and evaluation files are kept with the corresponding `SKILL.md` so relative links continue to work.

## Install

Copy one skill into the personal skill directory for your agent:

```bash
# Claude Code
cp -R skills/shuorenhua ~/.claude/skills/

# Codex
cp -R skills/shuorenhua ~/.codex/skills/
```

Replace `shuorenhua` with another directory name to install a different skill. For a project-scoped installation, copy the directory into the project's `.claude/skills/` or `.codex/skills/` directory instead.

After installation, start a new session or reload the agent's skills. Skills can be selected automatically from the description in `SKILL.md`; mentioning the skill name explicitly is the most predictable way to invoke one.

## Updating snapshots

These directories are stored as reviewed snapshots. Update a skill deliberately, preserve its supporting files, and review the complete diff before committing.
