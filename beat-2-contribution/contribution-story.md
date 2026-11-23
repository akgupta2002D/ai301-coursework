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

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

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

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

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
