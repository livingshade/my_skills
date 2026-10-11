# my_skills

Personal collection of reusable Agent Skills.

## Included skills

| Skill | Purpose | Source |
| --- | --- | --- |
| [`strict-code-review`](skills/strict-code-review/) | Reviews PRs with technical context, a P0-P4 rubric, goal completeness, and evidence-based code quality analysis. | [zhaochenyang20/sglang-diffusion-routing issue #32](https://github.com/zhaochenyang20/sglang-diffusion-routing/issues/32) |
| [`anti-slop`](skills/anti-slop/) | Keeps ordinary model output direct, literal, natural, and appropriately formatted. | [OpenAI GPT-6 Astra model guidance](https://developers.openai.com/api/docs/guides/latest-model?model=gpt-6-astra) · [Anthropic Claude Fable 5.1 prompting guide](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-1) |
| [`dr`](skills/dr/) | Completes work directly, writes from the accepted result, gives clear judgments, and filters candidates without defensive hedging or strawman contrasts. | Local personal skill snapshot · [LB623/no-negative-echo](https://github.com/LB623/no-negative-echo) · [OpenAI GPT-6 Astra model guidance](https://developers.openai.com/api/docs/guides/latest-model?model=gpt-6-astra) · [Anthropic Claude Fable 5.1 prompting guide](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-1) |
| [`cyclomatic-complexity`](skills/cyclomatic-complexity/) | Measures cyclomatic complexity and guides behavior-preserving refactors of branching code. | [saurabhkumar8112/cyclomatic-complexity-skill](https://github.com/saurabhkumar8112/cyclomatic-complexity-skill) |
| [`karpathy-guidelines`](skills/karpathy-guidelines/) | Reduces common LLM coding mistakes through simplicity, surgical changes, explicit assumptions, and verifiable goals. | [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) |
| [`test-driven-development`](skills/tdd/) | Guides test-first development with a feasibility gate and practical rules for meaningful tests. | Local personal skill snapshot |

Each skill is self-contained under `skills/<name>/`. Its directory contains `SKILL.md`, the files loaded or referenced during normal use, and any license file shipped by the upstream project. When upstream declares the license only in skill metadata, that declaration remains in `SKILL.md`.

## Install

Copy one skill into the personal skill directory for your agent:

```bash
# Claude Code
cp -R skills/anti-slop ~/.claude/skills/

# Codex
cp -R skills/anti-slop ~/.codex/skills/
```

Replace `anti-slop` with another directory name to install a different skill. For a project-scoped installation, copy the directory into the project's `.claude/skills/` or `.codex/skills/` directory instead.

After installation, start a new session or reload the agent's skills. Skills can be selected automatically from the description in `SKILL.md`; mentioning the skill name explicitly is the most predictable way to invoke one.

## Updating snapshots

These directories are stored as reviewed snapshots. Update a skill deliberately, preserve its supporting files, and review the complete diff before committing.
