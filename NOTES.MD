here's the flow of git: Working Directory => Staging Area (index) => Commit History (objects)

# git init -> create a local repository (by default will give you a master)
# rm -rf .git -> delete git
# git init -b main -> create local repos with git main NOT master
# git rm --cached <file> -f -> to unstage, f for forces the removal
# git add <file> or git add . or git add --a -> add changes from all tracked or untracked files
# git commit or git commit -m "commit message" -> to commit the changes

main (root-commit) 3bed821 -> this number is checksum (3bed821), checksum is something as a fingerprint for a data.
                              so example, every time I change something in a data, it will change the checksum as well. git creates a checksum for every commit, even for small changes. checksum is
                              consisted of 40 characters, but it's only showing the 7 characters

# git log -> give you the log of all the commit
# git log --oneline -> give you log summary of the commit
# git restore <file> -> to discard changes in working directory
# git commit -a -m "second commit" -> -a here is to move the file from working directory to the commits (skip staging area)
# git diff -> can be used if you have file in the working directory, and if you want to check the differences
# git diff --staged  -> can be used if your file is into staging area, then you have to use --staged to check the changes
# tig -> better version of UI to check git log
# -a -> means of adding directly in one line itself

PS*: to delete certain file in our git repos, firstly, you have to delete your file from the Git.
     by git rm --cached <file> and now it's untracked (U) and you can happily delete the local file now.

# git clone <url address> -> to clone git repos
# ls -a -> to show up all of type of files, including hidden file
# git branch -M main -> got main branch which previously you need to do "git init" first

PS*: after above git command, we need to create remote repos, but the problem is how will you connect your local
     repos to the remote repos? that's a big issue.
     now basically there are two optons one is HTTPS and one is SSH.
     so first of all, when we use HTTPS, basically to push, how do i know that who should be allowed?
     now thats where when we use HTTPS, it will ask you to login. 
     otherwise, you can use SSH, the problem with login is you have to login every time.
     what if you can set up your machine once with a key and then every time you do something,
     it will automatically detect who you are. and thats why you have to setup the SSH.
     in order do this, can be used "ssh-keygen -o" and when you enter it will be generating private/public keys.
     once the SHA256 file is created, we just need to go back to the folder for one step and type "ls -a".
     then will appear .ssh folder, then goes to the folder by typing "cd .ssh" -> "ls -a" -> "cat id_rsa.pub" -> we got the KEY.
     then, copy the entire key -> Goes to Github -> Settings -> SSH and GPG keys -> New SSH Key -> Add SSH Key. that's it.
     and now your client and your server is connected. so connected does not mean it's continuously connected.
     every time you push something, it'll verify, okay, it's coming from a right person because I can see the SSH key,
     which is mentioned here. and on the local machine, they are the same.

# git remote add origin git@github.com:thatperson63/git-course.git -> we are doing git remote cuz thats remote repos of Github
# git push -u origin main -> pushes your local main branch to origin/main and sets origin/main as the upstream (tracking)    branch for main
# git push origin main (without -u) -> just pushes; no upstream is configured
# git branch -vv -> displays which remote branch each local branch tracks
# git branch --set-upstream-to=origin/main main -> explicitly set the upstream without pushing

PS*: once you mapped it, now we have to move this file, which is in the local repos, the remote is empty at this point.
     so what i will do is to push the code, by git push and enter. it's done. now to verify, just refresh the page and
     we got the files which we created in the local machine.

TLDR*: Use "git push -u origin main" the first time you push a branch. It saves you from typing the remote/branch every time 
       and enables git status/git pull to work smoothly. Without -u, you’ve just pushed, but Git won’t remember where to push/pull next time.

WHY*: Setting upstream affects several convenient defaults:
      1. Future pushes/pulls
         -> With upstream set: you can simply run git push or git pull (no extra args), and Git knows which remote branch to 
            use.
         -> Without upstream: git pull will error (fatal: No upstream branch), and git push may require specifying the remote/
            branch or rely on push. default behavior (see below).
      2. Status and tracking info
         -> git status shows “ahead/behind” counts relative to the upstream.
         -> git branch -vv displays which remote branch each local branch tracks.
      3. Merge/Rebase defaults
         -> git pull merges/rebases from the upstream branch.

