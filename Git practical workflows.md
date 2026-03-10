# **Complete Git tutorial** 

With realistic scenarios developer would encounter when managing a software project repository.

> - The tutorial focuses on **practical workflows**, not just commands. 
> - It simulates a real project with branching, merging, releases, hotfixes, mistakes, and recovery using `git reset`.
> - It's organize it into **8 progressive stages**, each with a **use case scenario**.

---


# Complete Git Tutorial: From Beginner to Advanced

*(Practical Workflow for Managing a Project Repository)*

---

# 1. What Git Actually Is (Conceptual Model)

Git is a **distributed version control system** created by Linus Torvalds.

Git tracks **snapshots of files over time**.

Think of it as:

```
Project Timeline

A --- B --- C --- D  (main branch)
```

Each letter is a **commit snapshot**.

Git stores:

* file changes
* author
* timestamp
* message
* commit hash

---

# 2. Installing Git

### Linux

```bash
sudo apt install git
```

### Mac

```bash
brew install git
```

### Windows

Download from
[https://git-scm.com](https://git-scm.com)

---

# 3. Basic Setup

Configure your identity.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@email.com"
```

Check configuration

```bash
git config --list
```

---

# 4. Create Your First Repository

Scenario:

You are starting a **data engineering project**.

Project structure:

```
data_pipeline_project/
   extract.py
   transform.py
   load.py
```

Initialize Git.

```bash
git init
```

Git creates

```
.git/
```

This folder stores the **entire repository history**.

---

# 5. First Commit

Add files.

```bash
git add .
```

Check status.

```bash
git status
```

Commit.

```bash
git commit -m "Initial data pipeline structure"
```

History now:

```
A (initial commit)
```

View history.

```bash
git log
```

---

# 6. Understanding the Three Git States

Git has **three areas**.

```
Working Directory
      ↓
Staging Area
      ↓
Repository
```

Commands:

| Area    | Command      |
| ------- | ------------ |
| working | edit files   |
| staging | `git add`    |
| repo    | `git commit` |

Example:

```
edit extract.py
git add extract.py
git commit
```

---

# 7. Ignoring Files

Create `.gitignore`.

Example:

```
__pycache__/
*.log
.env
```

Commit it.

```bash
git add .gitignore
git commit -m "Add gitignore"
```

---

# 8. Connecting to Remote Repository

Example with GitHub.

Create repository online.

Then link it.

```bash
git remote add origin https://github.com/user/project.git
```

Push.

```bash
git push -u origin main
```

---

# 9. Branching (Core Git Power)

Scenario:

You want to build a **new feature**.

Instead of editing `main`, create a branch.

```
main
  |
  A
```

Create branch.

```bash
git checkout -b feature-data-validation
```

Now timeline:

```
main: A
feature: A
```

---

# 10. Working in Feature Branch

Edit `transform.py`.

Commit changes.

```bash
git add transform.py
git commit -m "Add validation logic"
```

Timeline:

```
A --- B (feature branch)
|
main
```

---

# 11. Merging Feature into Main

Switch to main.

```bash
git checkout main
```

Merge feature.

```bash
git merge feature-data-validation
```

Timeline:

```
A --- B
       \
        C (merge)
```

Push release.

```bash
git push origin main
```

---

# 12. Real Professional Workflow (Git Flow Concept)

Branches:

```
main      → production
develop   → integration
feature   → new features
release   → release preparation
hotfix    → production fixes
```

Example:

```
main
 |
develop
 | \
 |  feature-login
 |  feature-report
```

---

# 13. Feature Development Scenario

Start feature.

```bash
git checkout develop
git checkout -b feature-reporting
```

Work and commit.

```
feature-reporting
    |
A---B---C
```

Merge back.

```bash
git checkout develop
git merge feature-reporting
```

Delete feature branch.

```bash
git branch -d feature-reporting
```

---

# 14. Creating a Release Branch

Scenario:

Version **v1.0** is ready.

Create release branch.

```bash
git checkout -b release-1.0
```

Fix bugs only.

```
develop
   |
   D
   |
release-1.0
```

Tag release.

```bash
git tag v1.0
```

Merge to main.

```bash
git checkout main
git merge release-1.0
```

Push.

```bash
git push origin main --tags
```

---

# 15. Production Bug (Hotfix Scenario)

User reports bug in production.

Create hotfix.

```bash
git checkout main
git checkout -b hotfix-null-error
```

Fix and commit.

Merge back.

```
main ← hotfix
develop ← hotfix
```

Commands:

```bash
git checkout main
git merge hotfix-null-error

git checkout develop
git merge hotfix-null-error
```

---

# 16. Merge Conflicts

Conflict occurs when two branches modify the same line.

Example file conflict.

```
<<<<<<< HEAD
old code
=======
new code
>>>>>>> feature-branch
```

Resolve manually.

Then:

```bash
git add file
git commit
```

---

# 17. Rebase (Cleaner History)

Merge creates merge commits.

Rebase creates **linear history**.

Before:

```
A---B---C main
     \
      D---E feature
```

Rebase:

```bash
git checkout feature
git rebase main
```

After:

```
A---B---C---D---E
```

---

# 18. Undoing Mistakes (Very Important)

## Undo staged file

```bash
git restore --staged file
```

---

## Undo working changes

```bash
git restore file
```

---

# 19. Git Reset (Advanced)

Used to move HEAD pointer.

Three types:

| type  | effect         |
| ----- | -------------- |
| soft  | keep staged    |
| mixed | keep files     |
| hard  | delete changes |

---

### Example Scenario

Commits:

```
A---B---C---D
```

You want to remove D.

Soft reset:

```bash
git reset --soft HEAD~1
```

Now:

```
A---B---C
```

But code still staged.

---

### Hard Reset

Dangerous.

```bash
git reset --hard HEAD~1
```

Removes commit **and file changes**.

---

# 20. Recovering Lost Commits (Lifesaver)

Git keeps hidden history.

View:

```bash
git reflog
```

Example:

```
abc123 HEAD@{0}
def456 HEAD@{1}
```

Restore commit.

```bash
git reset --hard abc123
```

---

# 21. Interactive Rebase (History Editing)

Clean commits before merging.

```bash
git rebase -i HEAD~3
```

Options:

```
pick
reword
squash
drop
```

Example squash.

```
pick A
squash B
squash C
```

Result:

```
A+B+C → single commit
```

---

# 22. Cherry Picking

Copy specific commit.

```bash
git cherry-pick <commit-hash>
```

Useful for **backport fixes**.

---

# 23. Stashing Work

Scenario:

You're working but must switch branches.

```bash
git stash
```

Work saved.

Restore later.

```bash
git stash pop
```

---

# 24. Git Tagging (Release Management)

Create version tag.

```bash
git tag v1.1
```

Annotated tag.

```bash
git tag -a v1.1 -m "Release version 1.1"
```

Push tags.

```bash
git push origin --tags
```

---

# 25. Realistic Team Collaboration Scenario

Example team structure:

```
main
 |
develop
 | \
 |  feature-analytics
 |  feature-api
```

Workflow:

1. developer creates feature branch
2. commits work
3. pushes branch
4. opens Pull Request
5. code review
6. merge to develop
7. release branch created
8. release merged to main

Used heavily on:

* GitHub
* GitLab
* Bitbucket

---

# 26. Best Practices

### Commit messages

Good:

```
Fix: handle null customer ID
```

Bad:

```
update
```

---

### Branch naming

```
feature-login
bugfix-null-error
hotfix-security
release-1.2
```

---

### Keep commits small

Each commit should represent **one logical change**.

---

# 27. Advanced Real-World Workflow

Professional teams often use:

### Trunk Based Development

```
main
 |
small feature branches
```

Merged quickly.

Used by:

* Google
* Facebook

---

# 28. Visualizing Git History

Useful tool:

```
git log --graph --oneline --all
```

Example:

```
*   merge feature
|\
| * feature commit
|/
* main commit
```

---

# 29. Common Git Problems

### Accidentally committed secret

Solution:

```
git reset
```

or

```
git filter-repo
```

---

### Wrong branch commit

Fix:

```
git cherry-pick
```

---

# 30. Recommended Git Learning Path

| Stage        | Skill                        |
| ------------ | ---------------------------- |
| Beginner     | add, commit, push            |
| Intermediate | branching, merging           |
| Advanced     | rebase, reset                |
| Expert       | workflows, CI/CD integration |

---
