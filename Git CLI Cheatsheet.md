# Git CLI Cheatsheet
- [[Git CLI Cheatsheet#Repo|Repo]]
- [[Git CLI Cheatsheet#Branch|Branch]]
- [[Git CLI Cheatsheet#Status|Status]]
- [[Git CLI Cheatsheet#Stage Add Unstage|State / Add / Unstage]]
- [[Git CLI Cheatsheet#Commit|Commit]]
- [[Git CLI Cheatsheet#Pull|Pull]]
- [[Git CLI Cheatsheet#Push|Push]]
- [[Git CLI Cheatsheet#Merge|Merge]]
- [[Git CLI Cheatsheet#Log Tree Compare|Log / Tree / Compare]]
- [[Git CLI Cheatsheet#Undo Reset Corrections|Undo / Reset / Corrections]]
- [[Git CLI Cheatsheet#Cherry Pick|Cherry Pick]]
- [[Git CLI Cheatsheet#Stash|Stash]]
- [[Git CLI Cheatsheet#Tags|Tags]]
- [[Git CLI Cheatsheet#Worktree|Worktree]]
- [[Git CLI Cheatsheet#Ignore|Ignore]]
- [[Git CLI Cheatsheet#fzf Commands|fzf Commands]]

---

## Repo
Create new Repo
```bash
$ git init
```

Clone Repo
```bash
$ git clone <url> <optional-local-name-of-repo>
```

Create a **backup** / zip archive of repo
```bash
$ git archive --format zip HEAD > <archive-name.zip>
```

---

## Branch
**List** Branches
```bash
$ git branch -a
```

**Create** Branch
```bash
$ git checkout -b <new-branch-name>
or
$ git branch <new-branch-name>
```

**Checkout remote** Branch locally
```bash
$ git switch <remote-branch-name> # without origin/ or remote/ prefix
or
$ git checkout -b <local-branch-name> origin/<remote-branch-name>
```

**Checkout** Branch that already exists locally
```bash
$ git checkout <branch-name>
```

**Delete** Branch locally
```bash
$ git branch -d <branch-name>
```

**Delete** Branch remote
```bash
$ git push origin --delete <remote-branch-name>
````

**List merged** Branches
```bash
$ git branch --merged
```

**List unmerged** Branches
```bash
$ git branch --no-merged
```

---

## Status
Check status of files
```bash
$ git status
or
$ git status -s # --short
```

List **unpushed commits**
```bash
$ git cherry -v
or
$ git log -p --decorate origin/<branch-name>..HEAD
```

View **changes in unpushed commits**
```bash
$ git log @{u}.. -p
```

Also see section [[Git CLI Cheatsheet#Diff|Diff]] below.

---

## Stage / Add / Unstage
**Stage all** files
```bash
$ git add --all
```

**Stage** one file 
```bash
$ git add <filename|ctrl+g+f>
```

**Interactive Stage** files and chunks
```bash
$ git add -i
```

Selection of files options:
- Space-separated numbers: `1 4 7`
- Select all: `*`
- Select interval of numbers: `3-8`
- Select from number to last: `5-`
- Unselect number: `-8`
- Combination of above: `1 3-6 8-`

**Stage chunk** of a file via `--patch`
```bash
$ git add -p <filename|ctrl+g+f>
```

**Stage chunks of a new file**
```bash
$ git add -N <new-file> # -N = --intent-to-add
$ git add -p
```

**Unstage all**
```bash
$ git reset
```

**Unstage** entire file
```bash
$ git reset -- <filename-of-staged-file>
```

**Unstage chunks** of file
```bash
$ git reset -p <filename>
```

---

## Commit
**Single Line** Message
```bash
$ git commit -m "<commit-message>"
```

**Multi-Line** Message and see what gets committed
```bash
$ git commit -v
```

---

## Pull
```bash
$ git pull
or
$ git pull origin <remote-branch-name>
```

## Push
Push existing branch to remote
```bash
$ git push
or
$ git push origin <branch-name>
```

Push new branch to remote
```bash
$ git push -u origin <new-branch-name>
or
$ git push -u origin HEAD
or
$ git push -u
```

---

## Merge
Merge branch in current branch
```bash
$ git merge <branch-name>
```

### Conflict Handling
**List conflicting files** and conflict markers
```bash
$ git diff --check
```

**See changes** in merge conflict
```bash
$ git diff
```

**Solve** conflicts in merge tool
```bash
$ git mergetool
```

**Show conflicting files** 
```bash
$ git diff --name-only --diff-filter=U
```

**About** merge
```bash
$ git merge --abort
```

**Reset file** back to unresolved state
```bash
$ git checkout -m FILE
```

---

## Log / Tree / Compare
**Simple** default git log adog: **a**ll **d**ecorate **o**neline **g**raph
```bash
$ git adog
or
$ git log --all --decorate --oneline --graph
```

**Custom ISO date** log
```bash
$ git lg
or
$ gitl
```

**Custom tree** log
```bash
$ git vtree
```

**Filter** log by date range and author
```bash
$ git lg --after="2022-02-08" --before="2022-02-12" --author="Florian Rehder"
```
(Shows log entries from `2022-02-08`, `2022-02-09`, `2022-02-10` and `2022-02-11`. Includes `--after` date but excludes `--before` date.)

See what **changed in a specific branch**
```bash
# without checkout
$ git log <master|branch-to-compare-with>..<branch-with-changes>

# with checkout
$ git checkout <branch-with-changes>
$ git cherry -v <master|branch-to-compare-with>
or
$ git checkout <branch-with-changes>
$ git log <master|branch-to-compare-with>..
```

---

## Undo / Reset / Corrections
**Discard everything** in head
```bash
$ git checkout .
or
$ git restore .
```

**Discard file**
```bash
$ git checkout -- <file>
or
$ git restore <file>
```

**Discard chunks** of file
```bash
$ git restore -p <file>
```

**Change commit message** of last commit
```bash
$ git commit --amend
or
$ git commit -v --amend
```

**Add file** to last commit without changing the message
```bash
$ git add <filename>
$ git commit --amend --no-edit
```

**Undo** last unpushed commit (*keep the work*)
```bash
$ git reset --soft HEAD~1
```

**Undo** last unpushed commit (*destroying the work*)
```bash
$ git reset --hard HEAD~1
```

Revert changes of specified commit (new commit with opposite changes)
```bash
$ git revert <commit>
```

**Revert last 3 changes after push**
```bash
$ git revert --no-commit HEAD~3..
$ git commit -m "stuff was reverted"
```

**Pushed to wrong branch**
```bash
$ git checkout wrong_branch
$ git revert commitsha1
$ git revert commitsha2
$ git checkout right_branch
$ git cherry-pick commitsha1
$ git cherry-pick commitsha2
```

---

## Cherry Pick

Cherry-pick commit and apply to current branch
```bash
$ git cherry-pick <commit-hash>
```

---

## Stash
Save changes to stash (cleanup working directory)
```bash
$ git stash
```

List stash entries
```bash
$ git stash list
```

Get and remove entry from stash
```bash
$ git stash pop <stash>
```

Delete entry from stash
```bash
$ git stash drop <stash>
```

View changes / diff of stash
```bash
# Most recent stash
$ git stash show -p

# Stash #4
$ git stash show -p stash@{4}
```

Diff stash against branch
```bash
$ git diff stash@{0} master
```

Abort *stash pop* which has conflicts without loosing stash item
```bash
$ git reset --merge
```

---

## Tags
Pull all tags from remote
```bash
$ git fetch --tags
```

List all tags (`ctrl+g+t`)
```bash
$ git tag
```

Create a tag
```bash
$ git tag <new-tag-name>
```

Push tag to remote
```bash
$ git push --tags
```

---

## Diff

Check changes in worktree
```bash
$ git diff
or
$ git difftool
```

Check what was staged
```bash
$ git diff --staged
or
$ git difftool --staged
```

Change difftool only for this one use
```bash
$ git difftool --extcmd=/opt/homebrew/bin/ksdiff
```

Compare file/View diff of file between two branches
```bash
$ git diff branchname1..master -- path/to/file.ext
or
$ git difftool branchname1..master -- path/to/file.ext
```

---

## Worktree
Create Worktree
```bash
$ cd master-somerepo

$ git worktree add ~/feature-branch feature/feature-branch
# git worktree add /path/to/save/worktree/to branchname-without-origin

$ cd ~/feature-branch
```

List Worktrees
```bash
$ cd master-somerepo

$ git worktree list
# /path/to/master-somerepo   hash [master]
# /path/to/feature-branch    hash [feature/feature-branch]
```

Remove Worktree
```bash
$ cd master-somerepo

$ git worktree list
# /path/to/master-somerepo   hash [master]
# /path/to/feature-branch    hash [feature/feature-branch]

$ git worktree remove feature-branch
```

Create Worktree from current Branch
```bash
$ cd some-repo
$ git checkout master
$ git worktree add -b feature/FE-123-some-feature ~/FE-123-some-feature HEAD # Create new worktree based on HEAD, which is master at the moment
$ cd ~/FE-123-some-feature
# Some work
$ git push -u origin HEAD # push branch "feature/FE-123-some-feature" to remote which doesn't exist yet
```
Create a new worktree, based on branch *master*.

---

## Ignore

Check what *gitignore* rule is applied to file:
```bash
$ git check-ignore -v --no-index path/to/file
```

---

## fzf Commands

| Function | Shortcut |
|---|---|
| Branches | `ctrl+g+b` |
| Files | `ctrl+g+f` |
| Logs / History | `ctrl+g+h` |
| Stashes | `ctrl+g+s` |
| Tags | `ctrl+g+t` |
| Branches for Switch | `ctrl+g+w`|

---

## References
- Add / Stage chunk: <https://stackoverflow.com/a/1085191/4637297>
- Interactive Staging: <https://git-scm.com/book/en/v2/Git-Tools-Interactive-Staging>
- Unstage: <https://stackoverflow.com/a/6919257/4637297>
- Discard changes: <https://stackoverflow.com/a/52713/4637297>
- Add file to previous commit: <https://stackoverflow.com/a/14096741/4637297>
- Delete last commit: <https://stackoverflow.com/a/3197432/4637297>
- Git Cheatsheet / Export archive: <https://www.gitfox.app/cheatsheet/>
- Git Log simple tree: <https://stackoverflow.com/a/9074343/4637297>
- Git Log fancy vtree: <https://stackoverflow.com/a/22481650/4637297>
- Check out remote branch: <https://stackoverflow.com/a/1783426/4637297>
- See what changed in a specific branch: <https://stackoverflow.com/a/4649377/4637297>
- See changes of unpushed commits: <https://stackoverflow.com/a/8182309/4637297>
- Interactive staging file selection: <https://coderwall.com/p/u4vjkw/git-add-interactive-or-how-to-make-amazing-commits>
- Git Merge - Show conflicting files: <https://stackoverflow.com/a/10874862/4637297>
- Stash diff: <https://stackoverflow.com/a/11671652/4637297>
- Diff file on two branches: <https://stackoverflow.com/a/4099805/4637297>
- Check ignore rules: <https://stackoverflow.com/a/49638370/4637297>
- Abort stash pop with conflicts: <https://stackoverflow.com/a/60444590/4637297>
- Reset merge conflict file back to unresolved: <https://stackoverflow.com/a/14409744/4637297>
- Revert last 3 commits after push: <https://stackoverflow.com/a/43081965>

---

### Visualizing Branch Topology
- https://stackoverflow.com/questions/1838873/visualizing-branch-topology-in-git#answer-7509303
- https://stackoverflow.com/questions/1838873/visualizing-branch-topology-in-git
- http://think-like-a-git.net/sections/graphs-and-git/visualizing-your-git-repository.html

### Git Cheatsheets
- https://gist.github.com/Fractalbonita/6faaaa1e2b181efdce1423fab4d18662#sharing--updating-projects