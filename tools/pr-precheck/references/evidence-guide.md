# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**Where it lives**
- **Eval bundle:**
  - The **Plan context** section holds the plan's scope. Read its `Files:` line, its "Not in scope" sentence, and the numbered steps, which together are the boundary. Its `Test plan:` sentence is the test plan (used in the next family). There is no separate deviation block, so a deviation can only appear in the Candidate PR's description or commits.
  - Read these against **Candidate PR > Diff** (changed files, `---`/`+++` headers, and `+`/`-` lines), **Commits**, and **Description**.
- **Live mode:**
  - `plan.md`, including its files list, its steps, and its `## Deviations` section.
  - The diff from `git diff main...HEAD` and `git diff main...HEAD --stat` for the file list.
  - The draft description in `pr_draft.md` (Summary, Changes, Notes for Reviewers).

**What good looks like**
- Every changed file in the diff is named by the plan's `Files:` line or its folder, or is covered by a deviation or a disclosed note.
- Every step the plan promises has a matching hunk, or the description says it was left out.
- Every fidelity claim in the description ("removes X", "rendering is unchanged", "does not touch Y") matches the diff.
- Silent drift is a changed file or hunk the plan never named, a promised step with no hunk, or a description claiming more or less than the diff delivers, with no note explaining it.

## Test evidence (harness category: not-tested)

**Where it lives**
- **Eval bundle:**
  - **Candidate PR > Test evidence**, read against the **Plan context** test plan and the repro evidence in **Plan context**.
  - Each result should appear as a command line (`$ ...`) followed by what it printed. Lines starting with `#` are notes, not output.
  - The repo's own checks come from the **Repo facts** block. If it lists no checks or template, the repo's-checks and template checks have nothing to enforce.
- **Live mode:**
  - `test_evidence.md` (also pasted into the Testing section of `pr_draft.md`).
  - The checks the PR template lists (`make test-unit`, `make test-integration`, `make lint`, `make typecheck`), and your unit 2 repro steps for the before and after.

**What good looks like**
- Each claimed result has a command and printed output. The before (on main) and the after (on the branch) are both shown, and every case the test plan names is re-run or its omission is explained.
- A failing check reported honestly with its reason counts as evidence.
- "Tests pass", a `# printed OK` note, or a result described only in prose is not proof.

## Diff quality (harness category: unreviewable)

**Where it lives**
- **Eval bundle:** **Candidate PR > Diff** and **Commits**.
- **Live mode:** `git diff main...HEAD`, `git diff main...HEAD --stat`, and `git log main..HEAD --oneline`.

**What good looks like**
- The fix is visible and nothing unrelated rides along: only hunks that serve the plan's change.
- Debris tells to scan for in every added line: debug prints, commented-out code, leftover TODOs, stray or generated files, whitespace or formatting churn, and drive-by edits to code the plan did not name.
- A one-line removal with a clear commit message is a reviewable change.

## Standards and comms (harness category: standards-wall)

**Where it lives**
- **Eval bundle:**
  - **Repo facts** states the repo's asks: its PR template (or that it has none), its contribution policy, and its AI policy (or that it has none).
  - **Issue** and **Thread highlights** show any explicit maintainer direction.
  - **Candidate PR > Title**, **Description**, and **Commits** show how the PR honors those asks.
  - Take every ask only from the bundle. If **Repo facts** says there is no template or no AI policy, that ask does not exist for this package, so a missing section or disclosure is not a failure.
- **Live mode:**
  - `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in the Path Review repo, plus the repo's AI policy.
  - `pr_draft.md` for the title and filled template, and the branch name from `git branch --show-current`.

**What good looks like**
- Every section the repo asks for is filled with real content for this change, not a placeholder.
- The issue is referenced (`Closes #N` or the repo's equivalent) and matches the plan's issue.
- AI use is disclosed in the author's own words where the repo asks for it.
- Explicit maintainer direction in the thread is engaged, not ignored.
- Compliant means the stated ask is met. Boilerplate or a visibly skipped ask is not compliant.
- (Whether the description's claims match the diff belongs under plan fidelity, above.)