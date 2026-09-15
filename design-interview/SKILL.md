---
name: design-interview
description: Stress-test an idea, project or feature through a short prioritized interview that creates or updates a decision document, only when the user explicitly invokes design-interview.
---

# Design Interview

## Invocation

Run only when explicitly invoked by the user by name. Reading this file, task similarity or another skill's recommendation is not invocation. Do not invoke other workflow skills automatically. Use the user's language.

Always turn the discussion into a document. Do not start implementation or publish to a tracker.

## Ground and narrow

Read the idea, known constraints and previous answers. When available, inspect relevant project instructions, architecture notes, contracts and representative code before asking about repository facts. Distinguish facts you can investigate, reversible engineering choices you can recommend, and consequential decisions requiring the user's judgment.

For a large project, outline a few outcome-oriented slices and recommend the highest-risk or most foundational one. Announce the active scope and deferred topics. Follow an explicit user priority; otherwise select a reasonable first slice and let the user redirect. Ask for scope selection only if the alternatives change the intended outcome materially. Keep project-wide constraints visible while narrowing. Do not interview all slices in succession automatically.

## Spend a question budget

Use the user's budget when specified; otherwise allow **at most six decision questions for the whole invocation**, usually one or two per message. This is a ceiling, not a target. Count scope-selection questions, clarifications and follow-ups. Several independently answerable decisions count as several questions even inside one numbered item. Do not hide an interview in a checklist or request “comments on everything”.

Prioritize questions by the cost of a wrong assumption, irreversibility, impact on contracts or acceptance criteria, and how many downstream decisions they unblock. Challenge contradictions, weak premises and failure scenarios. Ask only when the answer could materially change the design and cannot be established from available evidence.

For each question, briefly state why it matters and offer a recommended answer with its main trade-off. Let the user disagree in their own terms. Avoid questionnaires about obvious defaults, low-impact naming, speculative future features, or facts you can inspect. Do not invent alternatives where only one is credible.

After each answer, update your understanding and reprioritize remaining questions. Never ask a question again merely because it appeared in an earlier checklist. Use remaining turns on newly exposed high-impact risks, not exhaustive traversal. Treat “you decide” as permission to make a reasoned choice within the user's constraints, not an invitation to keep questioning.

## Preserve context and stop

Keep one compact checkpoint: active scope, settled decisions and rationale, explicitly assumed defaults, unresolved blockers, deferred topics, and questions used/remaining. Summarize it when scope changes or the conversation needs a handoff; do not reprint the whole discussion after every answer. On resumption, use that checkpoint instead of restarting the interview or resetting the budget.

Stop early when the remaining uncertainty can be handled through explicit reversible assumptions or investigation. At the budget limit, deliver the best supported conclusion. Do not silently guess consequential unresolved contracts or call a blocked design ready. Name the remaining blocker and recommend the next focused session or experiment. Start a new question budget only when the user explicitly requests more interview questions or an interview for another slice. A resumed conversation or a request to finish the document does not reset it. For an explicitly requested new round without a specified budget, allow up to six questions and retain prior decisions.

## Document throughout and deliver

Return a concise decision brief: recommended direction, decisions made, important objections addressed, assumptions, blockers and the smallest next step. Distinguish user decisions from model recommendations. A brief is sufficient; do not generate a separate implementation plan. The design document captures context and decisions for later decomposition into specifications; do not create those specifications during this skill.

Create or update the requested document or the project's existing design document with the initial framing, then update it after each answered round and when concluding. Keep a single source of truth and preserve unrelated content. If none exists, use `docs/design/<topic>.md` within the working project; without a project, create `<topic>-design.md` in the available persistent document workspace. Maintain the checkpoint there. If saving is blocked, preserve the complete draft in the response and disclose that it was not saved; do not silently switch to a discussion-only workflow. Update a glossary only when terminology was settled and an ADR only for a consequential architectural decision, following project conventions. Mark assumptions and unresolved matters explicitly; do not present proposals as approved decisions. Report the document location and a short summary of decisions and remaining blockers.
