# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-matches-repro | The "Cause:" paragraph under Candidate plan, compared with the Repro evidence section (its steps, Expected, and Actual lines) | P if the cause names a specific file, function, or behavior AND the Repro evidence directly shows it, including any workaround the steps reveal. F if the cause is missing, contradicts the Repro evidence, points somewhere the evidence does not, or no repro evidence is quoted. ? if the cause is plausible but the evidence neither supports nor contradicts it. | required |
| scope-has-limits | The "Change:" paragraph under Candidate plan (its In and Out parts) | P if it names each file or function it will change, including where the Repro evidence shows the bug lives, AND states at least one thing it will not change. If the issue names no file, naming the module or function from the Repro evidence counts. F if the change is unbounded, names no place, or states no exclusion. | required |
| executable-without-asking | The "Change:" paragraph under Candidate plan | P if a stranger could start the work without asking the author, because the plan names the code area, module, or path to change AND the action to take. The exact function is not required if the plan says how it will be pinned down (for example, tracing with debug logs). F if it only states the goal, names no place, or leaves the action undefined. | required |
| test-shows-fix-worked | The "Test:" paragraph under Candidate plan, compared with the numbered steps in Repro evidence | P if it re-runs the repro steps (or an equivalent check) and states the result that must now be true, so the check would fail before the fix and pass after. A manual check passes if it meets this. F if there is no check, no expected result, or a check that would also pass before the fix. | required |
| fits-thread-and-conventions | The Candidate plan comment, compared with Thread highlights and the Repo facts section (contribution policy, bug-report template, any AI disclosure requirement) | P if the comment does not duplicate or contradict anything in the thread (linked PRs, other plans, maintainer direction) and follows every convention Repo facts states. If Repo facts states an AI disclosure requirement, the comment itself must disclose AI use (tool and extent); a comment with no disclosure is F. If Repo facts states no AI policy, no disclosure is needed. F also if it ignores an existing fix or maintainer direction, violates another stated convention, or guarantees a deadline or tells others to stay off the issue. Saying it will take the issue or will report back is not a guarantee or a reservation. | required |
| states-its-unknowns | The Candidate plan and its comment | P if it names something it does not know or could break, or the plan claims no more than the Repro evidence shows. F only if it presents an unconfirmed cause as established. | preferred |

## Verdict rule

The verdict is ready only if every required check is P. Any required F means hold. A ? on a required check counts as F, so the verdict is hold. Preferred checks never change the verdict, but list their grades in the feedback.