Typical Workflow*:
First time you publish a new branch:
# git push -u origin main   # sets upstream
Later:
# git push                   # uses origin/main automatically
# git pull                   # pulls from origin/main

If you forgot -u, You can set it later:
# git push -u origin main -> Option A: push and set upstream in one go
# git branch --set-upstream-to=origin/main main -> Option B: explicitly set the upstream without pushing
# git push --set-upstream origin feature/login -> -u is the same as --set-upstream. in this case, they’re kinda synonyms
# git config --global push.default simple -> config that affects behavior without upstream:

simple (default):
-> pushes the current branch to the same-named branch on the upstream remote only if upstream is set; otherwise it complains.
Other modes:
-> current: pushes to a same-named branch on the remote even without upstream.
-> upstream: pushes to the configured upstream only.
-> matching: pushes all branches with matching names (legacy; be careful).

# git branch -vv -> check your upstream/tracking

PS*: speaking of origin from git push. when we talked about the remote server, we only have one.
# git remote -v -> this is the command in which you can see which is your origin.
                   origin  git@github.com:thatperson63/git-course.git (fetch) -> from server to the client
                   origin  git@github.com:thatperson63/git-course.git (push) -> from client to the server
                   for fetch and push, we have the same link cuz we only have one remote repo there.
                   by default the name is origin and you can change it as well.
                   you can name it or you can connect to different repositories, not just one.
                   you can pull and push to any other repositories if you want.

PS*: there are two options for tagging, one is annotated tagging or you can use lightweight tagging.
     1. annotated tagging is if you really want to release this to the users and if you want to give
        more information about the user, or who is committing it or who owns it.
        used for official releases or when you need history and verification.
   # git tag -a v1.0 -m "Release version 1.0" -> sample of annotated tagging
     2. lightweight is basically a pointer to a specific commit.
        used for quick internal markers or temporary references.
   # git tag v1.0 -> sample of lightweight tagging
# git tag -> this will give you the information of tag version
# git show v1.0 -> this will give you description about this particular tag
# git push origin v1.0 -> this will push your code from local to the cloud 
# git log --pretty=oneline -> this will gives you to see the commits and the message there
                              also will get an example by looking at other's people commit
                              like how they message something. we can see also some numbers,
                              issue numbers, and upgrade numbers
# git clone <copy the repo via SSH/HTTPS> -> to clone repo from server to the client side
# git checkout is older version and the new one is git switch.
# git checkout -b feature1: create new branch (-b / --branch)
# git switch -c feature1: create new branch (-c / --create)
# git branch: check how many branch did you have
# git branch <branch name>: create new branch
# git branch --all: to check the remote branch as well
# git branch -: shortcut to get back to the previous branch ("-" here is to go back to the previous branch)
# git branch -d <branch name>: to delete the branch
PS*:
everytime you basically do the commit, lets say when you do the first commit, what Git does is, it creates something called a snapshot. So it will take a snapshot of your files, right? And then it will give it its own commit number. Thats your checksum basically. We have seen the hexacode values, right? so thats your checksum. And then there's one more thing called pointer.
For example: when we typed "git log"
# $ git log
commit f999bd77e14d1c10c0a28cd628591e666cd008c1 (HEAD -> feature2, origin/main, main)
Author: Ryan Maulana <ryan.maulana@lge.com>
Date:   Thu Jan 22 10:40:29 2026 +0700

    added the improvement from feature1 branch
# git merge feature2: merging the changes made by feature2 to the current branch
PS*:
lets say we have 4 branch as such main, feature1, feature2, feature3.
then we wanted to make those branches into one line.
this is a difference between merge and rebase.

## Merge vs Rebase: Key Differences

### git merge
- Creates a new "merge commit" that combines the branches
- Preserves the complete history of both branches
- Results in a branching history graph (shows where branches diverged and merged)
- Command: `git merge feature1`
- **Use when**: You want to preserve the context of when branches were created and merged

**History looks like:**
```
main:      A---B---C-------M (merge commit)
                 \         /
feature1:         D---E---F
```

