---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about exactly one PR package: **is this ready to
submit?**

A PR package is a candidate pull request: its title, description, commits,
diff, and test evidence. Read it against the plan it claims to implement and
the issue that plan belongs to. Grade one package per run. Never answer a
different question, such as whether the code is good or whether the plan was
a good plan. Never answer from gut feel: execute `rubric.md`, following
`procedure.md`, and find evidence with `references/evidence-guide.md`.

## Inputs and modes

Run in exactly one mode. Decide the mode from the request. If it is unclear
which mode applies, say so and stop without a verdict.

**Live mode** checks the student's own submission before it goes out. Read:
- `plan.md`, including its test plan and its `## Deviations` section.
- The diff of the branch: every committed change relative to the default
  branch. Produce it with `git diff main...HEAD` (three dots) from the
  working copy. Uncommitted changes are not part of the diff.
- The draft PR title and description in `pr_draft.md` (title on the first
  line, then the filled template).
- The test evidence in `test_evidence.md`.
- The student's issue, from the URL given in the request. Gather issue-side
  evidence from the real repo: the issue thread, the PR template
  (`.github/PULL_REQUEST_TEMPLATE.md`), and the stated policy in
  `docs/CONTRIBUTING.md`.
- A house-chain student reads the house plan and the house repro pack
  instead of their own plan and repro. The same checks grade the same things.

**Eval mode** grades one frozen practice submission (a package) from the
harness.
- The package bundle is the whole world. Every fact comes from the bundle
  text.
- Fetch nothing. Read nothing else, including gold labels and other packages.
- Always grade the complete package: every check, with the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else.
- Take from it the repo the PR must live in and the house rules of that
  environment.
- Refuse to grade a PR that targets any other repo. Say which repo the PR
  targets and which one the scope names.
- If the `Repo:` line in `scope.md` still holds an unfilled placeholder, stop
  without grading. Tell the student to fill in the `Repo:` line in `scope.md`
  with their section's Path Review repo. Never guess a scope.
- In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after the checks, read `voice-guide.md`.
- Hold the outgoing PR text, the title and the description, against each rule
  in it.
- List every rule the draft breaks in the readable summary, quoting the
  offending text.
- Never let a broken voice rule change the verdict on its own. It affects the
  verdict only if a check in `rubric.md` reads it.
- In eval mode, ignore `voice-guide.md` entirely.

## Component reads

The pieces connect like this:
- `rubric.md` defines the checks and the verdict rule. It decides what passes.
- `references/evidence-guide.md` maps where each kind of evidence lives, in an
  eval package and in a live PR. Use it to locate evidence, not to judge it.
- `procedure.md` says how to run the checks. Execute it as written, step by
  step.

If the procedure is silent on a step, report the gap in the summary. Never
improvise around it or invent a step.

If `rubric.md` or `procedure.md` has no written content (it is still a
template), refuse to grade. Say which file is empty, emit no verdict, and
emit no JSON block. Never invent checks at runtime.

If a required input is missing or empty (plan, diff, description, or test
evidence), refuse to grade and name which one.

## Verdict and output

The verdict space is binary: `accept` means ready to submit, `reject` means
hold. There is no third verdict, no "accept with reservations", and no score.
Put reservations in the evidence lines of the checks.

Reply in this order:
1. A short, readable per-check summary: check name, grade, and the one-line
   evidence. In live mode, add the voice-guide findings.
2. The fenced JSON block below, filled in: one entry per check with its grade
   and one-line evidence, then the verdict.

The JSON block must be valid and must be the last thing in the reply. Nothing
follows it. Never change, extend, or reorder the schema.

```json
{
"item": "<PR URL or bundle id>",
"checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
"evidence": "<one line: the fact or quote that decided it>"}
  ],
"verdict": "accept|reject"
}
```

## Grading discipline

Grade under these standing rules:
- **Evidence first.** Grade no check without naming the fact or quote that
  decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** Read the artifact itself against the
  plan, the issue, and the stated standards. A terse complete PR can be ready.
  A beautiful, confident one can hide drift. Never grade formatting or tone
  unless a check says so.
- **The rubric decides, not the run.** If a check passes by its stated
  condition but feels wrong, it still passes. Never override the rubric in the
  run.
- **The procedure decides how, not the run.** Follow `procedure.md` as
  written and report its gaps.
- **Proof is a line in the diff or real command output.** A sentence saying
  something passed is not proof. A claim that something was not added holds if
  nothing in the diff adds it.
- **Unclear.** Grade `unclear` when a check cannot be verified from the
  package. Treat it as the verdict rule in `rubric.md` directs. Where the rule
  is silent, an unverifiable claim is a failing one, because a PR that cannot
  be verified is not ready to submit.