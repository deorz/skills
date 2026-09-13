---
name: investigate
description: Investigate a codebase, diagnose a bug, evaluate a feature or run a bounded R&D experiment, only when the user explicitly invokes investigate.
---

# Investigate

## Invocation

Run only when the user explicitly invokes this skill by name. Task similarity, reading this file, or another skill's recommendation is not an invocation. Do not invoke other workflow skills automatically. If the user names several skills, follow the requested sequence. Use the user's language for deliverables.

## Frame the question

State the question, observable symptom or decision to resolve and what evidence would answer it. Bound the investigation to the relevant subsystem and risk; do not require an arbitrary number of hypotheses or experiments. Read the request, applicable project instructions, architecture notes and relevant history. Determine runtime/dependency versions, deployment configuration and how the available environment differs.

Map the relevant entry points, data/control flow, dependencies and existing analogous behavior. Investigate repository facts yourself; ask the user only for consequential choices or unavailable evidence. Follow external technical claims to version-appropriate primary sources: official documentation, implementation, specifications or release notes. Mark observations, deductions and hypotheses distinctly.

## Choose the useful investigation

**Bug or regression:** capture expected versus actual behavior and the conditions under which it occurs. Build the smallest practical reproducible signal: a test, fixture replay, command, trace or measurement. Confirm it exercises the reported failure, not an unrelated setup error. Trace the failing path and compare a working version or neighboring implementation. Rank plausible causes from evidence; state a falsifiable prediction and test one variable at a time. Record what each experiment rules out. If reproduction is unavailable, continue static analysis and propose a discriminating experiment, but do not label an untested theory a confirmed cause. For intermittent failures, record attempts and failure rate rather than claiming certainty from one successful run.

**Feature, architecture or project reconnaissance:** identify existing capabilities, constraints, extension points and affected contracts. Compare viable options on the dimensions that change the decision: fit, complexity, operational burden, compatibility and cost. Recommend one with explicit trade-offs and unresolved assumptions; do not invent alternatives just to fill a table.

**Feasibility or performance spike:** define the assumption, smallest useful experiment and success/failure signal before building it. Isolate disposable code and data from production paths; use a separate worktree when editing repository code. Record commands, environment, inputs and results. For performance, compare equivalent baselines and repeated measurements; avoid a universal claim from one microbenchmark. Label prototypes as experimental and preserve only evidence needed to reproduce the finding.

## Conclude with evidence

Stop when the question is answered to the needed confidence or the remaining uncertainty requires unavailable evidence. If probes stop adding information, reassess the hypothesis instead of stacking speculative fixes. State the specific missing log, trace, environment or experiment that would discriminate the remaining causes.

Deliver: answer/recommendation, supporting code locations or sources, experiments and outcomes, confidence/limitations, and the smallest next action. For a diagnosed bug, include the proposed correction and regression scenario. For a feature, include affected boundaries and decisions a specification must resolve.

Save substantial reusable findings in the requested location or the project's research convention; keep a small finding in the response. Remove your temporary instrumentation or clearly identify retained experimental artifacts. Do not turn a successful prototype into a production implementation, publish findings externally or invoke a follow-up skill automatically.
