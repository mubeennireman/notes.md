# Git Fundamentals --- Complete Notes

> Learn one section at a time. Run the commands in a practice folder and
> check the result with `git status`.

## Contents

1.  Git and GitHub
2.  Installation and configuration
3.  Essential terms
4.  Create a repository
5.  Status, diff, stage, commit
6.  History
7.  `.gitignore`
8.  Undo changes
9.  Branches and merging
10. Merge conflicts
11. GitHub remotes
12. Clone, fetch, pull, push
13. README Markdown
14. Tags and stash
15. Troubleshooting
16. Daily workflow
17. Practice checklist
18. Quick reference

------------------------------------------------------------------------

# 1. Git and GitHub

**Git** is a version control system that records file changes and lets
you inspect history, restore work, and develop features separately.

**GitHub** is an online service for hosting Git repositories and
collaborating. Git can be used locally without GitHub.

# 2. Installation and configuration

Check installation:

``` bash
git --version
```

Set the identity recorded in commits:

``` bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global --list
```

Optional: use VS Code as the Git editor:

``` bash
git config --global core.editor "code --wait"
```

`--global` applies to your user account. Without it, a configuration
command run inside a repository can apply only to that repository.

# 3. Essential terms

  Term                   Meaning
  ---------------------- -------------------------------------------------
  Repository             Project and its Git history
  Working tree           Files you currently edit
  Staging area / index   Changes selected for the next commit
  Commit                 A snapshot in project history
  Branch                 A named line of development
  HEAD                   Usually the currently checked-out commit/branch
  Remote                 A named connection to another repository
  Clone                  Create a local copy of a repository
  Merge                  Combine another branch into the current branch
  Conflict               Changes Git cannot combine automatically
  `.git`                 Hidden directory containing Git metadata

# 4. Create a repository

Start a new project:

``` bash
mkdir git-practice
cd git-practice
git init
echo "# Git Practice" > README.md
git status
```

Clone an existing project instead:

``` bash
git clone https://github.com/OWNER/REPOSITORY.git
cd REPOSITORY
```

Replace the example URL with the real repository URL.

# 5. Status, diff, stage, commit

The normal flow is:

``` text
Edit files → stage changes → commit changes
Working tree → staging area → repository history
```

Check status:

``` bash
git status
```

See unstaged edits:

``` bash
git diff
```

Stage one file or all changes under the current directory:

``` bash
git add README.md
git add .
```

Review staged changes and commit them:

``` bash
git diff --staged
git commit -m "Add project README"
```

A commit includes staged changes, not automatically every edit in the
folder. Good messages describe the change, e.g. `Fix navbar spacing`,
not just `changes`.

# 6. History

``` bash
git log --oneline
git log
git log --oneline --graph --decorate --all
git show
git show COMMIT_ID
```

Use an actual commit ID from your history in place of `COMMIT_ID`.

# 7. `.gitignore`

A `.gitignore` file tells Git to ignore matching **untracked** files.
Example:

``` gitignore
node_modules/
.env
.env.*
dist/
build/
*.log
.DS_Store
Thumbs.db
```

If a file is already tracked, `.gitignore` does not untrack it. To stop
tracking it but keep the local file:

``` bash
git rm --cached path/to/file
git commit -m "Stop tracking ignored file"
```

Never commit passwords, API keys, or tokens. If a secret was committed,
deleting it in a later commit does not erase it from history; revoke or
rotate it.

# 8. Undo changes safely

Check `git status` before using undo commands.

Discard unstaged edits to a file (these edits can be lost):

``` bash
git restore README.md
```

Unstage a file but keep its edits:

``` bash
git restore --staged README.md
```

Correct the most recent commit, usually before sharing it:

``` bash
git add README.md
git commit --amend -m "Correct README title"
```

Amending changes the commit identity. Avoid rewriting commits already
shared with others unless you understand the impact.

Create a new commit that reverses an earlier commit:

``` bash
git revert COMMIT_ID
```

`git reset --hard` can permanently discard staged and unstaged work. Do
not use it casually.

# 9. Branches and merging

List branches and create/switch to a new branch:

``` bash
git branch
git switch -c feature/navbar
```

Switch branches:

``` bash
git switch main
```

Merge `feature/navbar` into the branch you are currently on:

``` bash
git switch main
git merge feature/navbar
```

Delete a merged local branch:

``` bash
git branch -d feature/navbar
```

The receiving branch is the branch currently checked out. The main
branch may be named `main` or `master`.

# 10. Merge conflicts

A conflict means Git cannot automatically reconcile overlapping changes.
Conflict markers look like:

``` text
<<<<<<< HEAD
Content from the current branch
=======
Content from the branch being merged
>>>>>>> feature/navbar
```

Edit the file to keep the intended final content and remove all markers.
Then test, stage, and finish the merge:

``` bash
git add path/to/conflicted-file
git commit
```

To abandon an active merge:

``` bash
git merge --abort
```

Check `git status` to confirm what state the repository is in.

# 11. GitHub remotes

A remote is a named connection to another repository. `origin` is the
common default name.

