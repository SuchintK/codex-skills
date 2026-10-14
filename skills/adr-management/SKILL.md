---
name: adr-management
description: Evaluate architectural significance and manage Architecture Decision Records (ADRs) in software repositories. Use proactively when implementing or reviewing changes to system boundaries, APIs, persistence, messaging, authentication, infrastructure, major dependencies, or cross-cutting conventions—even if the user has not mentioned ADRs. Also use when proposing, reviewing, writing, deprecating, or superseding an ADR.
---

# ADR Management

Preserve **why** consequential engineering decisions were made, without turning routine development into paperwork. An ADR records a durable decision and its rationale—not a design specification, implementation plan, incident report, or PR description.

This skill has two modes:

1. **Proactive assessment:** During architecture-significant implementation, planning, or PR review, determine whether a decision needs recording **without waiting for the user to mention ADRs**. Normally this mode is read-only.
2. **ADR management:** When authorized, propose, create, review, deprecate, or supersede ADRs, keeping decision history and indexes correct.

## Non-negotiable rules

- **Never create an ADR for every PR.** Prefer silence for routine changes; when explicitly asked for an assessment, explain briefly why an ADR is or is not warranted.
- **Inspect, then judge.** Read applicable `AGENTS.md`, relevant code/diff/contracts, architecture documentation, and existing ADRs before recommending a decision. Search both current and historical decisions. Do not rely only on a user's framing, implementation rationale, or file count.
- **Separate facts, inference, and unknowns.** Base recommendations on repository evidence and stated requirements. Never invent benchmarks, alternatives, team consensus, links, dates, owners, or approvals.
- **Challenge the initial assessment.** Deliberately test a recommendation **for** an ADR for over-documentation, and a recommendation **against** an ADR for overlooked long-lived commitments. Give special attention to security, compatibility, operational obligations, and hidden precedents.
- **Approval before writes.** Do not create, accept, reject, deprecate, or supersede ADRs during ordinary coding or PR preparation without explicit user authorization. A direct instruction to create an ADR authorizes creating the file; it does **not** imply the architectural decision was accepted. Default to `Proposed` unless acceptance is clear. Explicitly obtain approval for replacing/retiring an accepted decision when not already requested.
- **Preserve decision history.** Never silently rewrite an accepted ADR's original context, decision, alternatives, or consequences. When a decision changes, write a new ADR and update only the earlier record's status/links/addendum. Preserve rejected, deprecated, and superseded records.
- **Follow local conventions first.** Use the defaults in this skill only when the repository has no established ADR process. Do not migrate or duplicate directories without permission.
- **Do not block unrelated implementation solely over missing documentation.** Surface material issues at a natural decision/review checkpoint; highlight conflicts with accepted ADRs and high-risk commitments early, before they become difficult to reverse.

## Proactive assessment: what to inspect

Use a lightweight mental scan for substantive engineering work. If there is no plausible architectural impact, continue without mentioning ADRs. If a trigger appears, examine the actual scope of the planned or implemented change.

**Potential triggers:**

- Changes to system/service boundaries, responsibilities, inter-service communication, or data ownership.
- New or changed public API/event contracts, integration patterns, or backward-compatibility strategy.
- Choice or replacement of a database, data model strategy, cache, broker, authentication/authorization approach, or foundational dependency.
- New deployment, infrastructure, reliability, consistency, security, observability, or scaling strategy.
- A cross-cutting rule that other modules/services/teams will be expected to follow.
- A migration or proposal that conflicts with, deprecates, or materially changes an existing ADR.

**Usually not triggers:** naming changes, localized bug fixes, ordinary dependency patch upgrades, adding a route that follows an existing API convention, refactoring without changing architectural responsibilities, routine query optimization, or implementing a decision already covered by an accepted ADR. The existence of a new dependency, endpoint, or large diff alone is **not** sufficient.

### Evidence-based decision test

Evaluate these dimensions rather than assigning an arbitrary numeric score:

