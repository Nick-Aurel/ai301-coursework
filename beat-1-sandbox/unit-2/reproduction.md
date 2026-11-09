# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

---

## Your identity upstream

**GitHub username**

Nick-Aurel

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64#issuecomment-6734056162

I'd like to work on the partial-overlap fixture in `test_query_with_partial_overlap`. Next I will set up the repo from SETUP.md, run `pytest tests/unit/test_relevance_scorer.py`, and post whether the query "Python Django web framework" really overlaps the chunk completely so the score is 1.0 against `assert 0.3 < score < 0.9`. If I cannot reproduce it, I will say so and include the environment and output.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64#issuecomment-6734123798

Reproduction — #64 partial-overlap fixture

Environment: PathReview `f89c06f` (main), Python 3.11.15, pytest 9.2.0, macOS 26.5.1 (arm64).

Steps:

```
git clone https://github.com/codepath/pathreview-ai301-fa26-s1.git
cd pathreview-ai301-fa26-s1
python3.11 -m venv .venv && .venv/bin/pip install -e ".[dev]"
.venv/bin/pytest tests/unit/test_relevance_scorer.py -q
.venv/bin/pytest tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap -v --runxfail
```

Observed:

Default file run (`pytest tests/unit/test_relevance_scorer.py -q`):

```
..x................                                                      [100%]
18 passed, 1 xfailed in 0.93s
```

The case is marked `@pytest.mark.xfail(strict=True, reason="issue #64: ...")`, so the default run hides the assertion. With `--runxfail`:

```
query = "Python Django web framework"
chunks = [{"text": "Django is a Python web framework for rapid development"}]
score = scorer.score(query, chunks)
>       assert 0.3 < score < 0.9  # Partial overlap should be in middle range
E       assert 1.0 < 0.9

tests/unit/test_relevance_scorer.py:58: AssertionError
2026-11-09 01:36:57 [info     ] relevance_scored               avg_score=1.0 chunks_count=1 query_len=4
```

Control: `test_query_with_perfect_keyword_match` in the same file still passes. The failure is only this fixture.

Expected: a "partial overlap" fixture should score strictly between 0.3 and 0.9.

Actual: all four query tokens (`python`, `django`, `web`, `framework`) appear in the chunk text, so keyword overlap is 4/4 and `RelevanceScorer.score()` returns 1.0. The assertion fails because the fixture is full coverage, not partial. Matches the issue (`assert 1.0 < 0.9`).

Result: Reproduced. Next I will change the fixture so the overlap is genuinely partial, then drop the xfail marker.

## Eval iterations

**Run history**

1. `--limit 3`: `agreement: 3/3 scored items` (pkg-01, pkg-02, pkg-03).
2. `--only pkg-04,pkg-05,pkg-06,pkg-07,pkg-08,pkg-09,pkg-10,pkg-11,pkg-20`: `agreement: 8/9 scored items`. pkg-05 was `accept / reject / NO`, failed: `steps_followable`.
3. After rewriting `steps_followable` so a short description of a small input is enough when the issue already names that input's shape, `--only pkg-05,pkg-01,pkg-06,pkg-18,pkg-12,pkg-13,pkg-14,pkg-15,pkg-16,pkg-17,pkg-19`: `agreement: 11/11 scored items`. Canaries pkg-01 (clear-accept), pkg-06 and pkg-18 (unfollowable-comms) still matched gold.
4. Confirming full run, the file `eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`. Categories: `clear-accept 8/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`.

**Package analysis**

pkg-05 (`conda/conda#16543`). Gold label: accept. First rubric: reject, on required check `steps_followable`. After the wording change: accept.

The report uses conda 26.7.0 on macOS, writes a minimal `env.yml` with a valid `dependencies:` list plus a `category:` section, runs `conda env update --quiet --json -f env.yml 2>/dev/null`, and shows `EnvironmentSectionNotValid` on stdout above the JSON, then `python3 -m json.tool` failing with `Expecting value: line 1 column 1`. That is the issue's `--json` leak. The first `steps_followable` wording required the file contents to be pasted, so the grader failed it ("env.yml described but contents never shown"). The issue already names that input shape, so the check was too strict. After the pass condition allowed a short description of a small input when the issue names the shape, the same package accepted. `control_or_delta` still failed (preferred) because there was no control run without the `category:` section; that did not change the verdict.

**Check rationale**

From `tools/repro-check/rubric.md`, the `steps_followable` row as uploaded:

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| steps_followable | The repro report's steps, commands, and any files or flags they use. | Pass if a stranger who has only this package could start from a stated starting state and reach the trigger using the commands or actions written there. A short description of a small input is enough when the issue already names that input's shape (for example "a valid dependencies list plus a category: section"). Fail if a step depends on a private repo, an unshared config, or a flag/file a stranger cannot obtain, or if the trigger the issue names is skipped. | required |

The first smoke-adjacent `--only` run rejected pkg-05 on this check. The parenthetical example is the conda `env.yml` case. I kept the private-repo / unshared-config fail so pkg-18 still rejects.

**Trade-offs**

The loosened `steps_followable` accepts pkg-05. It still rejects pkg-18, whose steps live in a private monorepo with an unshared `.golangci.yml`. Those two were the canaries on the `--only` regrade (`agreement: 11/11`). A still-stricter "paste every file" rule would have kept pkg-05 as a miss; this wording trades that miss for the private-repo fail, which is the unfollowable case the set actually wants.

---

Related paths: `eval-run.txt` in this directory; your skill's files in `tools/repro-check/`.
