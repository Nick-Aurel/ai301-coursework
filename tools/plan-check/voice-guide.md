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

### Rule: no date, no assign-me

I am not asking to be assigned, and I do not promise a patch by a
deadline. A plan comment may describe the change I intend to make. It
still does not name a date.

- Wrong: "Kindly assign this to me, I will fix it within 2 days guaranteed."
- Right: "I am going to change the fixture so the overlap is partial, drop the xfail, and re-run the same pytest."

### Rule: my proof, my words

If a classmate already posted a repro or a plan, I still write my own.
I never piggyback.

- Wrong: "Same as above, can confirm."
- Right: "Environment: macOS, Python 3.x, commit <sha>. I ran `pytest tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap -q --runxfail` and got `assert 1.0 < 0.9`."

### Rule: observed vs expected, from artifacts

When I post the repro, expected and actual come from the command
output I pasted, not from confidence.

- Wrong: "This is definitely still a bug, I verified the race."
- Right: "Observed: `E assert 1.0 < 0.9`. All four query tokens appear in the chunk, so the scorer returning 1.0 is full overlap, not partial."

### Rule: the plan comment states one change

The plan comment names the cause I am acting on, the file I will
change, what I will not change, and the result I expect when I re-run
the repro. It is my own wording of that plan.

- Wrong: "Same approach as above. I will fix the tests."
- Right: "The query and the chunk share all four tokens, so the score is 1.0. I will change the chunk in `test_query_with_partial_overlap` so one token is missing, remove the xfail, and expect a score strictly between 0.3 and 0.9."

### Rule: if a maintainer already pointed at the code, say so

If an owner or collaborator named a file or a patch, the plan comment
says whether I am following that or doing something narrower, and why.

- Wrong: "I will document a workaround in the README." (when the owner already isolated a source file and asked for a patched binary to be tested, and the comment never mentions it)
- Right: "The failing assertion is in the test fixture, not in the scorer. I will change that fixture and leave `RelevanceScorer.score()` as it is."

## Things I never post

- Course-repo paths, eval scores, or "the live skill accepted it"
- "Same as above" / "+1" / "any updates??"
- A guaranteed fix or a date
- "Kindly assign this to me"
- A claim that I reproduced before I have output from my own machine
