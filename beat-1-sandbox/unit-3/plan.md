# Plan: issue #64 partial-overlap fixture

## Diagnosis

The scorer is doing what its overlap rule says. The fixture named "partial overlap" is full overlap.

From the reproduction I posted on this issue, on PathReview `f89c06f` (main), Python 3.11.15, pytest 9.2.0, macOS 26.5.1 arm64:

```
.venv/bin/pytest tests/unit/test_relevance_scorer.py -q
```

```
..x................                                                      [100%]
18 passed, 1 xfailed in 0.93s
```

The case is marked `@pytest.mark.xfail(strict=True, reason="issue #64: ...")`, so that run hides the assertion. The same test with `--runxfail`:

```
query = "Python Django web framework"
chunks = [{"text": "Django is a Python web framework for rapid development"}]
score = scorer.score(query, chunks)
>       assert 0.3 < score < 0.9  # Partial overlap should be in middle range
E       assert 1.0 < 0.9

tests/unit/test_relevance_scorer.py:58: AssertionError
2026-11-09 01:36:57 [info     ] relevance_scored               avg_score=1.0 chunks_count=1 query_len=4
```

`query_len=4` and `avg_score=1.0` means all four tokens (`python`, `django`, `web`, `framework`) are in the chunk. `RelevanceScorer.score()` in `rag/evaluator/relevance_scorer.py` sets relevance to `len(query_tokens & chunk_tokens) / len(query_tokens)`. Four of four is 1.0, so `assert 0.3 < score < 0.9` fails as `assert 1.0 < 0.9`. The test `test_query_with_perfect_keyword_match` in the same file still passed, so this is the fixture, not a broken scorer.

On that same checkout I called the scorer with the query held fixed:

```
'Django is a Python web framework for rapid development' -> 1.0
'Django is a Python framework for rapid development' -> 0.75
```

The second string is the same chunk with `web` removed. Three of the four query tokens remain, and 0.75 sits inside `0.3 < score < 0.9`.

## Scope

In scope: `test_query_with_partial_overlap` in `tests/unit/test_relevance_scorer.py` only. Change the chunk so one query token is absent, and delete the strict `xfail` on that test. A strict xfail left in place becomes `XPASS` once the assertion passes.

Out of scope: `RelevanceScorer.score()` and `_tokenize`, the query string, the assertion bounds `0.3 < score < 0.9`, and every other test. The one exception is a type annotation on the existing `chunks = []` in `test_empty_chunks_list_returns_zero`, and only if the pre-commit mypy hook refuses the commit without it.

## Files

- `tests/unit/test_relevance_scorer.py`

## Approach

1. In `test_query_with_partial_overlap`, replace the chunk text `"Django is a Python web framework for rapid development"` with `"Django is a Python framework for rapid development"`.
2. Delete the `@pytest.mark.xfail(strict=True, reason="issue #64: ...")` decorator on that test, including the import of `pytest` only if nothing else in the file still uses it. The file uses `@pytest.mark.unit` and `@pytest.fixture`, so the import stays.

## Test plan

Re-run the two commands from the reproduction.

Before, already posted: `pytest tests/unit/test_relevance_scorer.py -q` prints `18 passed, 1 xfailed`. The single test with `--runxfail` fails with `E assert 1.0 < 0.9` and logs `avg_score=1.0`.

After: the file run prints `19 passed` and does not report an xfail. The single test, with or without `--runxfail`, passes, and the log line for that call shows `avg_score=0.75`.

## Risks and unknowns

`.pre-commit-config.yaml` runs mypy on staged Python files. This file already has `chunks = []` in `test_empty_chunks_list_returns_zero`, which this fix does not otherwise change. If the hook rejects the commit for a missing annotation on that line, the only extra edit will be the annotation the hook names. That is not part of the fixture fix, and I will record it under Deviations if it happens. CI's typecheck job does not include `tests/`, so this risk is the local commit hook, not the unit-test result.

I have not re-run the suite on any Python other than 3.11.15. The score is a token-set ratio, so the 0.75 result does not depend on the OS.

## Deviations

The fixture change is the one in the plan: the chunk in `test_query_with_partial_overlap` lost the word `web`, and the strict `xfail` on that test is gone. `RelevanceScorer` is untouched.

The mypy check the plan named did fire. Running `mypy tests/unit/test_relevance_scorer.py --ignore-missing-imports` (the same flag as the pre-commit hook) reported `Need type annotation for "chunks"` at `chunks = []` in `test_empty_chunks_list_returns_zero`. I added `chunks: list[dict] = []` and nothing else in that test. After that, the same mypy command reported no errors in the file. The file run is `19 passed`. The single test passes, and the scorer log for it is `avg_score=0.75`.
