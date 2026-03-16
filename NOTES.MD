# Git Notes

## Git Flow Overview

```
Working Directory  =>  Staging Area (index)  =>  Commit History (objects)
```

---

## Setting Up a Repository

```bash
git init                  # Create a local repository (default branch: master)
git init -b main          # Create local repo with 'main' branch instead of 'master'
rm -rf .git               # Delete git (removes the entire repository)
```

---

## Staging & Committing

```bash
git add <file>            # Stage a specific file
git add .                 # Stage all changes
git add --a               # Stage changes from all tracked or untracked files
git rm --cached <file> -f # Unstage a file (-f forces the removal)

git commit                         # Commit with editor
git commit -m "commit message"     # Commit with inline message
git commit -a -m "second commit"   # Stage + commit in one step (skips staging area)
```

> **Checksum:** Every commit gets a unique checksum (fingerprint) — 40 characters long, but Git typically shows only the first 7. Example: `3bed821`. Even small changes produce a different checksum.

---

## Viewing History & Differences

```bash
git log               # Full commit history
git log --oneline     # Compact summary of commits
git log --pretty=oneline  # Full hash + message per line
tig                   # Better UI to browse git log

git diff              # Differences in the working directory (unstaged)
git diff --staged     # Differences in the staging area
git diff <branch>     # Differences between current branch and another branch
```

---

## Undoing Changes

```bash
git restore <file>    # Discard changes in working directory
```

---

## Deleting Files from Git

> To safely remove a file from Git tracking:
> 1. Run `git rm --cached <file>` — the file becomes untracked (U)
> 2. Now you can safely delete the local file

---

## Connecting to a Remote Repository

Two options for authentication:

- **HTTPS** — prompts for login every time you push
- **SSH** — set up once with a key pair; authenticates automatically

### Setting Up SSH

```bash
ssh-keygen -o          # Generate private/public key pair
cd ~/.ssh
ls -a
cat id_rsa.pub         # Copy the full key
```

Then go to: **GitHub → Settings → SSH and GPG keys → New SSH Key → Add SSH Key**

### Adding a Remote & Pushing

```bash
git remote add origin git@github.com:username/repo.git
git remote -v                  # Check remote URLs (fetch and push)

git push -u origin main        # Push and set upstream tracking branch
git push origin main           # Push without setting upstream
```

> **Why `-u`?** Setting the upstream once with `-u` lets you use `git push` and `git pull` without extra arguments in the future, and enables `git status` to show ahead/behind counts.

### Checking & Setting Upstream

```bash
git branch -vv                               # Check which remote branch each local branch tracks
git branch --set-upstream-to=origin/main main  # Set upstream without pushing
git push --set-upstream origin feature/login   # -u and --set-upstream are synonyms
```

### Push Defaults (push.default config)

```bash
git config --global push.default simple
```

| Mode | Behavior |
|------|----------|
| `simple` (default) | Pushes to same-named branch only if upstream is set; otherwise complains |
| `current` | Pushes to same-named branch even without upstream |
| `upstream` | Pushes to the configured upstream only |
| `matching` | Pushes all branches with matching names (legacy) |

### Typical Push Workflow

```bash
# First time publishing a new branch
git push -u origin main

# Subsequent pushes/pulls
git push
git pull

# If you forgot -u
git push -u origin main                      # Option A: push and set upstream
git branch --set-upstream-to=origin/main main  # Option B: set without pushing
```

---

## Cloning

```bash
git clone <url>        # Clone a remote repository (SSH or HTTPS)
ls -a                  # Show all files, including hidden ones (like .git)
```

---

## Tagging

Two types of tags:

1. **Annotated** — for official releases; includes author, date, and message
2. **Lightweight** — a simple pointer to a specific commit; for internal markers

```bash
git tag -a v1.0 -m "Release version 1.0"  # Annotated tag
git tag v1.0                               # Lightweight tag
git tag                                    # List all tags
git show v1.0                              # Details about a tag
git push origin v1.0                       # Push a tag to remote
```

---

## Branching

```bash
git branch                  # List local branches
git branch <name>           # Create a new branch
git branch --all            # List local + remote branches
git branch -vv              # Show tracking info for each branch
git branch -M main          # Rename current branch to main

git checkout -b feature1    # Create and switch to a new branch (older syntax)
git switch -c feature1      # Create and switch to a new branch (newer syntax)

git branch -                # Switch to previous branch
git branch -d <name>        # Safe delete (fails if unmerged)
git branch -D <name>        # Force delete (no safety check)
```

---

## Merging

```bash
git merge feature2          # Merge feature2 into current branch
git diff <branch>           # Check differences before merging
```

**Merge creates a new merge commit and preserves full history:**

