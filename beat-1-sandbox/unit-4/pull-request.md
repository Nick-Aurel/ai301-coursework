# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/256

**Branch**

`fix/64-partial-overlap-fixture`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

I did one full run, so my history has one score:

1. `agreement: 19/20 scored items  (bar: 18/20: PASS)`

That run is the `eval-run.txt` committed in this folder. Per category: `clear-accept 6/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`.

**Package analysis**

**pkg-02** (Textualize/rich#4208, category clear-accept). My rubric decided **reject**, and the gold label is **accept**. The eval table reads `pkg-02  clear-accept    accept  reject   NO     failed: diff_clean`.

The fix itself matches the plan: all three `save_*` methods export with `clear=False` and clear the record only after the write, and the PR adds a test and a CHANGELOG entry. My rubric rejected it because of how the `save_html` and `save_svg` hunks are written. They remove lines and re-add identical ones:

```
-        html = self.export_html(
-            theme=theme,
-            clear=clear,
+        html = self.export_html(
+            theme=theme,
+            clear=False,
```

Only `clear=clear` -> `clear=False` really changes. `html = self.export_html(` and `theme=theme,` come out and go back in unchanged. My `diff_clean` row counts "re-printed identical lines" as debris, and it's a required check, so the verdict rule turned that into a reject. The gold note says the PR is "exactly the plan's shape". Those repeated lines are most likely how the diff was displayed, not churn the author added, so a human reviewer wouldn't hold the PR for them.

**Check rationale**

| diff_clean | Every hunk in the diff, plus the commit list. | Pass if every changed line serves the fix or its tests/changelog. Explanatory comments on the fix are fine, and so is deleting old code the fix replaces. Fail if the diff contains any debris: a debug print/log (`eprintln!("DEBUG`, `print(`, `console.log`) left in, commented-out code, a dead or unused function or variable added (including behind `allow(dead_code)`), a TODO/FIXME added with no work, whitespace/re-indent/re-wrap churn, re-printed identical lines, import reordering, or an edit to a file or function the fix does not need. | required |

This row is long because the unreviewable packages hide their debris in different places. A short pass condition like "the diff has no unrelated changes" leaves the grader to decide what counts as unrelated, and a confident description can talk it out of a fail. So I spelled out the debris concretely (debug prints, commented-out code, dead code behind `allow(dead_code)`, TODOs, re-indent churn, re-printed lines, import reordering). The grader then matches lines in the diff instead of judging intent, and the procedure and evidence guide list the same tells, so all three files agree. I also added two explicit allowances, "Explanatory comments on the fix are fine, and so is deleting old code the fix replaces", so a clean fix that removes the code it replaces doesn't get flagged as churn. I rejected making it a preferred check: debris is one of the four reject categories, so it has to be able to decide the verdict.

**Trade-offs**

The strictness of `diff_clean` changes one package: it turns **pkg-02** from accept into reject, because "re-printed identical lines" also matches a diff where unchanged lines happen to be shown as removed and re-added. That miss is what it costs me. In return, the same row catches all three unreviewable packages (`unreviewable 3/3`), and those are the PRs where debris really should block. I chose not to loosen it for this run. Dropping "re-printed identical lines", or adding a rule that ignores pairs of identical -/+ lines, would likely flip pkg-02 to agree. But it could also let real churn through in the unreviewable packages, and I'd need to re-run pkg-12, pkg-15 and pkg-18 with `--only` to show it doesn't. At 19/20, above the 18/20 bar, I accepted the one miss rather than risk a regression I hadn't tested. Nothing changed elsewhere: every other category agrees on every package (`not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`, and 6 of 7 clear-accepts).

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
