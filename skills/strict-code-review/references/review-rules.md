# P0-P4 Review Rules

Apply correctness before performance, then maintainability, style, and process. These rules adapt [issue #32](https://github.com/zhaochenyang20/sglang-diffusion-routing/issues/32) to reusable reviews. Keep findings tied to the actual change, runtime, and repository contracts.

## [P0-BLOCKER] Correctness

### Fail fast and validate at boundaries

- Check race conditions, indexing, tensor shape and dtype compatibility, resource leaks, and missing guards against reachable invalid states.
- In Python, reserve `assert` for internal programmer invariants that need not survive optimized execution. Reject invalid user input with explicit exceptions such as `ValueError` or `TypeError`.
- Validate at public APIs and system boundaries, including HTTP handlers and configuration parsing. Let internal helpers rely on established preconditions instead of repeating those checks.
- Prefer guard clauses and early returns when they make the normal path easier to follow.
- Challenge fallback values and defensive handling that conceal broken invariants or protect impossible states. Establish that a state is impossible before removing its guard; failure probability alone is insufficient.
- Keep each `try/except` region narrow and catch the exceptions the operation can reasonably handle. Flag broad catches that hide unrelated bugs or turn failure into apparent success.

### Concurrency and thread safety

- Make thread-safety guarantees clear for shared classes and functions.
- Keep locks narrow. Move blocking I/O and GPU work outside critical sections when ownership and ordering allow it; establish those guarantees before recommending lock removal.
- Prefer queues or other message passing where they simplify shared mutable state. Check lifetime, ordering, and shutdown behavior rather than treating a queue as proof of safety.

### Resource lifetime and bounds

- Require profiling evidence before recommending explicit intermediate-tensor deletion or `torch.cuda.empty_cache()` as a memory fix.
- Use context managers for files, sockets, and CUDA stream contexts when the API supports them; otherwise require explicit lifetime ownership and reliable cleanup.
- Give long-lived caches and buffers a bound, eviction policy, or demonstrably bounded lifecycle. Flag append-only accumulation in persistent loops.

## [P1-PERF] Performance

- Locate the inference or request hot path before judging overhead.
- Check frequent `.item()`, `.cpu()`, and `.tolist()` calls for device synchronization or host transfers. Explain the particular operation and call frequency involved.
- Keep GPU data processing vectorized and on device where possible. Investigate CPU fallbacks and repeated transfers on latency-critical paths.
- Check Python loops, repeated dispatch, and avoidable allocation in tight loops. Explain which work can be batched, reused, vectorized, or moved out of the loop.
- Prefer profiling or benchmark evidence for claimed gains; distinguish a plausible mechanism from a measured regression.

## [P2-MAINTAIN] Maintainability

### Structure and reuse

- The source guide calls for shared helpers when duplication exceeds 5 lines, splitting files above 2,000 lines, and keeping functions near or below 50 lines excluding docstrings.
- Use those thresholds to inspect cohesion and maintenance cost. Extract meaningful shared behavior or logical substeps; avoid abstractions that merely hide a line count or couple unrelated code.
- Prefer focused modules or helpers when a file or function has several responsibilities. Explain the resulting ownership and call structure.

### Names

- Use descriptive public API names, such as `request_count`. Retain established domain abbreviations; the source explicitly allows familiar terms such as `num`, `idx`, `cfg`, and `bs`.
- Prefer predicate prefixes for booleans: `is_`, `has_`, `should_`, or `can_`.
- Keep paired operations symmetric: `start/stop`, `begin/end`, `open/close`, or `send/recv`.
- Qualify vague names such as `data`, `result`, `info`, `tmp`, `manager`, and `handler` when their role is otherwise unclear.

### Imports and constants

- Group standard-library, third-party, and local imports separately, in that order, subject to the repository's formatter.
- Avoid wildcard imports. Load expensive optional dependencies inside the function that requires them when this preserves the dependency contract.
- Break circular imports by clarifying dependency direction or extracting shared interfaces.
- Name values that encode a policy or domain constant. Obvious arithmetic or indexing uses of `0`, `1`, and `-1` do not need named constants.

## [P3-STYLE] Style

- Prefer functions whose results and side effects are explicit. Avoid mutating input arguments unless the contract calls for it; document intentional in-place forward-pass operations used to save memory.
- Keep constructors focused on necessary dependencies and parameters. Avoid passing a large configuration object when only a small, stable subset is needed.
- Prefer explicit attributes over unnecessary `getattr`/`setattr` indirection.
- Reserve conditional expressions for simple choices. Move substantial branch-specific work into meaningful private helpers.
- Ensure every reachable branch defines the values or return behavior promised by the function. Use complete `if/elif/else` alternatives where needed; early-return, raise, and continue guards need no redundant `else`.
- Treat nesting beyond three levels as a prompt to simplify control flow with guards, helpers, flatter branches, or a dispatch table when appropriate.
- Include type hints on function signatures, especially public APIs. Use an underscore prefix for module or class implementation helpers according to the language and repository conventions.
- Remove leftover debugging comments and ad hoc logs while preserving useful operational logging.
- The source project calls for English code comments and removal of Chinese comments. Apply that convention in repositories that adopt it; follow the target repository's language policy elsewhere. This does not set the language of the review report.

## [P4-PROCESS] Process

- Look for copyable verification commands in the PR description, with the environment or prerequisites needed to interpret them.
- Expect CI coverage for important behavior changes. Identify the concrete contract or failure mode missing from coverage.
- Prefer assertions about observable inputs and outputs over incidental internal representation.
- Fix random seeds where tests depend on randomness, and identify other uncontrolled sources of nondeterminism when relevant.
- Keep each test centered on one concept; several assertions can jointly establish that concept.
- Distinguish missing evidence from a demonstrated bug. Scale the verification request to the change and obey the user's execution constraints.
