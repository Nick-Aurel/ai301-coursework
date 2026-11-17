# Rubric: is this pull request ready to submit?

Grade only from the evidence named in each row. Read the diff itself
against the plan, and the test evidence against the plan's test plan.
Never let the description's account of the diff stand in for the diff.
A terse PR can be ready; a polished one can still be a hold. A PR that
honestly discloses a shortfall (in the plan's deviation or deferral
notes AND in the description) can be ready.

"The plan" always means the plan text plus its deviation, deferral,
and known-limit notes. "In scope" includes, even when the plan's file
list does not name them: tests and fixtures for the planned change,
and changelog/whatsnew entries the repo's stated asks require.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan_fidelity | Every hunk in the diff (file headers and changed lines) read against the plan's stated change, files, scope, and not-in-scope lines, plus its deviation notes. Then each change the plan commits to, looked for in the diff. | Pass if every hunk implements the planned change (or its tests, fixtures, or a repo-required changelog entry) or is covered by a deviation note, AND every change the plan commits to appears in the diff or is named as left out in the plan's notes and the description. Fail if the diff changes behavior, adds an option/flag/setting, renames or refactors code, or touches a file the plan does not cover and no deviation note covers it (more than the plan), or if a planned change (for example a docs update, a second fix, a test) is missing with no note (less than the plan). A behavior-preserving rewrite of a function the plan never touches is drift. | required |
| description_honest | Each factual claim in the PR title and description about what the change does, which files or docs it touches, and its fidelity ("implements the plan exactly", "no changes beyond the plan", "no functional changes outside X", "docs now document Y"), read against the diff. | Pass if every such claim is true of the diff, and any shortfall the diff has is stated rather than hidden. Fail if any claim is contradicted by the diff: it claims fidelity or "nothing else" while the diff contains unplanned changes, or claims a change (docs, a test, an entry) that is not in the diff. | required |
| repro_rerun | The test evidence (and before/after shown in the description), read against the plan's test plan: each repro, failure mode, or command it says will be re-run and the expected-after result it states. | Pass if, for every failing scenario the plan's test plan names, the evidence shows that scenario run on the branch with an observable result (printed output, exit code, value, assertion, or byte comparison) that matches the plan's expected-after, and a before is visible (in the PR or as the repro evidence the plan built on). Fail if the evidence is only a statement ("tested locally, works now", "verified on my machine"), if it runs only a control or a path the fix does not change (the case that never broke), or if it re-runs one of several failing scenarios the plan named and is silent on the rest. | required |
| repo_checks_run | The test evidence and description, read against the plan's test plan commands and the repo-facts/contributing guide's stated test command (for example `cargo test`, `yarn test`, `go test`, `pytest`, `make test-unit`). | Pass if the repo's own suite or the command the plan names is shown run with its outcome (a pass/fail count, or "<named command> passes"), or a failing check is reported honestly with its reason. Fail if no suite or check run is shown at all, or the outcome is missing. | required |
| diff_clean | Every hunk in the diff, plus the commit list. | Pass if every changed line serves the fix or its tests/changelog. Explanatory comments on the fix are fine, and so is deleting old code the fix replaces. Fail if the diff contains any debris: a debug print/log (`eprintln!("DEBUG`, `print(`, `console.log`) left in, commented-out code, a dead or unused function or variable added (including behind `allow(dead_code)`), a TODO/FIXME added with no work, whitespace/re-indent/re-wrap churn, re-printed identical lines, import reordering, or an edit to a file or function the fix does not need. | required |
| template_asks_met | The repo-facts block's PR-template and contributing-guide asks (live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`), read against the description and the diff. | Pass if every concrete item the repo asks a PR to contain is present: each template section or checklist the template requires has real content (or "N/A" with a reason), an issue-closing reference is present when asked for (`closes #N`/`fixes #N`), and asked-for files (a changelog or whatsnew entry, tests) are in the diff. Fail only when an asked item is visibly absent: a required checklist or section is missing or left as boilerplate, a required closing reference is missing, or a required changelog/whatsnew entry is absent from the diff. Never fail on a quality judgment (how good a title or summary reads) or on process steps that live outside the PR text (CLA signing, vouch flow, discussion approval). If the repo states no template and no asks, pass. | required |
| ai_disclosure | The repo-facts contribution-policy line (live: the AI section of `docs/CONTRIBUTING.md` and the course rule that every PR discloses AI use), read against the description. Treat every package as AI-assisted work; the absence of a disclosure is not evidence that no AI was used. | Fail only when the policy requires disclosing AI use for pull requests or "in any form" (wording such as "must be disclosed", "disclose the tool used") AND the description does not disclose the tool or that AI assisted, and the extent when the policy asks for it. Pass when the policy has no disclosure requirement (silent, or only asks that comments be human-written or that the author understand the code), or when the description discloses as the policy asks. In live mode the course always requires the disclosure. | required |
| thread_direction | Thread highlights whose role is OWNER, COLLABORATOR, or MEMBER that tell the PR what to do or not do (a file, approach, constraint such as "no private APIs", or a question the fix must answer), read against the diff and description. | Pass if there is no such direction, or the PR follows it, or the description names it and explains the difference. Fail if the diff does what such a direction ruled out, or ignores a question it was asked to answer, with no mention. | preferred |
| commits_clear | The commit list. | Pass if each commit message says what it changes. Fail on messages such as "wip", "fix", "misc cleanups while debugging". Never changes the verdict. | preferred |

## Verdict rule

Accept only if every required check (`plan_fidelity`,
`description_honest`, `repro_rerun`, `repo_checks_run`, `diff_clean`,
`template_asks_met`, `ai_disclosure`) is `pass`. A `fail` on any
required check is a reject. `unclear` means the evidence that row
names is absent from the package; on a required check it counts as
`fail`. The preferred checks (`thread_direction`, `commits_clear`)
never change the verdict, whatever their grade. The deciding check
quoted for a reject is the first failing required check in table
order.
