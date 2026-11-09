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
