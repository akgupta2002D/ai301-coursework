# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diff stays inside the plan | The diff's changed files and hunks, read against the plan's stated files and boundary ("Files:" list; a changelog entry counts as naming the changelog file) and its Deviations notes | Every changed file and hunk falls inside the plan's boundary or is explained in the plan's Deviations or the description. Fail if the diff touches something the plan does not name and nothing discloses it (unrelated cleanup, reformatting, extra feature). | required |
| Plan is delivered or the shortfall is disclosed | The plan's promised changes, read against the diff, the description, and the Deviations notes | Every change the plan promises appears in the diff, or the description or Deviations says what was left out. Fail if the diff quietly does less than the plan. A shortfall that is honestly disclosed passes. A limit the plan itself states up front (a case it says it will not cover) is not a shortfall. | required |
| Description claims are true of the diff | Each claim in the description's Summary, Changes, and Notes (what was changed, fixed, or not added, and any claim that the PR follows the plan), read against the diff | Every claim can be found in the diff. A claim that something was not added passes if nothing in the diff adds it. Fail if any claim is contradicted by the diff or has no matching change, including a claim of fidelity to the plan that the diff breaks. | required |
| Test results are observable | The test evidence (the Testing section, test_evidence.md, or the package's test evidence block) | Only the decisive result is judged here: the after run of the plan's repro on the branch (or, if the plan names no repro, the test that exercises the change). It passes when it shows the command or steps that were run and the observed output, either printed or stated concretely: an exact message, value, row, or count. Output quoted in a `#` or `--` comment beside the command counts. For an interactive or visual behavior that cannot be printed, a before and after description of the steps taken and what appeared (such as the exact toast text) counts. A check that failed and is reported honestly with the reason counts as evidence and passes. Fail only if the decisive result is missing, or is a bare verdict ("tests pass", "verified", "works", "# ok") that names no observed output. Other claimed results (suite runs, fmt, lint, controls, secondary behaviors) never fail this check on their own; the next two checks judge them. | required |
| Repo's own checks were run | The test evidence, read against the checks the repo's PR template or contribution docs list (live: `make test-unit`, `make test-integration`, `make lint`, `make typecheck`) | The test command the repo asks for this change (the suite or the relevant tests) appears in the test evidence with its outcome stated concretely (a count, a pass for a named spec, or a failure with its reason). Formatting, lint, typecheck, and generate steps need an outcome only if the description claims or ticks them, or in live mode, where all four listed make targets must appear. `make test-integration` stopping with "no tests ran" is accepted as is. An unticked CI box is not a failure, because CI only runs after the PR opens. Pass if the repo facts list no checks. Fail if the test command is missing without a stated reason, or a claimed check has no stated outcome. | required |
| Plan's test plan is covered | The test evidence and the diff's added tests, read against the plan's test plan and the repro evidence in the plan context (live: the unit 2 output) | Every case the test plan names is covered in one of three ways. (1) The issue's repro is shown re-run on the branch with its observed result. (2) A named control, secondary case, or test run is covered when the evidence states its outcome in a sentence, even briefly ("controls unchanged", "the control still stashes", "the spec passes, including the new case"); a stated outcome is enough, and it need not be re-run with printed output. (3) A case is exercised by a test the diff adds whose run is reported passing in the test evidence, or the description or Deviations says why it was not covered. The before state may come from the plan's repro evidence and need not be re-run on main. Fail only if a named case is not mentioned anywhere in the evidence, the description, or Deviations, or the evidence says a case failed and gives no reason. | required |
| Diff has no debris or unrelated hunks | Every added line in the diff | No debug prints, commented-out code, leftover TODOs, stray files, or hunks unrelated to the change that a reviewer would have to dig around. Fail if any such line is present. | required |
| Change is understandable from the title and description | The PR title and description, read against the diff | A reviewer could say what the change does and why from the title and Summary without reverse-engineering the diff. Note a miss in the evidence line. | preferred |
| Template is filled with real content | The description, read against the repo's PR template (the repo-facts block's template asks in an eval package, `.github/PULL_REQUEST_TEMPLATE.md` in live mode) | Every section the template asks for has real content for this change, not placeholder text or an empty heading. Testing boxes are ticked only where the evidence supports them. Pass if the repo facts state there is no PR template. Fail if a section is blank or boilerplate, or a ticked box is contradicted by the evidence. | required |
| AI use is disclosed | The description's Notes for Reviewers (or the section the repo's AI policy names), read against the repo's stated AI policy | The description states in the author's own words how AI was used on this change, or that it was not used. Pass if the repo facts state no AI policy and ask for no disclosure. Fail if the repo asks for disclosure and the description has none, or only a generic line that says nothing about this change. | required |
| Repo's other stated asks are followed | The repo-facts block (eval) or `docs/CONTRIBUTING.md` and the template (live), read against the PR, including the issue reference and, in live mode only, the branch name | Every ask the repo states is met: the issue is referenced (live: after `Closes #`) and matches the plan's issue; in live mode the branch name has a type prefix, issue number, and short description; and any rule the repo states for this kind of change (such as removing a seeded `xfail` marker for the issue, a CHANGELOG entry, or keeping the PR small) is followed. Pass if the repo states no other asks. Fail if an explicit ask is skipped. | required |

## Verdict rule

1. Grade every check `pass`, `fail`, or `unclear`.
2. **Accept** only if every `required` check is `pass`.
3. **Reject** if any `required` check is `fail`.
4. **`unclear` counts as `fail`.** A required check that cannot be verified from the package holds it. A PR that cannot be verified is not ready to submit.
5. `preferred` checks never change the verdict. Record a preferred miss in its evidence line only.
6. **Honest shortfalls can be ready.** If the PR leaves something out or departs from the plan and the description or Deviations says so, the checks about drift and the test plan pass on that disclosure. Do not reject a PR just because it does less than everything. Reject it when the shortfall is silent, or when it breaks a different required check.
7. Grade the outcome itself, not the polish. A short, complete PR with real evidence is ready. A polished one that hides drift is not.