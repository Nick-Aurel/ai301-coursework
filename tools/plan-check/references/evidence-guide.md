# Evidence guide: where evidence lives in a plan package

Use this map to find the evidence each rubric check names. In eval
mode, stay inside the bundle. In live mode, also look at the live
issue and the student's draft files.

## Diagnosis and grounding

- Where it lives: in eval, the plan's diagnosis or "Cause" sentences,
  and the "Repro evidence" block (controls, timings, artifacts,
  expected/actual). The issue body says what symptom to explain. Thread
  highlights are not the evidence for this check. In live mode, the
  draft `plan.md` cause, and the student's posted repro comment on the
  issue (or the house repro pack quoted in the drafts).
- What good looks like: the stated mechanism is one the repro results
  still allow. A control that succeeds on the blamed component, or a
  timing that stays slow with that component out of the loop, means
  the cause does not fit. Naming no mechanism is not a cause.

## Scope

- Where it lives: in eval, the plan's in-scope, not-in-scope, and
  deferred sentences, and the numbered changes the plan says it will
  make. Compare with the issue body's reported symptom. In live mode,
  the same sentences in `plan.md`.
- What good looks like: one change for that symptom, with an explicit
  boundary. A deferred rework the plan says it will not do is in
  bounds. A migration, a new subsystem, or a stack of while-in-the-area
  fixes committed as this change is not, even when the first item is
  the right fix.

## Executability

- Where it lives: in eval, the plan's files or code areas and the
  approach steps. In live mode, the same part of `plan.md`.
- What good looks like: a named file or area and an edit a stranger
  could start (add a call, clamp a subtraction, normalize a pattern).
  An unknown about the exact line inside that area is still startable.
  "Profile, then optimize whatever shows up" and "somewhere" are not.

## Test plan

- Where it lives: in eval, the plan's test-plan sentences, read next
  to the repro evidence's steps and the issue's expected behavior. In
  live mode, the test plan in `plan.md`.
- What good looks like: a command or the repro steps, plus a concrete
  result for this bug (exit code, printed value, color, timing, count,
  or an assertion). "Run the suite and make sure nothing regresses,"
  with no result for the reported behavior, does not decide the bug.
  "Feel fast" and "feel broken" are not results.

## Honesty

- Where it lives: in eval, the plan's risk, unknown, or deviation
  sentences. In live mode, the same sentences in `plan.md`, including
  a filled-in deviations section after a build.
- What good looks like: an unmeasured cause or cost is labeled as
  unknown. A plan that simply does not claim an unmeasured result as
  fact meets this. This family never changes the verdict.

## Comms

- Where it lives: in eval, the thread highlights and the plan comment,
  plus the repo-facts contribution-policy line. A direction counts only
  from a role of OWNER, COLLABORATOR, or MEMBER. In live mode, the
  issue thread, the draft `comment.md`, and the repo's CONTRIBUTING or
  AI policy docs.
- What good looks like: if a maintainer named a file, a patch to test,
  or an approach to use or avoid, the plan or the comment follows it
  or names it and explains a narrower alternative. Silence in a thread
  with no such direction is fine. On AI: fail the disclosure check only
  when the policy requires disclosing AI use on issues or comments and
  the plan comment does not. A pull-request-only rule, or a rule that
  comments be human-written, is not that requirement.
