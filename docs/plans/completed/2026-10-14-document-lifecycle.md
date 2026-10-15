# Execution Plan: Document Lifecycle And Split Plans

Date: 2026-10-14
Depends on: none

## Status

Completed

## Outcome

A clean consumer installed from this branch learns which documents track code,
which stay frozen, and when a durable plan may be split into spec, design, and
tasks files, without contradicting its own Harness entrypoint.

## Context

- Owner choices in the initiating task (2026-10-14): keep documentation checks
  consumer-owned (decision 0028 stands) and amend decision 0019 item 5 to allow
  split plans.
- Consumer evidence: mmotion-finance found about 25 places where per-change
  design documents disagreed with code.
- Decisions 0019, 0020, 0028; new decision 0029.

## Scope

In scope:

- Decision 0029, `docs/patterns/document-lifecycle.md`, and
  `docs/templates/split-plan/` replacing `docs/templates/high-risk-story/`.
- Workflow, entry block, documentation map, plan and decision READMEs
  (upstream and consumer assets).
- Manifest, embedded distribution, release classification, and contract tests.

Out of scope:

- A documentation validator in the `harness` binary (decision 0028).
- Release, tag, push, or merge.

## Approach

1. Record the authority as decision 0029 before changing guidance.
2. Keep the entry block and workflow inside existing budgets: the block changes
   one word (`file` to `plan`); details live in the installed pattern.
3. Wire new files through the manifest and embedded distribution.
4. Add positive and negative contract assertions.

## Risks And Recovery

- Risk: entry budget overrun. Measured 1131 of 1150 words and 1591 of 1600
  block bytes.
- Risk: split plans regrow a story control plane. The pattern and decision
  forbid status or trace records beside a plan.
- Recovery: revert the branch; no consumer state or schema changes.

## Progress

- [x] Decision 0029 and index.
- [x] Pattern and split-plan templates.
- [x] Workflow, entry block, maps, and READMEs.
- [x] Manifest, embedded distribution, release classification.
- [x] Contract tests.

## Decisions

- 2026-10-14: Replace `high-risk-story` rather than keep both, so agents see one
  split shape.
- 2026-10-14: Name plans `YYYY-MM-DD-<slug>` and state required order with
  `Depends on:`; decisions and change requests keep sequence numbers. Owner
  request: plan names showed no order. Existing `p1-*` records are not renamed.
- 2026-10-14: After code review, the single-file template's Result also lists
  promotions; the installed pattern no longer cites upstream decision 0029;
  `spec.md` names the pattern by path instead of a relative link that broke when
  copied; decision 0019 records the amendment on its status line. The
  upstream-ID guard moved to the installer test so it checks every installed
  file, case-insensitively.

## Validation

- Focused proof: `tests/docs/test-doc-contracts.sh` fails for the intended
  reason when the old "Do not split" sentence returns, when
  `docs/templates/high-risk-story/` returns, and when a pattern section is
  removed.
- Integration proof: Bash installer modes confirm a fresh install contains the
  pattern and all three split-plan templates and no `high-risk-story`.
- Repository-required checks: `scripts/validate-premerge.sh` passed in a Linux
  container (rust:1-bookworm) on an LF copy of the branch. On a Windows Git Bash
  checkout, the CRLF-sensitive and installer tests fail identically on `main`.

## Result

The outcome exists on branch `feat/document-lifecycle-split-plans`.

- Promoted to Truth documents: `docs/WORKFLOW.md`,
  `docs/patterns/document-lifecycle.md`, `docs/templates/exec-plan.md`,
  `docs/templates/split-plan/`, plan and decision READMEs (upstream and
  consumer assets).
- Promoted to decisions: 0029; 0019 status line records the amendment.
- `tests/installer/test-install-harness-modes.ps1` passed on Windows
  PowerShell 5.1 against the branch build.
- Limitation: no release candidate was built or published.
