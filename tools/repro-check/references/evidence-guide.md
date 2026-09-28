# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

- **Where it lives:** In an eval bundle, the repro report's environment section (or equivalent front-matter) plus the repo-facts block, which states what the issue/repo actually targets (OS, language/runtime version, dependency versions, or a pinned container/image). In live mode, the student's draft repro report, checked against the repo's own setup docs (README, CONTRIBUTING, or a Dockerfile/lockfile in the repo).
- **What good looks like:** Every value the repo-facts block calls load-bearing (the runtime version, the dependency versions tied to the bug) is named explicitly in the report, or a deliberate difference is called out and justified (e.g. "reproduced on a newer patch version; behavior unchanged"). A report that omits a version the repo-facts block flags as relevant, or writes "latest"/"my machine" instead of a concrete value, is insufficient — someone else could not recreate the same runtime from it.

## Steps

- **Where it lives:** The repro report's steps section, read start to finish as written, against the starting state (a clean checkout, a specific commit/branch, any seed data) named in the same section or in the repo-facts block.
- **What good looks like:** A stranger starting from the stated starting state and running only the listed actions, in order, with no step skipped or assumed, reaches the reported failure. Any input, flag, file, or environment variable the steps depend on is stated inline, not left for the reader to infer from context or from familiarity with the repo.

## Behavior shown

- **Where it lives:** The pasted output excerpts, logs, or screenshots in the repro report, read against the issue's own description of the failure (error text, stack trace, wrong output, or symptom) in the issue context.
- **What good looks like:** The artifact shows the same failure the issue names — matching input path (input-fidelity) and matching failure type (failure-mode-match): a panic shown for an issue reporting a panic, the same wrong output shape, the same error class. An artifact that shows a different exception, a different code path, or a superficially similar but distinct symptom is evidence of an adjacent bug, not this one, even if it looks like a "failure."

## Honesty

- **Where it lives:** The repro report's stated conclusion (the sentence or paragraph asserting what was found), read directly against the output excerpt in "Behavior shown" that it cites as support.
- **What good looks like:** The conclusion claims exactly what the cited output demonstrates — no more. "I could not reproduce this after N attempts; here is what I ran and what happened instead" is a full pass when the excerpt backs it up, even though it doesn't confirm the bug. A conclusion that says "confirmed" or "reproduced" when the excerpt shows a different failure, a partial failure, or no failure at all is claiming more than its evidence shows.

## Comms

- **Where it lives:** The claim comment and repro comment as posted (or
    drafted, in live mode), read against: (a) the issue itself, for
    specificity, and (b) that specific package's stated repo policy —
    in an eval bundle, the repo-facts block, which may or may not name
    an AI-assistance disclosure requirement depending on the package; in
    live mode, this repo's docs/CONTRIBUTING.md, which states branch
    naming, commit format, and PR process rules but no comment-level
    disclosure requirement. Do not assume every repo requires AI
    disclosure — check what that specific package or repo actually
    states before failing a comment on that basis. Course-level rules in
    this course repo's own scope.md (e.g. "post your own claim even if
    claimed," "never piggyback") are a separate, procedural check: they
    govern whether you should post at all, not whether the package's
    wording passes, and are not eval-scored since scope.md is ignored in
    eval mode.
- **What good looks like:** The claim names the specific issue and
  states what will be investigated next, without asserting a fix or a
  date (a promise, not a completion claim). The repro comment states
  the outcome plainly and in the poster's own words, backed by their
  own evidence, rather than in boilerplate or a reference to someone
  else's proof ("steps 1-4 above reproduce the crash," never "same as
  above, can confirm"). Any policy that specific package or repo states
  as required — for example an AI-assistance disclosure named in an
  eval package's repo-facts block — is present in the comment text
  itself, not just true in spirit.