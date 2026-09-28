# Rubric: is this a good first issue?

Grade only from the evidence named in each row. In eval mode, measure every
day-count against the bundle's capture date, not against today. In live
mode, measure day-counts against today. A bot author is a username ending
in `[bot]`.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| repo_alive | Repo facts: the `archived:` flag, and the dates and authors of the "last 5 default-branch commits". | Pass only if `archived: no` AND at least one of those five commits has a date within 365 days of the capture date (live mode: within 365 days of today). A commit whose author is a bot counts toward that 365-day window only when the commit message says it merges a human's pull request. Otherwise fail. | required |
| ai_policy_allows | Repo facts: the "contribution policy" line, including any quoted ban or condition. | Fail only when that line states an outright ban on AI-generated code or documentation (wording such as "we do not accept AI-generated code" or "we do not accept AI-generated documentation"). Pass when the line is silent, and pass when it only sets conditions (disclosure, personal understanding, testing, or human review of AI output), including "discourages" or "closed if there is no human review". | required |
| unclaimed | Repo facts: "this issue: assignees:" and "linked PRs:" (state of each PR). Also the Comments section: comment date, author_association, and whether the text is a claim. | Pass only if all three are true. (1) Assignees is `none`. (2) No linked PR has state `open` (closed or merged PRs do not fail). (3) No non-bot comment dated within 180 days before the capture date (live mode: within 180 days of today) says the commenter is taking or currently doing the work ("I'll take this", "I will work on", "I've started", "working on this", "take a swing"). A claim older than that 180-day window does not fail. A later OWNER, MEMBER, or COLLABORATOR comment that says the claim was abandoned, or that explicitly invites someone else to take the issue, clears a claim inside the window. A bot nudge does not clear an assignee or an open linked PR. | required |
| newcomer_scope | The issue body and the Comments section. Grade the size of the work, not how polished the write-up is. | Pass when the issue asks for one bounded change: one bug, one docs effort, or one UI fix, including a short body and a maintainer-filed bug that only names the broken behavior. Several files that all document or fix the same named workflow are still one change: a new docs page plus updates to the pages that currently describe the old workflow is one deliverable, not a list of tasks. Fail only if one of these is true: the issue calls itself a megaissue, umbrella, or tracking issue; the body is a list of other issues, or a menu of independent features a contributor is invited to pick among; the work is "add this anywhere in the codebase" with no single module or workflow as the whole task; the thread is a design debate and no maintainer has settled the approach; the body is only a usage question ("how do I"); a maintainer says the fix must change core internals (parser, compiler, or equivalent); or the change depends on a product choice the issue leaves open (an asset marked TBD, or a new feature with no maintainer-settled spec). A short list of examples of the same bug is still one change. | required |
| friendly_label | The issue's `labels:` line. | Pass if any label is `good first issue`, `Easy to Fix`, or `help wanted` (match case-insensitively). Otherwise fail. This check never changes the verdict. | preferred |
| names_the_change | The issue body. | Pass if the body names a concrete command, file, function, UI element, or behavior to change. Otherwise fail. This check never changes the verdict. | preferred |

## Verdict rule

Accept only if every required check (`repo_alive`, `ai_policy_allows`, `unclaimed`, `newcomer_scope`) is `pass`. A `fail` or `unclear` on any required check is a reject. Preferred checks (`friendly_label`, `names_the_change`) never change the verdict; report them, and use them only to rank issues that are accepted. There is no third verdict.
