---
name: review-spec
description: Independently review a project vision or change specification against its intent, behavioral contracts and codebase, resolve findings, and optionally iterate authorized spec fixes. Use only when the user explicitly invokes review-spec.
---

# Review Spec

Find gaps that would make another engineer build the wrong thing, then establish whether the specification is ready for its intended next step. Keep author context with the coordinator and obtain independent readings from fresh reviewers.

## Invocation and scope

- Run only when the user explicitly invokes `review-spec` or `$review-spec`. Reading this file, a similar task, or another skill recommending review does not invoke it.
- Do not automatically start another workflow skill, implementation, or code review. A specification and its review report are the outputs of this workflow.
- Default to review only: inspect sources, report findings, and propose fixes. Edit the specification or its supporting documents only when the user authorizes those edits. Authorization in the initial request is sufficient; do not request it again for each supported correction.
- Do not modify production code or tests. Distinguish a proposed specification change from one actually applied.
- Use the user's language for the report and questions; preserve established document conventions when editing.
- If delegated as a reviewer, follow only the shared reviewer contract and your assigned lens below. Return findings to the coordinator; do not orchestrate reviewers, edit files, or start a review loop.

## Establish the review inputs

1. Identify the specification, its purpose and stage, relevant project instructions, explicit source documents, and available codebase. Use the invocation and existing context before asking for missing information.
2. State the intended next step: exploring a vision, deciding an architecture, or implementing a bounded change. Match review depth to that stage. A vision needs clear outcomes and constraints; implementation readiness additionally needs observable behavior, boundaries, failure handling, and acceptance evidence appropriate to the change.
3. Identify which requirements preserve existing behavior and which deliberately change or supersede it. Treat unsupported assumptions as assumptions, not requirements. Current code and tests describe existing behavior; they cannot alone establish the intended behavior of a change.
4. Establish a stable input version for each round. Record the specification and source paths with a revision or content hashes, including relevant uncommitted changes. Give every reviewer access to that same version. Pin or copy inputs when concurrent changes are possible; invalidate a round if relevant inputs change during it.
5. Keep author discussion, prior reviewer reports, and the coordinator's preferred answer out of reviewer inputs. Reviewers receive the specification, source documents explicitly supplied for this review or linked by the specification, relevant repository instructions, access to the same source snapshot, and their lens. A requirement known only from discussion is a possible documentation gap. If edits are authorized, record supported decisions in the specification or an explicit source before beginning a round; otherwise assess the gap during resolution.

Use one aggregate report, following the repository's review-document convention or placing `<spec-stem>.review.md` alongside the specification. A requested chat-only review needs no file. Do not create a file per reviewer or turn the report into a second specification.

## Run independent review rounds

Use actual fresh-context subagents for these four lenses. Inspect the host's delegation interface and disable inherited conversation/history; do not assume a particular tool or option name. Start new reviewers in every round, including confirmation rounds.

| Lens | Task |
| --- | --- |
| Comprehension | Read the specification and those explicit source documents without relying on implementation knowledge. Restate the problem, intended behavior, scope, and acceptance conditions. Identify plausible conflicting interpretations and decisions an implementer would have to invent. |
| Architecture | Assess whether the proposed approach solves the stated problem within its constraints. Check boundaries, dependencies, reuse, complexity, and consequential alternatives against the available project evidence. |
| Behavioral contracts | Examine invariants, state transitions, inputs and outputs, failure cases, compatibility, and observable acceptance criteria. Separate retained contracts from intentional changes and identify meaningful validation at stable boundaries. |
| Codebase fit | Inspect the actual repository to check named components, APIs, data models, integration points, dependencies, and implementation assumptions. Identify concrete mismatches and missing work that would prevent the stated behavior. |

The comprehension reviewer does not receive a codebase tour or private author context. Other reviewers may navigate the source snapshot independently. All reviewers may follow explicit specification references and must disclose inaccessible evidence.

- Dispatch one lens per reviewer. Run concurrently where supported, or in batches when slots are limited. Do not replace a missing lens with the coordinator's own reading.
- Pass the shared contract, assigned lens, stage, input manifest, and report destination or return channel. State that the delegated invocation is reviewer-only. Reviewers must not see each other's findings or prior dispositions.
- Keep reviewers read-only. Do not edit the specification until all results for the round are collected.
- Retry a failed reviewer once with the same inputs and a fresh context. Missing, failed, or inaccessible required coverage makes the round incomplete; it is never a clean result. Do not claim codebase fit was verified when no relevant codebase was available.
- For an explicitly greenfield vision with no existing system in scope, the codebase-fit reviewer may complete its lens by confirming that applicability and checking available dependency or integration evidence. Record existing-code fit as not applicable, not verified. This counts as coverage appropriate to that stage; it cannot excuse an inaccessible existing codebase that the specification depends on.
- If the host cannot launch fresh independent contexts, provide useful preliminary observations and mark independent review unavailable. Do not imitate reviewers in one context or claim that this workflow completed.

### Shared reviewer contract

Report issues that could change the implementation, invalidate a decision, break a required behavior, or prevent meaningful acceptance. Scale expectations to the stated stage and scope.

For each finding, return:

- A concise title, severity, and confidence.
- The specification section or location and a short description of the claim being challenged.
- A concrete failure scenario or conflicting interpretation and its consequence.
- Supporting evidence with source locations. Label inferences and unavailable evidence explicitly.
- The smallest useful correction or the decision needed; do not invent the decision.

