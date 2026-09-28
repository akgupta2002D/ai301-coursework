# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| input-fidelity | The repro report's input/command, compared against the issue's stated reproduction input | Pass if the input matches or is shown provably equivalent, such that it hits the same code path; fail if it's a different construct | required |
| failure-mode-match | The repro report's pasted output/log excerpt, compared against the failure type the issue describes (panic vs. plain error vs. wrong output) | Pass if the same class of failure occurs, or if the report shows a good-faith attempt that triggered no failure at all and states that plainly (an honest non-reproduction — the honesty of that claim is judged by analysis-honesty, not here); fail if a failure did occur but is a different or adjacent type than the issue describes | required |
| analysis-honesty | The repro report's stated conclusion, compared against the output excerpt it cites as support | Pass if the conclusion is fully supported by the shown output (including an honest "could not reproduce" backed by evidence); fail if it asserts confirmation the shown output doesn't earn | required |
| env-recorded | The repro report's environment section (OS, language/runtime version, dependency versions, or a pinned container image) | Pass if the recorded values are specific and complete enough that someone else could recreate the same runtime; fail if values are missing, vague, or omitted | required |
| steps-followable | The repro report's steps section, read as a sequence to execute from a clean checkout | Pass if following the listed steps, with nothing left implicit, would independently reach the same failure; fail if a step is missing or assumes undocumented state | required |
| conventions-respected | The claim comment and repro comment text, checked against (a) any policy that specific repo actually states for comments/contributions (e.g. an AI-disclosure requirement — not an issue-intake template's fields), and (b) the general claim-comment standard in evidence-guide.md's Comms section, independent of whether the repo writes it down: the claim names the specific issue and stays investigative, without asserting a fix, a timeline guarantee, or an exclusive reservation it hasn't earned | Pass if both (a) and (b) hold; fail if any stated repo policy is violated, or if the claim comment asserts an unearned fix/timeline/reservation or substitutes generic flattery for engagement with the issue's specifics | required |


## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->

Accept only if every required check passes. Any fail on a required check means reject. Unclear on a required check counts as fail, except where a pass condition above says otherwise. There are no preferred checks, so fit (from scope.md) only ranks accepted issues.
