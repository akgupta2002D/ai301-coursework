# Procedure: how this tool grades a PR package

## Read order

Read in this order, and write down what each part gives you before moving on. The plan comes first because every side-by-side check later measures the diff, the description, and the evidence against it. Reading the diff first would let the diff define what the change is supposed to be.

1. **Mode and scope.**
   - Decide the mode from the request.
   - Live mode: read `scope.md` first. Stop without grading if its `Repo:` line is a placeholder or the PR targets a different repo.
   - Eval mode: skip `scope.md`. The package is the whole world.
2. **`rubric.md`, then `references/evidence-guide.md`.** Note the check names and where each kind of evidence lives (plan fidelity, test evidence, diff quality, standards and comms).
3. **The plan.** Live: `plan.md`. Eval: the plan context in the package. Write down:
   - the files and folders the plan names (a changelog entry counts as naming the changelog file),
   - the changes it promises,
   - its test plan (every case and crash it names),
   - any Deviations notes.
4. **The issue.** Live: the thread at the URL given. Eval: the frozen issue copy. Write down the issue number and the repro steps.
5. **The repo's asks.** Live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`. Eval: the repo-facts block in the package, and nothing else. Write down the template sections, the checks the template lists, the disclosure ask, the branch-name rule, and any other stated rule.
6. **The PR title and description.** Live: `pr_draft.md`. Eval: the description in the package. Write down every claim, one per sentence or bullet (what changed, what was fixed, what was not added, what was tested, any claim of following the plan), plus the issue number after `Closes #` and the AI-use disclosure text.
7. **The diff.** Live: run `git diff main...HEAD` and `git diff main...HEAD --stat`. Eval: the diff in the package. Write down every changed file and, for each file, what its hunks do.
8. **The test evidence.** Live: `test_evidence.md`. Eval: the test evidence in the package. Write down each command and the output printed under it.
9. **Live mode only: `voice-guide.md`,** after the checks are graded.

If a required input (plan, diff, description, test evidence) is missing or empty, stop and name which one. Grade nothing.

## Evidence gathering

Do each gather before grading. Record the answer, and where you found it.

1. **Diff files against the plan's scope** (for "Diff stays inside the plan"). For each changed file in the diff, write `yes` if the plan names the file or its folder, or a Deviations note covers it, and `no` otherwise. For each `no`, note what the hunks do and whether the description discloses it.
2. **Plan promises against the diff** (for "Plan is delivered or the shortfall is disclosed"). For each change the plan promises, find the diff hunk that makes it. For each promise with no hunk, note whether the description or Deviations says it was left out.
3. **Description claims against the diff** (for "Description claims are true of the diff"). For each claim you listed, find the diff lines that show it, and record the lines or what is missing. For a claim that something was not added, check that nothing in the diff adds it, and record `nothing in the diff adds it`. For a claim of fidelity to the plan, compare it with your result from gather 1 and 2.
4. **Claimed results against the evidence** (for "Test results are observable"). Find only the decisive result: the after run of the plan's repro on the branch. Record its command or steps and the observed output, noting whether it is printed, quoted in a `#` or `--` comment, or described for an interactive behavior. Record `bare verdict` if all it says is that something passed, worked, or was verified, with no observed output. Do not record other claimed results here.
5. **Repo's checks against the evidence** (for "Repo's own checks were run"). Find the test command the repo asks for this change and record its outcome. For fmt, lint, typecheck, and generate steps, record an outcome only if the description claims or ticks them (live mode: all four make targets). Record any that are missing, and whether a reason is stated.
6. **Test plan against the evidence** (for "Plan's test plan is covered"). List every case the test plan names (the repro, each control, each test run). For each, record how it is covered: the repro re-run on the branch with its observed result; a control or secondary case whose outcome is stated in a sentence; a test the diff adds whose run is reported passing; or an explanation in the description or Deviations. Take the before state from the plan's repro evidence if the test evidence does not repeat it. Record a case only as uncovered if it appears nowhere in the evidence, the description, or Deviations.
7. **Debris scan** (for "Diff has no debris or unrelated hunks"). Read every added line. Record debug prints, commented-out code, leftover TODOs, stray files, and hunks unrelated to the change, each with its file and line.
8. **Title and description read** (for "Change is understandable from the title and description"). Record whether the title and Summary say what the change does and why, without the diff.
9. **Template sections** (for "Template is filled with real content"). List each template section and record `real content`, `placeholder`, or `empty`. For each ticked box, record whether the test evidence supports it.
10. **Disclosure** (for "AI use is disclosed"). Find the AI-use text in the Notes for Reviewers (or the section the repo's AI policy names). Record whether it says in the author's own words how AI was used on this change, or is generic or absent.
11. **Repo's other asks** (for "Repo's other stated asks are followed"). Record the issue number after `Closes #` against the plan's issue, the branch name against the rule (type prefix, issue number, short description), and any other rule the repo states for this kind of change, each met or not met.

## Check execution

1. Run the checks in the order they appear in `rubric.md`.
2. For each check, apply its pass condition exactly as written, using only the facts you gathered. Do not add conditions and do not relax them. The rubric decides, not the run.
3. Grade `pass`, `fail`, or `unclear`:
   - `pass` if the condition is met.
   - `fail` if the evidence is present and the condition is not met.
   - `unclear` if the evidence the check needs is genuinely absent or cannot be verified from the package. Do not guess, and do not fetch anything in eval mode.
4. Write one evidence line per check: the fact or quote that decided it (a file name, a diff line, a command and its output, or the missing section). The line must name something findable.
5. Apply the disclosure rule in the same way every time: a shortfall or departure that the description or Deviations states honestly satisfies the drift checks and the test plan check. A silent one does not.
6. Grade a check from your gathered notes. Re-read the package only when your notes cannot decide it, or when two checks overlap and the answer depends on the same lines.
7. Never grade formatting, length, or tone. Grade the outcome the artifact shows.
8. If the procedure is silent on something you need, say so in the summary under the gaps, and then grade the affected check `unclear`. Do not invent a step.
9. Live mode only: after grading, hold the PR title and description against each rule in `voice-guide.md` and list every broken rule with the offending text. Do not change any grade or the verdict because of it.

## Verdict assembly

1. Collect all grades in rubric order.
2. Apply the verdict rule in `rubric.md`:
   - any `required` check graded `fail` means `reject`;
   - any `required` check graded `unclear` also means `reject`, because `unclear` counts as `fail`;
   - `accept` only if every `required` check is `pass`;
   - `preferred` checks never change the verdict. Record a preferred miss in its evidence line only.
3. Pick the deciding check:
   - on `reject`, it is the first `required` check, in rubric order, graded `fail` or `unclear`;
   - on `accept`, there is none.
4. Quote the deciding check in the summary: its name, its grade, and the one-line fact that decided it. If more checks failed, list their names after it, without re-quoting.
5. Print the readable per-check summary: every check, its grade, its one-line evidence. In live mode, add the voice-guide findings.
6. End with the fenced JSON block from `SKILL.md`: one entry per check in rubric order with its grade and one-line evidence, then the verdict. The JSON block is valid, it is the last thing in the reply, and nothing follows it.
7. The same grades always produce the same verdict. Do not soften, add a third verdict, or move a reservation into the verdict. Reservations go in the evidence lines.