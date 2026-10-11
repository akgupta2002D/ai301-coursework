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

akgupta2002D

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/65#issuecomment-6114633868

Reproduced #65 on commit f89c06f. With `pytest --runxfail tests/unit/test_review_service.py -q` I get 13 failed, 6 passed, with `AttributeError: 'coroutine' object has no attribute 'first'` (get_review tests) and `'all'` (list_reviews tests), matching the issue. Without `--runxfail` the same 13 report as xfailed because of their strict markers.

**Cause:** the bug is in the test fixtures, not the service. Each of the 13 tests builds `mock_result = AsyncMock()`, so `mock_result.scalars()` returns a coroutine. `review_service.py` is correct and I will not change it.

**Plan:** in `tests/unit/test_review_service.py` only:
- switch `mock_result` to `MagicMock()` in those 13 tests (keep `execute` as an `AsyncMock`)
- remove the 13 strict xfail markers, per CONTRIBUTING
- update `test_list_reviews_ordered_by_created_at`, which asserts one `execute()` call although `list_reviews` makes two (count query, then page query)

Not touching: the service, the 6 passing `create_review` tests, or tightening weak assertions.

**Test plan:** the same pytest command should go from 13 failed / 6 passed to 19 passed with the markers removed, plus `make check` and `make test-unit`.

I noticed PR #88 already takes this approach for this issue. My plan was built from my own reproduction and reaches the same fix, so if #88 merges first I will close my branch instead of opening a competing PR. Happy to follow whatever the maintainers prefer.

AI disclosure: Claude helped me read the code and draft this plan. I ran the reproduction myself and reviewed the plan before posting.

---

## Your branch

**Branch**

test/65-fix-async-result-mocks

**Evidence**

Before (Unit 2 reproduction, commit f89c06f; `--runxfail` bypasses the strict xfail markers):

```
$ pytest --runxfail tests/unit/test_review_service.py -q
FAILED ...test_get_review_returns_review_for_correct_owner - AttributeError: 'coroutine' object has no attribute 'first'
FAILED ...test_list_reviews_returns_paginated_results - AttributeError: 'coroutine' object has no attribute 'all'
[... 11 more, same two error messages]
13 failed, 6 passed, 1 warning in 0.64s
```

Midway through the build (mock_result changed to MagicMock, markers not yet removed):

```
$ pytest --runxfail tests/unit/test_review_service.py -q
18 passed, 1 failed
FAILED test_list_reviews_ordered_by_created_at: Expected 'execute' to have been called once. Called 2 times.
```

After (branch test/65-fix-async-result-mocks: 13 xfail markers removed, ordered test now asserts `execute.call_count == 2`):

```
$ pytest tests/unit/test_review_service.py -q
19 passed

$ make test-unit
========================================= warnings summary =========================================
core/config.py:7
  /Users/ankitgupta/Classes/CodepathAI301/pathreview-ai301-fa26-s1/core/config.py:7: PydanticDeprecatedSince20: Support for class-based `config` is deprecated, use ConfigDict instead. Deprecated in Pydantic V2.0 to be removed in V3.0. See Pydantic V2 Migration Guide at https://errors.pydantic.dev/2.13/migration/
    class Settings(BaseSettings):

-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
============================ 388 passed, 40 xfailed, 1 warning in 7.00s
```

`git diff --stat` shows only `tests/unit/test_review_service.py` (16 insertions, 55 deletions). `core/services/review_service.py` is untouched.

`make check`: ruff and black pass (110 files left unchanged). The mypy step fails on my machine both with and without this change, and mypy does not check `tests/`:

```
$ make check
.venv/bin/ruff check .
All checks passed!
.venv/bin/black .
All done! ✨ 🍰 ✨
110 files left unchanged.
.venv/bin/mypy api/ core/ ingestion/ rag/ agent/ safety/
.venv/lib/python3.13/site-packages/numpy/__init__.pyi:737: error: Type statement is only supported in Python 3.12 and greater  [syntax]
Found 1 error in 1 file (errors prevented further checking)
make: *** [typecheck] Error 2
```

## Eval iterations

**Run history**

Run 1 (full run): 17/20 (bar: 18/20: below the bar). clear-accept 5/7, scope-creep 4/4, thread-convention 1/2, unbuildable 3/3, wrong-cause 4/4. Misses: pkg-02, pkg-14, pkg-20.

Run 2 (full run, after revising fits-thread-and-conventions and executable-without-asking): 18/20 (bar: 18/20: PASS). clear-accept 5/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4. Misses: pkg-09, pkg-14. This run was written to `eval-run.txt`.

**Package analysis**

pkg-20 (ghostty-org/ghostty#11261). My rubric's verdict: reject. Gold label: reject, so we agree. In Run 1 my rubric graded it accept, which was wrong.

The plan itself is strong. Its diagnosis says `prev` "goes stale when a mid-print operation (hyperlink storage growth) reallocates the page", and the repro control, "the same first test with the hyperlink start removed (no page growth mid-print) passes", isolates that trigger. The plan follows the maintainer's direction in the thread and names what it won't do. diagnosis-matches-repro, scope-has-limits, executable-without-asking and test-shows-fix-worked all pass. The reject comes from fits-thread-and-conventions. Repo facts for ghostty states: "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance". The plan comment says it reproduced both fuzz cases and describes the generation counter, but it contains no AI disclosure. In Run 1 my pass condition said only "follows every stated convention", and the grader let it through. I rewrote the check so that when Repo facts states an AI disclosure requirement, the comment itself must disclose, and Run 2 rejected it.

**Check rationale**

fits-thread-and-conventions | The Candidate plan comment, compared with Thread highlights and the Repo facts section (contribution policy, bug-report template, any AI disclosure requirement) | P if the comment does not duplicate or contradict anything in the thread (linked PRs, other plans, maintainer direction) and follows every convention Repo facts states. If Repo facts states an AI disclosure requirement, the comment itself must disclose AI use (tool and extent); a comment with no disclosure is F. If Repo facts states no AI policy, no disclosure is needed. F also if it ignores an existing fix or maintainer direction, violates another stated convention, or guarantees a deadline or tells others to stay off the issue. Saying it will take the issue or will report back is not a guarantee or a reservation. | required

It reads this way because of two Run 1 misses. pkg-02 (gold accept) was rejected on this check, and its plan comment says "I can take this one" and "Report-back once the test is in", which my earlier clause about promising a timeline or reserving the issue read as a violation. So I added the last sentence saying those phrases are not a guarantee or a reservation. pkg-20 (gold reject) was accepted because "follows every stated convention" did not make the grader enforce the AI policy, so I added the explicit disclosure rule, including the case where a repo states no AI policy.

**Trade-offs**

The explicit disclosure rule gives up leniency. A comment from a repo with an AI policy that is otherwise good, but puts its disclosure somewhere other than the comment, now fails. Run 2 shows a cost of this check: pkg-09 (gold accept) was graded reject on fits-thread-and-conventions, so the stricter wording may be rejecting something it should accept. I kept it because pkg-20 and the thread-convention category (now 2/2) depend on it, and the run still met the 18/20 bar.

The carve-out for "will take the issue" and "will report back" gives up some protection. A comment that implies a timeline in softer words could slip through, since only a guarantee or an explicit reservation fails. I accepted that because rejecting ordinary claim language, as happened to pkg-02 in Run 1, was the worse error.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in `tools/plan-check/`.