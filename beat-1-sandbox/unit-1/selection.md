# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64

**Verdict output**

```
## Summary

**#73 — README/.env.example key mismatch:** repo_alive ✅, ai_policy_allows ✅, newcomer_scope ✅ (two files, one docs deliverable), but **unclaimed fails** — PR #77 ("docs: align OpenRouter environment setup", open, body says `Closes #73`) is an open linked PR, not a claim comment, so the house rule doesn't save it. **Reject.**

**#69 — output parser crashes on JSON array:** all required checks pass. No open PR references #69; the only cross-reference is a closed, unrelated course-meta PR. Claim comments from classmates don't block per the house rule. **Accept** — but it touches `rag/generator/output_parser.py`, which brushes against the "avoid core internals such as parsers" line in the fit profile, even though it's an LLM-output parser, not a language/compiler parser.

**#64 — relevance scorer test fixture bug:** all required checks pass, same reasoning as #69. **Accept**, and it's the better fit: a small, self-contained Python test-fixture fix with zero parser/core-internals overlap.

**Ranking (accepted, by fit):** #64 first — closest match to "one bounded Python bug, no core internals"; #69 second — accepted, but the "parser" filename cuts against the stated preference to avoid core internals.
```

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64",
    "checks": [
      {"name": "repo_alive", "grade": "pass", "evidence": "archived: false; commit f89c06f dated 2026-09-16, within 365 days of 2026-09-28"},
      {"name": "ai_policy_allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI-use language at all (silence)"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; no open PR references #64 (only closed, unrelated PR #1 cross-referenced); claim comments are classmates, ignored per house rule"},
      {"name": "newcomer_scope", "grade": "pass", "evidence": "single bounded fix: correct test_query_with_partial_overlap fixture in one file"},
      {"name": "friendly_label", "grade": "pass", "evidence": "labels: ['bug','good first issue','tests','tier-1']"},
      {"name": "names_the_change", "grade": "pass", "evidence": "names exact test function and file: test_query_with_partial_overlap in tests/unit/test_relevance_scorer.py"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69",
    "checks": [
      {"name": "repo_alive", "grade": "pass", "evidence": "archived: false; commit f89c06f dated 2026-09-16, within 365 days of 2026-09-28"},
      {"name": "ai_policy_allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI-use language at all (silence)"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; no open PR references #69 (only closed, unrelated PR #1 cross-referenced); claim comments are classmates, ignored per house rule"},
      {"name": "newcomer_scope", "grade": "pass", "evidence": "single bounded bug: guard one .items() call for array fallback in one file"},
      {"name": "friendly_label", "grade": "pass", "evidence": "labels: ['bug','good first issue','rag','tier-1']"},
      {"name": "names_the_change", "grade": "pass", "evidence": "names exact file rag/generator/output_parser.py and the crashing .items() call"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73",
    "checks": [
      {"name": "repo_alive", "grade": "pass", "evidence": "archived: false; commit f89c06f dated 2026-09-16, within 365 days of 2026-09-28"},
      {"name": "ai_policy_allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI-use language at all (silence)"},
      {"name": "unclaimed", "grade": "fail", "evidence": "open PR #77 'docs: align OpenRouter environment setup' body reads 'Closes #73' — an open linked PR, not a claim comment, so the house rule does not clear it"},
      {"name": "newcomer_scope", "grade": "pass", "evidence": "single docs deliverable: align README.md and .env.example on the OpenRouter key"},
      {"name": "friendly_label", "grade": "pass", "evidence": "labels: ['bug','good first issue','docs','tier-1']"},
      {"name": "names_the_change", "grade": "pass", "evidence": "names exact files README.md and .env.example and the disagreeing variable OPENROUTER_API_KEY"}
    ],
    "verdict": "reject"
  }
]
```

---

## Eval iterations

**Run history**

1. `--limit 3`: `agreement: 2/3 scored items`. issue-01 was `accept / reject / NO`, failed: newcomer_scope.
2. After rewriting `newcomer_scope`, `--only issue-01,issue-05,issue-10,issue-12,issue-15,issue-20`: `agreement: 6/6 scored items`.
3. `--only issue-04,issue-06,issue-07,issue-08,issue-09,issue-11,issue-13,issue-14,issue-16,issue-17,issue-18,issue-19`: `agreement: 12/12 scored items`.
4. Confirming full run, the file `eval-run.txt`: `agreement: 19/20 scored items  (bar: 18/20: PASS)`.

**Issue analysis**

issue-19. Gold label: accept. This rubric: reject. The note in `eval-run.txt` is `failed: newcomer_scope, friendly_label (preferred)`.

The bundle body (`zxcalc/zxlive#517`) says a large-subgraph selection freezes the UI, then lists two causes and three more changes: multiprocessing, matching only expanded rewrite categories, and applying the rewrite on another thread. `newcomer_scope` fails "a menu of independent features a contributor is invited to pick among." That list is what the check failed on, so the verdict rule rejected the issue. `friendly_label` also failed and is marked preferred, so it did not decide the verdict. The gold note calls the same text one maintainer-diagnosed performance bug with named causes.

**Check rationale**

From `tools/issue-select/rubric.md`, the `newcomer_scope` row as uploaded:

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| newcomer_scope | The issue body and the Comments section. Grade the size of the work, not how polished the write-up is. | Pass when the issue asks for one bounded change: one bug, one docs effort, or one UI fix, including a short body and a maintainer-filed bug that only names the broken behavior. Several files that all document or fix the same named workflow are still one change: a new docs page plus updates to the pages that currently describe the old workflow is one deliverable, not a list of tasks. Fail only if one of these is true: the issue calls itself a megaissue, umbrella, or tracking issue; the body is a list of other issues, or a menu of independent features a contributor is invited to pick among; the work is "add this anywhere in the codebase" with no single module or workflow as the whole task; the thread is a design debate and no maintainer has settled the approach; the body is only a usage question ("how do I"); a maintainer says the fix must change core internals (parser, compiler, or equivalent); or the change depends on a product choice the issue leaves open (an asset marked TBD, or a new feature with no maintainer-settled spec). A short list of examples of the same bug is still one change. | required |

The first smoke run rejected issue-01, a single PyPI-docs workflow that also names the old pages to update. The sentence "Several files that all document or fix the same named workflow are still one change" is the fix for that miss. The "menu of independent features" fail stayed, because issue-05 and issue-10 are menus and still needed to reject.

**Trade-offs**

`newcomer_scope` still rejects issue-19. The "several files of one workflow" sentence covers issue-01. It does not cover a list of extra performance projects attached to one bug, so issue-19 stays a miss. I left the wording there after the full run: 19/20 already passes the bar, and dropping the menu fail would put the scope category at risk. issue-05 and issue-10, re-run with `--only`, still rejected under this wording (`agreement: 6/6 scored items` on that batch).

---

## Selection rationale

**Selection rationale**

1. Issue 64 is one test-fixture fix in `tests/unit/test_relevance_scorer.py`. The query "Python Django web framework" overlaps the chunk completely, so a correct scorer returns 1.0 and `assert 1.0 < 0.9` fails. That is a small Python change I can finish, and it matches the fit profile better than the parser file in issue 69.
2. The verdict correctly accepted 64 and 69 and rejected 73 because pull request 77 is open and says `Closes #73`. The rubric cannot see that I would rather not start in a file named `output_parser.py`. The fit profile ranked 64 first after both were accepted.
3. Claiming should be straightforward if the issue is still unassigned and no open pull request has appeared since the live run. Classmate claim comments do not block it under the Path Review house rule. I have not commented on the issue.