```
Before:
main:      A---B---C
                 \
feature1:         D---E---F

After (git merge feature1):
main:      A---B---C-------M
                 \         /
feature1:         D---E---F
```

---

## Rebasing

```bash
git rebase main             # Replay current branch's commits on top of main
git rebase --continue       # Continue rebase after resolving a conflict
git rebase --abort          # Cancel rebase and return to pre-rebase state
```

**Rebase rewrites commit history into a linear timeline:**

```
Before:
main:      A---B---C
                 \
feature1:         D---E---F

After (git rebase main, while on feature1):
main:      A---B---C
                    \
feature1:            D'---E'---F'  (commits rewritten)
```

### Resolving Rebase Conflicts

```bash
# Step 1: Check status
git status

# Step 2: Open the conflicted file and resolve conflict markers
<<<<<<< HEAD
(content from base branch)
=======
(content from your feature branch)
>>>>>>> feature1

# Remove the markers and keep the desired content

# Step 3: Mark as resolved
git add <conflicted-file>

# Step 4: Continue the rebase
git rebase --continue
```

---

## Merge vs Rebase — Decision Guide

| Scenario | Recommended |
|----------|-------------|
| Updating `main` with feature changes | ✅ `git merge feature1` (while on `main`) |
| Updating a feature with latest `main` | ✅ `git rebase main` (while on `feature1`) |
| Shared/public branches | ✅ Always use merge |
| Local branch cleanup | ✅ Rebase is fine |

> **Golden Rule:** Never rebase commits that have already been pushed to a shared/public repository.

### Complete Best Practice Workflow

```bash
# 1. Sync feature branch with latest main (optional but recommended)
git checkout feature1
git rebase main
# resolve conflicts if needed

# 2. Switch to main and merge
git checkout main
git merge feature1

# 3. Push to remote
git push origin main

# 4. Clean up
git branch -d feature1
```

---

## Restoring Branches After Rebase

After rebasing, original commits become orphaned but remain in the reflog temporarily.

### Method 1 — Using git reflog (recently rebased)

```bash
git reflog feature1                  # Find commit before rebase
git reset --hard feature1@{2}        # Restore to that position
```

### Method 2 — Using ORIG_HEAD (immediately after rebase)

```bash
git reset --hard ORIG_HEAD
```

### Method 3 — Specific commit hash

```bash
git reset --hard <commit-hash>
```

### Method 4 — From remote (if pushed before rebase)

```bash
git fetch origin
git reset --hard origin/feature1
```

> **Note:** Reflog entries expire after 30 days by default and are local only (not synced to remote).

### Prevention — Create a backup before rebasing

```bash
git branch feature1-backup           # Backup branch
git rebase main                      # Safe to rebase now

# To restore if needed
git branch -D feature1
git checkout -b feature1 feature1-backup
```

---

## Deleting Branches: `-d` vs `-D`

| Flag | Behavior |
|------|----------|
| `-d` | Safe delete — refuses if branch has unmerged commits |
| `-D` | Force delete — no safety check, can lose unmerged work |

```bash
git branch -d feature2    # Fails if unmerged (shows error)
git branch -D feature2    # Always succeeds
```

### Decision Tree

```
Do you need the changes from the branch?
│
├─ YES → Merge first, then delete
│         git checkout main
│         git merge feature2
│         git branch -d feature2
│
├─ NOT SURE → Check commits first
│              git log main..feature2
│
└─ NO → Force delete
        git branch -D feature2
```

### Safe Archive Before Force Deleting

```bash
git tag backup-feature2 feature2     # Save a tag backup
git branch -D feature2               # Force delete

# Restore later if needed
git checkout -b feature2-restored backup-feature2

# Remove backup tag when no longer needed
git tag -d backup-feature2
```

---

## Deleting Remote Branches

> Deleting a branch locally with `git branch -d/-D` does **not** affect the remote.

```bash
# Delete a remote branch
git push origin --delete feature1

# Delete multiple remote branches at once
git push origin --delete feature1 feature2 feature3

# Older colon syntax (same effect)
git push origin :feature1
```

### Clean Up Local Remote-Tracking References

After deleting remote branches, remove stale local references:

```bash
git fetch --prune            # Remove stale remote-tracking refs
git fetch -p                 # Shorthand
git remote prune origin      # Prune a specific remote
```

### Complete Branch Deletion Workflow

```bash
git branch -d feature1                   # 1. Delete local branch
git push origin --delete feature1        # 2. Delete remote branch
git fetch --prune                        # 3. Clean up stale refs
git branch --all                         # 4. Verify
```

### One-Liner: Delete Both Local and Remote

