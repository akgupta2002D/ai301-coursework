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
**Pull Request:** https://github.com/omerbek/repodx/pull/84

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

**Unit 7: Installed the ship-gate per project, wrote a two-check manifest, and fixed my own pr-precheck rubric**

I installed the ship-gate inside my repodx fork's `.claude/` rather than `~/.claude/`, and
wrote a manifest of the two checks CONTRIBUTING.md names: `python3 -m unittest discover`
and `python3 repodx.py . --fail-on info`, which enforces "must keep scoring 100". On my
real branch both checks passed, but my own pr-precheck rejected it because its rubric
demanded an issue number in the branch name, a course default repodx never asks for, and
it only recognized `Closes #` and `docs/CONTRIBUTING.md`. I changed that rubric row and
procedure to follow the repo's stated rules, and the gate then passed and let PR #84
open.

**Unit 8: Installed the PR monitor in my fork, wrote a playbook from repodx's real review times, and caught its misread of my close**

I installed the PR monitor in my repodx fork's `.claude/` and wrote my playbook from
data: September's five outside PRs all got a first response within 11 hours, but the
repo had gone quiet since, so I set the first nudge at 7 quiet days. The first run
drafted a wait note, which I kept off the thread, and flagged that CI hadn't reported; I
confirmed the workflow was waiting on maintainer approval and rewrote my nudge rule to
say "tests pass locally". When the maintainer then merged my commits through his own PR
#85 and closed #84, the monitor saw only "closed, 0 comments" and drafted a question
asking whether it was a no; I checked #85's diff, discarded that draft, wrote a short
thank-you instead, and added a playbook rule to check the timeline for "closed this in
#N" before treating a close as a rejection.

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

RepoDx is meant to stop people, often people building with AI tools, from pushing live
API keys. It recognizes keys from about 25 providers, but not Linear. A Linear personal
API key (`lin_api_` followed by 40 letters and digits) can sit in a file and RepoDx will
report the repo as safe to push.

**Expected Behavior**

A `lin_api_` key in a scanned file is reported as a critical secret named "Linear API
key", with its file and line, exactly like the other provider keys (one level less
severe in test or example folders, as for every other secret).

**Current Behavior**

Nothing is reported. `scan_line_for_secrets` returns `[]` for a line holding a Linear key,
and a full scan of a folder whose only file holds one finds 0 critical issues, while the
same setup with an npm token is flagged (see Reproduction Evidence).

**Affected Components**

- `repodx.py` `SECRET_PATTERNS`: the provider regex list; it has no Linear entry.
- `repodx.py` `CANDIDATE_LITERALS` and `has_candidate()`: the fast pre-filter; it has no
  `lin_api_`, so `check_file_contents` (lines 789 and 797) never hands a Linear line to
  the scanner.
- `tests/test_repodx.py` `SecretScanTests`: where provider detection is tested.

**Toolkit**

- Tool: my repo memory file (`CLAUDE.local.md` in my repodx fork), written from one
  scoped read of the secret scanner and its tests, plus Claude Code to look up the key
  format.
- What it produced: the map of the four pieces above (pattern list, pre-filter, scanner,
  test), and the key format. Linear doesn't publish a length; the issue says 40, and
  gitleaks' default rule is `lin_api_(?i)[a-z0-9]{40}`, so I used 40.
- Difficulties: the issue only mentions the pattern and the literal as steps to follow;
  why the literal matters (the double `has_candidate` gate) only showed up when I read
  `check_file_contents` and then tested it in my reproduction.
- What changed: I added the pre-filter to Affected Components and to my test plan, so
  the fix is checked with a full file scan and not only one line.

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

Detection happens in `check_file_contents` in `repodx.py`. For each file it first asks
`has_candidate(text)` (line 789), which checks for any string in `CANDIDATE_LITERALS`;
for each line it asks again with `has_candidate(line)` (line 797), and only then calls
`scan_line_for_secrets`, which runs every regex in `COMPILED_SECRET_PATTERNS` (built from
`SECRET_PATTERNS`) and drops matches that `looks_fake()`. Linear keys fail at both
layers: `lin_api_` is not in `CANDIDATE_LITERALS`, so the line never reaches the scanner,
and there is no Linear regex in `SECRET_PATTERNS`, so the scanner wouldn't match it
anyway. My reproduction's step 5 shows that fixing only the regex still misses the key in
a real file scan.

