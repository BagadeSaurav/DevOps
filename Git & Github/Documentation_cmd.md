# Git & GitHub Full Documentation

## 🚀 Git Basics - List of All Commands

-   `git init`\
    Creates a repository

-   `git clone <repository-url>`\
    Example: `git clone https://github.com/username/repo.git`\
    Clones a repository

-   `git remote add origin <repository-url>`\
    Connects local repo to GitHub

-   `git status`\
    Checks the repository status

-   `git add .`\
    Adds changes to staging

-   `git commit -m "Your commit message"`\
    Commits changes

-   `git log`\
    Shows commit history

-   `git show <commit-id>`\
    Views changes from specific commit

-   `git push origin main`\
    Pushes code to GitHub

-   `git pull origin main`\
    Pulls changes from GitHub

-   `git fetch origin`\
    Downloads remote changes (no merge)

-   `git branch`, `git branch <name>`, `git branch -m <new-name>`\
    Manages branches

-   `git checkout <branch-name>`\
    Switches to another branch

-   `git merge <branch-name>`\
    Merges branch into current

-   `git rm <filename>`\
    Removes file

-   `git restore <filename>`\
    Restores file to last commit

-   `git diff`, `git diff --cached`\
    Shows differences

-   Stashing:

    -   `git stash`
    -   `git stash list`
    -   `git stash apply`
    -   `git stash pop`
    -   `git stash drop`

-   `git rebase <branch>`\
    Rebase commits

-   `git tag -a <version> -m "message"`\
    Tags important commits

-   `git clean -n`, `git clean -f`\
    Cleans untracked files

-   `.gitignore` file to exclude files

------------------------------------------------------------------------

## 🧱 Git Basics: Step-by-Step Guide

### 1. Initialize a Repository

``` bash
git init
```

### 2. Clone a Repository from GitHub

``` bash
git clone https://github.com/username/repo.git
```

### 3. Connect Local Repo to GitHub

``` bash
git remote add origin https://github.com/username/repo.git
git remote -v
```

### 4. Check Status

``` bash
git status
```

### 5. Stage Files

``` bash
git add .
```

### 6. Commit Changes

``` bash
git commit -m "Your commit message"
```

### 7. View Commit History

``` bash
git log
```

### 8. Show Commit Details

``` bash
git show <commit-id>
```

### 9. Push Code

``` bash
git push origin main
```

### 10. Pull Changes

``` bash
git pull origin main
```

### 11. Fetch Only

``` bash
git fetch origin
```

### Branching & Merging

``` bash
git branch                    # List all branches
git branch dev                # Create new branch 'dev'
git branch -m main new-main   # Rename main to new-mai
git checkout dev
git merge dev
```

### Managing Files

``` bash
git rm filename
git restore filename
git diff               # View unstaged changes
git diff --cached      # View staged changes
```

### Stashing

```bash
git stash            # Save current changes
git stash list       # Show all stashed entries
git stash apply      # Reapply the last stash
git stash pop        # Reapply and delete the stash
git stash drop       # Delete a specific stash
```

### Rebase

``` bash
git rebase main
```

### Tagging

``` bash
git tag              # List tags
git tag -a v1.0 -m "First release"
git push origin v1.0
```

### Clean

``` bash
git clean -n     # Preview what will be deleted
git clean -f     # Force delete untracked files
```

### .gitignore Example

    node_modules/
    .env
    *.log

## ✅ Recommended Workflow

``` bash
git init                          # Step 1
git remote add origin <url>       # Connect to GitHub
git add .                         # Stage changes
git commit -m "Initial commit"    # Commit changes
git push -u origin main           # Push to GitHub

# Daily work cycle

git pull origin main              # Sync with remote
git add .                         # Stage work
git commit -m "Work updates"
git push origin main              # Push updates
```

------------------------------------------------------------------------

## 💻 Install Git on Ubuntu

``` bash
sudo apt update
sudo apt install git
git --version
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --list
```
