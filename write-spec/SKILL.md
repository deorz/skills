---
name: write-spec
description: Create a project vision or implementation-ready feature specification only when the user explicitly invokes write-spec.
---

# Write Spec

## Invocation

Run only when the user explicitly invokes this skill by name. Task similarity, reading this file, or another skill's recommendation is not an invocation. Do not invoke other workflow skills automatically. If the user names several skills, follow the requested sequence. Use the user's language for deliverables.

## Ground the decisions

Read the request and existing decisions first. In an existing project, inspect applicable agent instructions, architecture notes, affected interfaces, and representative code and tests. Identify supported runtimes and dependencies from manifests, locks, build/deployment configuration and CI; distinguish intended targets from the locally available environment. For a new project, state proposed choices as proposals.

Investigate facts yourself. Ask only questions whose answers materially change behavior, scope, contracts or irreversible choices; ask at most two at a time and give a recommendation. State reversible assumptions. Challenge contradictions and unnecessary scope without turning the conversation into an exhaustive interview.

## Produce one specification

Use one document as the source of truth; do not generate a separate implementation plan. Scale detail to uncertainty and scope. Omit sections that add no decisions.

- **Outcome and boundaries:** problem, intended users, observable success, included and excluded work.
- **Context and approach:** current behavior, relevant modules and conventions, chosen approach and consequential trade-offs. Cite useful existing paths as navigation aids, not an exhaustive file-edit inventory.
- **Contracts and behavior:** inputs/outputs; field types, requiredness, defaults and validation; errors; state transitions and invariants. Describe APIs, events, persistence or UI behavior at the level the task needs. Use a schema or short example where prose would be ambiguous.
- **Integration and constraints:** supported versions, compatibility, dependencies, ownership and module boundaries. Address transactions, retries/idempotency, concurrency, security, performance, migrations and rollout only where relevant.
- **Acceptance and verification:** numbered observable criteria, meaningful negative/boundary cases, existing test seams and analogous tests. Identify actual project gate commands and prerequisites where discoverable; distinguish confirmed commands from proposals.
- **Uncertainty:** assumptions, unresolved decisions and evidence needed to settle them. Mark implementation-blocking questions explicitly.

For a project vision, define capabilities, system boundaries, MVP and success criteria; detail the first useful slice and leave later contracts explicitly unresolved. For a bounded feature, resolve implementation-critical contracts. Add milestones, dependencies or migration order inside this document only when sequencing matters; avoid step-by-step coding instructions and speculative implementations.

## Finish

Check for contradictions, untestable requirements and hidden product decisions. Mark the result as a vision, draft with blockers, or implementation-ready specification; do not call it approved without user approval. Save it in the requested location or the project's existing specification location; absent a convention, use `docs/specs/<topic>.md`. Update the same document when decisions change.

Report the location, key decisions and remaining blockers. Creating the specification does not start implementation or publish it to a tracker.
