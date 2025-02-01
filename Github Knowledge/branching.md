
# Git Branching Guide

This document provides an overview of essential Git branching operations, including creating, switching, merging, and deleting branches, as well as handling branches from remote repositories.

---

## 1. Creating a New Branch
To create a new branch:
```bash
git branch <branch-name>
```
This creates a branch but does not switch to it. To create and switch to a branch in one step:
```bash
git checkout -b <branch-name>
```

## 2. Switching Branches
To switch to an existing branch:
```bash
git checkout <branch-name>
```

In newer versions of Git, you can also use:
```bash
git switch <branch-name>
```

## 3. Viewing All Branches
To list all local branches:
```bash
git branch
```

To list all branches, including remote branches:
```bash
git branch -a
```

## 4. Renaming a Branch
To rename the branch you are currently on:
```bash
git branch -m <new-branch-name>
```

To rename another branch:
```bash
git branch -m <old-branch-name> <new-branch-name>
```

## 5. Merging Branches
To merge another branch into your current branch:
```bash
git merge <branch-name>
```

This will combine changes from `<branch-name>` into the branch you currently have checked out.

## 6. Deleting a Branch
To delete a branch locally (only if it has been fully merged):
```bash
git branch -d <branch-name>
```

To force delete a branch (use with caution):
```bash
git branch -D <branch-name>
```

## 7. Working with Remote Branches

### 7.1 Fetching Remote Branches
To update your list of remote branches:
```bash
git fetch --all
```

### 7.2 Tracking a Remote Branch Locally
To create a local branch that tracks a remote branch:
```bash
git checkout -b <branch-name> origin/<branch-name>
```

Or set an existing branch to track a remote branch:
```bash
git branch --set-upstream-to=origin/<branch-name> <branch-name>
```

### 7.3 Pushing a Branch to Remote
To push a new local branch to the remote repository:
```bash
git push -u origin <branch-name>
```

### 7.4 Deleting a Remote Branch
To delete a branch from the remote repository:
```bash
git push origin --delete <branch-name>
```

---

## 8. Viewing Branch History
To view the commit history of a specific branch:
```bash
git log <branch-name> --oneline
```

To view only the commits that are in `<branch-name>` but not in `main`:
```bash
git log <branch-name> ^main --oneline
```

---

## Summary of Common Commands

| Action                             | Command                                     |
|------------------------------------|---------------------------------------------|
| Create a new branch                | `git branch <branch-name>`                  |
| Create and switch to a new branch  | `git checkout -b <branch-name>`             |
| Switch to an existing branch       | `git checkout <branch-name>`                |
| List all branches                  | `git branch -a`                             |
| Merge a branch                     | `git merge <branch-name>`                   |
| Delete a local branch              | `git branch -d <branch-name>`               |
| Delete a remote branch             | `git push origin --delete <branch-name>`    |
| Track a remote branch locally      | `git checkout -b <branch-name> origin/<branch-name>` |
| Push a local branch to remote      | `git push -u origin <branch-name>`          |

---

This guide covers basic and commonly used Git branching commands to help you manage branches effectively.