```bash
git branch -d feature1 && git push origin --delete feature1
git branch -D feature1 && git push origin --delete feature1  # Force
```

### Quick Reference

| Task | Command |
|------|---------|
| View all branches | `git branch --all` |
| Delete local branch | `git branch -d feature1` |
| Force delete local branch | `git branch -D feature1` |
| Delete remote branch | `git push origin --delete feature1` |
| Delete multiple remote branches | `git push origin --delete feature1 feature2` |
| Clean up stale references | `git fetch --prune` |
| Delete local + remote together | `git branch -d feature1 && git push origin --delete feature1` |

### Common Mistakes

```bash
# ❌ Thinking local deletion also deletes remote
git branch -d feature1   # Remote still exists!

# ❌ Forgetting to prune after remote deletion
git push origin --delete feature1
git branch --all         # Still shows remotes/origin/feature1 (cached)
# Fix: git fetch --prune

# ❌ Deleting the wrong branch
git push origin --delete main   # Double-check before running!
```

### Recovering a Deleted Remote Branch

```bash
# If you still have it locally
git push origin feature1

# If you have a backup tag
git push origin backup/feature1:feature1

# If you have the commit hash
git push origin <commit-hash>:refs/heads/feature1
```

---

## Time Travel (Checking Out Older Commits)

You can go back to any previous commit — useful for creating a lite/stable version of a project.

```bash
git log --oneline                    # Find the commit hash you want
git checkout <commit-hash>           # Detach HEAD and go to that commit
git checkout -b lite-version         # Create a new branch from this point
git remote -v                        # Check remote
git push origin lite-version         # Push the new branch to remote
```

> **Detached HEAD state:** When you check out an old commit, you're no longer on any branch. Any changes you make won't affect existing branches unless you create a new one with `git checkout -b`.

---

## Git Stash

Stash lets you save uncommitted changes temporarily so you can switch branches without committing.

```bash
git stash                  # Save current changes (appears deleted, but saved)
git stash list             # View all stashes
git stash apply            # Re-apply the most recent stash
git switch main            # Switch branches cleanly after stashing
```

---

## Pulling from Remote

```bash
git pull origin main       # Pull latest changes from remote main
```

If you have divergent branches (different commits locally and remotely), Git will ask you to specify a strategy:

```bash
git config pull.rebase false   # Use merge strategy (rebase false = merge)
git config pull.rebase true    # Use rebase strategy
```

After configuring, resolve any merge conflicts as usual:

```bash
git status
git add .
git commit -m "merge"
```

---

## Merge Conflicts

Conflicts occur when two branches modify the same part of a file. Git marks the conflict in the file:

```
<<<<<<< HEAD
(content from current branch)
=======
(content from incoming branch)
>>>>>>> feature3
```

To resolve:
1. Decide which version to keep (or combine them)
2. Remove all conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
3. Save the file
4. Run `git add .` then `git commit`

> In VS Code, you'll see UI options like "Accept Current Change", "Accept Incoming Change", etc. In the terminal, use `git diff <branch>` to review differences manually.

---

## Forking

Forking creates your own copy of someone else's repository on your GitHub account.

- You **cannot** push directly to a repository you don't own or contribute to
- Fork the repo → clone your fork → make changes → push to your fork
- Then create a **Pull Request** to propose your changes to the original repo

---

## Pull Requests (PR)

A Pull Request asks the original repo owner to review and merge your changes.

**Workflow:**
1. Fork the repo
2. Clone your fork locally
3. Make changes, add, commit, and push to your fork
4. On GitHub: **Pull Requests → New Pull Request → Create Pull Request**
5. The reviewer reviews the diff, leaves comments if needed
6. Reviewer clicks **Merge Pull Request** to accept

> Once merged, the PR is closed and the changes appear in the main repository.

---

## Summary

| Topic | Key Points |
|-------|-----------|
| **Merge** | Preserves full history; creates a merge commit; branching graph |
| **Rebase** | Rewrites history; linear timeline; changes commit hashes |
| **Conflicts** | Resolve manually → `git add` → commit or `--continue` |
| **Restore after rebase** | Use `git reflog` + `git reset --hard` |
| **Updating main** | Always use `git merge` to bring in feature changes |
| **Updating feature** | Use `git rebase main` to sync with latest main |
| **Delete local branch** | `-d` for safety, `-D` only when certain |
| **Delete remote branch** | `git push origin --delete <branch>` |
| **Clean up refs** | `git fetch --prune` after remote deletions |
| **Golden Rule** | Never rebase commits pushed to a shared/public repository |
| **Best Practice** | Rebase for local cleanup; merge for integrating shared work |
| **Safety tip** | Create backup branches/tags before deleting important work |