Use `P0` for an immediate critical blocker, `P1` for a major correctness or feasibility problem, `P2` for another actionable gap, and `P3` for optional polish. Importance comes from the demonstrated consequence, not from a topic label such as security or architecture. A material unknown may require a user decision without being a proven defect.

Also return the inputs inspected, coverage limits, and a compact interpretation of the specification. The comprehension reviewer must explicitly paraphrase the problem, behavior, scope, and acceptance conditions. An empty finding list means no supported issue found within the stated coverage; it is not proof of correctness.

Do not demand an exhaustive file list, implementation plan, every function signature, speculative infrastructure, or a preferred architecture. Do not flag deliberate behavior changes merely because existing code or tests differ. Check test expectations against independently stated requirements; reject assertions that only mirror an implementation. Reuse or revise existing behavioral coverage where appropriate, and identify obsolete expectations when requirements supersede them. Do not require tests for cosmetic or otherwise low-impact changes without a concrete risk.

## Resolve findings with evidence

After a complete round, compare the independent paraphrases with the documented intent and the coordinator's author context. Merge duplicate root causes without losing distinct scenarios or source evidence. Do not decide correctness by majority vote.

Assign stable finding IDs and one of these dispositions:

| Disposition | Required evidence and next step |
| --- | --- |
| Confirmed | Record the supported gap and proposed correction. It remains open until corrected and independently rechecked. |
| Refuted | Cite evidence showing that the alleged scenario cannot occur or falls outside documented scope. Explain why the evidence answers the finding. |
| Needs user decision | State the unresolved choice, its consequences, and a recommendation when justified. Ask only questions that materially affect the specification. |
| Risk accepted | Record the user's explicit acceptance, scope, and residual consequence. The coordinator cannot accept a material risk on the user's behalf. |

“We discussed this already,” confidence in the author, or a prior approval is not sufficient to refute a finding. If correct implementation depends on hidden context, make that context durable in the specification or an explicit reference when authorized. Otherwise leave the documentation gap open.

When fixes are authorized, apply supported clarifications and corrections to the specification, preserving its role as the single source of truth. Do not silently choose new product behavior, architecture, or scope. Batch the consequential unanswered questions after completing the useful review and supported edits. Preserve a resumable report while awaiting answers.

An edit moves a confirmed finding to **pending verification**, not closed. Have fresh reviewers examine the resulting inputs without receiving the earlier diagnosis. Close the finding only when their evidence covers the affected scenario and the coordinator verifies the correction. Absence from a later finding list alone is insufficient. Record a refutation or accepted risk for the same issue and evidence scope; new evidence or changed inputs can reopen it. Keep any residual accepted risk visible in the final result.

## Bound the loop and measure convergence

Use at most **three rounds** by default, counting attempts, and require **two consecutive complete clean rounds on the same final inputs** for convergence. Honor an explicit user budget or stopping instruction. A reviewer retry belongs to its current round; it does not create an unlimited retry budget.

A clean round requires all four independent lenses, adequate access to required evidence, and no unresolved material findings after evidence-based disposition. Optional `P3` polish does not block it. An explicitly accepted risk may remain visible, but an unresolved decision, an unapplied material correction, or an unverified fix prevents a clean result.

- Any relevant input edit resets the clean streak to zero. The round that found a problem is not retroactively clean because the coordinator edited the document afterward.
- In review-only mode, finish the current round and resolution, then stop if material corrections or user decisions remain. Do not repeatedly review an unchanged document hoping the findings disappear. If the first round is clean, run a second fresh round to check convergence within the budget.
- In fix-authorized mode, resolve supported findings, then rerun all four lenses on the new inputs. A narrow check can help verify a correction but cannot replace a complete clean round.
- If a reviewer still fails after its retry, required evidence is unavailable, or a decision blocks progress, stop with the corresponding limitation after collecting other useful results.
- Stop on convergence, the round limit, an explicit user stop, or a concrete blocker. Hitting the limit does not imply approval or a known defect: report the outstanding findings and achieved coverage separately.
- Preserve the round count, input versions, clean streak, and dispositions across compaction or resumption. Reset the streak when inputs change, not the spent budget. Obtain a new budget only when further rounds are needed; do not restart silently.

## Report the result

Keep one report updated with enough evidence to review the result and resume work:

1. **Scope and inputs:** intended stage, review-only or fix-authorized mode, specification and source versions, available evidence, and round budget.
2. **Independent interpretation:** concise paraphrase of what another engineer would build, including consequential differences between readings.
3. **Findings and decisions:** stable IDs, severity, scenario, locations, evidence, disposition, and any supported changes actually made. Separate open issues, verified fixes, refutations, and accepted risks.
4. **Coverage and convergence:** lenses completed per round, input versions, incomplete or invalidated rounds, clean streak, and stopping reason.
5. **Next step:** remaining corrections or consequential questions and the recommendation justified by the evidence.

State document readiness separately from review completion. For example, “no unresolved material findings; one clean round; confirmation incomplete” is different from “changes required” and from “two clean independent rounds; ready for the stated next step.” Do not call a specification user-approved unless the user actually approved it, and do not imply correctness beyond inspected evidence.

Finish with the report location or requested inline report, the material findings and decisions, any edits applied, and the exact coverage or convergence limitation. Do not start implementation automatically.
