---
name: implement-change
description: Implement a requested feature or fix in an isolated branch or worktree with project conventions and verified quality gates, only on explicit invocation of implement-change.
---

# Implement Change

## Invocation

Run only when the user explicitly invokes this skill by name. Task similarity, reading this file, or another skill's recommendation is not an invocation. Do not invoke other workflow skills automatically. If the user names several skills, follow the requested sequence. Use the user's language for deliverables.

## Establish the working contract

Use the user's request or existing specification directly; a separate spec or plan is not a prerequisite. Resolve only ambiguities that block correct implementation. Read applicable agent instructions, contribution docs, CI, task runners and representative neighboring code/tests. Identify established architecture, error handling, typing, dependency injection, fixtures and observability patterns.

Determine supported runtime/compiler and dependency versions from manifests, locks, toolchain files, containers and CI. Check the actual local toolchain. Follow project version policy and the supported range, not the newest syntax your machine accepts. Use current idioms allowed by that range and project conventions; add compatibility code or Python `__future__` imports only for a concrete requirement, including annotation semantics. Do not modernize unrelated code or upgrade dependencies to accommodate your implementation.

## Isolate and establish a baseline

Inspect branch, base commit, worktrees and staged/unstaged/untracked changes. Reuse an existing task-specific branch/worktree. Otherwise create a dedicated branch in a clean checkout; prefer a worktree when the current checkout has unrelated work or must remain available. Respect the user's preference and harness-managed workspaces. Keep manual worktrees outside tracked content or verify the chosen directory is ignored. Never reset, stash or overwrite unrelated work automatically.

Set up the environment using the project's package manager and lockfile. Inventory applicable gates from project commands and CI, including tests, formatting, lint, types, build, coverage, benchmarks and other configured checks. Establish a relevant baseline before edits. Record pre-existing failures; continue independent work where possible without disguising them as passing or expanding scope to fix them.

## Test selection gate

Before writing tests for a change, inspect existing coverage at the relevant test seams and choose what to reuse. If existing tests cover the affected logic and are suitable for the changed behavior, extend or parameterize them rather than adding overlapping tests. Add new tests only for scenarios that existing tests cannot cover clearly and appropriately; reuse established fixtures and helpers. Do not force unrelated scenarios into one test or weaken valid assertions to avoid adding a test.

Proceed only after identifying the existing tests to update or the specific coverage gap that requires new tests. Keep the resulting coverage focused and maintainable: no duplicate scenarios, parallel test suites or unnecessary fixtures for the same behavior.

## Implement and verify

Implement coherent behavioral slices using existing abstractions and conventions. Preserve behavior outside the request; explain necessary deviations from local patterns instead of silently introducing a competing architecture.

Follow the test selection decision when updating coverage for changed behavior. For a bug, reproduce the actual symptom and verify a regression test fails for that reason before the fix where feasible. Use small test cycles; avoid tests that merely mirror internals and blanket TDD ceremonies for nonbehavioral edits.

Before declaring completion or any authorized push, run every applicable required project gate on the final working state, including the full required test suite. Run existing benchmarks when required or relevant to a performance-sensitive change; compare baseline and candidate under equivalent conditions. Reuse results only while their inputs and environment remain unchanged; after edits rerun affected gates. Do not weaken checks, thresholds or assertions to obtain green results.

Review the final diff against the request, remove temporary diagnostics and check scope. This self-check does not invoke another skill. A blocked or unrun required gate means validation is incomplete; explain the blocker and exact remaining command. Do not use repeated pushes as a substitute for local verification. When publishing is authorized, consolidate the verified change into one push unless the user requests otherwise; report remote-only checks separately.

## Report

State implemented behavior, workspace/branch, material deviations and remaining risks. Give gate commands with pass/fail/blocked status and relevant results; distinguish code completion from validation completion. Commit, publish, merge or remove worktrees only within the user's authorization; otherwise leave the reviewable changes available.
