# Execution Plans

Execution plans are Git-native working memory for complex tasks. They preserve
enough context for another agent or human to resume work without reconstructing
intent from chat history or a partial diff.

## When To Create A Plan

Use an ephemeral plan for bounded, single-session work.

Create one durable plan when work spans sessions, coordinates contributors, has
meaningful dependencies or ordering, requires recovery steps, or would be unsafe
to resume from the diff alone.

Use `docs/templates/exec-plan.md` and place the file under `active/`. Use a
`docs/templates/split-plan/` directory (spec, design, tasks) only under the
conditions in `docs/patterns/document-lifecycle.md`; it is still one plan.
For an explicitly authorized baseline-to-rerun Harness experiment, use
`docs/templates/harness-improvement.md` instead.

## Lifecycle

```text
docs/plans/active/YYYY-MM-DD-<slug>.md  or  docs/plans/active/YYYY-MM-DD-<slug>/
  -> update progress and decisions during implementation
  -> promote lasting behavior into Truth documents
  -> record final validation and result
  -> move to docs/plans/completed/
```

Name plans with the date work started so they sort in start order; add
`Depends on:` when one plan must wait for another.

The plan is the primary task artifact. Promote a lasting product or architecture
decision into `docs/decisions/`; keep task-local choices in the plan. A
completed plan is a record: do not edit it to match later code.

## Active Plans

No active execution plans are currently indexed.
