# Procedure: how this skill grades a plan package

## Read order

1. Read the issue text first. Note the symptom it reports and any file, function, or error message it names.
2. Read the Repro evidence section next, before the plan. Note what behavior it pins down: the exact failure or error text, and the file or function it points at. Reading it first means the plan's cause is judged against the evidence, not against how confident the plan sounds.
3. Read the plan: diagnosis, scope, files to touch, approach, test plan, and risks and unknowns. Note which of these sections exist.
4. Read the thread highlights and any linked pull requests or other plans, then read the Repo facts section, including its contribution policy line, bug-report template line, and any AI disclosure requirement.
5. Read the plan comment last, so it is compared against the plan and the thread.

## Evidence gathering

1. diagnosis-matches-repro: copy the plan's stated cause in one sentence. Copy the line from the Repro evidence section that is closest to it. Record whether both point at the same file, function, or behavior.
2. scope-has-limits: list every file the plan says it will change. List every exclusion it states. Record the file where the Repro evidence shows the bug lives.
3. test-shows-fix-worked: copy the plan's test steps and the result it expects after the fix. Record whether the same steps would fail before the fix and pass after.
4. fits-thread-and-conventions: list any existing fix, linked pull request, or plan in the thread highlights. List every convention stated in Repo facts (contribution policy, AI disclosure requirement, bug-report template). Record whether the plan comment contradicts or duplicates any of them. Record whether Repo facts states an AI disclosure requirement, and whether the plan comment contains a disclosure naming the tool and the extent of assistance.
5. risks-and-unknowns: copy the first risk or open question the plan names.
6. If a piece of evidence is not in the package, record "not found" for it. Do not infer it and do not use outside knowledge of the bug.

## Check execution

1. Run the checks in the order rubric.md lists them, using only the evidence recorded in the previous stage. A check may be graded from its recorded evidence without re-reading the whole package. Re-read only the section that evidence came from if it is unclear.
2. Grade P when the pass condition is fully met. Grade F when the evidence is "not found" or contradicts the pass condition. Grade ? when the evidence exists but you cannot tell whether it meets the pass condition.
3. For diagnosis-matches-repro, compare the cause against the Repro evidence, not against the plan's confidence. A cause the Repro evidence does not support is F, even if the plan states it clearly.
4. Write one line per check: the grade, and the evidence that decided it, quoted.

## Verdict assembly

1. List each required check with its grade.
2. Apply the verdict rule in rubric.md exactly. All required checks P means ready (accept). Any required F means hold (reject). A ? on a required check counts as F, so it also means hold.
3. List preferred-check grades as feedback only. They never change the verdict.
4. In the output, quote the evidence for the deciding check: the first required check that failed, or if none failed, the diagnosis-matches-repro evidence. Then print the grades with their one-line reasons, followed by the JSON verdict block the skill specifies.