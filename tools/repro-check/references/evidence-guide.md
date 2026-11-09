# Evidence guide: where proof lives in a reproduction package

Use this map to find the evidence each rubric check names. In eval
mode, stay inside the bundle. In live mode, also look at the live
issue and the student's draft files.

## Environment

- Where it lives: in eval, the "Candidate repro report" section,
  usually an `Environment:` line or the first paragraph. Cross-check
  the issue body's version/OS and the repo-facts "bug reports" asks.
  In live mode, the draft repro comment, plus the issue body's
  environment and the repo's bug template.
- What good looks like: the report names an OS or platform and the
  version or commit of the software under test. If those differ from
  the issue's target, the report says so. Missing both, or missing the
  software version on an issue that names one, is not enough.

## Steps

- Where it lives: in eval, the "Candidate repro report" steps,
  commands, and any pasted files or flags. The issue body's "Steps to
  reproduce" is the trigger to compare against. In live mode, the
  draft repro comment and the issue body's steps.
- What good looks like: a stranger could copy the commands (or follow
  the named UI actions) from a stated starting state through the
  issue's trigger. A short description of a tiny input is enough when
  the issue already names that input's shape. A private repo, an
  unshared config, or a skipped trigger is not followable. Exact
  commands beat "set up the project".

## Behavior shown

- Where it lives: in eval, fenced output, logs, exit codes, or
  screenshot descriptions inside the "Candidate repro report". Read
  them against the issue body's actual/expected behavior, not against
  the report's adjectives. In live mode, the same artifacts in the
  draft, compared with the live issue body.
- What good looks like: the artifact is the issue's symptom from the
  issue's trigger (missing header, named traceback, panic, freeze,
  wrong score). A different error from a different command, an old
  version's leftover behavior presented as current evidence, or
  "bat errors" when the issue is a capacity-overflow crash, is the
  wrong target. For cannot-reproduce, the artifacts show the attempt
  at that trigger and that the symptom did not appear.

## Honesty

- Where it lives: the "Candidate claim comment" plus the repro
  report's expected/actual (or "Result:" / "Reproduced." /
  "could not provoke it") sentences, compared with the artifacts in
  that same report. In live mode, the two drafts compared with each
  other and with the shown output.
- What good looks like: "reproduced" only when the artifact is the
  issue's symptom; "cannot reproduce" only when the attempt is shown
  and the missing trigger is named. Setup banners, version strings,
  or "the tabs are visible" are not the bug. A confident root cause
  with no transcript is not evidence.

## Comms

- Where it lives: the "Candidate claim comment" against the issue
  title/body; both comments against repo-facts "contribution policy"
  (AI disclosure, "own words" rules, bug-report asks). In live mode,
  the drafts, the issue, and `CONTRIBUTING.md` / `AI_POLICY.md` on
  the repo.
- What good looks like: the claim names this issue's symptom or
  trigger and a next step that is investigation or a report, not a
  guaranteed fix or a date. +1 and assign-me boilerplate fail.
  If the policy requires disclosing AI on issues or comments, at
  least one comment does. Silence, or a PR-only AI rule, is not a
  disclosure wall for comments.
