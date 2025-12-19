# Git Step-by-Step Examples on Windows (Git Bash / PowerShell)

> **Mini-lab goal:** Create a repo, add/modify files, and practice common Git commands with real examples:  
> `add`, `commit`, `push`, `pull`, `clone`, `branch`, `merge`, `rebase`, `revert`, `reset`, `stash`, and more.

---

## 0) One-time setup (Windows)

### Install Git
1. Install **Git for Windows**
2. Open **Git Bash** (recommended) or **PowerShell**

### Configure identity (required for commits)
```bash
git config --global user.name "Gopi Suresh"
git config --global user.email "youremail@example.com"
```

### Optional but useful settings (Windows line endings + defaults)
```bash
git config --global core.autocrlf true
git config --global init.defaultBranch main
git config --global pull.rebase false   # default pull uses merge (safe for beginners)
```

Check config:
```bash
git config --global --list
```

---

## 1) Create a repo, add a file, commit

### Step 1: Create a folder and initialize Git
```bash
mkdir GitDemo
cd GitDemo
git init
```

### Step 2: Create a file (README.md)
```bash
echo "# Git Demo" > README.md
```

### Step 3: Check status
```bash
git status
```
You’ll see `README.md` as **untracked**.

### Step 4: Stage and commit
```bash
git add README.md
git commit -m "Add README"
```

---

## 2) Modify a file → stage → commit

### Step 1: Add more content
```bash
echo "Version 1" >> README.md
```

### Step 2: See what changed
```bash
git status
git diff
```

### Step 3: Stage and commit the change
```bash
git add README.md
git commit -m "Update README with Version 1"
```

---

## 3) Connect to GitHub (remote) and push

### Step 1: Create a GitHub repo
Example repo name: `git-demo`  
Copy the HTTPS URL like:
`https://github.com/<yourname>/git-demo.git`

### Step 2: Add remote
```bash
git remote add origin https://github.com/<yourname>/git-demo.git
git remote -v
```

### Step 3: Push
```bash
git push -u origin main
```

---

## 4) Git clone (download repo to a new folder)

```bash
cd ..
git clone https://github.com/<yourname>/git-demo.git GitDemo_Clone
cd GitDemo_Clone
git log --oneline
```

---

## 5) Git pull syntax (very important)

### ✅ Syntax
```bash
git pull <remote> <branch>
```

### Example
```bash
git pull origin main
```

### What pull actually does
By default: **pull = fetch + merge**
```bash
git fetch origin
git merge origin/main
```

---

## 6) Branching + merge (real example)

**Goal:** Create a feature branch, change file, merge into main.

### Step 1: Create and switch to new branch
```bash
git switch -c feature-login
```

### Step 2: Create a new file
```bash
echo "Login feature started" > login.txt
git add login.txt
git commit -m "Add login draft"
```

### Step 3: Switch back to main
```bash
git switch main
```

### Step 4: Merge feature branch into main
```bash
git merge feature-login
```

### Push merged changes
```bash
git push
```

> ✅ **Merge** keeps full branch history (may create a merge commit).

---

## 7) Merge conflict (how to see & fix)

### Step 1: Make a change on main
```bash
echo "MAIN change line" >> README.md
git add README.md
git commit -m "Main updates README"
```

### Step 2: Switch to feature branch and change same area
```bash
git switch feature-login
echo "FEATURE change line" >> README.md
git add README.md
git commit -m "Feature updates README"
```

### Step 3: Merge and get conflict
```bash
git switch main
git merge feature-login
```

Git will say **CONFLICT**.

### Step 4: Open `README.md` and you’ll see markers like:
```text
<<<<<<< HEAD
MAIN change line
=======
FEATURE change line
>>>>>>> feature-login
```

Fix it by keeping the correct final content, then:
```bash
git add README.md
git commit -m "Resolve merge conflict in README"
```

---

## 8) Rebase (step-by-step)

### When to use rebase
You want a **clean straight history** (no merge commits), replaying your commits on top of latest `main`.

### Steps
```bash
git switch feature-login
git fetch origin
git rebase main
```

If conflict happens:
```bash
# fix files, then:
git add <file>
git rebase --continue
```

If you want to cancel rebase:
```bash
git rebase --abort
```

### After rebase, push (important!)
If branch was already pushed earlier:
```bash
git push --force-with-lease
```

> ✅ `--force-with-lease` is safer than `--force`.

---

## 9) Revert (undo safely in shared branch)

### Revert creates a NEW commit that cancels an old commit
```bash
git log --oneline
```

Suppose commit id is `a1b2c3d`.

```bash
git revert a1b2c3d
git push
```

> ✅ Best for **main/shared branches**.

---

## 10) Reset (undo locally – be careful)

> Reset moves `HEAD` pointer. It can remove commits locally.

### A) Keep changes, unstage only
```bash
git reset
```

### B) Remove commit but keep file changes
```bash
git reset --soft HEAD~1
```

### C) Remove commit and also remove changes (DANGEROUS)
```bash
git reset --hard HEAD~1
```

> If you already pushed and others pulled → **don’t reset main**. Use **revert** instead.

---

## 11) Stash (save work temporarily)

### Save uncommitted work
```bash
git stash
```

### List stashes
```bash
git stash list
```

### Apply stash back
```bash
git stash pop
```

---

## 12) Other important commands (with mini examples)

### Check file history
```bash
git log -- README.md
```

### Show who changed which line
```bash
git blame README.md
```

### View commits (graph)
```bash
git log --oneline --graph --decorate --all
```

### Create a tag (release)
```bash
git tag v1.0
git push origin v1.0
```

### Delete a branch
```bash
git branch -d feature-login         # safe delete (merged)
git branch -D feature-login         # force delete
```

### Rename a branch
```bash
git branch -m oldname newname
```

### Remove file from Git but keep on disk
```bash
git rm --cached secrets.txt
git commit -m "Stop tracking secrets.txt"
```

### `.gitignore` example
```bash
echo "*.log" > .gitignore
echo "temp/" >> .gitignore
git add .gitignore
git commit -m "Add gitignore"
```

### Fix “wrong last commit message”
```bash
git commit --amend -m "Correct message"
```

### Recover lost commits (life saver)
```bash
git reflog
# then you can reset back using the reflog hash
```

---

## Quick differences (must-know)

- **clone**: first time download repo  
- **fetch**: download updates only (no merge)  
- **pull**: fetch + merge (or fetch + rebase)  
- **merge**: combines branches, preserves full history  
- **rebase**: rewrites history for linear commits  
- **revert**: safe undo by new commit  
- **reset**: moves HEAD (can remove commits locally)  
- **stash**: temporarily save uncommitted work  

---

### Tip: Useful aliases (optional)
```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm "commit -m"
```

Now you can do:
```bash
git st
git br
```
