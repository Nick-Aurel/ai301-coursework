# Procedure: how this skill grades a plan package

These steps grade a plan. They do not write one. Follow them in order.
Do only what they say. In eval mode the bundle is the whole world: do
not fetch anything. In live mode, use the evidence guide's live
locations for the same parts.

## Read order

1. Read the issue title and body. Note the symptom and the expected
   behavior. Do not decide a cause from the issue text alone.
2. Read the repro evidence next, before the plan. Note each control,
   timing, and artifact, and write down which mechanism those results
   isolate and which they rule out. This note is the standard the
   diagnosis is graded against. Reading the plan first is what lets a
   polished, wrong cause through.
3. Read the thread highlights. List only directions from a commenter
   whose role is OWNER, COLLABORATOR, or MEMBER: a named file, a patch
   to test, an approach to use, or an approach to avoid. A comment
   whose role is NONE is not a direction, even when it states a theory.
4. Read the repo-facts contribution-policy line. Note whether it
   requires disclosing AI use on issues or comments, or only constrains
   pull requests or asks for human-written comments.
5. Read the candidate plan. Note, separately: the stated cause, what is
   in scope, what is out of scope or deferred, the files or areas, the
   edit those steps describe, the test plan, and any labeled unknown.
6. Read the candidate plan comment last. Note whether it mentions a
   maintainer direction from step 3, and whether it discloses AI
   assistance.

## Evidence gathering

1. For `cause_grounded`, record the plan's stated mechanism and the
   repro-evidence results from step 2. Do not use a thread comment's
   theory as the evidence the cause is judged against.
2. For `scope_bounded`, record the work the plan commits to doing, and
   the work it says it will not do or will defer. Compare that list
   with the symptom from step 1.
3. For `can_start`, record every file or code area the plan names and
   the edit it describes there. "Investigate" and "optimize whatever
   shows up" are not edits.
4. For `test_decisive`, record the command or repro steps the test plan
   will run, and the concrete result it says counts as success.
5. For `thread_followed`, record the maintainer directions from step 3
   and whether the plan or the plan comment follows one, or names it
   and explains a difference.
6. For `ai_disclosure`, record the policy sentence from step 4 and
   whether the plan comment discloses a tool or that AI assistance was
   used.
7. For `unknowns_honest`, record any sentence that states a risk or
   unknown, and any sentence that states an unmeasured cause or cost
   as fact.

## Check execution

1. Grade the checks in this order: `cause_grounded`, `scope_bounded`,
   `can_start`, `test_decisive`, `thread_followed`, `ai_disclosure`,
   `unknowns_honest`.
2. For each check, apply only that row's pass condition to the evidence
   gathered for it. Grade `pass`, `fail`, or `unclear`.
3. Grade `unclear` only when the evidence that row names is absent from
   the package. A short plan is not absent evidence.
4. Do not raise a grade because the plan is long, polished, or
   confident. Do not lower a grade because it is terse.
5. Quote one line, or one fact, as the evidence for each grade.

## Verdict assembly

1. Apply the verdict rule in `rubric.md`. Accept only when every
   required check is `pass`.
2. A `fail` or `unclear` on any required check is `reject`.
3. Ignore `unknowns_honest` when deciding the verdict.
4. Set `verdict` to `accept` or `reject`. There is no third value.
5. Emit the short summary, one line per check, then the fenced JSON
   block the skill requires. The JSON block is last. Put nothing
   after it.