| Dimension | Question |
| --- | --- |
| Scope & precedent | Does this set or change a pattern beyond one isolated implementation? |
| Reversibility | Would reversal require a costly migration, coordination, or contract change? |
| Longevity | Does it constrain future implementations or team conventions? |
| Alternatives | Were there meaningful architectural choices and non-obvious trade-offs? |
| Operations & risk | Does it materially change security, reliability, cost, compliance, scalability, or operational ownership? |
| Compatibility | Does it alter published interfaces, data semantics, or external consumers? |
| Existing decisions | Is there a current ADR to reference, a contradiction to resolve, or a decision to supersede? |

**There is no quota or score threshold.** One high-impact criterion (e.g., an authentication trust-boundary change) may justify an ADR. Multiple weak signals may still not. Consider whether future maintainers would genuinely benefit from the rationale six months from now.

Choose one assessment:

- **ADR recommended:** A consequential, enduring architectural decision is present and not adequately recorded.
- **ADR worth considering:** Architectural effects are plausible, but scope, assumptions, or documentation value are uncertain.
- **No new ADR needed:** Routine/local change, or an existing ADR already captures the rationale (reference it if useful).

### Communication policy

- **Routine work / no ADR:** Proceed normally. Do not add an ADR section to every response or generate files.
- **Potential impact:** At an appropriate planning or PR-review point, briefly explain what decision may need preserving, cite files/ADRs or describe evidence, and give the recommendation and the principal reason **against** it. Ask for direction only when a genuine architectural choice or ADR write requires it.
- **Conflict with accepted ADR:** Flag the specific ADR and mismatch promptly; recommend conforming, reconsidering, or proposing a superseding decision. Do not silently overwrite history.
- **When explicitly asked to assess:** Always report one of the three outcomes and why, even if no ADR is needed.
- **During PR-description work:** Perform a fresh architectural-impact check rather than just trusting earlier conclusions. Include an ADR link in the PR description only if relevant; flag missing documentation without creating it.

Suggested concise format **only when an architectural concern is detected**:

```text
Architecture decision: <what is being decided>
Assessment: ADR recommended | Worth considering | No new ADR needed
Evidence: <relevant code/contracts/current ADRs; assumptions clearly identified>
Counterpoint: <strongest reason the recommendation might be wrong>
Next step: <reference existing ADR | evaluate options | request approval | none>
```

## ADR creation and management workflow

1. **Discover:** Locate repository instructions, relevant architecture/code/contracts, current and historical ADRs, and the index. For cross-repository decisions, locate the authoritative owner; avoid duplicate canonical ADRs.
2. **Classify:** New decision, implementation of an existing decision, clarification, deprecation, or proposed replacement? If an accepted ADR still applies, prefer linking to it.
3. **Evaluate:** State the problem, decision drivers, constraints, scope, uncertainty, and viable options (including keeping the current approach when relevant). Compare benefits, drawbacks, reversibility, migration effects, and what is deliberately sacrificed. Prefer verifiable project evidence.
4. **Agree:** Present recommendation and alternatives. If the user's request was analysis only, **stop without writes**. Confirm substantive decision and authorization before creating or modifying records. Do not infer accepted status from the fact that implementation exists.
5. **Record:** Write only authorized changes, using local conventions or the defaults below. One primary architectural decision per record; aim for roughly one page unless complexity demands more.
6. **Maintain:** Update index and reciprocal references; cross-link real issues, RFCs, and implementation PRs where available. Never invent references.
7. **Verify:** Check statuses, numbering, links, accuracy, and historical integrity. Report paths changed, status, important evidence gaps, and any pending decision/review.

## Default location and naming

When the repository has no existing convention:

```text
docs/adr/
├── README.md                   # ADR index: Number | Decision | Status | Date | Link
├── 0001-use-postgresql.md
└── 0002-adopt-redis-caching.md
```

- Allocate the next unused four-digit number, even if previous ADRs are rejected or superseded. Do not renumber existing ADRs.
- Use descriptive, decision-oriented kebab-case filenames; retain established filenames once published.
- Keep one authoritative ADR for a cross-repository decision and link to it from affected repositories.
- Keep the `README.md` index current; preserve existing index style if present.

### Minimal ADR template

Use meaningful analysis, not unfilled placeholders. Adapt headings to repository conventions. Optional entries can be omitted when unknown or unnecessary.