### git rebase
- Replays your commits on top of another branch
- Creates a linear, straight-line history
- Rewrites commit history (changes commit hashes)
- Command: `git rebase main` (while on feature1)
- **Use when**: You want a clean, linear history

**History looks like:**
```
main:      A---B---C
                    \
feature1:            D'---E'---F' (commits rewritten)
```

## Handling Rebase Conflicts

When you encounter conflicts during a rebase (like "CONFLICT (content): Merge conflict in NOTES.MD"):

### Step 1: Check conflict status
```bash
git status
```

### Step 2: Resolve the conflict
Open the conflicted file and look for conflict markers:
```
<<<<<<< HEAD
(content from base branch)
=======
(content from your feature branch)
>>>>>>> feature1
```

**Options:**
- Keep HEAD content (base branch)
- Keep your feature branch content
- Combine both
- Write new content

Remove the markers (`<<<<<<<`, `=======`, `>>>>>>>`) after deciding.

### Step 3: Mark as resolved
```bash
git add <conflicted-file>
```

### Step 4: Continue the rebase
```bash
git rebase --continue
```

### Alternative: Abort if needed
```bash
git rebase --abort  # Returns to state before rebase
```

**Note**: You may need to resolve conflicts for each commit during the rebase process.

## Restoring Branches After Rebase

### Understanding the Problem
After rebasing, the branch pointer moves to the new rebased commits. The original commits still exist (temporarily) but are no longer referenced by the branch.

**Before rebase:**
```
main:      A---B---C
                 \
feature1:         D---E---F
```

**After rebase:**
```
main:      A---B---C---D'---E'---F' (feature1 now points here)
                 \
                  D---E---F (orphaned commits, still in reflog)
```

### Method 1: Using git reflog (Recently rebased)

Git keeps a reference log of where your branch pointers have been.
```bash
# View the reflog to find the commit before rebase
git reflog feature1

# Output example:
# a1b2c3d feature1@{0}: rebase finished: returning to refs/heads/feature1
# e4f5g6h feature1@{1}: checkout: moving from feature1 to main
# e4f5g6h feature1@{2}: commit: last commit before rebase
```
```bash
# Reset the branch to its original position
git reset --hard feature1@{2}

# Or use the specific commit hash
git reset --hard e4f5g6h
```

### Method 2: Using ORIG_HEAD (Immediately after rebase)

Git automatically stores the previous HEAD position in ORIG_HEAD.
```bash
# Immediately after rebase, restore the branch
git reset --hard ORIG_HEAD
```

### Method 3: If you have the original commit hash

If you noted the commit hash before rebasing:
```bash
# Reset to that specific commit
git reset --hard <commit-hash>
```

### Method 4: Restoring from remote (If pushed before rebase)

If the original branch was pushed to remote before rebasing:
```bash
# Fetch the remote state
git fetch origin

# Reset local branch to match remote
git reset --hard origin/feature1
```

### Example: Restoring Multiple Branches

If you rebased feature1, feature2, and feature3:
```bash
# For each branch
git checkout feature1
git reflog feature1
git reset --hard feature1@{<number-before-rebase>}

git checkout feature2
git reflog feature2
git reset --hard feature2@{<number-before-rebase>}

git checkout feature3
git reflog feature3
git reset --hard feature3@{<number-before-rebase>}
```

### Important Notes
- **Reflog entries expire**: By default, unreachable commits are kept for 30 days
- **Local only**: Reflog is local to your repository; it doesn't sync with remote
- **Act quickly**: The sooner you restore, the easier it is to find in reflog
- **Force push warning**: If you already pushed the rebased commits, restoring will create divergent histories

### Prevention: Create backup branches

Before rebasing, create backup branches:
```bash
# Before rebasing feature1
git branch feature1-backup

# Now you can safely rebase
git rebase main

# If you need to restore
git branch -D feature1
git checkout -b feature1 feature1-backup
```

## Practical Scenario: Updating Main Branch with Feature Branch

### Scenario: You're on `main` and want to get updates from `feature1`

**Recommended Approach: Use `git merge`**
```bash
# Make sure you're on main
git checkout main

# Merge feature1 into main
git merge feature1
```

### Why `git merge` is better for updating main:

