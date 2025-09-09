Git & GitHub documentation:


# Git & GitHub Complete Documentation

## 🚀 What is Git?
Git is a distributed version control system that helps developers track changes in source code during software development.

Imagine you're writing code or working on a project. Over time, you make changes — some good, some maybe not. Git allows you to:
- ✅ Track every change made to your files
- ⏪ Go back to previous versions if something breaks
- 🤝 Work with teammates without overwriting each other’s work

## ⚡ Key Features of Git

| Feature                | Description                                                               |
| ---------------------- | ------------------------------------------------------------------------- |
| ✅ Version Tracking    | Keeps history of file changes and who made them                         |
| 🤝 Collaboration       | Lets multiple people work on the same project at the same time          |
| 🔁 Branching & Merging | Work on features separately without affecting main code               |
| 💻 Distributed         | Every developer has a full copy of the project                         |
| ⏪ Undo/Restore        | Roll back to previous states of the project easily                     |

## 🌐 Real-World Use Case
Suppose you're developing a website.  
- You make a mistake while changing a file → Git lets you undo it.  
- Your friend wants to add a new feature → She can work in a separate branch.  
- Later, both of you can merge changes safely.

## 📊 Git Workflow Diagram

```

+----------------+
\|  Working Dir   | git init
+----------------+
|
v
git add \<file(s)>
|
v
+----------------+
\|  Staging Area  |
+----------------+
|
v
git commit -m "message"
|
v
+----------------+
\|   Local Repo   |
+----------------+
|
v
git push origin main
|
v
+----------------+
\|  Remote Repo   |
+----------------+

````

## 📚 Git Command Flow

| Stage             | Action                         | Git Command                           |
| ----------------- | ------------------------------ | ------------------------------------- |
| Edit files        | Make changes                   | Manual                              |
| Stage files       | Add to staging area            | `git add .` or `git add filename`   |
| Commit changes    | Save snapshot locally          | `git commit -m "message"`            |
| Push changes      | Upload to GitHub               | `git push origin main`               |

## 🌐 What is GitHub?
GitHub is a web-based platform hosting Git repositories online.  
It makes collaboration easier by:
- Hosting repositories in the cloud
- Visual interface & cloud storage
- Issue tracking, code review with PRs

## ⚔️ Git vs GitHub

| Git                      | GitHub                                |
| ------------------------ | ------------------------------------- |
| Local tool (installed)   | Online platform (github.com)         |
| Tracks local file changes| Hosts Git repositories in the cloud |
| CLI or GUI usage         | Browser + Git commands               |
| No account needed        | Requires GitHub account              |

---

## ⚡ Git Basics Commands

```bash
# Initialize repo
git init

# Clone repo
git clone https://github.com/username/repo.git

# Connect remote
git remote add origin <url>

# Check status
git status

# Add files
git add .

# Commit changes
git commit -m "Your commit message"

# View log
git log

# Show changes
git show <commit-id>

# Push to remote
git push origin main

# Pull from remote
git pull origin main

# Fetch remote
git fetch origin

# Manage branches
git branch -m main
git branch <name>
git branch

# Switch branch
git checkout <branch-name>

# Merge branch
git merge main

# Remove file
git rm filename

# Restore file
git restore filename

# View diff
git diff
git diff --cached

# Stash changes
git stash
git stash list
git stash apply
git stash pop
git stash drop

# Rebase branch
git rebase main

# Tag commits
git tag
git tag -a v1.0 -m "Version 1.0"

# Clean untracked files
git clean -n
git clean -f

# Create .gitignore
# Add patterns of files/folders to ignore
````

---

## 👥 GitHub Workflow Diagram (Team Collaboration)

```
+---------------------+
| GitHub Repository   |
+----------+----------+
           ^
           |
    git push / pull
           |
+----------+----------+
|                     |
| Developer A         | Developer B
+----------+----------+----------------+
      |                     |
      v                     v
Create Feature Branch   Create Feature Branch
      |                     |
      v                     v
Commit & Push          Commit & Push
      |                     |
      v                     v
    Pull Request → Review PR
      |                     |
      v                     v
 Merge to `main` ← Approve & Merge
```

## 🧱 GitHub Team Flow (Steps Summary)

| Step                     | Description                           |
| ------------------------ | ------------------------------------- |
| Fork or Clone            | Developers copy the repo              |
| Create a Branch          | Work on feature/login branch          |
| Commit and Push          | Push changes                          |
| Create Pull Request (PR) | Propose changes for review            |
| Review & Merge           | Merge into main branch after approval |

---

## 🏗 Example Repository Structure

```
project-name/
│
├── README.md               # Project overview
├── .gitignore              # Files to ignore
├── requirements.txt        # Dependencies
├── LICENSE                 # License file
│
├── src/                    # Source code
│   ├── main.py
│   └── helpers.py
│
├── tests/                  # Unit tests
│   └── test_main.py
│
├── docs/                   # Documentation
│   └── setup-guide.md
│
└── .github/                # GitHub config
    └── workflows/
        └── ci.yml          # CI/CD configuration
```

---

## 🧠 Fork & PR Flow Summary

| Step                 | Command/Action                                                 |
| -------------------- | -------------------------------------------------------------- |
| Fork repo            | GitHub → Fork button                                           |
| Clone fork           | `git clone https://github.com/your-username/repo.git`          |
| Add upstream         | `git remote add upstream https://github.com/original/repo.git` |
| Create branch        | `git checkout -b feature-branch`                               |
| Make & commit        | `git add .` → `git commit -m "Message"`                        |
| Push branch          | `git push origin feature-branch`                               |
| Create PR            | GitHub → Open PR                                               |
| Sync fork (optional) | `git pull upstream main` → `git push origin main`              |

```
