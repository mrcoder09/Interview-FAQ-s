# Git Code Merging Scenarios

## 1. Merging Main Branch Changes into Your Working Branch

### Use Case:
If the `main` branch has new changes after another developer merged their PR, and you don't have those changes in your current working branch.

### Steps:

1. **Check Status:**
   ```sh
   git status
   ```
2. **Push Your Local Code to Your Branch:**
   ```sh
   git add -A && git commit -m "Your commit message"
   git push
   ```
3. **Discard Unnecessary Local Changes (If Needed):**
   ```sh
   git checkout -- .
   ```
4. **Switch to `main` Branch:**
   ```sh
   git checkout main
   ```
5. **Pull Latest Changes from `main`:**
   ```sh
   git pull
   ```
6. **Check Status:**
   ```sh
   git status
   ```
7. **Switch Back to Your Working Branch:**
   ```sh
   git checkout dev-satnam  # Replace with your branch name
   ```
8. **Check Status Again:**
   ```sh
   git status
   ```
9. **Merge `main` into Your Branch:**
   ```sh
   git merge --no-ff origin/main
   ```
   - You may encounter conflicts; resolve them using your preferred tool (e.g., SourceTree, VS Code, or command-line editor).
10. **Resolve Conflicts & Commit Changes:**
    ```sh
    git add -A && git commit -m "Resolved merge conflicts"
    git push
    ```
11. **Your Working Branch Now Has the Latest Code!**
12. **Create a PR from Your Working Branch to `main` via GitHub.**
13. **Merge PR Once Approved.**

---

## 2. Merging One Branch into Another (Branch A → Branch B)

### Use Case:
You need to merge changes from `Branch A` into `Branch B`, which already contains code.

### Steps:

1. **Check Status:**
   ```sh
   git status
   ```
2. **Push Local Code to Branch A:**
   ```sh
   git add -A && git commit -m "Your commit message"
   git push
   ```
3. **Create a PR in GitHub:**
   - Select `Branch A` as the Source and `Branch B` as the Target.
4. **Resolve Conflicts (If Any) Using GitHub’s Inline Tool.**
5. **Merge the PR.**
6. **Pull the Latest Changes in Your Local Repository:**
   ```sh
   git pull
   ```

---

## 3. Merging Branch A into a Newly Created Branch B (With Code from Branch C)

### Use Case:
You need to merge `Branch A` into a newly created `Branch B`, which should initially have code from `Branch C`.

### Steps:

1. **Check Status:**
   ```sh
   git status
   ```
2. **Push Local Code to Branch A:**
   ```sh
   git add -A && git commit -m "Your commit message"
   git push
   ```
3. **Switch to Branch C:**
   ```sh
   git checkout branch-c  # Replace with actual branch name
   ```
4. **Pull the Latest Changes from Branch C:**
   ```sh
   git pull
   ```
5. **Create a New Branch (Branch B) from Branch C:**
   ```sh
   git checkout -b branch-b  # Replace with actual branch name
   ```
6. **Commit and Push the New Branch:**
   ```sh
   git add -A && git commit -m "Created new branch from Branch C"
   git push --set-upstream origin branch-b
   ```
7. **Now `Branch B` Exists Remotely with Code from `Branch C`.**
8. **Create a PR in GitHub:**
   - Select `Branch A` as the Source and `Branch B` as the Target.
9. **Resolve Conflicts (If Any) Using GitHub’s Inline Tool.**
10. **Merge the PR.**
11. **Pull the Latest Changes in Your Local Repository:**
    ```sh
    git pull
    ```

---

## Additional Best Practices
- Always take a backup of important work before performing merges.
- Use `git stash` if you have uncommitted changes and need to switch branches.
- Keep your branches updated frequently to avoid large merge conflicts.
- Use descriptive commit messages.
- Ensure proper code reviews before merging PRs.
- Use `git rebase` instead of merge when you want a cleaner commit history (with caution).

---

By following these structured workflows, you can efficiently merge code and manage branches in Git without unnecessary conflicts or code loss.