```markdown
# ADR-0001: <Decision-oriented title>

- **Status:** Proposed
- **Date:** YYYY-MM-DD
- **Owner:** <team or role, if known>

## Context

<Problem, decision drivers, relevant system facts, scope, and constraints.>

## Decision

<Proposed/approved choice and primary rationale.>

## Alternatives Considered

- **<Option A>:** <Benefits, costs, and reason not selected.>
- **<Option B>:** <Benefits, costs, and reason not selected.>

## Consequences

**Positive**
- <Benefits.>

**Negative / Risks**
- <Trade-offs, accepted costs, operational burden, and migration risks.>

## Deferred Scope

<!-- Optional: Omit if nothing has been explicitly deferred. -->

- <Capability intentionally excluded from the current scope.>
- <Whether future implementation is committed or merely proposed.>

## Future Considerations

<!-- Optional: Omit if there are no meaningful future considerations. -->

- <Potential architectural improvements or alternatives that may become relevant later.>
- <Possible future requirements that could affect this decision.>

## Revisit Triggers

<!-- Optional: Omit if no meaningful triggers are identified. -->

Reevaluate this decision if:

- <Specific requirement or constraint changes.>
- <A measurable technical or operational threshold is reached.>
- <An assumption underlying this decision becomes invalid.>

## References

- <Existing ADR, relevant design/RFC, issue, benchmark, PR; omit if none.>
```

If only one option is viable, explain why rather than inventing competing choices. Do not guess an owner or claim approval. Use the repository's applicable ISO date. Add **Reassessment Triggers** only for useful, testable conditions—not arbitrary review deadlines.

## Lifecycle and historical integrity

```text
Proposed ──> Accepted ──> Superseded
    │            └──────> Deprecated
    └──────> Rejected
```

- **Proposed:** Evaluation in progress; never imply approval.
- **Accepted:** Decision approved/current; implementation completion is tracked separately in issues/PRs.
- **Rejected:** Evaluated but not adopted; preserve useful rejected analyses.
- **Deprecated:** No longer applicable or recommended, without necessarily having a replacement.
- **Superseded:** Explicitly replaced by a newer ADR.

When replacing an accepted decision, **with authorization**:

1. Record what changed in requirements, assumptions, or constraints.
2. Create a **new numbered ADR**, link to the earlier one (`Supersedes: ADR-NNNN`), and address migration and compatibility consequences.
3. Change the old ADR's status to **Superseded**, add `Superseded by: ADR-MMMM`, and update the index. Preserve the original context, decision, alternatives, and consequences verbatim.
4. Verify reciprocal links and dates. For deprecation without replacement, mark **Deprecated** with a short dated note; for historical factual corrections, prefer a labeled addendum.

Never mark an old accepted ADR as `Rejected` merely because a newer decision prevailed. Do not delete old records to clean up the directory.

## References

When creating or updating an ADR:
- Reference relevant design documents, RFCs, issues, and implementation PRs when available.
- Avoid duplicating information already maintained in those documents.
- Keep references optional and relevant.

## Future Scope and Revisit Policy
- Clearly distinguish committed future decisions from possibilities.
- Do not turn ADRs into feature backlogs or implementation roadmaps.
- Include these sections only when they add meaningful context.

## Final quality check

- [ ] Inspected relevant repository code/contracts, instructions, and existing ADRs, or explicitly noted missing access.
- [ ] Evaluated architectural impact without relying solely on the user's framing or the proposed implementation.
- [ ] Tested **both** overlooked significance and unnecessary documentation; considered the strongest counterargument.
- [ ] No unnecessary ADR for a routine change, no duplicate of an existing accepted decision, and no silent contradiction.
- [ ] For a new ADR: one decision, concrete context/rationale, real alternatives (if any), and positive **and** negative consequences.
- [ ] Status and writes match explicit authorization; owner/date/evidence are real rather than inferred.
- [ ] Historical accepted rationale preserved, supersession backlinks and index valid, references real.
- [ ] Output proportionate: silent for ordinary work, concise rationale when flagged, exact changed paths when written.
