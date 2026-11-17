# Procedure: how this tool grades a PR package

These steps grade a pull request. They do not fix it. Follow them in
order and do only what they say. In eval mode the bundle is the whole
world: fetch nothing. In live mode, take each part from the live
location `references/evidence-guide.md` names for it.

## Read order

Read the plan before the PR, and the diff before the description. The
plan is the standard; the diff is the thing; the description is a
claim about the thing. Reading the description first is what lets a
confident "implements the plan exactly" carry a drifting diff through.

1. Read the issue title and body. Note the reported symptom and the
   expected behavior.
2. Read the thread highlights. List only directions from a role of
   OWNER, COLLABORATOR, or MEMBER (a file, an approach, a constraint,
   a question for the fix). A role of NONE is not a direction.
3. Read the repo-facts block (live: `.github/PULL_REQUEST_TEMPLATE.md`
   and `docs/CONTRIBUTING.md`). Write down, as a list:
   a. every concrete item the PR template or contributing guide asks a
      PR to contain (sections, checklist, `closes #`/`fixes #`
      reference, changelog/whatsnew entry, tests);
   b. the stated test command, if any;
   c. the AI-policy sentence, and whether it requires disclosure for
      PRs or in any form.
4. Read the plan context (live: `plan.md`, including `## Deviations`).
   Write down, separately: (a) each change the plan commits to;
   (b) the files it names; (c) its not-in-scope lines; (d) every
   deviation, deferral, or known-limit note; (e) each failing scenario
   its test plan says will be re-run, with the expected-after result;
   (f) any suite command the test plan names.
5. Read the diff (live: `git diff main...HEAD`). For each file header,
   list the file and summarize each hunk in one line: what it changes.
6. Read the commit list (live: `git log main..HEAD --oneline`).
7. Read the test evidence (live: `test_evidence.md`, plus any
   before/after pasted in the description).
8. Read the PR title and description last (live: `pr_draft.md`). List
   every factual claim it makes about the change, its files, its docs,
   its fidelity to the plan, and any shortfall it states.

## Evidence gathering

1. `plan_fidelity`: pair the hunk list from step 5 with the plan notes
   from step 4. Mark each hunk "planned" (implements 4a, or is a test,
   fixture, or repo-required changelog entry for it), "deviation-noted"
   (covered by 4d), or "unplanned". Then mark each item of 4a "in
   diff" or "missing", and for each missing item record whether 4d
   AND the description both name it as left out.
2. `description_honest`: pair each claim from step 8 with the hunk
   list from step 5. Mark each claim "true", or "false" with the hunk
   (or absent file) that contradicts it.
3. `repro_rerun`: pair each scenario in 4e with the test evidence from
   step 7. For each, record whether it was run on the branch, the
   observable result shown (output, exit code, value, comparison), and
   whether that result matches the expected-after. Note whether a
   before is visible. Note if the only runs shown are a control or a
   path the diff does not change.
4. `repo_checks_run`: record the suite or check commands shown in the
   evidence (from 3b and 4f) and the outcome printed for each.
5. `diff_clean`: scan every hunk and the commits for debris: debug
   print/log lines, commented-out code, added dead or unused code,
   added TODO/FIXME, whitespace or re-indent churn, re-printed
   identical lines, import reordering, edits to code the fix does not
   need. Record each one with its file.
6. `template_asks_met`: pair each item from 3a with the description
   and the diff. Mark each "present", "N/A with reason", or "absent".
   Skip process items outside the PR text (CLA, vouch flow, discussion
   approval) and quality judgments.
7. `ai_disclosure`: record the policy sentence from 3c and the
   description's disclosure sentence, if any.
8. `thread_direction`: pair each direction from step 2 with the diff
   and description.
9. `commits_clear`: record any commit message that does not say what
   it changes.

## Check execution

1. Grade the checks in rubric order: `plan_fidelity`,
   `description_honest`, `repro_rerun`, `repo_checks_run`,
   `diff_clean`, `template_asks_met`, `ai_disclosure`,
   `thread_direction`, `commits_clear`.
2. For each check, apply only that row's pass condition to the
   evidence gathered for it in the step above. Grade `pass`, `fail`,
   or `unclear`. Do not re-read the whole package for a check; go back
   only to the part that row names.
3. Grade every check, even after a required check has failed. Eval
   mode always grades the complete package.
4. Grade `unclear` only when the evidence the row names is absent from
   the package (for example, no test evidence section at all). A short
   or terse part is not absent evidence.
5. One fact can fail more than one check (an unplanned hunk can fail
   `plan_fidelity` and, if the description claims "exactly as
   planned", `description_honest`). Grade each check on its own row.
6. Do not raise a grade because the PR is long, polished, or
   confident, or lower one because it is terse.
7. Write one line of evidence per check: the hunk, quote, or fact that
   decided it.

## Verdict assembly

1. Apply the verdict rule in `rubric.md`: accept only when every
   required check is `pass`.
2. Count `unclear` on a required check as `fail`.
3. Ignore `thread_direction` and `commits_clear` when deciding the
   verdict.
4. If the verdict is reject, the deciding check is the first failing
   (or unclear) required check in rubric order; name it and quote its
   evidence line in the summary.
5. Set `verdict` to `accept` or `reject`. There is no third value.
6. Emit the summary (one line per check, the deciding check, and in
   live mode the Voice notes and fixes), then the fenced JSON block
   with every check in rubric order. The JSON block is last; put
   nothing after it.