1. **Main branch should preserve history** - `main` is typically a shared/stable branch
2. **Safe for collaboration** - Other developers might have `main` checked out
3. **Clear integration point** - The merge commit shows when `feature1` was integrated
4. **No history rewriting** - Doesn't change existing commits that others might depend on

### When to use `git rebase` instead:

Rebase is appropriate for the **opposite direction** - updating your feature branch:
```bash
# Update feature1 with latest changes from main
git checkout feature1
git rebase main  # This moves feature1's commits on top of main
```

This keeps `feature1` up-to-date with `main` before merging back.

### Complete Best Practice Workflow:
```bash
# 1. Update feature1 with latest main changes (optional but recommended)
git checkout feature1
git rebase main
# (resolve any conflicts if they occur)

# 2. Switch to main and merge feature1
git checkout main
git merge feature1

# 3. Push to remote
git push origin main

# 4. Optionally delete feature1 if work is complete
git branch -d feature1
```

### Visual Representation:

**Using merge (recommended for updating main):**
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

**If you used rebase (NOT recommended for main):**
```
Before:
main:      A---B---C
                 \
feature1:         D---E---F

After (git rebase feature1) - DON'T DO THIS on main:
main:      D'---E'---F'---A'---B'---C'
(This rewrites main's history - BAD for shared branches!)
```

### Quick Decision Guide:

**Updating main with feature changes:**
- ✅ **Use `git merge feature1`** (while on `main`)
- ❌ **Don't use `git rebase`** 

**Updating feature with main changes:**
- ✅ **Use `git rebase main`** (while on `feature1`)
- ✅ **Or use `git merge main`** (safer but creates merge commits)

**Rule of thumb**: 
- **Merge INTO main** - brings feature work into the main branch
- **Rebase feature ON main** - updates your feature with latest main changes

## Deleting Branches: -d vs -D

### Understanding the Error
When trying to delete a branch with unmerged commits:
```
warning: not deleting branch 'feature2' that is not yet merged to 'refs/heads/origin/main'
error: the branch 'feature2' is not fully merged
```

This happens because Git is protecting you from losing work.

### `-d` (lowercase) - Safe Delete
```bash
git branch -d feature2
```

**Characteristics:**
- **Safety check enabled**: Git refuses to delete if branch has unmerged commits
- **Protection mode**: Prevents accidental loss of work
- **Equivalent to**: `git branch --delete`

**When it works:**
- ✅ Branch has been fully merged into HEAD (current branch)
- ✅ Branch has been fully merged into upstream (if tracking remote)
- ❌ Fails if branch has unmerged work (shows error like above)

**Use when:**
- You want Git to verify the branch is merged before deleting
- You're not 100% sure if the work has been integrated
- You want to be safe and avoid mistakes

### `-D` (uppercase) - Force Delete
```bash
git branch -D feature2
```

**Characteristics:**
- **No safety checks**: Deletes branch regardless of merge status
- **Destructive**: Can lose commits if they're not merged anywhere
- **Equivalent to**: `git branch --delete --force`