**Proposed Solution**

Add one entry, `("Linear API key", r"\blin_api_[A-Za-z0-9]{40}\b")`, to `SECRET_PATTERNS`,
and add `"lin_api_"` to `CANDIDATE_LITERALS`. Length 40 follows the issue and gitleaks'
default rule, `lin_api_(?i)[a-z0-9]{40}` (read from `config/gitleaks.toml` in the gitleaks
repo on 2026-11-22). Risk I'm accepting: Linear doesn't document the length, and one
third-party page (SailPoint's secret catalogue) shows a 32-character example. With
`{40}` and `\b`, a 32-character key would not be reported. I'm keeping exact 40 because
a range like `{32,}` would also match longer non-key strings and raise false alarms,
which CONTRIBUTING.md asks us to avoid; I'll name this in the PR so the maintainer can
choose differently. Severity stays critical, like every provider key, and test or example
folders still downgrade it through the existing `TEST_PATH_DOWNGRADE`.

What I am deliberately not changing: `looks_fake`, severities, `FIXES` (the existing
"secret" fix text already applies), the scanning loop, README and `docs/README.tr.md`
provider lists, versions, and CHANGELOG. Past provider PRs (#43 Perplexity, #47 Replicate,
#60 DigitalOcean; checked with `gh pr view`) changed only `repodx.py` and the tests, and releases are the maintainer's job. I'm also not detecting
Linear OAuth tokens (`lin_oauth_`), which the issue doesn't ask for.

**Implementation Plan**

1. `repodx.py`: add the Linear tuple to `SECRET_PATTERNS` and `"lin_api_"` to
   `CANDIDATE_LITERALS`.
2. `tests/test_repodx.py`: keep the Linear entry already committed in
   `test_detects_provider_keys` (it fails today, commit bcac446), and add one file-level
   test: a temporary repo with `src/config.js` holding a fake key built with `fake(...)`
   must give exactly one critical "Linear API key" finding at the right line. Add one
   boundary check: 39 characters after the prefix is not reported. That one passes
   today too; it guards against false alarms, it isn't proof of the fix.
3. Run `python3 -m unittest discover` and `python3 repodx.py . --fail-on info` (must
   stay 100), re-run my reproduction steps 1, 3 and 5 for before/after output, and run
   RepoDx on a few real repositories to check for false alarms, as CONTRIBUTING.md asks.

Deviations: none yet.

**Toolkit**

- Tool: plan-check, with the wild scope, on `plan.md` (this section) and a draft plan
  comment I kept off the thread. I ran it twice, headless, from my repodx fork.
- What it produced: a provisional reject on the first run, then accept with all five
  required checks passing on the second. It confirmed the two `has_candidate` gates at
  lines 789 and 797 against the code and matched the plan's cause to my repro steps 1,
  4 and 5.
- Difficulties: the first run couldn't read the issue thread (the headless session
  blocked `gh`), so two checks came out unclear, which my verdict rule counts as fail.
  It also expected a posted repro comment, but the course keeps the repro off the
  thread. Its notes showed my plan had dropped the 32-character example from my repro,
  and that two claims (the gitleaks rule, the past-PR precedent) had no evidence in the
  package. It also flagged gaps in my own skill: an empty `references/` folder and a
  procedure that never says how to map a live `plan.md` onto its eval-bundle sections.
- What changed: I gave it a local snapshot of the thread and my repro report, and
  pointed it at them. I added the 32-character risk and why I keep exact 40, named
  where the gitleaks rule and PR precedent came from, and reworded the 39-character test
  as a false-alarm guard rather than proof. The skill gaps are logged for later; I didn't
  patch the skill mid-build.

---

## Testing Strategy

Four observable checks, all run on branch `fix/linear-api-key` (79b236b) against `main`
(e845025):

1. **My reproduction, before and after.** Same steps as Reproduction Process. Before:
   `[1] lin_api_ (40 chars) has_candidate: False scan_line: []` and a full scan of a file
   that reads `process.env.FOO` and holds the key gives `Found: 0 critical`. After:
   `scan_line: [('secret', 'critical', 'Linear API key')]` and the full scan gives
   `Found: 1 critical` with `[CRITICAL] Linear API key - config.js:2 (lin_ap...nC)`, the
   value masked. The npm control is flagged both times. A 32-character key is still not
   reported, by design (exact 40, the risk named in my plan).
2. **New tests fail without the fix and pass with it.** With `repodx.py` reverted to
   main: `FAILED (failures=2)` (`test_detects_linear_api_key_in_a_file` and the Linear
   case of `test_detects_provider_keys`). With only the regex added and no `lin_api_`
   in `CANDIDATE_LITERALS`: `FAILED (failures=1)`, the file-level test, which is what
   proves the second half of the fix is needed. `test_linear_api_key_needs_forty_characters`
   passes before and after; it guards against false alarms and isn't proof of the fix.
3. **The repo's own checks** (my ship manifest, from CONTRIBUTING.md and CI):
   `python3 -m unittest discover` gives `Ran 117 tests ... OK` on Python 3.13.2 and on
   system Python 3.9.6 (the oldest version CI tests), and `python3 repodx.py . --fail-on
   info` gives `RepoDx: 100/100 (A), 0 critical, 0 warnings, 0 info`, exit 0.
4. **No false alarms on real repositories**, as CONTRIBUTING.md asks. Main's `repodx.py`
   and the branch give identical scores and finding counts on four real local repos
   (ai301-coursework 78/5, pathreview-ai301-fa26-s1 92/1, ai301-unit6-starter 78/5,
   ai301-unit7-starter 78/5), with 0 Linear findings.

**Toolkit**

- Tool: Claude Code, re-running my reproduction script against a `git worktree` of main
  and against the branch, plus my ship manifest's two checks, saved to `test_evidence.md`.
- What it produced: the before/after output above, the two "without the fix" runs, the
  manifest checks on two Python versions, and the four-repo false-alarm table.
- Difficulties: the plain "tests pass" result couldn't show the `CANDIDATE_LITERALS`
  half of the fix, because the issue's own test calls `scan_line_for_secrets` directly
  and skips the pre-filter. And `repodx.py .` scans my untracked local files too
  (`.claude/`, my plan and evidence files), so a pasted key there would fail the
  self-scan.
- What changed: I added a regex-only run to show the file-level test is the one that
  catches a half fix, and I kept every key in my notes split across strings or masked so
  the self-scan stays at 100.

---

## Implementation Notes

**2026-11-22**

Claimed the issue, reproduced it, and committed a failing test on `fix/linear-api-key`
(bcac446): the issue's own entry in `test_detects_provider_keys`. Reading
`check_file_contents` while writing my repo memory file showed the issue has two gaps,
not one: the regex is missing, and `lin_api_` is missing from `CANDIDATE_LITERALS`, so
the line is filtered out at `repodx.py:789` and again at `:797`. I first wrote that an
env-reading file would still get scanned; repro-check caught that the per-line check
at 797 blocks it, and step 5 of my reproduction now proves it.

**2026-11-23**

Decided the key length. Linear doesn't document it; the issue says about 40, one
third-party page shows 32, and gitleaks' default rule (read from its `gitleaks.toml`) is
exactly 40. I chose exact 40 with `\b`, like the neighboring patterns, because a range
like `{32,}` would also match longer non-key strings, and named the 32 risk in the plan.
Checked how past provider PRs were shaped (#43, #47, #60 with `gh pr view`): only
`repodx.py` plus tests, no README or CHANGELOG, so I left those alone. Then built it:
2c4353c adds a file-level test (`src/config.js`, one critical "Linear API key" at line 2)
and a 39-character false-alarm guard; 79b236b adds the pattern after Replicate and
`"lin_api_"` to `CANDIDATE_LITERALS`. Wrong turn avoided: the issue's step-3 test alone
passes with just the regex, so without the file-level test a half fix would have looked
done. No deviations from the plan.

**2026-11-23 (later)**

Wrote the PR description and ran the ship-gate: both repo checks passed, but my
pr-precheck rejected the branch name (no issue number), a rule repodx doesn't set. I fixed
my rubric instead of renaming the branch, the gate passed, and I opened PR #84 against
upstream `main`. Then I installed the PR monitor and wrote my playbook; its first run
drafted a wait note (no nudge before 2026-11-30) and pointed out that CI hadn't run. The
`tests` workflow is at `action_required`, waiting on maintainer approval, so my PR's
"checks" are local runs until then.

**Toolkit**

- Tool: Claude Code in my repodx fork, with `CLAUDE.local.md` loaded for the scanner map
  and conventions; `gh pr view` and `gh api` for precedent and the gitleaks rule.
- What it produced: three commits on `fix/linear-api-key`, each saying what it does, and
  a small diff (21 insertions, 1 deletion: 3 lines changed in `repodx.py`, 19 added in tests).
- Difficulties: no official key spec, so the length was a judgment call; and my own
  first explanation of the env-var path was wrong until a tool checked it against the
  code.
- What changed: I wrote the length decision and its risk into the plan and PR instead of
  guessing silently, and I test the claim I got wrong (a regex-only build) instead of
  just deleting it.

---

## Pull Request

**PR Link:** https://github.com/omerbek/repodx/pull/84

**PR Description:** Title "Detect Linear API keys", body opening with `Fixes #63`. It
explains the two gaps and fixes both: the Linear pattern
`("Linear API key", r"\blin_api_[A-Za-z0-9]{40}\b")` in `SECRET_PATTERNS`, and `"lin_api_"`
in `CANDIDATE_LITERALS`, without which `check_file_contents` drops the line at lines 789
and 797. It lists the three tests (the issue's provider entry, a file-level test that
fails if only the regex is added, and a 39-character false-alarm guard), what it does not
change (severities, `looks_fake`, `FIXES`, README lists, versions, CHANGELOG, following
#43, #47 and #60), and the length question: exact 40 per the issue and gitleaks' default
rule, with the 32-character example named and a range offered if the maintainer prefers.
It includes before/after output (main: `scan_line: []`, `Found: 0 critical`; branch: one
critical "Linear API key" at `config.js:2`, masked), the failing-without-fix runs, the
checks (117 tests OK on Python 3.13.2 and 3.9.6, self-scan 100/100), the four-repo
false-alarm check, and an AI-use section in my own words.

**Maintainer Feedback:**

- 2026-11-23: about 20 minutes after I opened it, the maintainer (omerbek) applied my
  commits in his own PR, #85 "Detect Linear API keys (from #84 by akgupta2002D)", which
  merged and closed #84 and issue #63 (closed as completed). The merged change is mine
  unchanged: the exact-40 pattern, `lin_api_` in `CANDIDATE_LITERALS`, and all three
  tests. He only rebased it onto the Sentry detector from #83, which touched the same
  `CANDIDATE_LITERALS` line. #85's description credits "the detector and regression
  tests from #84 by @akgupta2002D". No review comments; the length question needed no
  change.
- 2026-11-23: I read #85's diff to confirm what landed, set aside the PR monitor's draft
  (it misread the close as unexplained), and drafted a one-line thank-you naming #85 and
  the conflict he resolved.

**Status:** Closed. The change was merged upstream in #85, which credits this PR and closed it.

**Toolkit**

- Tool: the ship-gate, installed per project in my fork's `.claude/ship-gate/` with its
  hook in the fork's `.claude/settings.json`, my ship manifest (`python3 -m unittest
  discover` and `python3 repodx.py . --fail-on info`), and my pr-precheck from unit 4.
  I ran it by hand on the branch before opening the PR, then opened the PR with
  `gh pr create`.
- What it produced: the hook test stopped `gh pr create --help` twice (voice guard and
  ship-gate). The first by-hand run passed both repo checks but my pr-precheck rejected
  the branch on "Repo's other stated asks are followed": branch `fix/linear-api-key` has
  no issue number. After my rubric fix it passed all checks, and passed again after I
  edited the AI-use section of the description.
- Difficulties: that reject came from my own rubric, not the repo. It hard-coded the
  course's branch format (type prefix, issue number, description) as if every repo asked
  for it, only accepted `Closes #`, and only looked for `docs/CONTRIBUTING.md`; repodx
  states no branch rule, asks for `Fixes #`, and keeps CONTRIBUTING.md at the root. The
  installer also assumes everything lives in `~/.claude/`, which I don't use.
- What changed: I fixed the rubric row and procedure in my pr-precheck to follow the
  repo's stated rule and accept `Fixes #`/`Resolves #` and any CONTRIBUTING.md location,
  instead of renaming my branch to satisfy a rule the repo never set. I installed the gate
  by hand into the fork's `.claude/`, which works because it finds its files relative to
  its own folder.

---

## Learnings and Reflections

**Technical Skills Gained**

- Reading a scanner's pipeline end to end: in RepoDx a secret has to pass a cheap
  substring pre-filter (`CANDIDATE_LITERALS` / `has_candidate`, checked per file and per
  line) before any regex runs, so a new detector is two changes, not one.
- Writing tests that prove the right thing: a line-level test can pass while the real
  feature is broken, so I added a file-level test through `build_report` and showed it
  fails on a regex-only build. And a guard test (39 characters) that passes before and
  after is a false-alarm check, not proof of a fix.
- Keeping secrets out of a secret scanner's own repo: fake keys built at runtime with
  `fake(...)`, masked output only, and checking the repo still scores 100/100.
- Matching a project's conventions from evidence: `gh pr view` on past provider PRs
  (#43, #47, #60) to see the expected shape, the gitleaks rule for an undocumented key
  length, and CI's Python matrix (3.9 and 3.13) to test on both.
- Git workflow for a fork: a feature branch with commits that each say what they do,
  `git worktree` to re-run my repro on main, `.git/info/exclude` for local files, and
  `gh repo set-default` so the PR targets upstream.

**Challenges Overcome**

- My scout's first picks fell through: two were fixed by others within days and the third
  was a weak fit. A fresh scan after the scope patch found repodx#63.
- I stated a code claim I hadn't checked (that an env-reading file would still get its
  lines scanned). repro-check caught it, and I turned it into reproduction step 5, which
  became the strongest evidence that both gaps need fixing.
- No official Linear key length: I documented the evidence (issue says 40, gitleaks
  uses exactly 40, one third-party example has 32), picked 40, and put the risk and a
  one-character alternative in the PR instead of guessing silently.
- My own pr-precheck blocked the PR over a branch-name rule repodx never set. I fixed the
  rubric to follow the repo's stated rules rather than bend my branch to my tool.
- The course tools assume everything lives in `~/.claude/`. I kept every tool scoped to
  the folder that uses it, which worked because each one finds its files relative to
  itself.
- Headless `claude -p` runs can't approve `gh` calls, so my scout and plan-check couldn't
  read GitHub. I re-ran interactively or gave the tool a local snapshot of the thread.

**What I'd Do Differently Next Time**

- Check that a picked issue is still open, and re-check right before claiming. Two of my
  original picks closed in under a week; my rubric now has that gap logged.
- Read the code path before writing any claim about it, not after a tool flags it.
- Not trust a single signal about a maintainer's pace: repodx looked quiet for two months,
  then merged my fix within half an hour of my opening the PR.
- Make my tools' assumptions explicit earlier (fixed dates in my search strategy, course
  defaults in my rubric), since each one cost me a failed run before I saw it.

---

## Resources Used

- omerbek/repodx CONTRIBUTING.md (claiming, `fake(...)`, the two test commands, the
  false-alarm step): https://github.com/omerbek/repodx/blob/main/CONTRIBUTING.md
- The issue itself, whose "How to do it" steps set the shape of the fix:
  https://github.com/omerbek/repodx/issues/63
- gitleaks' default rule for Linear keys, `lin_api_(?i)[a-z0-9]{40}`, which settled the
  length: https://github.com/gitleaks/gitleaks/blob/master/config/gitleaks.toml
- SailPoint's Linear API key entry, the 32-character example behind the length risk:
  https://documentation.sailpoint.com/entro/help/supported-secrets/specific-secrets/linear-api-key.html
- Past provider PRs #43, #47 and #60, which showed a fix touches only `repodx.py` and the
  tests: https://github.com/omerbek/repodx/pull/60
- repodx's CI workflow, for the Python versions to test (3.9 and 3.13):
  https://github.com/omerbek/repodx/blob/main/.github/workflows/tests.yml
