# Git Mind-Map

## 1. Basics of Git
- **What is Git?**
  - Distributed Version Control System
  - Tracks changes in source code
  - Collaboration tool for developers

- **Core Concepts**
  - Repository
  - Commit
  - Branch
  - Merge
  - Staging Area
  - Working Directory

---

## 2. Key Git Commands
### 2.1 Repository Management
- `git init`
  - Initializes a new Git repository
  - Example: `git init my-project`

- `git clone`
  - Clones an existing repository
  - Example: `git clone <repo_url>`

---

### 2.2 File Lifecycle Commands
- `git add`
  - Adds changes to the staging area
  - Example: `git add file.txt`

- `git status`
  - Shows current repository status
  - Example: `git status`

- `git commit`
  - Saves changes to the repository
  - Example: `git commit -m "Commit message"`

---

### 2.3 Branch Management
- `git branch`
  - Lists, creates, or deletes branches
  - Example: `git branch feature-branch`

- `git switch`
  - Newer command to switch branches
  - Example: `git switch feature-branch`

- `git merge`
  - Combines branches
  - Example: `git merge feature-branch`

- **Merge vs Rebase**
  - Merge:
    - Preserves commit history
    - Adds a merge commit
    - Example: `git merge branch`
  - Rebase:
    - Rewrites commit history
    - Avoids merge commits
    - Example: `git rebase branch`

---

### 2.4 Synchronization Commands
- `git fetch`
  - Fetches updates from the remote
  - Example: `git fetch origin`

- `git pull`
  - Fetches and merges updates
  - Example: `git pull origin main`

- `git push`
  - Pushes local commits to the remote repository
  - Example: `git push origin main`

---

### 2.5 Inspection Commands
- `git log`
  - Displays commit history
  - Example: `git log --oneline`

- `git diff`
  - Shows differences between commits
  - Example: `git diff HEAD~1 HEAD`

- `git show`
  - Displays details of a specific commit
  - Example: `git show <commit-hash>`

---

## 3. Advanced Topics
### 3.1 Cherry-picking
- `git cherry-pick`
  - Applies specific commits to another branch
  - Example: `git cherry-pick <commit-hash>`
  - Use case: Porting a fix or feature to another branch without merging
  - Internal: Reapplies the changes introduced in the selected commit(s)

---

### 3.2 Stashing
- `git stash`
  - Temporarily saves changes
  - Example: `git stash push -m "Work in progress"`

- `git stash apply`
  - Re-applies stashed changes
  - Example: `git stash apply stash@{0}`

---

### 3.3 Undoing Changes
- `git reset`
  - Moves HEAD and optionally updates the index
  - Example: `git reset --soft HEAD~1`

- `git revert`
  - Creates a new commit to undo changes
  - Example: `git revert <commit-hash>`

- **Reset vs Revert**
  - Reset:
    - Modifies history
    - Dangerous for shared branches
  - Revert:
    - Keeps history intact

---

### 3.4 Squashing Commits
- `git rebase -i`
  - Interactive rebase to squash commits
  - Example: `git rebase -i HEAD~3`
  - Use case: Clean up commit history before pushing

---

### 3.5 Bisecting for Debugging
- `git bisect`
  - Finds the commit introducing a bug
  - Example:
    ```bash
    git bisect start
    git bisect bad
    git bisect good <commit-hash>
    ```

---

### 3.6 Resetting and Cleaning
- `git clean`
  - Removes untracked files and directories
  - Example: `git clean -f -d`
  - Use case: Resetting the working directory

---

### 3.7 Interactive Add
- `git add -p`
  - Adds changes interactively
  - Example: `git add -p`
  - Use case: Staging only specific changes in a file

---

### 3.8 Working with Submodules
- `git submodule`
  - Manages nested repositories
  - Example:
    ```bash
    git submodule add <repo_url>
    git submodule update --init
    ```

---

### 3.9 Worktrees
- `git worktree`
  - Creates a separate working directory for a branch
  - Example:
    ```bash
    git worktree add ../branch-directory branch-name
    ```

---

## 4. Internal Workings
- **Object Model**
  - Objects in `.git` folder:
    - Blob: Stores file content
    - Tree: Represents directory structure
    - Commit: Links trees and parent commits

- **Branching**
  - Lightweight pointers to commits

- **HEAD**
  - Points to the current branch or commit

---

## 5. Troubleshooting
### 5.1 Conflict Resolution
- During `merge` or `rebase`, resolve conflicts manually:
  - Edit conflicting files
  - Add resolved files: `git add`
  - Complete process: `git merge --continue` or `git rebase --continue`

### 5.2 Abort Operations
- `git merge --abort`
  - Cancels an ongoing merge
- `git rebase --abort`
  - Cancels an ongoing rebase

---

## 6. Best Practices
- Commit frequently
- Use meaningful commit messages
- Branch for features
- Pull frequently to avoid conflicts
- Avoid force-push on shared branches

---

## 7. Examples
```bash
# Cherry-picking a commit
git checkout feature-branch
git cherry-pick <commit-hash>

# Rebasing a feature branch onto main
git checkout feature-branch
git rebase main

# Undoing the last commit
git reset --soft HEAD~1