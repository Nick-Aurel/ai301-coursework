# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

Nick-Aurel

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64#issuecomment-6735103658

The partial-overlap fixture is full overlap. On PathReview `f89c06f`, Python 3.11.15, pytest 9.2.0, macOS 26.5.1 arm64, `pytest tests/unit/test_relevance_scorer.py -q` reported `18 passed, 1 xfailed`. The test is marked `@pytest.mark.xfail(strict=True)`, so that run hides the assertion. The same test with `--runxfail` failed:

```
query = "Python Django web framework"
chunks = [{"text": "Django is a Python web framework for rapid development"}]
E       assert 1.0 < 0.9
relevance_scored  avg_score=1.0 chunks_count=1 query_len=4
```

All four query tokens are in the chunk, and `RelevanceScorer.score()` is the size of that overlap divided by `query_len`. Four of four is 1.0. The scorer matches that input. `test_query_with_perfect_keyword_match` in the same file still passed.

That 1.0 result is the reproduction I already posted. This check is new: on the same `f89c06f` checkout, with the query held fixed, the current chunk scores 1.0 and the same chunk with `web` removed, `"Django is a Python framework for rapid development"`, scores 0.75, which is inside `0.3 < score < 0.9`.

I will change only `test_query_with_partial_overlap` in `tests/unit/test_relevance_scorer.py`: use that chunk, and delete the strict `xfail` on that test. Leaving the marker in place would fail the run as an unexpected pass once the assertion holds. I will not change `rag/evaluator/relevance_scorer.py`, the query, the assertion bounds, or any other test.

Then I will re-run those two pytest commands. I expect the file run to report `19 passed`, and the single test to pass with `avg_score=0.75` instead of `assert 1.0 < 0.9`.

If the pre-commit mypy hook blocks the commit because `chunks = []` in `test_empty_chunks_list_returns_zero` has no annotation, I will add only that annotation. That line is already in the file and is not part of this fixture fix.

---

## Your branch

**Branch**

fix/64-partial-overlap-fixture

**Evidence**

Before, on `fix/64-partial-overlap-fixture` before the edit, same checkout `f89c06f`, Python 3.11.15, pytest 9.2.0:

```
.venv/bin/pytest tests/unit/test_relevance_scorer.py -q
```

```
..x................                                                      [100%]
18 passed, 1 xfailed, 1 warning in 0.42s
```

```
.venv/bin/pytest tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap -v --runxfail
```

```
>       assert 0.3 < score < 0.9  # Partial overlap should be in middle range
E       assert 1.0 < 0.9

tests/unit/test_relevance_scorer.py:58: AssertionError
----------------------------- Captured stdout call -----------------------------
2026-11-09 03:07:26 [info     ] relevance_scored               avg_score=1.0 chunks_count=1 query_len=4
FAILED tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap
1 failed, 2 warnings in 0.22s
```

After the fixture change and the `chunks: list[dict] = []` annotation:

```
.venv/bin/pytest tests/unit/test_relevance_scorer.py -q -p no:cacheprovider
```

```
...................                                                      [100%]
19 passed in 0.35s
```

```
.venv/bin/pytest tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap -q --runxfail -p no:cacheprovider -s
```

```
2026-11-09 03:08:16 [info     ] relevance_scored               avg_score=0.75 chunks_count=1 query_len=4
.
1 passed in 0.19s
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

One full scored run: 20/20. That is the agreement line in `eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`. Categories on that run: `clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.

Before that full run I graded only `calib-01`, `calib-03`, and `calib-04`. Those three matched gold (accept, reject, reject) and are not scored, so they are not an agreement score.

**Package analysis**

`pkg-20` (ghostty-org/ghostty#11261), category `thread-convention`. Gold label: reject. My rubric: reject.

The plan in that package is a bounded fix. It follows the thread's direction to recompute `prev` only when page capacity changes, names the files, and its test plan is `zig build test` with the two fuzz cases passing. `cause_grounded`, `scope_bounded`, `can_start`, `test_decisive`, and `thread_followed` passed. `ai_disclosure` failed. The repo-facts line says "All AI usage in any form must be disclosed", and the plan comment never discloses a tool or that AI assistance was used. That one required failure is the reject. The note on the gold label is the same fact: the plan is fine, and the comment has no disclosure where the policy requires one.

**Check rationale**

`test_decisive`, as it reads in the uploaded `rubric.md`:

"Pass if the test plan names what will be run (the repro steps, a named command, or a test that encodes them) and a concrete result for this bug (an exit code, a specific printed value, a color, a timing, a count, or an assertion about the symptom). Fail if success is a feeling ("feel fast," "feel broken," "look much better") or the only check is "run the full suite and make sure nothing regresses" with no result stated for the reported behavior."

It reads that way because the sample rubric's test check was "passes if the plan adds an automated test." That would have held `calib-01`, whose test is a re-run of the repro with the color flip as the observable result, and the staff label for `calib-01` is ready. The check asks for a named run and a concrete result instead. The suite-only sentence is there because `calib-04` is otherwise a bounded, grounded plan and its test plan is only `cargo test --workspace` and "make sure nothing regresses." The staff label for `calib-04` is hold.

**Trade-offs**

`test_decisive` holds a plan that is otherwise ready when the only success line is the full suite. That is `calib-04`: the cause fits the repro, the scope is one change, and the files are named, and this check is what rejects it. The same wording lets a manual re-run through, so it does not demand a new automated test. I did not loosen it after the full run. The 20/20 agreement, including `unbuildable 3/3` and `clear-accept 7/7`, is how I know that line did not flip a package the rubric had already agreed on.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
