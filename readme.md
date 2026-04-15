# Git-Experiment: Advanced Git Workflow & Version Control

Assignment: Enterprise-Level Git Workflow, Branching & History Management

---

## Repository Setup

**Initialize and clone:**

```bash
git init
git remote add origin https://github.com/Rafath-Auvee/Git-Experiment.git
```

**Create initial branches:**

```bash
git checkout -b develop
git checkout -b feature/login
git checkout main
```

---

## Task 1: Repository Initialization

Created the following branches from `main`:

| Branch | Purpose |
| --- | --- |
| `main` | Production-ready code |
| `develop` | Integration branch for features |
| `feature/login` | Login feature development |

**Commands used:**

```bash
git branch develop
git branch feature/login
git branch -a   # verify all branches
```

**Branch list output:**

```text
* main
  develop
  feature/login
  remotes/origin/main
  remotes/origin/develop
  remotes/origin/feature/login
```

---

## Task 2: Branching Workflow

### Branches Created

```bash
git checkout -b feature/payment
git checkout -b feature/profile
git checkout -b bugfix/login-error
```

| Branch | Type | Description |
| --- | --- | --- |
| `feature/payment` | Feature | Payment module |
| `feature/profile` | Feature | User profile module |
| `bugfix/login-error` | Bugfix | Fix null pointer error on login |

### Merge Strategy

`feature/payment` was merged into `develop` using a standard merge (preserves merge commit):

```bash
git checkout develop
git merge feature/payment
```

**Result — merge commit created:**

```text
820d9c1 Merge feature/payment into develop
```

The merge commit keeps a full record that two branches were combined — useful for tracking when a feature was integrated.

### Rebase Strategy

`feature/profile` was rebased onto `develop` before merging (linear history):

```bash
git checkout feature/profile
git rebase develop
git checkout develop
git merge feature/profile
```

**Result — linear commit on develop:**

```text
3a7e4a5 feat: add profile module
```

Rebase replays commits on top of the target branch, producing a clean straight-line history with no extra merge commit.

---

## Task 3: Commit History Management

### 5 Commits on `feature/login`

These commits were made while building the login feature:

```text
b9e4dea wip: login step 1
2232b5d wip: login step 2
ee35f8d wip: login step 3
4b9f02d wip: login step 4
87b3e7d wip: login step 5
```

### Interactive Rebase — Squash & Reword

All 5 commits were squashed into one clean commit and the message was reworded:

```bash
git checkout feature/login
git rebase -i HEAD~5
```

In the interactive rebase editor, the todo list was configured as:

```text
pick  b9e4dea wip: login step 1
squash 2232b5d wip: login step 2
squash ee35f8d wip: login step 3
squash 4b9f02d wip: login step 4
squash 87b3e7d wip: login step 5
```

The combined commit message was reworded to:

```text
feat: implement complete login feature

- add login form UI and validation
- integrate authentication logic
- handle session management and error states
```

**Result after squash + reword:**

```text
94546bd feat: implement complete login feature
```

**Force push the rewritten branch:**

```bash
git push origin feature/login --force
```

---

## Merge vs Rebase

| | Merge | Rebase |
| --- | --- | --- |
| History | Preserves full history with a merge commit | Rewrites history — replays commits on top of target |
| Commit graph | Non-linear (fork and join visible) | Linear (straight line) |
| When to use | When you want a record of when branches joined | When you want a clean, readable history |
| Safe on shared branches | Yes | No — rewriting shared history causes problems for teammates |

**Merge example:**

```text
A---B---C  (main)
         \
          D---E  (feature)
               \
                M  (merge commit)
```

**Rebase example:**

```text
A---B---C  (main)
             \
              D'--E'  (feature, replayed on top of C)
```

---

## Squash & Reword

### Squash

Squash combines multiple commits into a single one. Useful for cleaning up work-in-progress commits before merging a feature branch.

```bash
git rebase -i HEAD~5
# Mark commits 2-5 as "squash" in the editor
```

Before squash:

```text
b9e4dea wip: login step 1
2232b5d wip: login step 2
ee35f8d wip: login step 3
4b9f02d wip: login step 4
87b3e7d wip: login step 5
```

After squash — one clean commit:

```text
94546bd feat: implement complete login feature
```

### Reword

Reword changes a commit message without altering the commit's code changes.

```bash
git rebase -i HEAD~1
# Mark the commit as "reword" in the editor, then update the message
```

Use reword to fix typos, add context, or enforce a commit convention (e.g., `feat:`, `fix:`, `chore:`).

---

## Full Command Reference

```bash
# Repository setup
git init
git remote add origin <url>

# Branching
git checkout -b feature/payment
git checkout -b feature/profile
git checkout -b bugfix/login-error
git branch -a

# Merging
git checkout develop
git merge feature/payment

# Rebasing
git checkout feature/profile
git rebase develop

# Interactive rebase (squash + reword)
git rebase -i HEAD~5

# View history
git log --oneline --graph --all

# Push (with force for rewritten history)
git push origin feature/login --force
```

---

## Final Branch Graph

```text
* 94546bd feat: implement complete login feature  (feature/login)
* a2d39b1 sc
* 0f4e51f feature add login feature file
| * 3a7e4a5 feat: add profile module              (develop)
| * 820d9c1 Merge feature/payment into develop
|/|
| * 195c441 feat: add payment module
|/
| * 3e9760e fix: resolve login null pointer error (bugfix/login-error)
|/
* 3a2c3cb Add initial section 'Checking' to README (main)
```
