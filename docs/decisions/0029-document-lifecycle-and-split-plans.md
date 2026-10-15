# 0029 Document Lifecycle And Split Plans

Date: 2026-10-14

## Status

Accepted. Amends decision 0019 item 5. Keeps decision 0028, including its
rejected universal-validator alternative.

## Context

A consumer (mmotion-finance) found about 25 places where per-change product and
design documents disagreed with code. The documents were written for one
change, then left behind while code kept moving. Nothing told agents which
documents must track code and which must stay frozen, so drift was found by
manual inventory instead of by rule.

The same consumer runs a common split workflow for larger changes: a product
spec approved by a separate audience, a technical design, and a task list
(often named PRD, TDD, and TASK), plus architecture decision records. Decision
0019 allowed only one durable plan file, so its Harness block and the
consumer's own rules gave agents contradictory instructions.

Mainstream practice and agent spec tools (for example requirements, design,
and tasks files per feature) also split work this way when audiences differ.
Harness previously removed story packets because they came with lifecycle
state, trace scores, and orchestration. A file layout does not need any of
that.

## Decision

1. Every document belongs to one lifecycle class:
   - **Truth:** current product behavior, architecture, runbooks, and
     inventories. It must match code and is updated in the same change.
   - **Record:** accepted decisions and completed plans. They are not edited
     to match later code; a new decision supersedes an old one, and only the
     old status line changes to name its replacement.
   - **Working:** active plans. They stay current while active.
2. When code and a Truth document disagree, the change updates the Truth
   document. When it is unclear which side is right, the agent records both
   sides with citations and requests the decision instead of choosing.
3. A durable plan uses one `docs/templates/exec-plan.md` file by default. It
   may use the split plan (`docs/templates/split-plan/` spec, design, tasks)
   when a separate audience approves the spec, contributors own design and
   tasks separately, or the change has high recovery cost such as schema or
   infrastructure. Either shape is one plan and one lifecycle.
4. Completing a plan includes promotion: lasting behavior moves into Truth
   documents and lasting choices into decisions, and the plan's result lists
   what was promoted.
5. A consumer may keep existing paths and names for these documents (for
   example CR files outside `docs/plans/`) when its documentation map states
   the lifecycle class of each location.
6. Harness installs the lifecycle pattern and split-plan templates. Mechanical
   documentation checks stay consumer-owned and follow decision 0028: the
   pattern lists checks worth encoding, and the consumer implements them with
   its native validation owner.

## Alternatives Considered

1. **Keep one plan file only.** Rejected because changes with a separately
   approving audience end up with parallel documents anyway, outside Harness
   guidance and with contradictory agent instructions.
2. **Restore story packets with lifecycle state.** Rejected for the reasons
   that removed them: state, trace scores, and orchestration form a second
   control plane. The split plan is a file layout only.
3. **Ship a documentation validator in the `harness` binary.** Rejected for
   now under decision 0028: consumers own languages, commands, and
   enforcement topology. Revisit when several consumers carry materially
   identical checks.
4. **Leave document drift to review.** Rejected because the observed drift
   accumulated across reviewed changes; an explicit lifecycle class makes the
   expected update mechanical to check.

## Consequences

- Agents know which documents to update with code and which to supersede.
- Larger changes can use a familiar spec, design, and tasks split without
  contradicting the Harness entrypoint.
- The core payload grows by one pattern and three templates. The former
  `high-risk-story` compatibility templates are replaced.
- Consumers still write and own their documentation checks.

## Follow-Up

- Collect consumer documentation checks; consider a shared checker only with
  evidence from multiple consumers, through a decision that revisits 0028
  alternative 3 for documentation.
