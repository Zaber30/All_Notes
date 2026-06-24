# Git Complete Reference Guide
### From Basic to Advanced — Every Command with Description

---

## Table of Contents

1. [What is Git?](#1-what-is-git)
2. [Git Configuration](#2-git-configuration)
3. [Repository Setup](#3-repository-setup)
4. [Basic Snapshotting](#4-basic-snapshotting)
5. [Branching & Merging](#5-branching--merging)
6. [Remote Repositories](#6-remote-repositories)
7. [Inspection & Comparison](#7-inspection--comparison)
8. [Undoing Changes](#8-undoing-changes)
9. [Stashing](#9-stashing)
10. [Tagging](#10-tagging)
11. [Rebasing](#11-rebasing)
12. [Cherry-Picking](#12-cherry-picking)
13. [Submodules](#13-submodules)
14. [Git Hooks](#14-git-hooks)
15. [Advanced Log & Search](#15-advanced-log--search)
16. [Reflog](#16-reflog)
17. [Bisect (Bug Hunting)](#17-bisect-bug-hunting)
18. [Worktrees](#18-worktrees)
19. [Git Internals](#19-git-internals)
20. [Aliases & Productivity](#20-aliases--productivity)
21. [.gitignore](#21-gitignore)
22. [Git Flow & Workflows](#22-git-flow--workflows)
23. [Tips & Best Practices](#23-tips--best-practices)

---

## 1. What is Git?

Git is a **distributed version control system** created by Linus Torvalds in 2005. It tracks changes in source code, allows multiple developers to work in parallel, and provides a complete history of every modification.

**Key Concepts:**
- **Repository (repo):** A directory tracked by Git, containing all files and history.
- **Commit:** A snapshot of the project at a point in time.
- **Branch:** An independent line of development.
- **Remote:** A version of the repository hosted on a server (e.g., GitHub, GitLab).
- **Working Directory:** Where you edit files.
- **Staging Area (Index):** Where you prepare changes before committing.

---

## 2. Git Configuration

Configuration is stored at three levels: system (`/etc/gitconfig`), global (`~/.gitconfig`), and local (`.git/config`).

```bash
# Set your name (used in commits)
git config --global user.name "Your Name"

# Set your email (used in commits)
git config --global user.email "you@example.com"

# Set default text editor (e.g., VS Code, vim, nano)
git config --global core.editor "code --wait"
git config --global core.editor "vim"
git config --global core.editor "nano"

# Set default branch name to 'main' (modern standard)
git config --global init.defaultBranch main

# Enable colorized output in terminal
git config --global color.ui auto

# Set line-ending handling (Windows: true, Mac/Linux: input)
git config --global core.autocrlf true    # Windows
git config --global core.autocrlf input   # Mac/Linux

# Set merge tool
git config --global merge.tool vscode

# Set diff tool
git config --global diff.tool vscode

# Set credential helper to cache password temporarily
git config --global credential.helper cache

# Set credential helper to store password permanently (less secure)
git config --global credential.helper store

# View all configuration settings
git config --list

# View a specific config value
git config user.name

# View config with origin info (which file it comes from)
git config --list --show-origin

# Edit global config file directly in your editor
git config --global --edit

# Set local (project-only) config — overrides global
git config user.email "work@company.com"

# Remove a config entry
git config --global --unset user.name

# Set push behavior: push current branch to its upstream
git config --global push.default current

# Enable rerere (reuse recorded resolution — auto-fix repeated merge conflicts)
git config --global rerere.enabled true

# Set fetch to prune deleted remote branches automatically
git config --global fetch.prune true
```

---

## 3. Repository Setup

```bash
# Initialize a new Git repository in the current directory
git init

# Initialize a repo with a specific directory name
git init my-project

# Initialize a bare repository (used for servers/remotes, no working directory)
git init --bare my-repo.git

# Clone a remote repository into a new directory
git clone https://github.com/user/repo.git

# Clone into a specific folder name
git clone https://github.com/user/repo.git my-folder

# Clone only the latest commit (shallow clone — faster, smaller)
git clone --depth 1 https://github.com/user/repo.git

# Clone a specific branch only
git clone --branch develop --single-branch https://github.com/user/repo.git

# Clone with all submodules initialized
git clone --recurse-submodules https://github.com/user/repo.git

# Clone via SSH
git clone git@github.com:user/repo.git
```

---

## 4. Basic Snapshotting

### Checking Status

```bash
# Show the working tree status (modified, staged, untracked files)
git status

# Short/compact status output
git status -s
git status --short

# Show branch information along with short status
git status -sb
```

### Staging Files

```bash
# Stage a specific file
git add filename.txt

# Stage multiple files
git add file1.txt file2.txt

# Stage all changes in the current directory (tracked + untracked)
git add .

# Stage all changes in the entire repository
git add -A
git add --all

# Stage only modifications and deletions (not new/untracked files)
git add -u
git add --update

# Interactively choose which changes (hunks) to stage within a file
git add -p
git add --patch

# Stage a specific directory
git add src/

# Stage files matching a pattern
git add *.js
```

### Committing

```bash
# Commit staged changes with a message
git commit -m "Your commit message"

# Stage all tracked modified files and commit in one step (skips untracked)
git commit -am "Your commit message"
git commit -a -m "Your commit message"

# Open editor for a detailed multi-line commit message
git commit

# Amend the last commit (change message or add forgotten files)
# WARNING: Do not amend commits already pushed to a shared remote
git commit --amend

# Amend last commit message without opening editor
git commit --amend -m "Corrected commit message"

# Amend last commit but keep the same message
git commit --amend --no-edit

# Create an empty commit (useful for triggering CI/CD)
git commit --allow-empty -m "Trigger CI"

# Sign a commit with GPG
git commit -S -m "Signed commit"
```

### Removing & Moving Files

```bash
# Remove a file from working directory and stage the deletion
git rm filename.txt

# Remove a file from Git tracking only (keep the file on disk)
git rm --cached filename.txt

# Remove all .log files from tracking
git rm --cached *.log

# Forcefully remove a file that has staged changes
git rm -f filename.txt

# Rename a file and stage the rename
git mv old_name.txt new_name.txt

# Move a file to a different directory
git mv file.txt src/file.txt
```

---

## 5. Branching & Merging

### Creating & Switching Branches

```bash
# List all local branches (* marks current branch)
git branch

# List all branches including remote-tracking branches
git branch -a

# List remote branches only
git branch -r

# Create a new branch (does NOT switch to it)
git branch feature-login

# Switch to an existing branch
git checkout feature-login
git switch feature-login          # Modern syntax (Git 2.23+)

# Create a new branch and switch to it immediately
git checkout -b feature-login
git switch -c feature-login       # Modern syntax

# Create a branch from a specific commit or tag
git branch hotfix abc1234
git checkout -b hotfix v1.2.0

# Create a local branch tracking a remote branch
git checkout -b feature origin/feature
git checkout --track origin/feature

# Rename the current branch
git branch -m new-branch-name

# Rename a specific branch
git branch -m old-name new-name

# Delete a branch (safe — only if fully merged)
git branch -d feature-login

# Force delete a branch (even if not merged)
git branch -D feature-login

# Delete a remote branch
git push origin --delete feature-login
git push origin :feature-login    # Alternate syntax

# Show which branches are merged into current branch
git branch --merged

# Show branches NOT yet merged
git branch --no-merged

# Show last commit on each branch
git branch -v

# Show last commit and upstream tracking info
git branch -vv
```

### Merging

```bash
# Merge a branch into the current branch (creates a merge commit)
git merge feature-login

# Merge with a commit message even if fast-forward is possible
git merge --no-ff feature-login

# Fast-forward merge only (fails if not possible)
git merge --ff-only feature-login

# Merge without committing (review changes first)
git merge --no-commit feature-login

# Squash all commits from branch into one staged change
git merge --squash feature-login

# Abort a merge in progress (during conflict resolution)
git merge --abort

# Continue a merge after resolving conflicts
git merge --continue

# Mark a conflicted file as resolved
git add conflicted-file.txt
```

### Merge Conflicts

When Git can't auto-merge, it marks conflicts like this:

```
<<<<<<< HEAD
Your current branch's code
=======
Incoming branch's code
>>>>>>> feature-login
```

You must manually edit the file, remove the markers, then:

```bash
# After editing, stage the resolved file
git add resolved-file.txt

# Complete the merge
git commit

# Or use a merge tool to resolve conflicts visually
git mergetool
```

---

## 6. Remote Repositories

```bash
# List all remotes (short)
git remote

# List all remotes with URLs
git remote -v

# Add a new remote named 'origin'
git remote add origin https://github.com/user/repo.git

# Add a second remote (e.g., for deployment)
git remote add upstream https://github.com/original/repo.git

# Rename a remote
git remote rename origin main-remote

# Remove a remote
git remote remove upstream
git remote rm upstream

# Change a remote's URL
git remote set-url origin https://github.com/user/new-repo.git

# Show detailed info about a remote
git remote show origin

# Fetch changes from a remote (download but don't merge)
git fetch origin

# Fetch from all remotes
git fetch --all

# Fetch and prune deleted remote branches
git fetch --prune
git fetch -p

# Fetch a specific branch
git fetch origin feature-branch

# Pull = fetch + merge (updates current branch from its upstream)
git pull

# Pull with rebase instead of merge (cleaner linear history)
git pull --rebase
git pull -r

# Pull from a specific remote and branch
git pull origin main

# Push current branch to remote
git push

# Push a specific branch to remote
git push origin feature-login

# Push and set upstream tracking at the same time
git push -u origin feature-login
git push --set-upstream origin feature-login

# Push all local branches to remote
git push --all origin

# Push all tags to remote
git push --tags

# Force push (overwrites remote history — dangerous on shared branches)
git push --force
git push -f

# Safer force push (fails if remote has new commits you haven't fetched)
git push --force-with-lease

# Delete a remote branch
git push origin --delete feature-login

# Push a tag
git push origin v1.0.0
```

---

## 7. Inspection & Comparison

### git log — View Commit History

```bash
# Show full commit history
git log

# Compact one-line per commit
git log --oneline

# Show last N commits
git log -5
git log -n 10

# Show commits with diff (patch)
git log -p
git log --patch

# Show stat summary of changes per commit
git log --stat

# Show brief stat (files changed, insertions, deletions)
git log --shortstat

# Beautiful graph view of branches
git log --oneline --graph --all --decorate

# Log for a specific file
git log -- filename.txt

# Log with author filter
git log --author="John"

# Log between dates
git log --after="2024-01-01" --before="2024-12-31"
git log --since="2 weeks ago"
git log --until="yesterday"

# Log by commit message keyword
git log --grep="fix"

# Log commits that changed a specific string in code
git log -S "function_name"

# Log with a custom format
git log --pretty=format:"%h - %an, %ar : %s"

# Show commits in one branch but not another
git log main..feature-branch

# Show commits in either branch but not both (symmetric difference)
git log main...feature-branch

# Show commits that touched a specific function
git log -L :function_name:file.py

# Follow file renames in history
git log --follow -- old-name.txt

# Show commits from all branches with graph
git log --all --oneline --graph
```

### git diff — Compare Changes

```bash
# Show unstaged changes (working directory vs last commit)
git diff

# Show staged changes (index vs last commit)
git diff --staged
git diff --cached

# Show all changes (staged + unstaged) vs last commit
git diff HEAD

# Diff between two commits
git diff abc1234 def5678

# Diff between two branches
git diff main..feature-branch

# Diff a specific file
git diff -- filename.txt

# Diff showing only changed file names
git diff --name-only

# Diff showing file names and change status
git diff --name-status

# Diff with stat summary
git diff --stat

# Diff ignoring whitespace changes
git diff -w
git diff --ignore-all-space

# Diff between last two commits
git diff HEAD~1 HEAD

# Word-by-word diff (useful for prose)
git diff --word-diff

# Show diff in a GUI/external tool
git difftool
```

### git show — Inspect a Commit

```bash
# Show the most recent commit and its diff
git show

# Show a specific commit
git show abc1234

# Show a commit without the diff
git show --stat abc1234

# Show a specific file at a specific commit
git show abc1234:path/to/file.txt

# Show a tag
git show v1.0.0
```

### git blame — See Who Wrote Each Line

```bash
# Show who last modified each line of a file
git blame filename.txt

# Blame with line numbers and timestamps
git blame -l filename.txt

# Blame a specific range of lines
git blame -L 10,25 filename.txt

# Blame ignoring whitespace changes
git blame -w filename.txt

# Show the commit summary with blame
git blame --show-stats filename.txt
```

---

## 8. Undoing Changes

### Discard Changes in Working Directory

```bash
# Discard changes in a specific file (restore from last commit)
git checkout -- filename.txt
git restore filename.txt                   # Modern syntax (Git 2.23+)

# Discard all unstaged changes in working directory
git checkout -- .
git restore .

# Restore a file to its state in a specific commit
git restore --source=abc1234 filename.txt

# Interactively discard chunks of changes
git restore -p filename.txt
```

### Unstage Files

```bash
# Unstage a specific file (keep changes in working directory)
git reset HEAD filename.txt
git restore --staged filename.txt          # Modern syntax

# Unstage all staged files
git reset HEAD
git restore --staged .
```

### git reset — Undo Commits

```bash
# Soft reset: move HEAD back, keep changes staged
# Commits are undone, but changes remain in the index (staging area)
git reset --soft HEAD~1

# Mixed reset (default): move HEAD back, unstage changes, keep in working dir
git reset HEAD~1
git reset --mixed HEAD~1

# Hard reset: move HEAD back, discard ALL changes (working dir + index)
# WARNING: This permanently deletes uncommitted changes
git reset --hard HEAD~1

# Reset to a specific commit
git reset --hard abc1234

# Reset to match remote (discard all local commits since last push)
git reset --hard origin/main
```

### git revert — Safe Undo (Preserves History)

Unlike `reset`, `revert` creates a new commit that undoes the changes. Safe for shared branches.

```bash
# Revert the last commit (creates a new "undo" commit)
git revert HEAD

# Revert a specific commit
git revert abc1234

# Revert without opening the commit message editor
git revert --no-edit abc1234

# Revert multiple commits (creates one revert commit each)
git revert abc1234 def5678

# Revert a range of commits
git revert HEAD~3..HEAD

# Stage the revert but don't commit yet
git revert --no-commit abc1234

# Abort a revert in progress
git revert --abort
```

### git clean — Remove Untracked Files

```bash
# Preview what would be deleted (dry run)
git clean -n
git clean --dry-run

# Remove untracked files (irreversible!)
git clean -f

# Remove untracked files and directories
git clean -fd

# Remove untracked files including those in .gitignore
git clean -fdx

# Remove only ignored files
git clean -fX

# Interactively choose what to remove
git clean -i
```

---

## 9. Stashing

Stashing lets you temporarily save uncommitted work and restore it later.

```bash
# Stash current changes (staged + unstaged tracked files)
git stash
git stash push

# Stash with a descriptive message
git stash push -m "WIP: login feature"
git stash save "WIP: login feature"    # Older syntax

# Stash including untracked files
git stash -u
git stash --include-untracked

# Stash including ignored files
git stash -a
git stash --all

# Stash only staged changes
git stash --staged
git stash -S

# Stash a specific file
git stash push path/to/file.txt

# List all stashes
git stash list

# Apply the most recent stash (keeps stash in list)
git stash apply

# Apply a specific stash
git stash apply stash@{2}

# Pop the most recent stash (applies and removes from list)
git stash pop

# Pop a specific stash
git stash pop stash@{1}

# Show the diff of the most recent stash
git stash show
git stash show -p    # Show full patch diff

# Show diff of a specific stash
git stash show stash@{1} -p

# Drop (delete) the most recent stash
git stash drop

# Drop a specific stash
git stash drop stash@{2}

# Clear all stashes (irreversible!)
git stash clear

# Create a new branch from a stash (useful to test stash in isolation)
git stash branch new-branch stash@{0}
```

---

## 10. Tagging

Tags mark specific points in history — typically used for releases.

```bash
# List all tags
git tag

# List tags matching a pattern
git tag -l "v1.*"
git tag --list "v2.*"

# Create a lightweight tag (just a pointer to a commit)
git tag v1.0.0

# Create a lightweight tag on a specific commit
git tag v1.0.0 abc1234

# Create an annotated tag (stores tagger, date, message — recommended)
git tag -a v1.0.0 -m "Release version 1.0.0"

# Create an annotated tag on a specific commit
git tag -a v1.0.0 abc1234 -m "Hotfix release"

# Create a signed tag (GPG signed)
git tag -s v1.0.0 -m "Signed release"

# Show tag details and the tagged commit
git show v1.0.0

# Push a specific tag to remote
git push origin v1.0.0

# Push all tags to remote
git push origin --tags

# Push only annotated tags
git push origin --follow-tags

# Delete a local tag
git tag -d v1.0.0

# Delete a remote tag
git push origin --delete v1.0.0
git push origin :refs/tags/v1.0.0

# Checkout a tag (goes into detached HEAD state)
git checkout v1.0.0

# Create a branch from a tag
git checkout -b release-1.0 v1.0.0

# Verify a signed tag
git tag -v v1.0.0
```

---

## 11. Rebasing

Rebasing re-applies commits on top of another base commit, creating a cleaner, linear history.

```bash
# Rebase current branch onto main
git rebase main

# Rebase onto a specific branch
git rebase origin/main

# Abort a rebase in progress
git rebase --abort

# Continue a rebase after resolving conflicts
git rebase --continue

# Skip a conflicting commit during rebase
git rebase --skip

# Interactive rebase — edit, squash, reorder last N commits
git rebase -i HEAD~3
git rebase -i HEAD~5

# Interactive rebase from a specific commit (exclusive)
git rebase -i abc1234

# Interactive rebase commands (used inside the interactive editor):
# pick   = use commit as-is
# reword = use commit but edit its message
# edit   = use commit but pause for amending
# squash = meld into previous commit (keeps both messages)
# fixup  = meld into previous commit (discard this message)
# drop   = remove this commit entirely
# exec   = run a shell command

# Rebase preserving merge commits
git rebase --preserve-merges main
git rebase --rebase-merges main    # Git 2.22+ preferred

# Pull with rebase (avoid unnecessary merge commits)
git pull --rebase origin main

# Rebase only commits not yet on remote
git rebase @{upstream}
```

> **Rule:** Never rebase commits that have been pushed to a shared remote branch. It rewrites history and causes problems for others.

---

## 12. Cherry-Picking

Apply specific commits from another branch onto the current branch.

```bash
# Apply a specific commit to the current branch
git cherry-pick abc1234

# Cherry-pick multiple commits
git cherry-pick abc1234 def5678

# Cherry-pick a range of commits (exclusive start, inclusive end)
git cherry-pick abc1234..def5678

# Cherry-pick without creating a commit (stage changes only)
git cherry-pick --no-commit abc1234
git cherry-pick -n abc1234

# Cherry-pick and edit the commit message
git cherry-pick -e abc1234

# Abort a cherry-pick in progress
git cherry-pick --abort

# Continue after resolving conflicts
git cherry-pick --continue

# Skip a commit during cherry-pick
git cherry-pick --skip

# Preserve original author info in cherry-picked commit
git cherry-pick -x abc1234    # Adds "cherry picked from commit..." note
```

---

## 13. Submodules

Submodules allow you to include another Git repository inside your project.

```bash
# Add a submodule (clones repo into a subdirectory)
git submodule add https://github.com/user/library.git libs/library

# Add a submodule at a specific branch
git submodule add -b develop https://github.com/user/library.git libs/library

# Initialize submodules after cloning (if not cloned with --recurse-submodules)
git submodule init

# Download and checkout submodule content
git submodule update

# Init and update in one step
git submodule update --init

# Init and update recursively (for nested submodules)
git submodule update --init --recursive

# Update all submodules to latest commit on their tracked branch
git submodule update --remote

# Update a specific submodule to latest
git submodule update --remote libs/library

# Show status of all submodules
git submodule status

# Run a command in each submodule
git submodule foreach git pull origin main

# Run a command recursively in nested submodules
git submodule foreach --recursive git status

# Remove a submodule (multi-step process)
git submodule deinit -f libs/library
git rm -f libs/library
rm -rf .git/modules/libs/library

# Sync submodule URLs (after .gitmodules was changed)
git submodule sync
```

---

## 14. Git Hooks

Hooks are scripts in `.git/hooks/` that run automatically on events.

```bash
# List available hooks (samples are pre-created with .sample extension)
ls .git/hooks/

# Enable a hook by removing .sample extension
cp .git/hooks/pre-commit.sample .git/hooks/pre-commit
chmod +x .git/hooks/pre-commit
```

**Common hooks:**

| Hook | Triggers When |
|------|--------------|
| `pre-commit` | Before a commit is made |
| `commit-msg` | After commit message is entered |
| `post-commit` | After a commit is made |
| `pre-push` | Before `git push` |
| `pre-rebase` | Before rebasing |
| `post-merge` | After a merge |
| `post-checkout` | After `git checkout` |
| `post-receive` | After receiving a push (server-side) |
| `pre-receive` | Before receiving a push (server-side) |
| `update` | Once per updated branch (server-side) |

**Example pre-commit hook (runs tests before committing):**

```bash
#!/bin/sh
npm test
if [ $? -ne 0 ]; then
  echo "Tests failed. Commit aborted."
  exit 1
fi
```

```bash
# Skip hooks for a commit (use with caution)
git commit --no-verify -m "Skip hooks"
git push --no-verify
```

---

## 15. Advanced Log & Search

```bash
# Search the entire codebase (all commits) for a string
git log -S "search_term" --oneline

# Search using a regex pattern across all commits
git log -G "regex_pattern" --oneline

# Search commit messages with a keyword
git log --grep="keyword" --oneline

# Show changes to a specific function across all commits
git log -L :function_name:file.js

# Show what commits introduced or removed a specific string
git log --all -S "deleted_function"

# Find all commits that affected a file (even through renames)
git log --follow -- old-filename.txt

# Show the commit tree in a visual graph
git log --graph --oneline --all --decorate

# Format log output as a changelog
git log --pretty=format:"- %s (%h)" v1.0.0..HEAD

# Show commits between two tags
git log v1.0.0..v2.0.0 --oneline

# Show commits not yet in remote
git log origin/main..HEAD --oneline

# Show what changed in the last 24 hours
git log --since="24 hours ago" --oneline

# Grep through content of files at a specific commit
git grep "search_term" abc1234

# Search current working tree
git grep "function_name"

# Grep case-insensitively
git grep -i "search_term"

# Show file and line numbers in grep result
git grep -n "search_term"

# Count occurrences in grep
git grep -c "search_term"
```

---

## 16. Reflog

The reflog records every time HEAD moves, even from resets and rebases. It's your safety net.

```bash
# Show the full reflog of HEAD movements
git reflog

# Show reflog for a specific branch
git reflog show main

# Show reflog with dates
git reflog --date=relative

# Show reflog with absolute timestamps
git reflog --date=iso

# Recover a lost commit using reflog
git checkout abc1234       # go to the lost commit
git branch recovered-work  # save it as a branch

# Recover after a bad reset (go back to where HEAD was before reset)
git reset --hard HEAD@{1}  # HEAD@{1} = previous HEAD position

# Show reflog of stash operations
git reflog refs/stash

# Expire old reflog entries (cleanup)
git reflog expire --expire=30.days --all

# Delete reflog for a branch
git reflog delete main@{0}
```

---

## 17. Bisect (Bug Hunting)

`git bisect` uses binary search to find the commit that introduced a bug.

```bash
# Start bisect session
git bisect start

# Mark current state as bad (bug exists here)
git bisect bad

# Mark a known good commit (no bug here)
git bisect good v1.0.0
git bisect good abc1234

# Git checks out a commit halfway between good and bad
# Test your code, then mark it:
git bisect good    # if this commit has no bug
git bisect bad     # if this commit has the bug

# Git will continue narrowing down until it finds the culprit

# View the bisect log
git bisect log

# Automate bisect with a test script (0=good, 1=bad)
git bisect run npm test
git bisect run ./test.sh

# Reset back to original state when done
git bisect reset

# Reset to a specific commit after bisect
git bisect reset main
```

---

## 18. Worktrees

Worktrees let you check out multiple branches simultaneously in separate directories.

```bash
# Add a new worktree for a branch
git worktree add ../hotfix hotfix-branch

# Add a worktree and create a new branch
git worktree add -b new-feature ../new-feature main

# List all worktrees
git worktree list

# Remove a worktree (delete the directory first)
git worktree remove ../hotfix

# Prune stale worktree references
git worktree prune

# Lock a worktree (prevent accidental deletion)
git worktree lock ../hotfix

# Unlock a worktree
git worktree unlock ../hotfix
```

---

## 19. Git Internals

Understanding what Git stores under the hood.

```bash
# Show type of a Git object (blob, tree, commit, tag)
git cat-file -t abc1234

# Show content of a Git object
git cat-file -p abc1234

# Show the Git object database size
git count-objects -v

# List all objects in the object database
git cat-file --batch-all-objects --batch-check

# Show the contents of the index (staging area)
git ls-files
git ls-files --stage    # With object hashes

# Show the tree structure of the last commit
git ls-tree HEAD

# Show tree recursively
git ls-tree -r HEAD

# Show commits without references (orphan/dangling objects)
git fsck --unreachable

# Pack objects for efficiency
git gc

# Aggressive garbage collection and repacking
git gc --aggressive

# Verify the integrity of the object database
git fsck

# Show all branches and their SHA hashes
git show-ref

# Resolve a ref to a commit SHA
git rev-parse HEAD
git rev-parse main
git rev-parse v1.0.0

# Show parent of a commit
git rev-parse HEAD^
git rev-parse HEAD~2   # Two commits back

# List all commits reachable from HEAD
git rev-list HEAD

# Count total number of commits
git rev-list --count HEAD
```

---

## 20. Aliases & Productivity

```bash
# Create a short alias for common commands
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.df diff
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.last "log -1 HEAD"
git config --global alias.unstage "reset HEAD --"
git config --global alias.undo "reset --soft HEAD~1"
git config --global alias.plog "log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short"

# Use an alias
git st
git lg
git last

# Remove an alias
git config --global --unset alias.st

# Run a shell command as alias (prefix with !)
git config --global alias.root '!pwd'
git config --global alias.cleanup '!git branch --merged | grep -v main | xargs git branch -d'
```

---

## 21. .gitignore

The `.gitignore` file tells Git which files and directories to ignore.

```bash
# Create a .gitignore file
touch .gitignore
```

**Common patterns:**

```gitignore
# Ignore a specific file
secret.txt

# Ignore all .log files
*.log

# Ignore all files in a directory
node_modules/

# Ignore a specific directory
build/
dist/

# Ignore files with a specific extension in any subdirectory
**/*.pyc

# Ignore everything in a folder except one file
logs/*
!logs/important.log

# Ignore all files except .gitignore itself
*
!.gitignore

# Ignore files matching a pattern
temp_*

# Comments start with #
# OS files
.DS_Store
Thumbs.db
```

```bash
# Apply .gitignore to already-tracked files (must remove from cache first)
git rm -r --cached .
git add .
git commit -m "Apply .gitignore changes"

# View which .gitignore rule is ignoring a file
git check-ignore -v filename.txt

# Temporarily ignore a tracked file (local only, not committed)
git update-index --assume-unchanged filename.txt

# Re-track the file
git update-index --no-assume-unchanged filename.txt

# Skip worktree (similar, but for local config files)
git update-index --skip-worktree config/local.env
git update-index --no-skip-worktree config/local.env

# Global .gitignore for OS/editor files (applies to all repos)
git config --global core.excludesfile ~/.gitignore_global
```

---

## 22. Git Flow & Workflows

### Centralized Workflow
Everyone commits to `main`. Simple but doesn't scale well.

### Feature Branch Workflow
1. Create a branch: `git checkout -b feature/login`
2. Work, commit changes
3. Push: `git push -u origin feature/login`
4. Open Pull Request / Merge Request
5. Review, merge, delete branch

### Git Flow (Standard for releases)

```
main        — production-ready code (tagged releases)
develop     — integration branch for features
feature/*   — individual features (branch off develop)
release/*   — release preparation (branch off develop, merge to main+develop)
hotfix/*    — emergency fixes (branch off main, merge to main+develop)
```

```bash
# Install git-flow tool
apt-get install git-flow     # Linux
brew install git-flow        # Mac

# Initialize git-flow in a repo
git flow init

# Start a feature
git flow feature start login-page

# Finish a feature (merges to develop)
git flow feature finish login-page

# Start a release
git flow release start 1.0.0

# Finish a release (merges to main and develop, creates tag)
git flow release finish 1.0.0

# Start a hotfix
git flow hotfix start critical-bug

# Finish a hotfix
git flow hotfix finish critical-bug

# Publish feature/release/hotfix to remote
git flow feature publish login-page
```

### Trunk-Based Development
- All developers work directly on `main` (or short-lived feature branches < 1 day)
- Uses feature flags for incomplete features
- Requires strong CI/CD

---

## 23. Tips & Best Practices

```bash
# Write meaningful commit messages:
# Format: <type>(<scope>): <short description>
# Example:
#   feat(auth): add OAuth login support
#   fix(api): handle null response from server
#   docs(readme): update installation steps
#   refactor(cart): simplify total calculation
#   test(login): add unit tests for edge cases

# Show a summary of contributions by author
git shortlog -sn

# Show contribution count for a date range
git shortlog -sn --after="2024-01-01"

# Check if a branch has been merged
git branch --merged main

# See all changes since a specific tag
git log v1.0.0..HEAD --oneline

# Archive the repository as a zip or tar
git archive --format=zip HEAD > project.zip
git archive --format=tar HEAD | gzip > project.tar.gz

# Archive a specific branch or tag
git archive --format=zip v1.0.0 > release-v1.zip

# Bundle a repository for offline transfer
git bundle create repo.bundle --all

# Clone from a bundle
git clone repo.bundle new-repo

# Create a patch from a commit
git format-patch HEAD~3    # Last 3 commits as patch files
git format-patch abc1234

# Apply a patch
git apply patch-file.patch
git am patch-file.patch    # Preserves commit metadata

# Show size of files tracked by Git
git ls-files | xargs wc -l

# Find large files in repository history
git rev-list --objects --all | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | sort -k3 -n | tail -20

# Squash last N commits interactively
git rebase -i HEAD~4

# Keep your fork in sync with upstream
git remote add upstream https://github.com/original/repo.git
git fetch upstream
git merge upstream/main
# or:
git rebase upstream/main

# Sign all commits with GPG by default
git config --global commit.gpgsign true

# Check current HEAD
cat .git/HEAD

# Show what Git would do before actually doing it
git push --dry-run
git fetch --dry-run
```

---

## Quick Reference Cheat Sheet

| Command | Description |
|---------|-------------|
| `git init` | Create a new local repository |
| `git clone <url>` | Clone a remote repository |
| `git status` | Show working tree status |
| `git add .` | Stage all changes |
| `git commit -m "msg"` | Commit staged changes |
| `git push` | Push to remote |
| `git pull` | Fetch and merge from remote |
| `git branch` | List local branches |
| `git checkout -b <branch>` | Create and switch branch |
| `git merge <branch>` | Merge branch into current |
| `git rebase <branch>` | Rebase current onto branch |
| `git stash` | Stash uncommitted changes |
| `git log --oneline` | Compact commit history |
| `git diff` | Show unstaged changes |
| `git reset --hard HEAD` | Discard all local changes |
| `git revert HEAD` | Create undo commit |
| `git tag v1.0.0` | Create a tag |
| `git fetch --prune` | Sync remotes, remove deleted branches |
| `git cherry-pick <hash>` | Apply a commit to current branch |
| `git bisect start` | Start binary search for bug |
| `git reflog` | Show all HEAD movements |
| `git worktree add` | Check out another branch in new dir |
| `git submodule update --init` | Initialize submodules |

---

*This document covers every major Git command from beginner to advanced. Practice each section to build muscle memory. When in doubt, `git help <command>` opens the manual.*
