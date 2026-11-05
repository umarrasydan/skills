---
name: ship-pr
description: Take a finished branch through deslop, a thermo-nuclear review, a pull request, and review-comment resolution.
disable-model-invocation: true
---

# Ship PR

Run this when the branch's change is done and tested. Invoking this skill authorizes committing, pushing, opening the PR, and replying to and resolving its review threads. It does not authorize merging.

Work on the existing change branch. If you are on the default branch, create one under the user's branch naming convention first. Run every step against the full diff from the merge base with the PR's base branch, not only the last commit.

## Dependencies

Steps 1 and 2 load the `deslop` and `thermo-nuclear-code-quality-review` skills, which ship alongside this one. If either is missing, stop and give the user the install command:

```bash
npx skills add umarrasydan/skills --skill deslop --skill thermo-nuclear-code-quality-review
```

If the host refuses to start either one, for example because the user has Cursor's original thermo, which only the user may start, ask the user to run it on the branch, wait for its result, and continue from there. Do not reproduce it from its file or substitute a generic review.

## Steps

1. **Deslop.** Apply deslop to the branch diff and edit the slop out. Keep behavior unchanged.
2. **Thermo.** Run the thermo-nuclear review on the post-deslop diff, then make the changes it asks for. Apply a structural change only when it keeps behavior and you can verify it. If a finding needs a product or scope decision, leave the code as is and list it for the PR description.
3. **Verify.** Run the repository's relevant tests, type checks, and linters after steps 1 and 2. Fix failures that the cleanup caused. Report failures that existed before the branch rather than fixing them here.
4. **Open the PR.** Commit, keeping the cleanup separate from feature work that was already committed. Then fetch the base branch, rebase onto it, and push. Stop and ask the user if a conflict needs a product decision. Open the PR with `gh pr create`, or update it if one is already open for the branch. Fill the repository's PR template if it has one. The description states what changed, how it was verified, and any thermo findings left open from step 2. Follow the repository's draft convention; with none, open it ready for review.
5. **Resolve review comments.** Wait for automated reviewers and checks to finish. Use the host's PR event subscription if it has one, otherwise poll `gh pr checks` and the PR's review threads every few minutes. Then handle each unresolved thread:
   - Check the claim against the code before acting.
   - If it is right, fix it, verify, push, reply with the commit that fixed it, and resolve the thread.
   - If it is wrong or out of scope, reply with the reason and leave the thread for the reviewer to resolve.
   - If it needs the user's judgment, leave it open and list it in the report.

   Resolve a thread with `gh api graphql` and the `resolveReviewThread` mutation. Pushing a fix can start a new review round, so repeat this step until checks pass and no actionable threads remain. Stop waiting when a full poll interval brings no new reviews after checks finish, and report the reviewers who never responded.

## Done

The PR is open and pushed. Checks pass, or failures are reported with their cause. Every review thread is fixed and resolved, answered, or listed for the user. Report the PR URL, what deslop and thermo changed, any thermo findings deferred, and the threads still open.
