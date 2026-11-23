# Contribution: Detect Linear API keys (lin_api_...)

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Ankit Gupta
**GitHub Username:** akgupta2002D
**Issue:** https://github.com/omerbek/repodx/issues/63
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built an issue scout from my own gh queries, ranking filters, and rubric**

I built a scout from four `gh search issues` queries, six ranking filters, and a seven-check rubric, then ran a scan that graded five issues. It fell short in three ways: all five survivors were created within the hour because my tie-break was recency, it accepted a one-author repo with 3 stars, and it rejected an umbrella issue that named one small good-first-issue step. I fixed those with a `createdAt` field, a 7-day drop rule, a one-result-per-repo rule, a `repo-adoption` check, and a tracking-issue clause, but a second scan showed my guessed 90-day repo-age threshold rejects good fits like glassbeetle#316, so that rule needs revisiting. The scout also rejected my chosen issue, AetherSDR#7012, because a maintainer hasn't confirmed the design change, which I'll address when I claim it.

**Unit 6: Installed a project-scoped voice guard and wrote a repo memory file for repodx**

I extended my voice guide with Upstream rules for claims, questions, and pings, and
installed the voice guard per project instead of user-wide: the guard, its hook, and my
skills live in `ai301-unit6-starter/.claude/` (and a copy in my repodx fork, with the
voice guide linked back to one file), because I want tools scoped to the folders that use
them. The wiring test passed (the guard stopped `gh issue comment --help` with "no body
the guard can read"), but running issue-scout headless with `claude -p` failed outright,
since it can't approve the `gh` calls, so I re-ran it interactively. Writing
`CLAUDE.local.md` meant reading repodx's scanner, which surfaced two things the issue
doesn't mention: a pattern does nothing in a real file scan unless its prefix is also in
`CANDIDATE_LITERALS`, and `looks_fake` silently ignores test keys containing strings like
"12345678", so I recorded both before starting the fix.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched with my issue scout: four `gh search issues` queries, ranking filters, and a
seven-check rubric. Last week I graded three picks directly: aerostack2#1047 (accepted),
AetherSDR#7012 (rejected on scope), and glassbeetle#316 (rejected on repo age). My Unit 5
entry names AetherSDR#7012 as my choice, but after patching my rubric this week I
re-graded all three and the picture changed. #7012 now passed, flagged *ambitious*
because the change reverses a deliberate design choice a maintainer still has to confirm,
and it is C++/Qt GUI wording, a weak fit for the backend and Python work I want. The other
two had already been fixed and closed by other contributors within days. So I ran a fresh
scan: 398 unique results, 51 survivors, top 5 graded. Only one was accepted:
omerbek/repodx#63, *straightforward*, found by my Q4 query (api + Python + good first
issue). Before claiming, I read the whole thread (open, no comments, no assignee, no linked
PR) and the repo's CONTRIBUTING.md, which asks contributors to claim by comment and puts no
restriction on AI-assisted work.

