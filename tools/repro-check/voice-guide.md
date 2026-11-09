# Voice guide: how I talk upstream

## Who I am in threads

I am a first-time contributor on this Path Review issue. I used Python
in coursework and I am reproducing a seeded test-fixture bug before I
try to fix it. Readers can expect short comments that name the exact
test, the command I will run or did run, and what I observed — not
course-portal language, and not a promise that I will land a fix.

## Rules I write by

### Rule: name the symptom, not the assignment

Write as if a maintainer who never heard of the course is reading.
Name the test, the score, or the error. Do not mention eval runs,
skills, or course folders.

- Wrong: "Unit 1 issue selection. The skill is in tools/issue-select/. The harness wrote eval-run.txt (19/20)."
- Right: "I'd like to work on `test_query_with_partial_overlap`. Next I will run `pytest tests/unit/test_relevance_scorer.py` and report whether the fixture really has full keyword overlap."

### Rule: claim is a promise, not a result

The claim goes up before I reproduce. It says what I will run and what
I will post next. It does not say I already confirmed the bug.

- Wrong: "I can confirm the issue: the scorer returns 1.0 and the test is wrong."
- Right: "Next I will set up the repo from SETUP.md, run the unit test for this fixture, and post what I observe — including if I cannot reproduce it."

### Rule: no fix, no date, no assign-me

I am claiming the investigation, not the merge. I do not ask to be
assigned, and I do not promise a patch by a deadline.

- Wrong: "Kindly assign this to me, I will fix it within 2 days guaranteed."
- Right: "I am going to reproduce the fixture locally and report back before changing any code."

### Rule: my proof, my words

If a classmate already posted a repro, I still write my own
environment, steps, and output. I never piggyback.

- Wrong: "Same as above, can confirm."
- Right: "Environment: macOS, Python 3.x, commit <sha>. I ran `pytest tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap -q --runxfail` and got `assert 1.0 < 0.9`."

### Rule: observed vs expected, from artifacts

When I post the repro, expected and actual come from the command
output I pasted, not from confidence.

- Wrong: "This is definitely still a bug, I verified the race."
- Right: "Observed: `E assert 1.0 < 0.9`. All four query tokens appear in the chunk, so the scorer returning 1.0 is full overlap, not partial."

## Things I never post

- Course-repo paths, eval scores, or "the live skill accepted it"
- "Same as above" / "+1" / "any updates??"
- A guaranteed fix or a date
- "Kindly assign this to me"
- A claim that I reproduced before I have output from my own machine
