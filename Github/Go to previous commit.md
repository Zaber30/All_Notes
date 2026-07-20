To move back to a previous commit on both your local machine and your remote repository (like GitHub), you must rewrite the project history.

Step 1: Find the Commit Hash

First, locate the 7-character code of the exact commit you want to return to.
- Run: `git log --oneline`
- copy the code (e.g., `7a1b23c`).
Step 2: Reset Your Local Repository

This command will permanently erase all local commits made _after_ your target commit.

- Run: `git reset --hard <commit-hash>`

Step 3: Update the Remote Repository

Because your local history is now older than the remote history, a normal `git push` will be blocked. You must force the update.
- Run: `git push origin <branch-name> --force`  
    _(Replace `<branch-name>` with `main`, `master`, or your active branch name)._