**When it works:**
- ✅ Always works (unless branch doesn't exist)
- ⚠️ Will delete even if work is unmerged
- ⚠️ Commits may become orphaned and eventually garbage collected

**Use when:**
- You're absolutely certain you don't need the branch
- It's an experimental branch you want to discard
- You've already merged the work elsewhere

### Visual Example

**Scenario:**
```
main:      A---B---C
                 \
feature2:         D---E (unmerged commits)
```
```bash
# This will FAIL with safety error
git branch -d feature2
# Error: branch 'feature2' is not fully merged

# This will SUCCEED but commits D and E become orphaned
git branch -D feature2
# Deleted branch feature2 (was a1b2c3d)
```

### Comparison Table

| Feature | `-d` (safe) | `-D` (force) |
|---------|-------------|--------------|
| Safety check | Yes | No |
| Deletes merged branches | Yes | Yes |
| Deletes unmerged branches | No (shows error) | Yes |
| Risk of data loss | Low | High |
| Requires confirmation | Implicit (via merge check) | No |
| Use case | Standard cleanup | Discarding unwanted work |

### How to Fix the "not fully merged" Error

**Option 1: Merge first, then delete (Recommended if you want the work)**
```bash
# Switch to main
git checkout main

# Merge feature2 into main
git merge feature2

# Now safe delete will work
git branch -d feature2
```

**Option 2: Check what will be lost**
```bash
# See commits in feature2 that aren't in main
git log main..feature2

# View graphical representation
git log --oneline --graph --all
```

**Option 3: Force delete if you don't need it**
```bash
# Delete without merging (loses commits D and E)
git branch -D feature2
```

**Option 4: Archive instead of delete**
```bash
# Create a backup tag
git tag archive/feature2 feature2

# Now force delete
git branch -D feature2

# Later, if needed, restore from tag
git checkout -b feature2 archive/feature2
```

### Decision Tree
```
Do you need the changes from the branch?
│
├─ YES → Merge it first
│         git checkout main
│         git merge feature2
│         git branch -d feature2
│
├─ NOT SURE → Check commits first
│              git log main..feature2
│              (then decide)
│
└─ NO → Force delete
        git branch -D feature2
```

### Safety Best Practice

If uncertain, create a safety backup before force deleting:
```bash
# 1. Create backup tag
git tag backup-feature2 feature2

# 2. Force delete the branch
git branch -D feature2

# 3. If you need it back later
git checkout -b feature2-restored backup-feature2

# 4. Clean up backup tag when sure you don't need it
git tag -d backup-feature2
```

### Summary of -d vs -D

- **`-d`** = "Delete only if safe" (protective parent mode 👨‍👧)
  - Checks if merged
  - Prevents accidental data loss
  - Best for normal workflow cleanup

- **`-D`** = "Delete no matter what" (YOLO mode 🚀)
  - No safety checks
  - Can lose unmerged commits
  - Best for discarding experimental work

**Golden Rule**: When in doubt, use `-d` first. If Git complains, investigate why before using `-D`.

## Deleting Remote Branches

### Understanding Local vs Remote Branches

When you delete a branch locally with `git branch -d` or `git branch -D`, it **only deletes the local copy**. The remote repository is completely separate and unaffected.

**Current state after local deletion:**
```
Local Repository:               Remote Repository (origin):
* main                          * remotes/origin/main
  feature3                        remotes/origin/feature1 ← Still exists!
(feature1, feature2 deleted)      remotes/origin/feature2 ← Still exists!
                                  remotes/origin/feature3 ← Still exists!
```

### Why Remote Branches Persist

Git separates local and remote operations for several reasons:
- **Safety**: Prevents accidental deletion of shared work
- **Independence**: You can work offline without affecting others
- **Collaboration**: Other team members might still be using those branches
- **Intentional**: Deleting remote branches requires explicit action

### How to Delete Remote Branches

#### Method 1: Delete specific remote branch (Recommended)
```bash
# Syntax: git push <remote> --delete <branch-name>
git push origin --delete feature1
git push origin --delete feature2
```

**Output:**
```
To https://github.com/user/repo.git
 - [deleted]         feature1
```

#### Method 2: Using colon syntax (older style)
```bash
# Syntax: git push <remote> :<branch-name>
git push origin :feature1
git push origin :feature2
```

**Note**: The colon syntax means "push nothing to feature1", which deletes it.

#### Method 3: Delete multiple branches at once
```bash
# Delete several branches in one command
git push origin --delete feature1 feature2 feature3
```

### Clean Up Local Remote-Tracking References

After deleting remote branches, your local Git still has references to them. Clean them up:
```bash
# Remove stale remote-tracking branches
git fetch --prune

# Or shorter version
git fetch -p

# Or clean up specific remote
git remote prune origin
```

**What this does:**
- Removes local references to remote branches that no longer exist
- Cleans up `remotes/origin/feature1`, `remotes/origin/feature2`, etc.
- Does NOT delete any actual branches (local or remote)

### Complete Branch Deletion Workflow
```bash
# Step 1: Delete local branch
git branch -d feature1

# Step 2: Delete remote branch
git push origin --delete feature1

# Step 3: Clean up stale references
git fetch --prune

# Step 4: Verify everything is clean
git branch --all
```

### One-Liner: Delete Both Local and Remote
```bash
# Delete local AND remote in one command
git branch -d feature1 && git push origin --delete feature1

# Force delete local AND remote
git branch -D feature1 && git push origin --delete feature1
```

### Visual Workflow

**Initial state:**
```
Your Local Machine            Remote Server (GitHub/GitLab)
├─ main                       ├─ main
├─ feature1                   ├─ feature1
├─ feature2                   ├─ feature2
└─ feature3                   └─ feature3
```

**After `git branch -d feature1` (local only):**
```
Your Local Machine            Remote Server (GitHub/GitLab)
├─ main                       ├─ main
├─ feature2                   ├─ feature1 ← Still exists!
└─ feature3                   ├─ feature2
                              └─ feature3
```

**After `git push origin --delete feature1`:**
```
Your Local Machine            Remote Server (GitHub/GitLab)
├─ main                       ├─ main
├─ feature2                   ├─ feature2
└─ feature3                   └─ feature3
```

**After `git fetch --prune`:**
```
Local tracking refs cleaned up:
✓ remotes/origin/feature1 removed from local cache
```

### For Your Current Situation

Based on the screenshot showing `remotes/origin/feature1`, `feature2`, `feature3`:
```bash
# Delete remote feature1
git push origin --delete feature1

# Delete remote feature2
git push origin --delete feature2

# If you want to keep feature3 locally but delete remotely
git push origin --delete feature3

# Clean up local references
git fetch --prune

# Verify all remote branches are deleted
git branch --all
```

### Expected Output
```bash
$ git branch --all
* main
  feature3
  remotes/origin/main
```

### Quick Reference Table

| Task | Command |
|------|---------|
| View all branches (local + remote) | `git branch --all` or `git branch -a` |
| Delete local branch | `git branch -d feature1` |
| Force delete local branch | `git branch -D feature1` |
| Delete remote branch | `git push origin --delete feature1` |
| Delete multiple remote branches | `git push origin --delete feature1 feature2` |
| Clean up stale references | `git fetch --prune` or `git fetch -p` |
| Delete local + remote together | `git branch -d feature1 && git push origin --delete feature1` |

### Common Mistakes to Avoid

❌ **Mistake 1**: Thinking local deletion also deletes remote
```bash
git branch -d feature1  # Only deletes locally!
# Remote still exists!
```

❌ **Mistake 2**: Forgetting to prune after remote deletion
```bash
git push origin --delete feature1
git branch --all  # Still shows remotes/origin/feature1 (cached)
# Need to run: git fetch --prune
```

❌ **Mistake 3**: Deleting wrong branch
```bash
git push origin --delete main  # Oops! Deleted main branch!
# Be careful with branch names
```

### Safety Tips

1. **Double-check before deleting remote branches**
```bash
   # List all branches first
   git branch --all
   
   # Confirm which one you want to delete
   git push origin --delete feature1
```

2. **Create backups for important branches**
```bash
   # Backup before deletion
   git tag backup/feature1 feature1
   git push origin --delete feature1
```

3. **Communicate with team**
   - Let team members know before deleting shared remote branches
   - Check if anyone is actively working on the branch

### Recovery: Restoring Deleted Remote Branch

If you accidentally deleted a remote branch:
```bash
# If you still have it locally
git push origin feature1

# If you deleted locally too, but have a backup tag
git push origin backup/feature1:feature1

# If you have the commit hash
git push origin <commit-hash>:refs/heads/feature1
```

## Summary
- **Merge**: Preserves history, creates merge commits, branching graph
- **Rebase**: Rewrites history, creates linear timeline, cleaner but changes commits
- **Conflicts**: Resolve manually, mark with `git add`, then continue
- **Restore after rebase**: Use `git reflog` and `git reset --hard`
- **Updating main**: Always use `git merge` to bring in feature changes
- **Updating feature**: Use `git rebase` to sync with main changes
- **Deleting local branches**: Use `-d` for safety, `-D` only when certain
- **Deleting remote branches**: Use `git push origin --delete <branch>`
- **Clean up references**: Use `git fetch --prune` after remote deletions
- **Golden Rule**: Never rebase commits that have been pushed to a shared/public repository
- **Best Practice**: Use rebase for local cleanup, merge for integrating shared work
- **Safety tip**: Create backup branches/tags before deleting important work