---
name: review-code
description: Perform a comprehensive review of a PR, diff or codebase, including requirements and project quality gates, only when the user explicitly invokes review-code.
---

# Review Code

## Invocation

Run only when the user explicitly invokes this skill by name. Task similarity, reading this file, or another skill's recommendation is not an invocation. Do not invoke other workflow skills automatically. If the user names several skills, follow the requested sequence. Use the user's language for deliverables.

## Pin scope and reconstruct context

Identify the requested PR, revision range, working changes or codebase area. For a PR/branch, resolve the actual target and merge base; record base/head SHAs. For working changes include staged, unstaged and relevant untracked files. For a general review, define inspected subsystems instead of inventing a diff. State a reasonable inferred scope; ask only if materially ambiguous.

Read the original request/specification and relevant issue/PR discussion when available. Use explicit user references first; corroborate inferred sources. If requirements are unavailable, continue reviewing correctness and standards, and state that requirements coverage cannot be established.

Read applicable agent instructions, architecture decisions, CI, gate commands and representative implementations/tests. Inspect full affected functions, callers, consumers, schema/migrations and failure paths beyond the diff. Determine supported runtimes and dependency versions from project configuration and compare the actual environment. Judge syntax, APIs and compatibility code against those versions and conventions, not personal preferences.

## Review along three dimensions

- **Requirements:** map acceptance criteria to implementation and tests; identify missing, partial or contradictory behavior and unjustified scope expansion.
- **Correctness and integration:** trace concrete scenarios through boundaries. Check data integrity, transaction boundaries, concurrency, retries/idempotency, authorization, error propagation, compatibility, resource lifetime, migration/rollback and performance where applicable. Inspect whether tests can catch the relevant failure rather than merely passing.
- **Project fit:** check architectural boundaries, established patterns, dependencies, typing, observability and maintainability. Separate documented violations from optional design preferences; do not prescribe abstractions just because a code-smell label fits.

For a general codebase review, apply these dimensions across the stated scope, separating existing defects from change-related regressions. Report coverage limits; never claim a whole-project audit after sampling a small area.

## Execute the gates

Use the reviewed snapshot and the project's environment/task runner. Run all applicable configured checks: required test suites, formatting in check mode, lint, types, build, coverage, benchmarks and other project gates. Use an isolated workspace when running them could disturb user work. Avoid autofix modes and do not modify production code during review unless fixes were requested.

Inspect existing CI for the exact reviewed SHA as additional evidence; do not equate an older green run with the current change. Report unavailable platforms, services or credentials as blocked checks. Compare suspected failures with the base where feasible. For performance claims, use reproducible equivalent baseline/candidate measurements; label unmeasured concerns as hypotheses.

## Report actionable findings

Lead with verified findings ordered by severity. For each, provide location, triggering scenario, impact, evidence and a minimal remedy. Use P0 for immediate critical impact, P1 for high-impact defects to fix before merge, P2 for ordinary actionable defects, and P3 for low-impact issues. Qualify confidence when evidence is incomplete. Deduplicate tool diagnostics and reasoning findings. Keep optional suggestions separate; do not inflate them into defects.

Then report requirements coverage, gate commands/results, inspected scope and unresolved limitations. Passing tests do not prove requirements coverage. If no actionable defects were found, say so with the verification limits. Do not post comments, submit reviews or change PR state unless explicitly authorized.
