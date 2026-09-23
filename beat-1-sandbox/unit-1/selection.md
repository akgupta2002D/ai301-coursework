# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/65

**Verdict output**

ankitgupta@Ankits-MacBook-Pro-8 ai301-coursework % cat /tmp/verdict-65.txt
**Live mode**, one candidate, in scope (`codepath/pathreview-ai301-fa26-s1`). Graded against today, 2026-09-22.

- **maintainer-alive** — pass. Maintainer `Aburke225` pushed to `main` on 2026-09-16 (6 days ago); repo activity as recent as 2026-09-21. Well inside 90 days.
- **repo-in-use** — pass. Most recent human, non-bot commit is 2026-09-16 (6 days). No releases exist, but the commit branch of the condition is satisfied.
- **scope-fits** — pass. One cohesive bug in one file: rework the mock session in `tests/unit/test_review_service.py` so `result.scalars()` returns a `MagicMock` rather than a coroutine. Reproduction command given, expectedoutcome stated (13 failed / 6 passed). No comments at all, so no maintainer has flagged design discussion or blockage. Labels `bug`, `tests`, `tier-1 (Starter difficulty)` are consistent but weren't relied on.
- **unclaimed** — pass. No assignees, zero comments, timeline shows only three label events by the author. The repo's only PR is #74 (for issue #60).
- **ai-policy-allows** — pass. `docs/CONTRIBUTING.md` and the PR template state no AI restriction; silence passes.
- **no-failed-attempts** — pass. Zero linked PRs, zero closed-unmerged attempts, zero claims.

Every required check passes → **accept**. No preferred checks in your rubric, so nothing further to rank on; onfit it's a good match anyway — bounded Python/pytest/async backend work, exactly the "bounded backend bug" profile you described, with no build-tooling or design decisions involved.

One practical note that doesn't touch any check: the 13 failing tests are already carrying `@pytest.mark.xfail(strict=True, ...)` markers, and `docs/CONTRIBUTING.md` says removing those markers is part of fixing a seeded bug — strict xfail means CI goes red if you fix the mocks but leave the markers in. Also expect your first PR to sit at "waiting for approval to run workflows" until a maintainer releases CI.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/65",
  "checks": [
    {"name": "maintainer-alive", "grade": "pass",
     "evidence": "Maintainer Aburke225 committed to main on 2026-09-16, 6 days before the 2026-09-22 grading date"},
    {"name": "repo-in-use", "grade": "pass",
     "evidence": "Most recent human non-bot commit to main is 2026-09-16 (6 days old); repo not archived"},
    {"name": "scope-fits", "grade": "pass",
     "evidence": "Single bounded fix in one file: 'Rework the mock setup (e.g. AsyncMock for execute, MagicMockfor the result object)' with a repro command; no maintainer comment blocking it"},
    {"name": "unclaimed", "grade": "pass",
     "evidence": "assignees: [], zero comments, timeline has only 3 label events; repo's sole PR (#74) targets issue #60"},
    {"name": "ai-policy-allows", "grade": "pass",
     "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md contain no AI prohibition or disclosure requirement"},
    {"name": "no-failed-attempts", "grade": "pass",
     "evidence": "No linked PRs open or closed against issue #65, and no claim/abandon history in the empty thread"}
  ],
  "verdict": "accept"
}

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```
paste the output here, including the closing JSON block
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

Run 1 (fresh rubric, before scope-fits fix): 16/20 agreement — categories: claimed 4/4, clear-accept 5/8, dead-repo 3/3, policy 1/1, scope 3/4.
Run 2 (targeted, --only issue-01,04,19,15, after first scope-fits tighten): 2/4 agreement.
Run 3 (targeted, --only issue-01,04,19,15, after second scope-fits tighten + no-failed-attempts added): 4/4 agreement.
Run 4 (full run): 18/20 agreement — categories: claimed 4/4, clear-accept 7/8, dead-repo 3/3, policy 1/1, scope 3/4.
Run 5 (full run, stability re-check): 18/20 agreement — identical verdicts to Run 4.
Run 6 (full run, --save-run eval-run.txt): agreement: 18/20 scored items (bar: 18/20: PASS) — matches the committed eval-run.txt.

**Issue analysis**

issue-15 (zulip/zulip#19589). Gold label: reject. My rubric's first version graded it accept — every original check passed (maintainer-alive, repo-in-use, unclaimed, ai-policy-allows, scope-fits all cleared, since the last claim was well outside the 30-day window and no PR was currently open). The reasoning my rubric missed: the issue had 97 comments spanning four years, with at least seven different contributors claiming and then abandoning it, and two linked PRs opened and closed without merging. My original unclaimed check only looked at whether a PR was currently open, not whether multiple PRs had already failed to land — so it couldn't see that this issue is a graveyard, not a simple "available" issue.


**Check rationale**

no-failed-attempts | Repo-facts block (linked PRs and their status, including closed ones) and the comment thread (claim/abandon history) | Fewer than 2 linked PRs have been opened and closed without merging, and the issue has not been claimed and abandoned by 3 or more different people | required
I added this check because my original unclaimed check only evaluated present-tense claim status and had no way to detect a history of repeated failed attempts. issue-15 showed this gap directly: it passed every original check but was actually a long-running graveyard issue that multiple contributors had claimed and dropped.

**Trade-offs**

This check uses a coarse threshold (2 closed PRs / 3 abandoners) rather than judging why past attempts failed — an issue with two closed PRs due to unrelated maintainer preference changes would still fail this check even if the issue itself is easy. I accepted this trade-off because distinguishing "closed for being hard" from "closed for being unrelated" would need evidence sources I don't reliably have. My rubric still misses issue-20 (a false accept, gold=reject) even after this change, and issue-04 flipped between accept and reject across two full runs before settling on reject — evidence that scope-fits's line between "one cohesive change" and "too sprawling" is still sensitive to how the model reads borderline cases.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
   Good. I am interested in developing my evaluation and testing skills. And I should have time to work on it, around 4-6 hours on week 2.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
   I am new to this so verdict seemed to notice most things I was. I weighed on what I felt most interested and also what skills I have which the verdict couldn't fully capture.
3. The anticipated difficulty in claiming it.]
   Medium to Hard.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
