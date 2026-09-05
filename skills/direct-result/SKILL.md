---
name: direct-result
description: Skip unrequested defensive verification and testing after operations or code changes, while honoring explicit requests for reviews or checks. Use when the user asks for direct execution, no verification, no tests, or fewer defensive checks.
---

# Direct Result

Favor the shortest path from the requested action to the delivered result.

## Do the requested action once

- Perform the primary action needed to produce the requested result.
- When the user explicitly requests a review, test, validation, audit, verification, or post-operation check, treat it as part of the primary action and perform it as requested.
- After that action returns, stop operating and report its direct result.
- Do not add unrequested follow-up commands, tool calls, or reads whose main purpose is gaining confidence that the action worked.
- Do not retry a successful action merely to confirm it.

## Skip unrequested post-operation verification

Unless the user explicitly requests them, do not perform defensive checks such as:

- querying status, state, health, process, job, deployment, repository, or endpoint state after the action;
- reopening or rereading written files, records, messages, pages, or configuration;
- comparing diffs, metadata, timestamps, sizes, counts, or before/after state;
- calculating or comparing hashes, checksums, signatures, or file-content fingerprints;
- checking file existence or searching for the produced content after writing it;
- running validation, audit, smoke-check, dry-run, or consistency-check commands;
- adding extra retries, waits, polling, or fallback attempts solely as verification.

Do not describe the result as verified, validated, tested, confirmed, or proven unless the requested checks actually produced that result. Report only what the primary operation directly returned. If it returned an error or ambiguous result, state that plainly without launching unrequested verification.

## Do not add unrequested code testing

For code creation or modification, unless the user explicitly requests otherwise:

- Do not write, update, generate, or run tests of any kind, including unit, integration, end-to-end, regression, snapshot, property, performance, acceptance, or smoke tests.
- Do not run test suites, linters, type checkers, static analyzers, format checks, coverage tools, compile checks, or builds whose purpose is to validate the change.
- Do not manually exercise the changed behavior, start an application to inspect it, or inspect logs as a substitute for tests.
- Do not add assertions, diagnostic probes, temporary instrumentation, or verification-only code.

If the user explicitly requests tests, validation, auditing, review, verification, or confirmation, perform the requested work normally. Do not ask the user to suspend this skill. Keep the checks within the requested scope and do not append additional defensive checks afterward.

## Keep necessary boundaries

- A prerequisite read or check is allowed only when it is necessary to understand the target, avoid overwriting unrelated user work, satisfy an authorization requirement, or perform the requested action at all. Keep it minimal and do not repeat it afterward.
- A build, conversion, export, or render is allowed when it is the requested deliverable or the operation that produces the deliverable; do not run it again to verify the output.
- An explicit user request for review or verification overrides this skill's default preference to skip it.
- Follow higher-priority safety, permission, destructive-action, and mandatory tool-workflow requirements. This skill does not authorize bypassing them.
- Never claim success that the primary tool or command did not report.

## Report concisely

Return the result, changed artifact, or direct operation output. When checks were explicitly requested, include their results. Otherwise, mention that checks or tests were skipped only when that fact materially affects the handoff or the user asks.
