# Plan: issue #65, review_service unit tests misconfigure async mocks

## Diagnosis

The bug is in the test fixtures, not in the service. In `tests/unit/test_review_service.py`, each of the 13 affected tests builds the query result as `mock_result = AsyncMock()` and then sets `mock_result.scalars.return_value.first.return_value` (or `.all.return_value`). Child attributes of an `AsyncMock` are async too, so `mock_result.scalars()` returns an unawaited coroutine. The service then calls `.first()` or `.all()` on that coroutine. The service code is correct: `get_review` does `result = await db.execute(stmt)` and `return result.scalars().first()`, and `list_reviews` does the same with `.all()`. The result of `await db.execute(...)` should be a synchronous object, so `mock_result` should be a `MagicMock` while `execute` stays an `AsyncMock`.

## Repro evidence

From my Unit 2 reproduction (commit f89c06f, macOS, Python 3.13.2 in the repo's .venv, pytest 9.1.1, pytest-asyncio 1.4.0, SQLAlchemy 2.1.1), running with `--runxfail` to bypass the strict xfail markers:

    pytest --runxfail tests/unit/test_review_service.py -q
    FAILED ...test_get_review_returns_review_for_correct_owner - AttributeError: 'coroutine' object has no attribute 'first'
    FAILED ...test_list_reviews_returns_paginated_results - AttributeError: 'coroutine' object has no attribute 'all'
    [... 11 more, same two error messages]
    13 failed, 6 passed, 1 warning in 0.64s

Without `--runxfail` the same 13 tests report as xfailed. Their markers read `@pytest.mark.xfail(strict=True, reason="issue #65: review_service tests misconfigure async mocks")`. The 6 passing tests are the `create_review` tests, which never read a query result. The error appears only in tests that build `mock_result = AsyncMock()`, which fits the fixture cause above.

## Scope

In scope:
- In `tests/unit/test_review_service.py`, change `mock_result = AsyncMock()` to `MagicMock()` in the 13 tests that use it, and import `MagicMock`.
- Remove the `@pytest.mark.xfail(strict=True, ...)` marker from those same 13 tests, as CONTRIBUTING requires for seeded bugs.
- Fix the one assertion that fails once the mocks work: `test_list_reviews_ordered_by_created_at` asserts `execute.assert_called_once()`, but `list_reviews` calls `execute` twice (a count query, then the page query). Change it to assert two calls.

Not in scope:
- `core/services/review_service.py` stays unchanged. It is correct, and changing it would hide the fixture bug.
- The 6 passing `create_review` tests.
- The weak assertions in some tests (for example `assert len(reviews) > 0 or len(reviews) == 0`). Tightening them is a separate change.
- Lint or type baseline suppressions beyond any that reference #65.

## Files I will touch

- `tests/unit/test_review_service.py` (the only file)

## Approach

1. Create a branch `test/65-fix-async-result-mocks` from `main`.
2. Import `MagicMock` alongside `AsyncMock` and `Mock`.
3. In each of the 5 `get_review` tests and 8 `list_reviews` tests that carry the xfail marker, replace `mock_result = AsyncMock()` with `mock_result = MagicMock()`. Leave `mock_db_session.execute = AsyncMock(return_value=mock_result)` as is, because `execute` really is awaited.
4. Delete the 13 xfail marker lines.
5. In `test_list_reviews_ordered_by_created_at`, replace `assert_called_once()` with an assertion that `execute` was called twice.
6. Run the checks below, then `make check && make test-unit`.

## Test plan

Before (from Unit 2): `pytest --runxfail tests/unit/test_review_service.py -q` gives 13 failed, 6 passed, with the two AttributeError messages.

After the change, with the markers removed so `--runxfail` is no longer needed: `pytest tests/unit/test_review_service.py -q` should show 19 passed, 0 failed, 0 xfailed, and no `XPASS(strict)` failures.

Also expected: `make check` and `make test-unit` pass, and `git diff --stat` shows only `tests/unit/test_review_service.py`.

## Risks and unknowns

- I have not run the fix yet. Predictions about which assertions break once the mocks work come from reading `list_reviews`, so another test may need a small adjustment, which I will record under Deviations.
- Another contributor's PR (#88) already targets this issue with a similar fixture fix. If it merges first, my branch is redundant and I will close it or rebase onto it and say so.
- I do not know whether `pyproject.toml` has a baseline suppression tied to #65. I will `grep "#65" pyproject.toml` before building and remove it if present.
- The conventional-commit scopes listed in CONTRIBUTING (ingestion, rag, agent, safety, api, frontend) do not include a review-service scope. I will use `test(api)` and mention it in the PR.

## Deviations


Nothing in the build differed from the plan. The diagnosis, scope, and file list all held: the bug was in the test fixtures, the only file I changed was `tests/unit/test_review_service.py`, and `core/services/review_service.py` was untouched.

The one prediction in the plan's risks was that `test_list_reviews_ordered_by_created_at` would still fail once the mocks worked, because `list_reviews` calls `execute` twice and the test asserted it was called once. That is what happened. After switching `mock_result` to `MagicMock()`, `pytest --runxfail` gave 18 passed and 1 failed (that test), and changing the assertion to `call_count == 2` fixed it. No other test needed adjusting.

After removing the 13 xfail markers, `pytest tests/unit/test_review_service.py -q` shows 19 passed, and `make test-unit` shows 388 passed, 40 xfailed. The 40 remaining xfails are other seeded bugs. 

One thing outside the plan: `make typecheck` fails on my machine with a numpy stub error (`numpy/__init__.pyi:737: Type statement is only supported in Python 3.12 and greater`). It happens on clean `main` too, and mypy doesn't check `tests/`, so it isn't caused by this change. `ruff` and `black` pass. [CI result on my fork: ___.]

Because nothing changed, my posted plan comment is still accurate and I did not re-run the plan check or add a new comment on the issue.