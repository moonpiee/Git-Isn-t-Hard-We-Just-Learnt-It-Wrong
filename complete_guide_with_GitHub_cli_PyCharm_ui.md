# 🐙 GitHub & Git — Complete Reference Guide
### End-to-End: Git CLI + PyCharm UI

> **How to use this guide:** Every scenario shows the terminal command(s) first, then the exact PyCharm UI equivalent. You never need to guess — just find your scenario.

---

## 📑 Table of Contents

1. [One-Time Setup](#1-one-time-setup)
2. [Starting a Project](#2-starting-a-project)
3. [Daily Development Workflow](#3-daily-development-workflow)
4. [Branching](#4-branching)
5. [Staging & Committing](#5-staging--committing)
6. [Pushing & Pulling](#6-pushing--pulling)
7. [Merging](#7-merging)
8. [Rebasing](#8-rebasing)
9. [Stashing](#9-stashing)
10. [Undoing & Fixing Mistakes](#10-undoing--fixing-mistakes)
11. [Viewing History & Diffs](#11-viewing-history--diffs)
12. [Remote Management](#12-remote-management)
13. [Pull Requests (PRs)](#13-pull-requests-prs)
14. [Tags & Releases](#14-tags--releases)
15. [Cherry-Picking](#15-cherry-picking)
16. [Conflict Resolution](#16-conflict-resolution)
17. [Submodules](#17-submodules)
18. [Git Aliases & Config Tips](#18-git-aliases--config-tips)
19. [Advanced / Power Tools](#19-advanced--power-tools)
20. [Emergency Cheatsheet](#20-emergency-cheatsheet)

---

## 1. One-Time Setup

### Scenario: First time using Git on your machine

#### 🖥️ Terminal
```bash
git config --global user.name "Your Name"
git config --global user.email "you@apple.com"
git config --global core.editor "vim"          # or "nano", "code --wait"
git config --global init.defaultBranch main
git config --list                               # verify all settings
```

#### 🧠 PyCharm UI
> PyCharm uses your system Git automatically. To verify/change:
- `PyCharm → Settings (⌘,) → Version Control → Git`
  - Confirm path to `git` executable (auto-detected)
- `Settings → Version Control → Git → Configure Git`
  - Set user name/email if needed

---

### Scenario: Set up SSH key for GitHub

#### 🖥️ Terminal
```bash
ssh-keygen -t ed25519 -C "you@apple.com"       # generate key
eval "$(ssh-agent -s)"                          # start agent
ssh-add ~/.ssh/id_ed25519                       # add key to agent
cat ~/.ssh/id_ed25519.pub                       # copy this → paste in GitHub Settings → SSH Keys
ssh -T git@github.com                           # test connection
```

#### 🧠 PyCharm UI
- `Settings → Version Control → GitHub`
- Click **+** → **Log In via GitHub** (OAuth) or paste token
- Or: `Add Account → Log In with Token`

---

## 2. Starting a Project

### Scenario: Clone an existing repository

#### 🖥️ Terminal
```bash
git clone https://github.com/org/repo.git
git clone git@github.com:org/repo.git           # via SSH
git clone https://github.com/org/repo.git mydir # clone into specific folder
git clone --depth 1 https://github.com/org/repo.git  # shallow clone (fast)
```

#### 🧠 PyCharm UI
- `File → New → Project from Version Control`
- Paste the repo URL → Choose local directory → Click **Clone**
- Or: Welcome screen → **Get from VCS**

---

### Scenario: Initialize a new local repo

#### 🖥️ Terminal
```bash
cd my-project
git init
git remote add origin git@github.com:you/my-project.git
git add .
git commit -m "Initial commit"
git push -u origin main
```

#### 🧠 PyCharm UI
- Open project folder in PyCharm
- `VCS → Create Git Repository` → select the folder
- Then: `Git → Manage Remotes` → Add origin URL
- Use the **Commit** panel to stage + commit
- Then **Push** (⌘⇧K)

---

## 3. Daily Development Workflow

### Scenario: Check current status of your working tree

#### 🖥️ Terminal
```bash
git status                      # full status
git status -s                   # short/compact view
```

#### 🧠 PyCharm UI
- Bottom-left: **Git** tab → **Local Changes** sub-tab
- Color coding: green = staged, red/orange = modified/untracked
- Top-right status bar shows current branch

---

### Scenario: See what files have changed

#### 🖥️ Terminal
```bash
git diff                        # unstaged changes
git diff --staged               # staged changes (what will be committed)
git diff HEAD                   # all changes vs last commit
git diff branch1..branch2       # compare two branches
```

#### 🧠 PyCharm UI
- `Git` tool window → **Local Changes** → double-click any file → diff view opens
- Or: right-click file → `Git → Compare with...`
- `Git → Compare with Branch` to compare branches

---

## 4. Branching

### Scenario: Create a new branch and switch to it

#### 🖥️ Terminal
```bash
git branch feature/my-feature          # create branch
git checkout feature/my-feature        # switch to it
# OR in one step (preferred):
git checkout -b feature/my-feature
# Modern Git (2.23+):
git switch -c feature/my-feature
```

#### 🧠 PyCharm UI
- Bottom-right corner: click the **branch name**
- Select **New Branch** → type name → ✅ "Checkout branch"
- Or: `Git → Branches → + New Branch`

---

### Scenario: List all branches

#### 🖥️ Terminal
```bash
git branch                      # local branches
git branch -r                   # remote branches
git branch -a                   # all (local + remote)
git branch -v                   # with last commit message
```

#### 🧠 PyCharm UI
- Bottom-right: click branch name → full list appears
- Shows **Local Branches** and **Remote Branches** sections

---

### Scenario: Switch to an existing branch

#### 🖥️ Terminal
```bash
git checkout main
git switch main                 # modern syntax
```

#### 🧠 PyCharm UI
- Bottom-right: click current branch name → click the target branch → **Checkout**

---

### Scenario: Delete a branch

#### 🖥️ Terminal
```bash
git branch -d feature/my-feature       # delete (safe — won't delete unmerged)
git branch -D feature/my-feature       # force delete (even if unmerged)
git push origin --delete feature/my-feature  # delete remote branch
```

#### 🧠 PyCharm UI
- Bottom-right branch list → hover target branch → **Delete**
- For remote: hover under **Remote Branches** → **Delete**

---

### Scenario: Rename a branch

#### 🖥️ Terminal
```bash
git branch -m old-name new-name        # rename local branch
git push origin --delete old-name      # delete old remote
git push origin new-name               # push new name
git push origin -u new-name            # set upstream
```

#### 🧠 PyCharm UI
- Branch list → right-click branch → **Rename**

---

### Scenario: Track a remote branch locally

#### 🖥️ Terminal
```bash
git checkout -b feature/x origin/feature/x     # create + track
git branch --set-upstream-to=origin/feature/x  # set tracking on existing branch
```

#### 🧠 PyCharm UI
- Remote branch → **Checkout** — PyCharm auto-sets tracking

---

## 5. Staging & Committing

### Scenario: Stage files for commit

#### 🖥️ Terminal
```bash
git add filename.py             # stage a specific file
git add src/                    # stage a directory
git add .                       # stage all changes in current dir
git add -p                      # interactive: stage hunks (partial file)
git add -u                      # stage all tracked modified files (not untracked)
```

#### 🧠 PyCharm UI
- **Commit** tool window (⌘K) → check/uncheck files
- Right-click a file → **+** (Stage) or use the checkboxes
- Click **Show Diff** to review before staging

---

### Scenario: Unstage a file

#### 🖥️ Terminal
```bash
git restore --staged filename.py       # modern (Git 2.23+)
git reset HEAD filename.py             # older equivalent
```

#### 🧠 PyCharm UI
- Commit tool window → uncheck the file to unstage

---

### Scenario: Commit staged changes

#### 🖥️ Terminal
```bash
git commit -m "feat: add login validation"
git commit                              # opens editor for detailed message
git commit -am "fix: typo"             # stage all tracked + commit in one step
```

#### 🧠 PyCharm UI
- `⌘K` → write commit message → click **Commit**
- Or: `Git → Commit...`

---

### Scenario: Amend the last commit (message or files)

#### 🖥️ Terminal
```bash
git commit --amend -m "corrected message"      # change message
git add forgotten_file.py
git commit --amend --no-edit               # add file, keep message
```

> ⚠️ Never amend commits already pushed to a shared remote.

#### 🧠 PyCharm UI
- Commit tool window → check **Amend** checkbox (bottom of panel)
- Edit message if needed → **Commit**

---

### Scenario: Write a good commit message

```
<type>(<scope>): <short summary>

<body — explain WHY, not what>

<footer: refs, breaking changes>
```

**Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

**Example:**
```
feat(auth): add biometric login support

Adds Face ID / Touch ID fallback for users on iOS 17+.
Resolves edge case where token expired silently.

Closes #234
```

---

## 6. Pushing & Pulling

### Scenario: Push your branch to remote

#### 🖥️ Terminal
```bash
git push                                        # push current branch (if tracking set)
git push origin feature/my-feature             # explicit push
git push -u origin feature/my-feature          # push + set upstream tracking
git push --force-with-lease                    # safe force push (after rebase)
git push --force                               # ⚠️ force push — dangerous on shared branches
```

#### 🧠 PyCharm UI
- `⌘⇧K` → Push dialog → select remote/branch → **Push**
- Check **Force push** only if you know what you're doing

---

### Scenario: Pull latest changes

#### 🖥️ Terminal
```bash
git pull                                # pull + merge (default)
git pull --rebase                       # pull + rebase (cleaner history)
git pull origin main                    # explicitly pull main from origin
git fetch && git merge origin/main      # manual: fetch then merge
```

#### 🧠 PyCharm UI
- `⌘T` or `Git → Pull`
- Choose: **Merge** or **Rebase** strategy in the dialog

---

### Scenario: Fetch without merging

#### 🖥️ Terminal
```bash
git fetch                       # fetch all remotes
git fetch origin                # fetch specific remote
git fetch --prune               # fetch + remove deleted remote branches
```

#### 🧠 PyCharm UI
- `Git → Fetch` — updates remote tracking branches without touching your working tree

---

## 7. Merging

### Scenario: Merge a feature branch into main

#### 🖥️ Terminal
```bash
git checkout main
git merge feature/my-feature                   # merge (creates merge commit)
git merge --no-ff feature/my-feature           # always create merge commit (keeps history)
git merge --squash feature/my-feature          # squash all commits into one
git merge --abort                              # abort an in-progress merge
```

#### 🧠 PyCharm UI
- Switch to `main`
- Bottom-right branch list → find `feature/my-feature` → **Merge into Current**
- Merge commit message appears automatically

---

### Scenario: Fast-forward merge

#### 🖥️ Terminal
```bash
git merge --ff-only feature/my-feature     # only merges if FF is possible
```

#### 🧠 PyCharm UI
- No direct FF-only toggle, but if no divergence exists, PyCharm performs FF by default

---

## 8. Rebasing

### Scenario: Rebase your feature branch onto main

#### 🖥️ Terminal
```bash
git checkout feature/my-feature
git rebase main                         # rebase onto main
git rebase origin/main                  # rebase onto remote main
git rebase --abort                      # cancel rebase
git rebase --continue                   # after resolving conflicts, continue
git rebase --skip                       # skip a conflicting commit
```

#### 🧠 PyCharm UI
- `Git → Rebase...`
- Or: branch list → right-click `main` → **Rebase Current Branch onto This**

---

### Scenario: Interactive rebase (squash, reorder, edit commits)

#### 🖥️ Terminal
```bash
git rebase -i HEAD~3            # interactively edit last 3 commits
```
In the editor that opens:
```
pick abc1234 feat: add login
squash def5678 fix: login typo     # squash into previous
reword ghi9012 chore: cleanup      # change message
drop jkl3456 temp debug log        # remove entirely
```

#### 🧠 PyCharm UI
- `Git → Log` tab → select commits (hold Shift/Cmd) → right-click → **Interactively Rebase from Here**
- Drag to reorder, toggle squash/fixup/drop/reword per commit

---

## 9. Stashing

### Scenario: Temporarily save uncommitted changes to switch context

#### 🖥️ Terminal
```bash
git stash                           # stash all changes
git stash push -m "WIP: login fix"  # stash with a name
git stash list                      # see all stashes
git stash pop                       # apply most recent + remove from stash
git stash apply stash@{2}           # apply specific stash (keep in list)
git stash drop stash@{0}            # delete a stash
git stash clear                     # delete ALL stashes
git stash branch feature/new stash@{0}  # create branch from stash
```

#### 🧠 PyCharm UI
- `Git → Stash Changes` → name it → **Create Stash**
- `Git → Unstash Changes` → select stash → **Apply Stash** or **Pop Stash**
- Or via **Git** tool window → **Local Changes** → stash icon (📦)

---

## 10. Undoing & Fixing Mistakes

### Scenario: Discard all uncommitted changes to a file

#### 🖥️ Terminal
```bash
git checkout -- filename.py         # old syntax
git restore filename.py             # modern (Git 2.23+)
git restore .                       # discard ALL unstaged changes
```

#### 🧠 PyCharm UI
- Right-click file → `Git → Rollback` (⌘Z if just edited, or Rollback for Git-level undo)
- Or: Local Changes → right-click → **Rollback**

---

### Scenario: Undo the last commit (keep changes in working tree)

#### 🖥️ Terminal
```bash
git reset --soft HEAD~1         # undo commit, keep changes staged
git reset --mixed HEAD~1        # undo commit, keep changes unstaged (default)
git reset --hard HEAD~1         # undo commit, DISCARD all changes ⚠️
```

#### 🧠 PyCharm UI
- `Git → Log` → right-click latest commit → **Undo Commit**
- PyCharm keeps changes in your working tree (soft reset)

---

### Scenario: Revert a pushed commit (safe — creates a new "undo" commit)

#### 🖥️ Terminal
```bash
git revert abc1234              # creates a new commit that undoes abc1234
git revert HEAD                 # revert most recent commit
git revert HEAD~3..HEAD         # revert last 3 commits
git revert --no-commit abc1234  # stage the revert without committing yet
```

#### 🧠 PyCharm UI
- `Git → Log` → right-click commit → **Revert Commit**
- Review changes → **Commit**

---

### Scenario: Recover a deleted branch or lost commit

#### 🖥️ Terminal
```bash
git reflog                          # shows ALL recent HEAD movements
git checkout -b recovered abc1234   # restore from that commit hash
git fsck --lost-found               # find dangling commits
```

#### 🧠 PyCharm UI
- No direct reflog UI — use Terminal (`⌥F12`) within PyCharm for `git reflog`

---

### Scenario: Reset a file to match a specific commit

#### 🖥️ Terminal
```bash
git checkout abc1234 -- filename.py     # restore file from commit abc1234
git restore --source=abc1234 filename.py  # modern equivalent
```

#### 🧠 PyCharm UI
- `Git → Log` → find the commit → right-click → **Checkout Revision** (for whole tree)
- Or: in the diff view, use the **Restore** arrow to bring back specific lines

---

### Scenario: Remove untracked files

#### 🖥️ Terminal
```bash
git clean -n                    # dry run — shows what WOULD be deleted
git clean -f                    # delete untracked files
git clean -fd                   # delete untracked files + directories
git clean -fdx                  # delete untracked + ignored files
```

#### 🧠 PyCharm UI
- `Git → Local Changes` → right-click untracked files → **Delete** (careful!)
- No bulk "git clean" button — use Terminal for safety

---

## 11. Viewing History & Diffs

### Scenario: View commit log

#### 🖥️ Terminal
```bash
git log                                  # full log
git log --oneline                        # compact one-line view
git log --oneline --graph --all          # visual branch graph
git log --author="Chandra"               # filter by author
git log --since="2 weeks ago"            # filter by time
git log --grep="login"                   # search commit messages
git log -p filename.py                   # show diffs for a file
git log --follow filename.py             # follow renames
git shortlog -sn                         # summarize by author
```

#### 🧠 PyCharm UI
- `Git → Log` tab (or `⌘9`) — full visual log with graph
- Filter bar at top: by branch, user, date, or text
- Click any commit to see changed files + full diff
- Right-click commit for context actions (revert, branch, cherry-pick...)

---

### Scenario: See who changed each line (blame)

#### 🖥️ Terminal
```bash
git blame filename.py                    # annotate every line
git blame -L 20,40 filename.py           # lines 20–40 only
```

#### 🧠 PyCharm UI
- Open file → right-click in editor gutter → **Annotate with Git Blame**
- Each line shows author + timestamp in the gutter
- Click annotation → jump to that commit in the Log

---

### Scenario: See what changed in a specific commit

#### 🖥️ Terminal
```bash
git show abc1234                         # show commit details + diff
git show abc1234 --name-only             # just filenames changed
git show abc1234:filename.py             # view file content at that commit
```

#### 🧠 PyCharm UI
- `Git → Log` → click the commit → bottom panel shows changed files + diff

---

### Scenario: Compare two branches

#### 🖥️ Terminal
```bash
git diff main..feature/my-feature               # diff between tips
git diff main...feature/my-feature              # diff from common ancestor
git log main..feature/my-feature --oneline      # commits in feature not in main
```

#### 🧠 PyCharm UI
- `Git → Compare with Branch` (from branch list, right-click)
- Or: Log tab → select both branches → **Show Diff**

---

## 12. Remote Management

### Scenario: View configured remotes

#### 🖥️ Terminal
```bash
git remote -v                   # list remotes with URLs
git remote show origin          # full info about 'origin'
```

#### 🧠 PyCharm UI
- `Git → Manage Remotes` → lists all remotes

---

### Scenario: Add / change / remove a remote

#### 🖥️ Terminal
```bash
git remote add upstream https://github.com/original/repo.git   # add
git remote set-url origin git@github.com:you/repo.git           # change URL
git remote remove upstream                                       # remove
git remote rename origin old-origin                             # rename
```

#### 🧠 PyCharm UI
- `Git → Manage Remotes` → **+** to add, pencil to edit, **–** to remove

---

### Scenario: Sync a fork with the upstream original

#### 🖥️ Terminal
```bash
git remote add upstream https://github.com/original/repo.git
git fetch upstream
git checkout main
git merge upstream/main          # or: git rebase upstream/main
git push origin main
```

#### 🧠 PyCharm UI
- Add upstream via `Git → Manage Remotes`
- `Git → Fetch` to get upstream changes
- Merge/rebase via the branch menu

---

## 13. Pull Requests (PRs)

### Scenario: Create a PR from the terminal (GitHub CLI)

#### 🖥️ Terminal
```bash
# Install: brew install gh
gh pr create --title "feat: add login" --body "Closes #42" --base main
gh pr create --draft                                    # open as draft
gh pr list                                              # list open PRs
gh pr checkout 123                                      # checkout a PR locally
gh pr review 123 --approve                              # approve
gh pr merge 123 --squash                                # merge with squash
gh pr status                                            # your PR status
```

#### 🧠 PyCharm UI
- Requires GitHub plugin (usually pre-installed)
- Top menu: `Git → GitHub → Create Pull Request`
- Or: push branch → PyCharm shows **Create Pull Request** notification
- `Git → GitHub → View Pull Requests` to list/review

---

### Scenario: Review a PR locally

#### 🖥️ Terminal
```bash
gh pr checkout 123                  # check out PR branch
git diff main...HEAD                # see what the PR changes
git log main..HEAD --oneline        # commits in the PR
```

#### 🧠 PyCharm UI
- `Git → GitHub → View Pull Requests` → select PR → **Checkout**
- Review diffs inline in PyCharm's diff viewer

---

### Scenario: Update your PR branch after main has moved

#### 🖥️ Terminal
```bash
git checkout feature/my-feature
git fetch origin
git rebase origin/main              # cleanest approach
git push --force-with-lease         # push updated branch
```

#### 🧠 PyCharm UI
- Switch to your feature branch
- `Git → Rebase onto...` → select `origin/main`
- Then **Push** with Force push option (select `--force-with-lease`)

---

## 14. Tags & Releases

### Scenario: Create and push a tag

#### 🖥️ Terminal
```bash
git tag v1.0.0                          # lightweight tag
git tag -a v1.0.0 -m "Release 1.0"     # annotated tag (preferred)
git tag -a v1.0.0 abc1234 -m "msg"     # tag a specific commit
git push origin v1.0.0                 # push single tag
git push origin --tags                 # push all tags
```

#### 🧠 PyCharm UI
- `Git → Log` → right-click commit → **New Tag** → name it → **Push tag to remote**
- Or: `Git → Tags` to manage existing tags

---

### Scenario: Delete a tag

#### 🖥️ Terminal
```bash
git tag -d v1.0.0                   # delete local tag
git push origin --delete v1.0.0     # delete remote tag
```

#### 🧠 PyCharm UI
- `Git → Log` → right-click tag → **Delete Tag**

---

## 15. Cherry-Picking

### Scenario: Apply a specific commit from another branch

#### 🖥️ Terminal
```bash
git cherry-pick abc1234                         # apply one commit
git cherry-pick abc1234 def5678                 # apply multiple commits
git cherry-pick abc1234..ghi9012                # apply a range
git cherry-pick --no-commit abc1234             # apply without auto-committing
git cherry-pick --abort                         # cancel in-progress cherry-pick
git cherry-pick --continue                      # after resolving conflicts
```

#### 🧠 PyCharm UI
- `Git → Log` → find the commit → right-click → **Cherry-Pick**
- If conflict: PyCharm shows the conflict resolution dialog automatically

---

## 16. Conflict Resolution

### Scenario: You have a merge/rebase conflict

#### 🖥️ Terminal
```bash
git status                          # shows conflicted files
# Edit files — look for:
# <<<<<<< HEAD
# your changes
# =======
# their changes
# >>>>>>> feature/branch

git add resolved_file.py            # mark as resolved
git commit                          # complete the merge
# Or for rebase:
git rebase --continue
```

#### 🧠 PyCharm UI
- PyCharm automatically detects conflicts and shows a **Merge Conflicts** notification
- Click **Resolve** → opens 3-panel merge tool:
  - **Left:** Your changes (HEAD)
  - **Center:** Result (edit here)
  - **Right:** Their changes (incoming)
- Use `>>` / `<<` arrows to accept hunks, or type directly in center
- Click **Apply** when done → file is marked resolved

---

### Scenario: Accept "ours" or "theirs" entirely

#### 🖥️ Terminal
```bash
git checkout --ours filename.py         # keep your version
git checkout --theirs filename.py       # keep their version
git add filename.py
```

#### 🧠 PyCharm UI
- In the merge tool: top-left accepts yours entirely, top-right accepts theirs entirely
- Or: right-click file in Local Changes → **Accept Ours / Accept Theirs**

---

## 17. Submodules

### Scenario: Add a submodule

#### 🖥️ Terminal
```bash
git submodule add https://github.com/org/lib.git libs/lib
git submodule init
git submodule update
git submodule update --init --recursive     # init + update all nested
```

#### 🧠 PyCharm UI
- Limited submodule support — PyCharm recognizes `.gitmodules` and shows submodule repos
- Use Terminal within PyCharm for submodule commands

---

### Scenario: Clone a repo that has submodules

#### 🖥️ Terminal
```bash
git clone --recurse-submodules https://github.com/org/repo.git
# If you already cloned without submodules:
git submodule update --init --recursive
```

#### 🧠 PyCharm UI
- `Get from VCS` → clone — PyCharm will prompt to init submodules after clone

---

## 18. Git Aliases & Config Tips

### Useful aliases to add once

#### 🖥️ Terminal
```bash
git config --global alias.st "status -s"
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.co "checkout"
git config --global alias.br "branch"
git config --global alias.unstage "restore --staged"
git config --global alias.last "log -1 HEAD"
git config --global alias.pushf "push --force-with-lease"
```

Usage:
```bash
git st       # short status
git lg       # pretty visual log
git co main  # checkout main
```

---

### Useful global config

```bash
git config --global pull.rebase true           # always rebase on pull
git config --global merge.tool vimdiff         # default merge tool
git config --global core.autocrlf input        # handle line endings (macOS/Linux)
git config --global rerere.enabled true        # remember conflict resolutions
git config --global fetch.prune true           # auto-prune on fetch
git config --global push.default current       # push current branch by name
```

---

## 19. Advanced / Power Tools

### Scenario: Binary search to find which commit introduced a bug

#### 🖥️ Terminal
```bash
git bisect start
git bisect bad                      # current commit is bad
git bisect good v1.0.0              # this tag/commit was known good
# Git checks out middle commit — test it, then:
git bisect good                     # or: git bisect bad
# Repeat until Git identifies the culprit commit
git bisect reset                    # return to original state
```

#### 🧠 PyCharm UI
- No direct bisect UI — use Terminal panel (`⌥F12`)

---

### Scenario: Search the entire codebase history for a string

#### 🖥️ Terminal
```bash
git log -S "password" --oneline             # commits that added/removed this string
git log -G "regex_pattern" --oneline        # commits matching regex in diff
git grep "TODO" $(git rev-list --all)       # search all commits for a string
```

#### 🧠 PyCharm UI
- `Find in Files` (⌘⇧F) for current working tree
- For history: `Git → Log` → search bar at top

---

### Scenario: Temporarily work on a commit without creating a branch (detached HEAD)

#### 🖥️ Terminal
```bash
git checkout abc1234                    # enter detached HEAD state
# Explore, test, or patch
git checkout -b hotfix/temp-fix         # save your work as a new branch before leaving!
```

#### 🧠 PyCharm UI
- Log → right-click commit → **Checkout Revision**
- Bottom-right shows `HEAD (detached)` — create a branch before committing anything

---

### Scenario: Patch/apply diffs across repos

#### 🖥️ Terminal
```bash
git diff > my-changes.patch             # export changes as patch
git apply my-changes.patch             # apply patch elsewhere
git format-patch HEAD~3                # email-style patches for last 3 commits
git am patches/*.patch                 # apply a mailbox of patches
```

---

### Scenario: Squash all commits on a branch before merging

#### 🖥️ Terminal
```bash
git checkout main
git merge --squash feature/my-feature
git commit -m "feat: implement full login flow"
```

#### 🧠 PyCharm UI
- `Git → Merge` → check **Squash commit** option

---

### Scenario: Large file storage (Git LFS)

#### 🖥️ Terminal
```bash
git lfs install
git lfs track "*.psd"
git add .gitattributes
git add design.psd
git commit -m "add LFS-tracked design file"
git push
```

#### 🧠 PyCharm UI
- PyCharm supports LFS files transparently once `git lfs` is installed

---

### Scenario: Partial staging (stage specific lines, not whole file)

#### 🖥️ Terminal
```bash
git add -p filename.py      # interactive hunk-by-hunk staging
# y = stage hunk, n = skip, s = split, e = edit manually
```

#### 🧠 PyCharm UI
- Open the **Commit** dialog (⌘K) → click file → see the diff
- Right-click specific lines in the diff → **Stage Selected Lines**

---

## 20. Emergency Cheatsheet

| Situation | Git Command | PyCharm |
|---|---|---|
| "I committed to main by mistake!" | `git reset --soft HEAD~1` → create branch | Log → Undo Commit |
| "I pushed to main by mistake!" | `git revert HEAD` + `git push` | Log → Revert Commit |
| "I accidentally deleted a branch!" | `git reflog` → find hash → `git checkout -b name hash` | Terminal: git reflog |
| "My rebase is a mess!" | `git rebase --abort` | Git → Abort Rebase |
| "I need to undo a bad merge!" | `git merge --abort` (if in progress) or `git reset --hard ORIG_HEAD` | Git → Abort Merge |
| "I lost my uncommitted changes!" | `git fsck --lost-found` (only if staged once) | — |
| "Wrong branch!" | `git stash` → switch → `git stash pop` | Stash Changes → switch → Unstash |
| "Pull created a monster merge commit!" | `git pull --rebase` next time | Pull dialog → choose Rebase |
| "I need to see what I did last week" | `git log --author=me --since="1 week ago"` | Log → filter by author + date |
| "Revert one file to last commit" | `git restore filename.py` | Right-click → Git → Rollback |
| "Delete all local untracked files" | `git clean -fd` (dry-run first: `-n`) | Terminal |
| "Force push safely after rebase" | `git push --force-with-lease` | Push → Force Push (w/ lease) |

---

## 🔑 Key Concepts Summary

| Concept | What It Means |
|---|---|
| **HEAD** | Pointer to your current commit / branch tip |
| **Origin** | Default name for your remote (GitHub) |
| **Upstream** | The original repo you forked from |
| **Staging area (index)** | Where you put changes before committing |
| **Working tree** | Your actual files on disk |
| **Fast-forward** | Merge that just moves a pointer, no merge commit needed |
| **Detached HEAD** | You're on a commit, not a branch |
| **Reflog** | Local history of every HEAD movement — your safety net |
| **force-with-lease** | Force push only if nobody else pushed since your last fetch |

---

*Last updated: May 2026 | Git 2.43+ and PyCharm 2024.x+*
