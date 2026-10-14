# Unit 4: Test and Submit

**PR link:** https://github.com/codepath/pathreview-ai301-fa26-s1/pull/183
**Branch:** test/65-fix-async-result-mocks

## Run history

Run 1 (full): 13/20. clear-accept 0/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3. All seven clear-accept packages were rejected, mostly on "Test results are observable" and "Plan's test plan is covered", because my first pass conditions wanted a pasted command and printed output for every claim, and a re-run of every case the test plan named.

Run 2 (partial, 11 packages): 6/11. I changed the observable check to focus on the decisive repro result and let a test added in the diff count toward the test plan. pkg-02 and pkg-16 flipped to accept. pkg-05, 08, 11, 13 and 19 still rejected, and the four not-tested packages stayed rejected.

Run 3 (partial, 14 packages): 12/14. I let output quoted in `#` or `--` comments and described interactive behavior count, made the observable check judge only the decisive result, and let a named control count when its outcome is stated. pkg-05, 11, 13 and 19 flipped to accept. pkg-16 flipped the other way, from accept to reject on "Description claims are true of the diff". Misses: pkg-08 and pkg-16. I added canaries from standards-wall (pkg-01, pkg-20) and unreviewable (pkg-12), and all three still rejected.

Run 4 (full): 19/20. clear-accept 6/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3. Miss: pkg-08, which failed "Repo's own checks were run". This run is the one written to eval-run.txt.

## Package analysis

pkg-02 (Textualize/rich#4208). My rubric's verdict: accept. Gold label: accept, so we agree. In Run 1 my rubric rejected it, which was wrong.

The package is a clean PR, and every required check passes on it. The plan's Files line names rich/console.py, tests/test_console.py and CHANGELOG.md, and the diff changes exactly those three files. Every claim in the description is visible in the diff: save_text, save_html and save_svg now export with clear=False, write the file, and clear the record only after the write succeeds, and the CHANGELOG entry is added. The decisive result is the issue's repro re-run, which shows the command with before and after output. Before, the record prints '' after the failed save; after, it still prints 'IMPORTANT OUTPUT\n'. The repo facts ask for a type-of-changes checklist and a CHANGELOG entry, and the description has "Type of changes: Bug fix" plus the entry. They state no AI policy, so no disclosure is required. The diff has no debris: the one comment in the added test is the issue link.

Run 1 failed it on "Test results are observable" and "Plan's test plan is covered" because of what those two conditions required. The observable check wanted a command and printed output for every claimed result, and pkg-02's pytest line ("1462 passed, 12 skipped") names a command and a concrete count but is not a pasted block. The test-plan check wanted every named case re-run, and the plan's case "a successful save still clears" is covered by the second half of the added test, not by a separate run. I rewrote the observable check to judge only the decisive repro result, and I let the test-plan check count a case exercised by a test the diff adds when the evidence reports the suite passing. Runs 2, 3 and 4 accepted it.

## Check rationale

Test results are observable | The test evidence (the Testing section, test_evidence.md, or the package's test evidence block) | Only the decisive result is judged here: the after run of the plan's repro on the branch (or, if the plan names no repro, the test that exercises the change). It passes when it shows the command or steps that were run and the observed output, either printed or stated concretely: an exact message, value, row, or count. Output quoted in a `#` or `--` comment beside the command counts. For an interactive or visual behavior that cannot be printed, a before and after description of the steps taken and what appeared (such as the exact toast text) counts. A check that failed and is reported honestly with the reason counts as evidence and passes. Fail only if the decisive result is missing, or is a bare verdict ("tests pass", "verified", "works", "# ok") that names no observed output. Other claimed results (suite runs, fmt, lint, controls, secondary behaviors) never fail this check on their own; the next two checks judge them. | required

It reads this way because of what the first versions got wrong. In Run 1, all seven clear-accept packages were rejected, and this check was among the failures on six of them. The packages show why: pkg-11 quotes its output in `#` comments beside the commands, pkg-08 is a terminal UI whose evidence describes the steps and the exact toast text, and pkg-05 prints its decisive before and after tables in full but also makes secondary claims such as fmt and clippy. Each is real evidence in a different shape. So I made the decisive result the only thing this check judges, accepted printed or concretely stated output in any of those shapes, and moved the other claims to the two checks after it. I kept the bare-verdict failure ("tests pass" with no command and no observed output), and the four not-tested packages (pkg-04, 07, 10, 14) were rejected in every run.

## Trade-offs

The loosened observable check gives up strictness about what counts as proof. A PR that states a concrete result in words, such as a count or an exact message, now passes even though I cannot verify it from a pasted block. I accepted that because the first version rejected every clear accept (0/7), and the check still fails a bare "tests pass" with no command and no observed output.

The run is also not fully stable. pkg-16 was accepted in Run 2, rejected in Run 3, and accepted in Run 4, and the "Description claims are true of the diff" check it failed on never changed in my rubric, so some of that is grader variance and not my rules. The remaining miss is pkg-08, which fails "Repo's own checks were run". I stopped there because the run met the 18/20 bar, and loosening that check further risked flipping a not-tested or standards-wall package that currently agrees.