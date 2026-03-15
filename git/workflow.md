# Git - Workflow and Commands

## Basic Workflow

```bash
git status                  # View changes
git add .                   # Stage all files
git commit -m "message"     # Commit
git push origin main        # Push to remote
git pull origin main        # Fetch changes
```

## Branches

```bash
git branch                      # List local branches
git branch -a                   # List all branches (local + remote)
git checkout -b feature/name    # Create and switch to a branch
git checkout main               # Switch back to main
git merge feature/name          # Merge a branch into the current one
git branch -d feature/name      # Delete a merged branch
```

## Conflict Resolution

```bash
# 1. Pull or merge -> conflict detected
git merge feature/name

# 2. Open conflicted files and resolve manually
#    <<<<<<< HEAD
#    your code
#    =======
#    their code
#    >>>>>>> feature/name

# 3. Mark as resolved and commit
git add .
git commit -m "fix: resolve merge conflict"
```

## History

```bash
git log --oneline           # Compact history
git log --graph --oneline   # With branch visualization
git diff                    # Unstaged changes
git diff --staged           # Staged changes
git blame file.txt          # Who modified each line
```

## Undoing Changes

```bash
git checkout -- file.txt    # Discard changes to a file (unstaged)
git reset HEAD file.txt     # Unstage a file
git revert <commit>         # Create a commit that undoes another
git stash                   # Set aside current changes
git stash pop               # Retrieve stashed changes
```

## Commit Best Practices

- Start with a verb: `add`, `fix`, `update`, `remove`, `refactor`
- Short message (< 50 characters) for the title
- Use prefixes: `feat:`, `fix:`, `docs:`, `ci:`, `refactor:`
- One commit = one logical change
