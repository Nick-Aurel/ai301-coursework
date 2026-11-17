---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer exactly one question about exactly one PR package: **is this
pull request ready to submit?** A PR package is a candidate pull
request (its title, description, commit list, unified diff, and test
evidence), read against the plan it claims to implement (including any
deviation notes) and the issue that plan belongs to, plus the repo's
stated standards (its PR template, contributing guide, and AI policy).

Do not grade whether the issue is worth fixing, whether the plan is a
good plan, or whether you would have written the fix differently. The
plan is the accepted standard here; you grade whether the PR delivers
it, proves it, keeps the diff reviewable, and meets the repo's asks.
Grade one package per run. Never answer from gut feel: answer by
executing `procedure.md`, which applies the checks in `rubric.md` to
evidence located with `references/evidence-guide.md`.

## Inputs and modes

Decide the mode first. If the input is a package bundle (a markdown or
JSON file with an eval package id such as `pkg-07` or `calib-01`, or
its inlined text), you are in eval mode. Otherwise you are in live
mode.

**Live mode** (the student's own PR, before it is opened). Read, from
the top folder of the student's clone of their Path Review fork, on
their branch:

- `plan.md`: the posted plan, including its `## Deviations` section.
  This is the scope and test plan the diff is graded against.
- The branch's diff: run `git diff main...HEAD` (three dots) from the
  working copy, plus `git log main..HEAD --oneline` for the commit
  list and `git diff main...HEAD --stat` for the changed-file list.
  Only committed changes count; uncommitted edits and untracked files
  are not part of the PR. `plan.md`, `comment.md`, `pr_draft.md`, and
  `test_evidence.md` must never appear in this diff; if one does, that
  is an unrelated file in the diff.
- `pr_draft.md`: the first line is the PR title; the rest is the PR
  description, filled into the repo's PR template.
- `test_evidence.md`: the captured before/after repro output and the
  output of the repo's own checks.
- The issue: fetch its body and thread with
  `gh issue view <number> --repo <scoped repo> --comments` (or the
  GitHub web page if `gh` is unavailable).
- The repo's standards: `.github/PULL_REQUEST_TEMPLATE.md` and
  `docs/CONTRIBUTING.md` in the working copy (fall back to the scoped
  repo on GitHub if they are missing locally).

A house-chain student (one routed to the instructor's house issue)
reads the house plan and the house repro pack in place of their own
plan and Unit 2 repro; every check grades the same things there.

If a live input is missing (no `plan.md`, no `pr_draft.md`, no
`test_evidence.md`, or an empty diff), do not invent it: the checks
that need it grade `unclear`, and say in the summary which file is
missing.

**Eval mode** (a frozen practice submission). The bundle is the whole
world. Every fact comes from the bundle text: its issue block, thread
highlights, repo-facts block, plan-context block, and candidate PR
(title, description, commits, diff, test evidence). Fetch nothing, run
nothing, read no other file in the repo, and do not visit the
`source:` issue. Eval mode always grades a complete package: every
check in `rubric.md`, the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else.

- If its `Repo:` line still carries a bracketed placeholder (such as
  `<ORG>/<PATH-REVIEW-REPO>`), stop without grading. Tell the student
  to fill the `Repo:` line in `scope.md` with their section's Path
  Review repo (shown in the install step of this unit's activity
  page), and emit nothing else. Never guess a scope.
- If the issue URL or the fork's upstream is not the scoped repo,
  refuse to grade and say which repo the scope allows.
- Apply its house rules as facts about the environment: the PR comes
  from a branch on the student's fork, one bounded change per PR, the
  repo's PR template is always used with every section filled
  (including the AI-use disclosure). Report a branch name that does
  not follow `<type>/<issue-number>-<slug>` in the summary.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` after the scope. Hold the outgoing
text, the PR title and description from `pr_draft.md`, against each of
its rules and its "Things I never post" list. For every rule the draft
breaks, report it in the summary under a "Voice" heading: quote the
rule's name and the offending line from the draft. The voice guide is
the student's personal style and carries no gold label, so it never
changes a check grade or the verdict on its own; only a rubric check
can do that.

In eval mode, ignore `voice-guide.md` entirely.

## Component reads

- `rubric.md` defines the checks (name, evidence, pass condition,
  weight) and the verdict rule. It is the only source of checks: do
  not add, drop, merge, or rename any.
- `references/evidence-guide.md` maps where each evidence family lives,
  in an eval bundle and in a live working copy, and what good looks
  like there. Use it whenever a rubric row names evidence.
- `procedure.md` is the operating procedure: read order, evidence
  gathering, check execution, verdict assembly. Execute it as written,
  step by step. Where it is silent on a step you need, do the
  narrowest thing the rubric row itself states and report the gap in
  the summary under "Procedure gaps"; never invent a new check or a
  new pass condition to cover it.

Refusal rule: if `rubric.md` has no filled check rows or no verdict
rule, or `procedure.md` has no written steps (only headings and
comments), stop and say the tool cannot grade without them. Do not
invent checks at runtime.

## Verdict and output

The verdict space is binary: `accept` means the PR is ready to submit
(open it); `reject` means hold it and fix what failed. There is no
third verdict and no score; reservations go in a check's evidence
line.

Before the JSON block, print a short readable summary: one line per
check (`name: grade: evidence`), the deciding check when the verdict
is reject, and, in live mode only, the Voice notes and a short list of
concrete fixes. Then end the reply with exactly this fenced JSON block,
filled in, valid, and last, with nothing after it. Use the check names
from `rubric.md` in rubric order. Set `item` to the PR URL or issue URL
in live mode and the bundle id (for example `pkg-07`) in eval mode.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first. Never grade a check without naming the fact, file,
  hunk, or quote that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish. A terse PR whose diff matches the
  plan and whose evidence shows the fix can be ready; a long, confident
  description can be hiding drift. Read the diff itself against the
  plan, never the description's account of the diff.
- The rubric decides, not you. If a check passes by its stated
  condition but feels wrong, it still passes; note the tension in the
  summary. The fix belongs in the rubric, not in the run.
- The procedure decides how, not you. Follow `procedure.md` as
  written and report its gaps instead of papering over them.
- An honest shortfall is not a failure. A PR that leaves out part of
  the plan, or needed a change the plan did not name, and says so in
  the plan's deviation notes and the description can be ready.
- `unclear` is treated as the rubric's verdict rule directs. Where the
  rule is silent, `unclear` counts as `fail`: a claim you cannot
  verify from the package is a PR that is not ready to submit.
