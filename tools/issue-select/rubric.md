# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks
# Rubric: is this a good first issue?

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | Repo-facts block (most recent merged PR date, most recent maintainer comment date) and the comment thread (dates of comments by maintainers or collaborators) | A maintainer merged a PR or posted a comment within 90 days of the snapshot date | required |
| repo-in-use | Repo-facts block (date of the most recent human, non-bot commit to the default branch, and date of the latest release) | The most recent human commit is within 90 days of the snapshot date, or the latest release is within 180 days of the snapshot date | required |
| scope-fits | Issue body and comment thread | The issue describes one cohesive change or a small set of closely related sub-tasks serving a single clear goal (one feature, one bug, one docs topic) — even if it touches multiple files or lists multiple similar items — and is not an open-ended or loosely-scoped collection of unrelated asks; and no maintainer comment says it needs design discussion, is blocked, is larger than described, or will not be accepted. Optional "additional suggestions" or "nice to have" extensions beyond the core ask do not fail this check on their own. A "good first issue" label alone is not evidence of fit. | required |
| unclaimed | Repo-facts block (assignees, linked PRs and their status) and the comment thread (claim comments such as "I'll take this" and their dates) | No assignee, no open linked PR, and no claim comment within 30 days of the snapshot date. A claim older than 30 days with no linked PR since counts as stale and does not block | required |
| ai-policy-allows | Repo-facts block (CONTRIBUTING, code of conduct, or AI-policy notes) and any maintainer statement about AI in the issue body or comment thread | Nothing in the bundle prohibits AI-assisted contributions to this repo or issue, and any disclosure requirement is one a contributor can follow. If the bundle mentions no AI policy at all, grade pass | required |
no-failed-attempts| Repo-facts block (linked PRs and their status, including closed ones) and the comment thread (claim/abandon history)	| Fewer than 2 linked PRs have been opened and closed without merging, and the issue has not been claimed and abandoned by 3 or more different people	| required



## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept only if every required check passes. Any fail on a required check means reject. Unclear on a required check counts as fail, except where a pass condition above says otherwise. There are no preferred checks, so fit (from scope.md) only ranks accepted issues.
