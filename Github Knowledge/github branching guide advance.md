# Git Branch Management Guide

## 1. How to Change the Upstream Branch of a Local Branch?

If your local branch is tracking the wrong remote branch and you want to change it, follow these steps:

### **Steps to Change the Upstream Branch**

```sh
# Switch to the branch you want to update
git checkout ss/dev_satnam

# Unset the current upstream branch
git branch --unset-upstream

# Set the new upstream branch
git branch --set-upstream-to=origin/ss/final_assessments
```

Alternatively, you can use:

```sh
git branch -u origin/ss/final_assessments
```

### **Verify the Change**

```sh
git status
```

This should now show:

> "Your branch is up to date with 'origin/ss/final\_assessments'."

## 2. How to Check the Upstream Branch?

You can check the upstream (tracking) branch of your local branch using the following commands:

### **Check for All Branches**

```sh
git branch -vv
```

This will list all branches and their tracking information.

### **Check for the Current Branch**

```sh
git rev-parse --abbrev-ref --symbolic-full-name @{u}
```

Or:

```sh
git branch --show-current --verbose
```

### **Check Remote and Upstream Details**

```sh
git remote show origin
```

This provides detailed information about the remote branches.

## 3. Additional Git Branch Scenarios

### **Renaming a Local Branch**

If you want to rename the current branch:

```sh
git branch -m new-branch-name
```

If renaming a different branch:

```sh
git branch -m old-branch-name new-branch-name
```

### **Deleting a Local Branch**

```sh
git branch -d branch-name
```

To force delete:

```sh
git branch -D branch-name
```

### **Deleting a Remote Branch**

```sh
git push origin --delete branch-name
```

### **Creating and Pushing a New Branch to Remote**

```sh
git checkout -b new-branch

git push -u origin new-branch
```

### **Fetching and Rebasing with an Upstream Branch**

```sh
git fetch origin

git rebase origin/main
```

### **Merging a Branch into Another**

```sh
git checkout main

git merge feature-branch
```

### **Stashing Changes Before Switching Branches**

```sh
git stash

git checkout another-branch
```

To apply the stashed changes:

```sh
git stash pop
```

### **Resetting a Branch to Match Remote**

```sh
git fetch origin

git reset --hard origin/main
```

This guide provides an overview of common Git branch management tasks. Happy coding! 🚀