``` bash
git remote -v
git remote add origin https://github.com/USERNAME/REPOSITORY.git
```

If `origin` already exists, inspect it with `git remote -v`. To change
its URL:

``` bash
git remote set-url origin https://github.com/USERNAME/REPOSITORY.git
```

# 12. Clone, fetch, pull, push

Clone a repository:

``` bash
git clone https://github.com/USERNAME/REPOSITORY.git
```

Download remote updates without integrating them into your current
branch:

``` bash
git fetch origin
```

Fetch and integrate updates into the current branch:

``` bash
git pull
```

Upload commits:

``` bash
git push
```

For a new local branch, set its upstream on the first push:

``` bash
git push -u origin main
```

## First push for a new local project

Create an empty repository on GitHub, then run these inside the local
project:

``` bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/USERNAME/REPOSITORY.git
git push -u origin main
```

Replace the URL. If the project already has commits or a remote, inspect
`git status` and `git remote -v` first.

GitHub HTTPS operations generally use browser/device authentication or a
personal access token, not your account password. Never share a token.

# 13. README Markdown

A `README.md` explains a project's purpose, setup, and usage.

```` markdown
# Project Name

## Features

- Feature one
- Feature two

## Installation

```bash
npm install
npm run dev
```

## Tasks

- [x] Create repository
- [ ] Add screenshots
````

VS Code preview shortcuts: - Open preview: `Ctrl + Shift + V` - Open
preview beside editor: `Ctrl + K`, then `V`

GitHub-specific Markdown features may not render identically in every
editor.

# 14. Tags and stash

Tags label commits, often for releases:

``` bash
git tag
git tag -a v1.0.0 -m "Version 1.0.0"
git push origin v1.0.0
```

Stash temporarily shelves work:

``` bash
git stash
git stash -u
git stash list
git stash apply
git stash pop
```

`git stash -u` includes untracked files. `apply` keeps the stash entry;
`pop` attempts to apply it and remove the entry.

# 15. Troubleshooting

**`fatal: not a git repository`**\
You may be in the wrong folder. Use `pwd`, `ls -la`, and navigate to the
project folder.

**`Author identity unknown`**\
Set `user.name` and `user.email` using the commands in Section 2.

**`nothing to commit`**\
Check `git status`, `git diff`, and `git diff --staged`. The file may be
unchanged, ignored, or not staged.

**`remote origin already exists`**\
Run `git remote -v`. Use `git remote set-url origin URL` if the existing
URL needs changing.

**Push rejected because the remote has new commits**\
Do not immediately force-push. Inspect the message and repository state.
Often you need to integrate remote changes, for example:

``` bash
git pull --rebase
```

Resolve conflicts if needed, then push. If unsure, stop and inspect
`git status`.

# 16. Daily workflow

For a small change:

``` bash
git status
# Edit and test your files
git diff
git add path/to/file
git diff --staged
git commit -m "Describe the change"
git status
```

When working with GitHub, first ensure your branch is up to date
according to your team's workflow, then commit and push. Be careful with
reset, rebase, and force-push commands on shared work.

# 17. Practice checklist

Use a disposable practice folder.

-   [ ] Check the Git version and configure your identity.
-   [ ] Initialize a repository and create `README.md`.
-   [ ] Stage and commit the file.
-   [ ] Edit the file and inspect `git diff`.
-   [ ] Stage changes and inspect `git diff --staged`.
-   [ ] Make a second commit and inspect `git log --oneline`.
-   [ ] Add `.gitignore` and ignore a sample `.log` file.
-   [ ] Create a feature branch, edit a file, and commit.
-   [ ] Merge the feature branch into `main`.
-   [ ] Create a GitHub repository and push your project.
-   [ ] Clone the remote into another folder.
-   [ ] Practise a merge conflict in a disposable repository.

Before running each command, predict what it will do. Afterwards, use
`git status` or `git log` to check your understanding.

# 18. Quick reference

  Goal                        Command
  --------------------------- -----------------------------
  Version                     `git --version`
  Initialize                  `git init`
  Clone                       `git clone URL`
  Status                      `git status`
  Unstaged diff               `git diff`
  Stage file                  `git add FILE`
  Commit                      `git commit -m "message"`
  History                     `git log --oneline`
  Branch list                 `git branch`
  Create/switch branch        `git switch -c BRANCH`
  Switch branch               `git switch BRANCH`
  Merge into current branch   `git merge BRANCH`
  Remotes                     `git remote -v`
  Fetch                       `git fetch`
  Pull                        `git pull`
  Push                        `git push`
  Unstage                     `git restore --staged FILE`
  Discard unstaged edits      `git restore FILE`
  Stash                       `git stash`
  Inspect commit              `git show`

## Recommended learning order

1.  `status`, `add`, `commit`, `log`
2.  `diff` and `.gitignore`
3.  branches and merges
4.  GitHub remotes, `pull`, and `push`
5.  conflicts, restore, revert, and stash

Focus on understanding and practising the basic workflow before trying
to memorize every command.
