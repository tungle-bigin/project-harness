# Document Lifecycle

Use this pattern to decide which documents change with code, which stay frozen,
and how a durable plan is shaped. It applies the authority gate in the
[repository workflow](../WORKFLOW.md).

## 1. Classify Every Location

| Class | Examples | Rule |
| --- | --- | --- |
| Truth | Product behavior, architecture, runbooks, requirement inventories, traceability tables | Must match code. Update it in the same change that changes the behavior. |
| Record | Accepted decisions, completed plans, completed change specs | Do not edit to match later code. Supersede a decision with a new one; only the old status line changes, for example `Superseded by NNNN`. |
| Working | Active plans | Keep current while active. Promote, then move to completed. |

State the class of every documentation location in the documentation map
(`docs/README.md`). A consumer may keep existing paths and names, such as change
specs outside `docs/plans/`, when the map states their class.

## 2. Resolve Drift

When code and a document disagree:

1. Find the class of the document.
2. **Truth:** if the code reflects accepted behavior, update the Truth document
   in the same change.
3. **Record:** leave it. Update the Truth document, or write a new decision when
   the lasting choice changed.
4. **Unclear which side is right:** record both sides with citations (code
   `path:line` and the document section) and request the decision. Do not pick
   a side.

## 3. Choose The Plan Shape

Default to one `docs/templates/exec-plan.md` file.

Use the split plan in `docs/templates/split-plan/` only when at least one holds:

- a separate audience approves the spec before design starts;
- different contributors own design and tasks;
- recovery cost is high, such as schema, data, or infrastructure changes.

```text
docs/plans/active/YYYY-MM-DD-<slug>/
  spec.md     outcome, users, scope, acceptance, clarify  (often called PRD)
  design.md   architecture, contracts, data, alternatives  (often called TDD)
  tasks.md    depends on, progress, decisions, analyze, validation, result  (often called TASK)
```

Each split part starts with `Plan:` (the directory name), `Part:` (`spec`,
`design`, or `tasks`), and `Siblings:` links, so a part opened, cited, or copied
alone still names its plan. A part that deliberately does not exist is listed as
`<part>: not applicable (reason)` instead of a link. A consumer with its own
naming, such as `cr_042_tdd_*.md`, uses its shared ID as `Plan:` and its own part
names.

Either shape is one plan with one lifecycle. Do not add status databases,
trace scores, or other records beside it.

## 4. Name And Order Plans

- Name a plan `YYYY-MM-DD-<slug>` with the date work started, for example
  `2026-10-14-document-lifecycle.md`. Names then sort in start order and need
  no number allocation, so parallel branches cannot collide.
- When a plan cannot start or finish before another, add
  `Depends on: <other plan name>` under its date; a split plan states it only
  in `tasks.md`. The date shows start order;
  only `Depends on` states a required order.
- Keep sequence numbers for documents others cite by ID, such as decisions
  (`NNNN`) or change requests (`CR-NNN`).
- Do not rename completed plans; they are records.

## 5. Promote On Completion

Before moving a plan to `docs/plans/completed/`:

1. Move lasting behavior into Truth documents.
2. Move lasting choices into decisions.
3. List both in the plan's result section.

A completed plan that is the only description of current behavior is drift.

## 6. Checks Worth Encoding

These are candidates for the [invariant encoding pattern](encoding-invariants.md).
Harness ships no validator; implement accepted ones with the repository's
native validation owner.

| Check | Catches |
| --- | --- |
| Every inventory ID appears in the traceability table, and the reverse | Requirements added or removed on one side only |
| A changed or removed requirement points to a decision that exists | Silent policy changes |
| Every part of a split plan names the same plan, every required part exists or is marked not applicable, and sibling links resolve | Orphaned or missing plan parts |
| Every `path:line` citation resolves to an existing file and line | Citations left behind by code changes |
| Each cited line range still has the content it had when cited, checked against a stored fingerprint; a block that only moved is relocated automatically | Citations that still resolve but now point at shifted or rewritten code |
| Decisions have the required sections and a status line; a superseded decision names an existing replacement | Edited or orphaned records |
| Changes to declared sensitive paths include a decision | Hard-to-reverse changes without a record |

Report enforcement levels as the invariant pattern requires: a local command,
an optional hook, CI invocation, and branch protection are separate facts.
