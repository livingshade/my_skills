---
name: strict-code-review
description: Review pull requests and code changes using a four-part report and a P0-P4 rubric for correctness, performance, maintainability, style, and process. Use for substantive code review, especially Python, GPU inference, and concurrent systems, or when asked to apply the sglang-diffusion-routing issue 32 review style.
license: MIT
metadata:
  source: https://github.com/zhaochenyang20/sglang-diffusion-routing/issues/32
  source-author: zhaochenyang20
  source-updated: "2026-09-18T18:04:35Z"
  adapted-on: "2026-09-19"
---

# Strict Code Review

Review whether a change is necessary, correct, efficient, and complete. Explain the technical reason behind each finding and propose a concrete correction. Favor simple implementations that fit the actual system.

Adapted from Zhao Chenyang's [Code Review Style Guide, issue #32](https://github.com/zhaochenyang20/sglang-diffusion-routing/issues/32). The source repository publishes the [MIT license](LICENSE). This is a deliberate snapshot; do not silently refresh it from upstream.

## Establish the context

Read the PR description or requested goal, the diff, applicable repository instructions, and the surrounding callers, contracts, and tests needed to understand the changed behavior. Inspect changed lines in context. Distinguish new regressions from pre-existing issues.

Read [references/review-rules.md](references/review-rules.md) for the full P0-P4 rubric. Apply the relevant language and runtime rules; GPU rules matter when the execution path actually uses a GPU. Respect explicit user scope and established repository conventions.

A review request authorizes inspection and reporting. Edit code or publish comments only when the user requests those actions. If checks are run, report their actual results; distinguish proposed reproduction commands from commands already executed. State any material gaps in available code or evidence.

## Report in four parts

Use the reader's language. For a full review, cover these dimensions in order. If the user requests a narrower review or a specific response schema, honor it while retaining the relevant analysis.

### 1. PR Summary & Technical Background / PR 总结与技术背景

Explain the behavior being changed and the mechanism that makes it work. Discuss only technical background needed to assess this change: for example, tensor layout and dtype, gradient flow, synchronization, CUDA execution, distributed communication, attention, KV-cache lifetime, speculative decoding, or tensor parallelism.

Assess the need for the change. Check whether existing functionality already solves the problem and whether the added complexity is justified by the stated goal.

### 2. Code Quality & Correctness / 代码质量与正确性

Report actionable findings using the rubric labels, ordered by actual impact. For each finding, include:

- The exact file and narrow line range, with a usable link when available.
- The applicable label and named rule from the reference.
- A concrete trigger or violated invariant, the resulting behavior, and why it matters.
- The smallest correction that addresses the cause, with a reproduction or useful test case when it clarifies the issue.

Example finding shape:

> **[P0-BLOCKER] Correctness — concurrent mutation of worker state** · `router.py:84`  
> A completion callback can remove a worker while this loop iterates over the shared mapping, causing request dispatch to raise. Read a stable snapshot under the existing synchronization mechanism, then perform dispatch after releasing it.

Use `[P0-BLOCKER]`, `[P1-PERF]`, `[P2-MAINTAIN]`, `[P3-STYLE]`, and `[P4-PROCESS]` as this guide's rubric labels. They group rules; assess release urgency from the actual impact. If the review platform requires a separate priority field, use its priority scale rather than mechanically copying the rubric number.

Do not invent a failing scenario, benchmark, or missing requirement. For performance concerns, identify the hot path and mechanism; quantify the effect only when evidence supports it. Omit speculative or purely cosmetic findings. If no actionable issues are found, say so and explain which contracts or design choices support that conclusion, together with any meaningful limits of the review.

### 3. Goal Completeness / 目标完整性

Compare the implementation with the author's stated outcome. Identify incomplete behavior, reachable edge cases, integration gaps, and undocumented assumptions. Explain whether the design is sufficient and proportionate to the goal. Reference findings already reported instead of duplicating them.

### 4. AI-Generated Code Detection / AI 生成代码检测

Inspect for generic boilerplate that conflicts with local conventions, unnecessary defensive branches or fallback defaults, broad exception swallowing, obvious comments that miss domain constraints, inconsistent naming, gratuitous abstractions, and textbook algorithms that ignore the system's actual requirements.

Report the observable quality problem and its consequence. Code style alone does not establish AI authorship or the author's understanding; do not present such a guess as fact or assign blame. If provenance is explicitly supplied, separate it from the technical assessment. If nothing concrete is found, say so briefly.

## Reviewer stance

Be demanding about correctness and performance, candid about evidence, and useful to the author. Explain why a proposed correction works. Understand system invariants before challenging a design. Avoid findings whose only effect is stylistic churn; do not manufacture criticism to fill a section.

The reference preserves the source's numerical thresholds and comment-language convention as review defaults with their original context. Apply them with judgment. Likewise, remove redundant defenses only when an established contract makes them unnecessary; an uncommon failure can still require handling.