It fits me for two reasons. RepoDx is a pre-push safety check for AI-built projects, and
I've worked on AI safety and evals before, so improving a tool that catches leaked secrets
in AI-assisted code is the kind of work I want to do more of. It also looks simple in a
good way: a bounded Python change where the test is part of the work. I add a pattern to
`SECRET_PATTERNS` and `CANDIDATE_LITERALS` in `repodx.py`, plus a test built with
`fake(...)` so no real key is committed. I've worked with regular expressions in
coursework, including building a compiler, so I can focus on doing it properly rather than
learning regex from scratch. I hope to learn how a real project wants a change shaped:
verifying a format against the provider's docs, writing a test that fails before the fix
and passes after, checking for false alarms on real repositories, and getting a PR through
review. My honest read of the scout: the scope patch did what it was meant to (it turned
#7012's open design edge into a difficulty note instead of a reject), but the wild exposed
new gaps. No check asks whether an issue is still open, so a closed, already-fixed issue
only failed because the scout judged a merged PR as a claim. My "younger than 7 days"
filter was written as a fixed date, so it silently dropped 273 results. And sorting by
fewest comments surfaced four solo repos that all failed adoption. I logged all three in
scan-notes.md; the next fixes are a relative date rule and an "issue is open" clause in
`unclaimed`.

Claim comment: https://github.com/omerbek/repodx/issues/63#issuecomment-7091187528

**Toolkit**

- Tool: issue-scout (my search strategy and rubric). I used it to re-grade last week's three
  picks directly, then ran a fresh live scan, and checked the chosen issue's thread myself
  before claiming.
- What it produced: the re-grade accepted only AetherSDR#7012 (ambitious) and rejected the
  other two as already fixed; the scan graded 5 of 51 survivors and accepted one,
  omerbek/repodx#63, with all 7 checks passing and the difficulty note "straightforward:
  the issue names where the change lives and the change is bounded."
- Difficulties: my original scope check rejected any issue with an open design edge, which
  would reject most real issues. The wild scan also showed that no check catches a closed
  issue, my age filter used a fixed date that dropped 273 results, and my ranking lifted
  tiny solo repos.
- What changed: I replaced `scope-fits` with `scope-feasible` (the unit-6 scope patch) and
  added the difficulty note, keeping one clause of my own: a maintainer saying an issue is
  blocked or will not be accepted still fails. I logged the closed-issue gap, the
  fixed-date filter, and the ranking bias in scan-notes.md as the next fixes.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

- macOS (Darwin 25.6.0), Python 3.13.2, no extra packages (RepoDx has no dependencies).
- RepoDx 0.5.0 (`__version__` in `repodx.py`), upstream `main` at commit `e845025`
  ("Merge pull request #82").
- Setup: forked omerbek/repodx to akgupta2002D/repodx, cloned the fork, ran the repo's
  own checks from CONTRIBUTING.md to get a clean baseline:
  - `python3 -m unittest discover`: `Ran 115 tests ... OK`
  - `python3 repodx.py .`: `Score: 100/100 (A)`, `Found: 0 critical, 0 warnings, 0 info`
- Challenges: none in setup. One format question: Linear does not publish a length for
  personal API keys. The issue says "about 40 alphanumeric characters"; the one example I
  found in third-party docs has 32. Today's behavior is the same for both lengths (below),
  so the reproduction does not depend on it. It matters for the fix, not the repro.

**Steps to Reproduce**

All keys are fake, built at runtime by joining parts, never a real key. The Linear body
`Q7zT4mWx9Rb2Kd5Nf8Lp3Hs6Vc1Yj0GuE4tA9BnC` is 40 random-looking characters, and both it and
its first 32 characters pass `looks_fake()` as `False` (step 1 prints this), so a miss is
not explained by the fake filter. Run everything from the clone root.

1. Line level, 40- and 32-character Linear keys:
   ```
   python3 - <<'PY'
   import repodx
   body = "Q7zT4mWx9Rb2Kd5Nf8Lp3Hs6Vc1Yj0GuE4tA9BnC"
   lin40 = "lin" + "_api_" + body
   lin32 = "lin" + "_api_" + body[:32]
   for label, k in [("lin_api_ (40 chars)", lin40), ("lin_api_ (32 chars)", lin32)]:
       print(label, "looks_fake:", repodx.looks_fake(k), "has_candidate:", repodx.has_candidate(k),
             "scan_line:", repodx.scan_line_for_secrets(f'const key = "{k}";'))
   PY
   ```
2. Control: the same line with an npm token, a provider RepoDx already supports:
   ```
   python3 - <<'PY'
   import repodx
   npm = "npm_" + "A1b2C3d4E5f6G7h8I9j0K1l2M3n4O5p6Q7r8"
   print("npm_ (control) scan_line:", [h[:3] for h in repodx.scan_line_for_secrets(f'const key = "{npm}";')])
   PY
   ```
3. File level: two temporary folders outside the repo, one `config.js` each, then a full scan:
   ```
   DIR=$(mktemp -d) && mkdir "$DIR/linear" "$DIR/npm"
   python3 - "$DIR" <<'PY'
   import sys, pathlib
   d = pathlib.Path(sys.argv[1])
   (d/"linear"/"config.js").write_text('const LINEAR_KEY = "' + "lin" + "_api_" + "Q7zT4mWx9Rb2Kd5Nf8Lp3Hs6Vc1Yj0GuE4tA9BnC" + '";\n')
   (d/"npm"/"config.js").write_text('const NPM_TOKEN = "' + "npm_" + "A1b2C3d4E5f6G7h8I9j0K1l2M3n4O5p6Q7r8" + '";\n')
   PY
   python3 repodx.py "$DIR/linear"
   python3 repodx.py "$DIR/npm"
   ```
4. Test suite: on branch `fix/linear-api-key` in my fork (commit below), one entry is added,
   `"Linear API key": fake("lin", "_api_", "Q7zT4mWx9Rb2Kd5Nf8Lp3Hs6Vc1Yj0GuE4tA9BnC")`, to
   `SecretScanTests.test_detects_provider_keys`. Run `python3 -m unittest discover`.
5. Does a regex alone fix it? Simulate a fix that adds only the pattern (in memory, no file
   edited) and scan a file that also reads an environment variable:
   ```
   DIR2=$(mktemp -d)
   python3 - "$DIR2" <<'PY'
   import sys, re, pathlib, repodx
   d = pathlib.Path(sys.argv[1])
   k = "lin" + "_api_" + "Q7zT4mWx9Rb2Kd5Nf8Lp3Hs6Vc1Yj0GuE4tA9BnC"
   (d/"config.js").write_text('const a = process.env.FOO;\nconst LINEAR_KEY = "' + k + '";\n')
   repodx.COMPILED_SECRET_PATTERNS.append(("Linear API key", re.compile(r"\blin_api_[A-Za-z0-9]{40}\b")))
   print("regex-only, scan_line:", [h[:3] for h in repodx.scan_line_for_secrets(f'const key = "{k}";')])
   print("regex-only, build_report critical:", [f["title"] for f in repodx.build_report(d)["findings"] if f["severity"] == "critical"])
   PY
   ```

**Expected:** a `lin_api_` key is reported as a critical secret, like other provider keys.
**Observed:** it is not reported anywhere.

**Reproduction Evidence**

- **Commit showing reproduction:** https://github.com/akgupta2002D/repodx/commit/bcac4462efe58588ed5c0695ced18eed5da55e4e
  (adds only the failing test case; no fix yet)
- **Output** (each line from the step in brackets):
  ```
  [1] lin_api_ (40 chars) looks_fake: False has_candidate: False scan_line: []
  [1] lin_api_ (32 chars) looks_fake: False has_candidate: False scan_line: []
  [2] npm_ (control) scan_line: [('secret', 'critical', 'npm access token')]

  [3] repodx.py $DIR/linear  ->  Score: 76/100 (C)
                                 Found: 0 critical, 3 warnings, 0 info
                                 (warnings: missing .gitignore, README, LICENSE)
  [3] repodx.py $DIR/npm     ->  Score: 51/100 (D)
                                 Found: 1 critical, 3 warnings, 0 info
                                 [CRITICAL] npm access token

  [4] FAIL: test_detects_provider_keys (...) (name='Linear API key')
      - []
      + [('secret', 'critical', 'Linear API key')]
      Ran 115 tests ... FAILED (failures=1)

  [5] regex-only, scan_line: [('secret', 'critical', 'Linear API key')]
  [5] regex-only, build_report critical: []
  ```
- **My findings:** the issue is real on current `main`, and there are two gaps that both
  need fixing. First, there is no Linear entry in `SECRET_PATTERNS`, so
  `scan_line_for_secrets` returns nothing (steps 1 and 4). Second, `lin_api_` is not in
  `CANDIDATE_LITERALS`, so `has_candidate()` is `False`. `check_file_contents` gates on it
  twice: once per file (`repodx.py:789`) and again per line (`repodx.py:797`) before it
  calls `scan_line_for_secrets`. So the line is skipped even in a file that reads
  environment variables. Step 5 shows it: with only the regex added, the line-level scan
  finds the key, but a full scan of a file containing `process.env.FOO` and the key still
  reports no critical finding. Adding only the regex would pass the unit test but miss
  keys in real scans, so my test plan needs a file-level check too.
  Open question for the fix: the exact length to match (40 per the issue, 32 in one
  third-party example, no official spec).

**Toolkit**

- Tool: repro-check, with the wild scope for real repos and my voice guide. I used it to
  grade my claim alone before posting, then my claim and repro report together.
- What it produced: the claim passed on the first run. The full package was rejected on
  the first run (analysis-honesty and steps-followable failed) and accepted 6/6 on the
  second.
- Difficulties: it caught a real error of mine. I claimed a file that reads environment
  variables would still get its lines scanned, but repodx.py:797 re-checks every line,
  so that was wrong, and it broke my own voice rule against claiming confidence I hadn't
  earned. It also flagged output lines (the npm control, the 32-character key) that no
  written step produced. It couldn't run the repo's code itself, so it graded my pasted
  output against the code.
- What changed: I removed the wrong claim and replaced it with a step that demonstrates
  the two checks (a regex-only fix still misses the key in a full scan), which turned a
  mistake into a stronger finding. I also wrote every command out in full and tagged
  each output line with its step.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
