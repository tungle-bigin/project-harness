# Execution Plans

Plans are Git-native working memory for complex tasks.

Use an ephemeral plan for bounded, single-session work. Create one plan under
`active/` when work spans sessions, coordinates contributors, has meaningful
dependencies, needs recovery, or cannot safely resume from its diff.

```text
docs/plans/active/YYYY-MM-DD-<slug>.md   (default: one file)
docs/plans/active/YYYY-MM-DD-<slug>/     (split: spec.md, design.md, tasks.md)
  -> keep progress, decisions, recovery, and validation current
  -> promote lasting behavior and choices, record the verified result
  -> move to docs/plans/completed/
```

Use `docs/templates/exec-plan.md`. Use `docs/templates/split-plan/` only under
the conditions in the [document lifecycle](../patterns/document-lifecycle.md).
Either shape is one plan; do not add trace or status records beside it.

## Active

No durable work is currently active.

## History

Completed plans may be removed from the current tree when decisions, code,
tests, and Git history preserve their lasting result. This keeps current
retrieval focused without deleting provenance.
