Result: Reproduced the effect described in #65. Running the service's test suite shows the same 13-test split the issue reports (6 passing, 13 affected), and the tests emit a RuntimeWarning: coroutine 'AsyncMockMixin._execute_mock_call' was never awaited — consistent with the mock returning an unawaited coroutine from result.scalars(), as the issue describes. One caveat: these 13 are currently marked @pytest.mark.xfail(strict=True, ...) in this repo state (per CONTRIBUTING.md's practice for seeded bugs), so pytest reports them as xfailed rather than failed — I did not force --runxfail to capture the literal AttributeError traceback the issue quotes, so I can confirm the same symptom and the same count, but not the exact error text, from this run.

Environment: macOS (darwin), Python 3.13.2 inside the repo's .venv, pytest 9.1.1, pytest-asyncio 1.4.0, SQLAlchemy 2.1.1. Working from my fork of codepath/pathreview-ai301-fa26-s1.

Steps:
git clone <your fork URL>
cd pathreview-ai301-fa26-s1
cp .env.example .env
docker compose up -d
make setup
source .venv/bin/activate   # required — see note below
pytest tests/unit/test_review_service.py -q

Note: running pytest without activating .venv first picks up a stray system Python 3.9 on my machine and fails at collection with ModuleNotFoundError: No module named 'structlog' — unrelated to this bug, but worth activating the venv explicitly before running.

Observed:
6 passed, 13 xfailed, 2 warnings in 0.66s
...
RuntimeWarning: coroutine 'AsyncMockMixin._execute_mock_call' was never awaited

Expected (per the issue): pytest tests/unit/test_review_service.py -q → 13 failed, 6 passed, with AttributeError: 'coroutine' object has no attribute 'first' (and 'all').

What differed: counts match (13 affected / 6 passing), and the coroutine-awaited warning matches the described root cause, but my run reports