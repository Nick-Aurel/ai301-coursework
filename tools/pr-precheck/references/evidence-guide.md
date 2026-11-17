# Evidence guide: where evidence lives in a PR package

Use this map to find the evidence each rubric check names. In eval
mode, stay inside the bundle. In live mode, read the student's working
copy on their branch and the scoped repo on GitHub.

## Plan fidelity (harness category: silent-drift)

Checks: `plan_fidelity`, `description_honest`.

- Where it lives, eval: the "Plan context" block (the "Plan:"
  paragraph's change, "Scope, one bounded change", "Not in scope",
  "Files", any "Known limit", "Deliberately deferred", or deviation
  note); the candidate PR's "Diff" section (each `--- a/` / `+++ b/`
  file header and its hunks); the "Title" and "Description" sections
  for fidelity claims.
- Where it lives, live: `plan.md` (its change, files, scope, and
  `## Deviations` section); the branch diff from
  `git diff main...HEAD` and the file list from
  `git diff main...HEAD --stat`; the title (first line) and body of
  `pr_draft.md`.
- What good looks like: every changed file and hunk implements the
  planned change, its tests, or a repo-required changelog entry, or is
  covered by a deviation note; every change the plan committed to is
  in the diff, or is named as left out both in the plan's notes and
  in the description; every claim the description makes ("implements
  the plan", "adds a test", "docs updated") is true of the diff.
- Silent drift, more than the plan: a new option or flag, a rename or
  refactor pass, or a rewrite of a file the plan never names, with no
  note (often under "no changes beyond the plan").
- Silent drift, less than the plan: a planned docs update, second fix,
  or test missing from the diff with no note, or claimed by the
  description anyway.
- Honest shortfall (passes): the plan records a deferral or known
  limit before the PR, and the description restates it.

## Test evidence (harness category: not-tested)

Checks: `repro_rerun`, `repo_checks_run`.

- Where it lives, eval: the "Test evidence" section, plus any
  before/after transcript in the "Description"; read against the plan
  context's "Test plan:" sentence (the scenarios it will re-run and
  the expected-after it states) and the "Repro evidence" line (the
  before). The repo-facts "pull requests" and "contribution policy"
  lines name the repo's test command.
- Where it lives, live: `test_evidence.md` (the Unit 2 repro steps run
  on `main` for the before and on the branch for the after, plus the
  output of `make test-unit`, `make test-integration`, `make lint`,
  and `make typecheck`), and the Testing section of `pr_draft.md`,
  read against the test plan in `plan.md`.
- What good looks like: each failing scenario the test plan names is
  shown run on the branch, with an observable result (printed output,
  exit code, value, assertion, `diff ... && echo IDENTICAL`) that
  matches the stated expected-after, with a before alongside; the
  repo's suite or the plan's command is shown with its outcome. A
  failing check reported honestly with its reason still counts as
  run (for Path Review, `make test-integration` stopping with "no
  tests ran" is expected).
- Not tested: "tested locally, works now", "verified on my machine";
  a run of only the control or an unchanged path (the case that never
  broke); one of two named failure modes re-run and the other ignored;
  no suite run shown.

## Diff quality (harness category: unreviewable)

Checks: `diff_clean`, `commits_clear`.

- Where it lives, eval: the candidate PR's "Diff" section (every `+`
  and `-` line) and the "Commits" list.
- Where it lives, live: `git diff main...HEAD` and
  `git log main..HEAD --oneline`.
- What good looks like: every changed line is the fix, its tests, or
  a required changelog entry; comments explain the fix; old code the
  fix replaces is deleted, not commented out; commit messages say what
  they change.
- Debris tells: `eprintln!("DEBUG ...")`, `print(`, `console.log`, or
  `dbg!` left in; commented-out code or a commented-out first attempt;
  an added function or variable nothing uses (often behind
  `#[allow(dead_code)]` or named `_unused...`); an added TODO/FIXME;
  re-indent or re-wrap churn; a hunk that removes and re-adds
  identical lines; import reordering; a drive-by edit elsewhere;
  commits named "wip", "fix", "fmt + cleanup", "misc cleanups".

## Standards and comms (harness category: standards-wall)

Checks: `template_asks_met`, `ai_disclosure`, `thread_direction`.

- Where it lives, eval: the "Repo facts" block's "pull requests" line
  (the template's sections, checklist, closing-reference and
  changelog/whatsnew asks) and "contribution policy" line (including
  the AI policy); the "Thread highlights" with each commenter's role;
  read against the candidate PR's "Description" and "Diff".
- Where it lives, live: `.github/PULL_REQUEST_TEMPLATE.md` and
  `docs/CONTRIBUTING.md` in the working copy (or the scoped repo on
  GitHub), the issue thread from `gh issue view <n> --comments`, read
  against `pr_draft.md` and the branch diff. The course also requires
  an own-words AI-use disclosure in "Notes for Reviewers".
- What good looks like: every section and checklist item the template
  asks for has real content or "N/A" with a reason; `Closes #<n>` is
  there when asked; an asked-for changelog or whatsnew entry is in the
  diff; when the policy requires AI disclosure, the description names
  the tool or that AI assisted, and the extent if asked; an
  OWNER/COLLABORATOR/MEMBER direction in the thread is followed or
  answered.
- Wall tells: a required checklist absent from the description, no
  closing reference where the template requires one, a required
  whatsnew/changelog entry missing from the diff, no AI disclosure
  under a policy that says "all AI usage must be disclosed". Not wall
  tells: a terse description that still covers the asks, process
  steps outside the PR (CLA, vouch flow), or a repo with no template
  and no asks.
- Whether the description's claims match the diff is plan fidelity,
  above.
