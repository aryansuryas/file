# 📘 Complete Git & Terminal Commands - Master Notes

---

## 📂 CHAPTER 1: TERMINAL / BASH BASIC COMMANDS

---

### 1.1 `touch` — Create a New File

```bash
touch filename.txt
```

- Creates an **empty file** named `filename.txt`
- If the file already exists, it **updates the timestamp**

```bash
touch index.html style.css app.js
```

> Creates **multiple files** at once.

---

### 1.2 `ls` — List Files & Directories

```bash
ls
```

| Command | Description |
|---------|-------------|
| `ls` | Lists files and folders in current directory |
| `ls -l` | **Long format** — shows permissions, owner, size, date |
| `ls -a` | Shows **all files** including **hidden files** (starting with `.`) |
| `ls -la` | **Long format + hidden files** combined |
| `ls -lah` | Long + hidden + **human-readable** file sizes (KB, MB) |

#### Example output of `ls -la`:

```
drwxr-xr-x  5 user group 4096 Jun 15 10:30 .
drwxr-xr-x  3 user group 4096 Jun 14 09:00 ..
-rw-r--r--  1 user group  200 Jun 15 10:30 index.html
drwxr-xr-x  2 user group 4096 Jun 15 10:25 .git
-rw-r--r--  1 user group   50 Jun 15 10:28 .gitignore
```

**Understanding the columns:**

```
d rwx r-x r-x   5   user   group   4096   Jun 15 10:30   .git
│  │   │   │    │     │       │      │        │             │
│  │   │   │    │     │       │      │        │             └─ Name
│  │   │   │    │     │       │      │        └─ Date Modified
│  │   │   │    │     │       │      └─ Size
│  │   │   │    │     │       └─ Group
│  │   │   │    │     └─ Owner
│  │   │   │    └─ Hard Links
│  │   │   └─ Others Permission
│  │   └─ Group Permission
│  └─ Owner Permission
└─ d=directory, -=file
```

**Permission meanings:**
- `r` = read (4)
- `w` = write (2)
- `x` = execute (1)
- `-` = no permission

---

### 1.3 `cd` — Change Directory

```bash
cd foldername        # Go INTO a folder
cd ..                # Go BACK one level (parent directory)
cd ../..             # Go BACK two levels
cd ~                 # Go to HOME directory
cd /                 # Go to ROOT directory
cd -                 # Go to PREVIOUS directory (toggle)
```

#### Full Navigation Example:

```bash
pwd                  # Print Working Directory — shows where you are
# Output: /home/user

cd Documents         # Go into Documents
pwd
# Output: /home/user/Documents

cd ..                # Go back to /home/user
cd ..                # Go back to /home

cd ~                 # Jump directly to home
```

---

### 1.4 Other Essential Terminal Commands

```bash
mkdir foldername          # Create a new folder
mkdir -p a/b/c            # Create nested folders
rm filename.txt           # Delete a file
rm -r foldername          # Delete a folder (recursive)
rm -rf foldername         # Force delete (no confirmation)
cp file1.txt file2.txt    # Copy file
mv file1.txt file2.txt    # Rename or Move file
cat filename.txt          # Display file content
clear                     # Clear terminal screen
pwd                       # Print current directory path
```

---

---

## 📂 CHAPTER 2: GIT INTRODUCTION & SETUP

---

### 2.1 What is Git?

```
Git is a DISTRIBUTED VERSION CONTROL SYSTEM (DVCS).
It tracks changes in your code over time.
Every developer has a FULL COPY of the repository.
```

### 2.2 Install Git

```bash
# Check if Git is installed
git --version

# Output: git version 2.42.0
```

---

### 2.3 Git Configuration — `git config`

**Three levels of configuration:**

| Level | Flag | Scope | File Location |
|-------|------|-------|---------------|
| System | `--system` | All users on machine | `/etc/gitconfig` |
| Global | `--global` | Current user (all repos) | `~/.gitconfig` |
| Local | `--local` | Current repository only | `.git/config` |

```bash
# SET your name and email (REQUIRED before first commit)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# VERIFY configuration
git config --global user.name
git config --global user.email

# List ALL configurations
git config --list

# Set default branch name
git config --global init.defaultBranch main

# Set default editor
git config --global core.editor "code --wait"
```

---

---

## 📂 CHAPTER 3: GIT WORKFLOW — The Three Stages

---

### 3.1 The Three Areas of Git

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│   WORKING DIRECTORY    STAGING AREA     REPOSITORY      │
│   (Your files on      (Index/Cache)    (.git folder)    │
│    your computer)                                       │
│                                                         │
│   ┌───────────┐      ┌───────────┐    ┌───────────┐    │
│   │           │      │           │    │           │    │
│   │  Edit     │─────>│  Stage    │───>│  Commit   │    │
│   │  files    │      │  changes  │    │  changes  │    │
│   │           │ git  │           │git │           │    │
│   │           │ add  │           │commit│          │    │
│   └───────────┘      └───────────┘    └───────────┘    │
│                                                         │
│        ◄──────── git restore ────────                   │
│                  git reset                              │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

### 3.2 Initialize a Repository — `git init`

```bash
mkdir my-project
cd my-project
git init
```

```
Output: Initialized empty Git repository in /home/user/my-project/.git/
```

> This creates a hidden `.git` folder that contains ALL version history.

---

### 3.3 Check Status — `git status`

```bash
git status
```

#### What you see in `git status`:

```
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        index.html        ← RED (not tracked yet)
        style.css         ← RED

nothing added to commit but untracked files present
```

**Color Coding:**

| Color | Meaning |
|-------|---------|
| 🔴 **RED** | File is in **Working Directory** — Untracked OR Modified but NOT staged |
| 🟢 **GREEN** | File is in **Staging Area** — Ready to be committed |
| Nothing shown | File is already committed and has NO changes |

**Status Types:**

```
Untracked  → New file, Git doesn't know about it yet
Modified   → File changed since last commit
Staged     → File is added to staging area, ready for commit
Deleted    → File was deleted
Renamed    → File was renamed
```

---

---

## 📂 CHAPTER 4: GIT ADD — Staging Changes

---

### 4.1 Three Types of `git add`

---

#### **TYPE 1: `git add <filename>`** — Add a SPECIFIC file

```bash
git add index.html
```

> Only `index.html` goes to staging area.

---

#### **TYPE 2: `git add .`** — Add all files in CURRENT FOLDER only

```bash
git add .
```

> Adds all changed/new files **inside the current directory and its subdirectories**.

> ⚠️ If you are inside a subfolder, it only adds files from THAT subfolder downward.

---

#### **TYPE 3: `git add -A` or `git add --all`** — Add ALL files from ENTIRE repository

```bash
git add -A
git add --all
```

> Adds **everything** — from the **root of the repository**, no matter which folder you are currently in.

---

### 4.2 Comparison Table

| Command | Scope | New Files | Modified Files | Deleted Files |
|---------|-------|-----------|----------------|---------------|
| `git add <file>` | Only that file | ✅ | ✅ | ✅ |
| `git add .` | Current directory & below | ✅ | ✅ | ✅ |
| `git add -A` | Entire repository | ✅ | ✅ | ✅ |

#### Example to understand the difference:

```
my-project/
├── index.html          (modified)
├── src/
│   ├── app.js          (modified)
│   └── utils.js        (new file)
└── docs/
    └── readme.txt      (modified)
```

```bash
# If you are inside /src/ folder:
cd src

git add .
# Only stages: app.js, utils.js (files inside src/)

git add -A
# Stages: index.html, app.js, utils.js, readme.txt (ALL files in entire repo)
```

---

---

## 📂 CHAPTER 5: GIT COMMIT

---

```bash
# Commit with a message
git commit -m "Your commit message here"

# Commit with detailed message (opens editor)
git commit

# Add + Commit in one step (only for TRACKED files, not new files)
git commit -am "message"
```

### View Commit History

```bash
git log                      # Full log
git log --oneline            # Compact one-line format
git log --oneline --graph    # Graphical branch view
git log -5                   # Last 5 commits
git log --all                # All branches
```

**Example output of `git log --oneline`:**

```
a1b2c3d (HEAD -> main) Add footer section
e4f5g6h Update navbar styling
i7j8k9l Initial commit
```

---

---

## 📂 CHAPTER 6: GIT RESET — Unstaging & Undoing

---

### 6.1 `git reset` — Unstage files (remove from staging area)

```bash
# Unstage a specific file
git reset filename.txt

# Unstage ALL files
git reset
```

> File goes from **GREEN (staged)** → back to **RED (working directory)**

---

### 6.2 `git reset HEAD~1` — Undo the Last Commit

```bash
git reset HEAD~1
```

> Removes the **last 1 commit** but **keeps the changes** in working directory.

```bash
git reset HEAD~3
```

> Removes the **last 3 commits** but keeps changes.

---

### 6.3 Three Modes of Reset

```bash
git reset --soft HEAD~1
# Removes commit, keeps changes in STAGING AREA (green)

git reset --mixed HEAD~1      # (DEFAULT)
# Removes commit, keeps changes in WORKING DIRECTORY (red)

git reset --hard HEAD~1
# ⚠️ DANGER: Removes commit AND DELETES all changes permanently
```

| Mode | Commit | Staging Area | Working Directory |
|------|--------|-------------|-------------------|
| `--soft` | ❌ Removed | ✅ Kept (green) | ✅ Kept |
| `--mixed` (default) | ❌ Removed | ❌ Cleared | ✅ Kept (red) |
| `--hard` | ❌ Removed | ❌ Cleared | ❌ DELETED |

### 6.4 Reset to a Specific Commit

```bash
git log --oneline
# a1b2c3d Latest commit
# e4f5g6h Second commit
# i7j8k9l Initial commit

git reset --hard e4f5g6h
# Now HEAD points to "Second commit"
# "Latest commit" is GONE
```

---

---

## 📂 CHAPTER 7: GIT RESTORE — Discard Changes

---

```bash
# Discard changes in WORKING DIRECTORY (go back to last committed version)
git restore filename.txt

# Unstage a file (remove from staging area, keep changes)
git restore --staged filename.txt

# Restore file from a specific commit
git restore --source=HEAD~2 filename.txt
```

### Difference between `reset` and `restore`:

| Feature | `git reset` | `git restore` |
|---------|-------------|---------------|
| Primary use | Move HEAD / undo commits | Discard file changes |
| Affects commits | Yes | No |
| Affects staging | Yes | Yes (with `--staged`) |
| Affects working dir | Depends on mode | Yes (default) |

---

---

## 📂 CHAPTER 8: VS CODE GIT INTEGRATION

---

### What You See in VS Code (Source Control Panel — `Ctrl+Shift+G`)

```
SOURCE CONTROL
─────────────────
Changes (Working Directory - RED equivalent)
├── M  index.html        ← Modified (shown in orange/yellow)
├── U  newfile.js         ← Untracked (shown in green with U)
├── D  old.css            ← Deleted (shown in red with D)

Staged Changes (Staging Area - GREEN equivalent)
├── A  newfile.js         ← Added (new file staged)
├── M  index.html         ← Modified and staged
```

### VS Code File Status Letters:

| Letter | Meaning | Color |
|--------|---------|-------|
| `U` | **Untracked** — New file, not in Git yet | Green |
| `M` | **Modified** — File changed | Orange/Yellow |
| `A` | **Added** — New file staged for commit | Green |
| `D` | **Deleted** — File deleted | Red |
| `R` | **Renamed** — File renamed | Blue |
| `C` | **Conflict** — Merge conflict | Red |

### VS Code Gutter Colors (left side of editor):

```
🟢 Green bar  = NEW lines added
🔵 Blue bar   = MODIFIED lines
🔴 Red arrow  = DELETED lines
```

### VS Code Bottom Status Bar:

```
┌────────────────────────────────────────────────┐
│  main ⟳  │  ↑1 ↓0  │  ✓  │                    │
│  │         │         │                          │
│  Branch    │ Sync    │ No errors                │
│  name      │ status  │                          │
└────────────────────────────────────────────────┘

↑1 = 1 commit to push
↓2 = 2 commits to pull
⟳  = Sync available
```

### VS Code Git Actions:

```
+ button     → git add (stage file)
- button     → git restore --staged (unstage)
↩ button     → git restore (discard changes)
✓ checkmark  → git commit (type message and click)
... menu     → More options (push, pull, branch, etc.)
```

---

---

## 📂 CHAPTER 9: GIT BRANCHES

---

### 9.1 What is a Branch?

```
A branch is a SEPARATE LINE OF DEVELOPMENT.
Default branch = main (or master).
You create branches to work on features WITHOUT affecting main code.
```

### 9.2 Branch Commands

```bash
# List all LOCAL branches
git branch

# List all branches (local + remote)
git branch -a

# List remote branches only
git branch -r

# Create a NEW branch
git branch feature-login

# DELETE a branch
git branch -d feature-login        # Safe delete (warns if unmerged)
git branch -D feature-login        # Force delete

# RENAME current branch
git branch -m new-name

# RENAME a specific branch
git branch -m old-name new-name
```

---

### 9.3 `git checkout` — Switch Branches

```bash
# Switch to an existing branch
git checkout feature-login

# Create AND switch to a new branch (shortcut)
git checkout -b feature-signup

# Switch back to main
git checkout main
```

### 9.4 `git switch` (Modern alternative to checkout for branches)

```bash
git switch feature-login          # Switch branch
git switch -c feature-signup      # Create + switch
git switch main                   # Back to main
```

---

### 9.5 Branch Workflow Diagram

```
         Feature Branch
              │
    ┌─────── C4 ── C5 ── C6   (feature-login)
    │
C1 ── C2 ── C3 ── C7 ── C8    (main)

C = Commit
```

---

---

## 📂 CHAPTER 10: GIT MERGE

---

### 10.1 How to Merge

```bash
# Step 1: Switch to the branch you want to merge INTO
git checkout main

# Step 2: Merge the feature branch into main
git merge feature-login
```

### 10.2 Types of Merge

#### **Fast-Forward Merge** (no divergence)

```
BEFORE:
main:    C1 ── C2
                 \
feature:          C3 ── C4

AFTER merge:
main:    C1 ── C2 ── C3 ── C4
(main pointer just moves forward)
```

#### **Three-Way Merge** (branches diverged)

```
BEFORE:
main:    C1 ── C2 ── C5
                 \
feature:          C3 ── C4

AFTER merge:
main:    C1 ── C2 ── C5 ── M (merge commit)
                 \         /
feature:          C3 ── C4
```

### 10.3 Merge Conflicts

When **same lines** are modified in both branches:

```
<<<<<<< HEAD
This is the content from CURRENT branch (main)
=======
This is the content from INCOMING branch (feature)
>>>>>>> feature-login
```

**How to resolve:**
1. Open the conflicted file
2. Choose which code to keep (or combine both)
3. Remove the `<<<<<<`, `======`, `>>>>>>` markers
4. Save the file
5. `git add .`
6. `git commit -m "Resolved merge conflict"`

---

---

## 📂 CHAPTER 11: REMOTE REPOSITORY (GitHub)

---

### 11.1 Git Clone — Copy a Remote Repository

```bash
git clone <repository-url>
```

```bash
# HTTPS clone
git clone https://github.com/username/repo-name.git

# SSH clone
git clone git@github.com:username/repo-name.git

# Clone into a specific folder name
git clone https://github.com/username/repo-name.git my-folder

# Clone a specific branch
git clone -b branch-name https://github.com/username/repo-name.git

# Shallow clone (only latest commit — faster for large repos)
git clone --depth 1 https://github.com/username/repo-name.git
```

**What `git clone` does:**
1. Creates a new folder with the repo name
2. Downloads ALL files, branches, and commit history
3. Sets up `origin` as the remote automatically
4. Checks out the default branch (main/master)

---

### 11.2 Remote Setup (for existing local repo)

```bash
# Add a remote repository
git remote add origin https://github.com/username/repo-name.git

# View remotes
git remote -v

# Output:
# origin  https://github.com/username/repo-name.git (fetch)
# origin  https://github.com/username/repo-name.git (push)

# Remove a remote
git remote remove origin

# Change remote URL
git remote set-url origin https://github.com/username/new-repo.git
```

---

### 11.3 Push — Upload to Remote

```bash
# First push (set upstream)
git push -u origin main

# After first push
git push

# Push a specific branch
git push origin feature-login

# Push all branches
git push --all

# Force push (⚠️ DANGEROUS — overwrites remote)
git push --force
git push -f
```

---

### 11.4 Pull — Download + Merge from Remote

```bash
git pull origin main

# Or simply (if upstream is set)
git pull
```

> **`git pull` = `git fetch` + `git merge`**

---

### 11.5 Fetch — Download Without Merging

```bash
git fetch origin

# Fetch a specific branch
git fetch origin main

# Fetch all remotes
git fetch --all
```

> Fetch downloads the latest data but does NOT change your working files.
> You then manually merge:

```bash
git fetch origin
git merge origin/main
```

---

### 11.6 Pull vs Fetch vs Merge — Complete Diagram

```
┌──────────────────────────────────────────────────────────┐
│                                                          │
│  REMOTE REPO (GitHub)                                    │
│  ┌─────────────────────────────┐                         │
│  │  C1 ── C2 ── C3 ── C4 ── C5│                         │
│  └─────────────────────────────┘                         │
│            │                │                            │
│        git fetch         git pull                        │
│            │           (fetch+merge)                     │
│            ▼                │                            │
│  LOCAL REMOTE TRACKING      │                            │
│  (origin/main)              │                            │
│  ┌─────────────────┐        │                            │
│  │ C1──C2──C3──C4──C5│      │                            │
│  └─────────────────┘        │                            │
│            │                │                            │
│        git merge            │                            │
│            │                │                            │
│            ▼                ▼                            │
│  LOCAL BRANCH (main)                                     │
│  ┌─────────────────────────────┐                         │
│  │  C1 ── C2 ── C3 ── C4 ── C5│                         │
│  └─────────────────────────────┘                         │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

| Command | Downloads? | Merges? | Safe? |
|---------|-----------|---------|-------|
| `git fetch` | ✅ Yes | ❌ No | ✅ Very Safe |
| `git merge` | ❌ No | ✅ Yes | ⚠️ May cause conflicts |
| `git pull` | ✅ Yes | ✅ Yes | ⚠️ May cause conflicts |

---

---

## 📂 CHAPTER 12: COMPLETE CLONING WORKFLOW

---

### Step-by-Step: Clone and Work on a Repository

```bash
# STEP 1: Clone the repository
git clone https://github.com/username/project.git

# STEP 2: Navigate into the project
cd project

# STEP 3: Check current status
git status

# STEP 4: Check which branch you're on
git branch

# STEP 5: Create a new feature branch
git checkout -b feature-awesome

# STEP 6: Make changes to files (edit, create, delete)
touch newfile.js
# ... edit files in VS Code ...

# STEP 7: Check what changed
git status
git diff

# STEP 8: Stage changes
git add .

# STEP 9: Commit changes
git commit -m "Add awesome feature"

# STEP 10: Push YOUR branch to remote
git push -u origin feature-awesome

# STEP 11: Go to GitHub → Create Pull Request

# STEP 12: After PR is merged, switch back to main
git checkout main

# STEP 13: Pull the latest main
git pull origin main

# STEP 14: Delete the feature branch (cleanup)
git branch -d feature-awesome
```

---

---

## 📂 CHAPTER 13: PULL REQUEST (PR) — Complete Guide

---

### 13.1 What is a Pull Request?

```
A Pull Request (PR) is a REQUEST to merge your branch into another branch
(usually main). It allows team members to REVIEW your code before merging.
```

### 13.2 Pull Request Workflow

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  1. Clone / Fork the repo                                       │
│         │                                                       │
│  2. Create a new branch                                         │
│         │                                                       │
│  3. Make changes & commit                                       │
│         │                                                       │
│  4. Push branch to remote (GitHub)                              │
│         │                                                       │
│  5. Go to GitHub → Click "Compare & Pull Request"               │
│         │                                                       │
│  6. Fill in PR details:                                         │
│     ├── Title: "Add login feature"                              │
│     ├── Description: What changed and why                       │
│     ├── Reviewers: Assign team members                          │
│     ├── Labels: bug, feature, enhancement                       │
│     └── Base branch: main ← Compare branch: feature-login      │
│         │                                                       │
│  7. Team reviews code                                           │
│     ├── Comments on specific lines                              │
│     ├── Request changes                                         │
│     └── Approve                                                 │
│         │                                                       │
│  8. Resolve any requested changes                               │
│     ├── Make changes locally                                    │
│     ├── Commit & push again (PR updates automatically)          │
│         │                                                       │
│  9. Merge the PR (on GitHub)                                    │
│     ├── Merge commit (keeps all commits)                        │
│     ├── Squash and merge (combines into 1 commit)               │
│     └── Rebase and merge (linear history)                       │
│         │                                                       │
│  10. Delete the feature branch (cleanup)                        │
│         │                                                       │
│  11. Pull latest main locally                                   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 13.3 Fork vs Clone

| Feature | Clone | Fork |
|---------|-------|------|
| Creates copy | On your local machine | On your GitHub account |
| Used for | Your own repos / team repos | Contributing to OTHERS' repos |
| Push access | If you have permission | To your fork, then PR to original |
| Command | `git clone <url>` | Click "Fork" on GitHub, then clone your fork |

### 13.4 Fork Workflow (Contributing to Open Source)

```bash
# 1. Fork on GitHub (click "Fork" button)

# 2. Clone YOUR fork
git clone https://github.com/YOUR-USERNAME/repo.git
cd repo

# 3. Add original repo as "upstream"
git remote add upstream https://github.com/ORIGINAL-OWNER/repo.git

# 4. Verify remotes
git remote -v
# origin    https://github.com/YOUR-USERNAME/repo.git (your fork)
# upstream  https://github.com/ORIGINAL-OWNER/repo.git (original)

# 5. Create feature branch
git checkout -b fix-typo

# 6. Make changes, commit, push to YOUR fork
git add .
git commit -m "Fix typo in README"
git push origin fix-typo

# 7. Go to GitHub → Create Pull Request (from your fork to original repo)

# 8. Keep your fork updated
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

---

---

## 📂 CHAPTER 14: `.md` FILE — MARKDOWN FORMAT

---

### 14.1 What is a `.md` File?

```
.md = Markdown file
It's a lightweight markup language for creating formatted text.
README.md is the first file people see on your GitHub repository.
```

### 14.2 Complete Markdown Syntax

```markdown
# Heading 1 (Largest)
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6 (Smallest)

**Bold Text**
*Italic Text*
***Bold and Italic***
~~Strikethrough~~

- Unordered List Item 1
- Unordered List Item 2
  - Nested Item

1. Ordered List Item 1
2. Ordered List Item 2
   1. Nested Ordered Item

[Link Text](https://www.example.com)

![Alt Text for Image](image-url.png)

> Blockquote — used for callouts or quotes

`inline code`

​```javascript
// Code block with syntax highlighting
function hello() {
  console.log("Hello World!");
}
​```

| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Data 1   | Data 2   | Data 3   |

---  (Horizontal line)

- [ ] Unchecked checkbox
- [x] Checked checkbox
```

### 14.3 Sample README.md Template

```markdown
# 🚀 Project Name

## 📖 Description
Brief description of what this project does.

## 🛠️ Technologies Used
- HTML5
- CSS3
- JavaScript
- React.js

## ⚙️ Installation

​```bash
# Clone the repository
git clone https://github.com/username/project.git

# Navigate to the project
cd project

# Install dependencies
npm install

# Start the development server
npm start
​```

## 📁 Project Structure

​```
project/
├── public/
│   └── index.html
├── src/
│   ├── components/
│   ├── App.js
│   └── index.js
├── package.json
├── .gitignore
└── README.md
​```

## 🤝 Contributing
1. Fork the project
2. Create your feature branch (`git checkout -b feature/amazing`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing`)
5. Open a Pull Request

## 📝 License
This project is licensed under the MIT License.

## 👤 Author
**Your Name** — [GitHub](https://github.com/username)
```

---

---

## 📂 CHAPTER 15: `.gitignore` — Ignoring Files

---

```bash
# Create .gitignore file
touch .gitignore
```

### Common `.gitignore` entries:

```gitignore
# Dependencies
node_modules/
vendor/

# Environment files (secrets, API keys)
.env
.env.local

# Build output
dist/
build/
*.min.js
*.min.css

# OS files
.DS_Store          # Mac
Thumbs.db          # Windows

# IDE files
.vscode/
.idea/
*.swp
*.swo

# Logs
*.log
npm-debug.log*

# Compiled files
*.class
*.o
*.pyc

# Archives
*.zip
*.tar.gz
```

---

---

## 📂 CHAPTER 16: COMPLETE GIT CHEAT SHEET

---

### SETUP

```bash
git config --global user.name "Name"
git config --global user.email "email@example.com"
git config --list
```

### CREATE

```bash
git init                              # Initialize new repo
git clone <url>                       # Clone existing repo
```

### BASIC WORKFLOW

```bash
git status                            # Check status
git add <file>                        # Stage specific file
git add .                             # Stage all (current dir)
git add -A                            # Stage all (entire repo)
git commit -m "message"               # Commit
git commit -am "message"              # Add tracked + commit
```

### BRANCHES

```bash
git branch                            # List branches
git branch <name>                     # Create branch
git checkout <name>                   # Switch branch
git checkout -b <name>                # Create + switch
git switch <name>                     # Switch (modern)
git switch -c <name>                  # Create + switch (modern)
git branch -d <name>                  # Delete branch
git branch -D <name>                  # Force delete
```

### MERGE

```bash
git checkout main                     # Go to target branch
git merge <branch-name>               # Merge branch into current
```

### REMOTE

```bash
git remote add origin <url>           # Add remote
git remote -v                         # View remotes
git push -u origin main               # First push
git push                              # Push
git pull                              # Pull (fetch + merge)
git fetch                             # Fetch only
```

### UNDO

```bash
git restore <file>                    # Discard working changes
git restore --staged <file>           # Unstage
git reset                             # Unstage all
git reset HEAD~1                      # Undo last commit (keep changes)
git reset --soft HEAD~1               # Undo commit (keep staged)
git reset --hard HEAD~1               # ⚠️ Undo commit + DELETE changes
```

### INSPECT

```bash
git log                               # Full history
git log --oneline                     # Compact history
git log --oneline --graph --all       # Visual branch history
git diff                              # Show unstaged changes
git diff --staged                     # Show staged changes
git diff branch1..branch2             # Compare branches
```

### STASH (Bonus)

```bash
git stash                             # Save changes temporarily
git stash list                        # List stashes
git stash pop                         # Apply and remove stash
git stash apply                       # Apply but keep stash
git stash drop                        # Delete a stash
```

---

---

## 📂 CHAPTER 17: COMPLETE WORKFLOW DIAGRAM

---

```
╔══════════════════════════════════════════════════════════════════╗
║                    COMPLETE GIT WORKFLOW                         ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  LOCAL MACHINE                          REMOTE (GitHub)          ║
║  ─────────────                          ───────────────          ║
║                                                                  ║
║  ┌──────────────┐                      ┌──────────────┐         ║
║  │   WORKING    │                      │   REMOTE     │         ║
║  │  DIRECTORY   │                      │   REPO       │         ║
║  │  (edit files)│                      │  (GitHub)    │         ║
║  └──────┬───────┘                      └──────┬───────┘         ║
║         │                                     │                  ║
║    git add                               git push                ║
║         │                                     ▲                  ║
║         ▼                                     │                  ║
║  ┌──────────────┐                             │                  ║
║  │   STAGING    │                             │                  ║
║  │    AREA      │                             │                  ║
║  │   (index)    │                             │                  ║
║  └──────┬───────┘                             │                  ║
║         │                                     │                  ║
║    git commit                                 │                  ║
║         │                                     │                  ║
║         ▼                                     │                  ║
║  ┌──────────────┐     git push        ┌───────┴──────┐          ║
║  │   LOCAL      │ ──────────────────> │   REMOTE     │          ║
║  │   REPO       │                     │   REPO       │          ║
║  │  (.git)      │ <────────────────── │  (GitHub)    │          ║
║  └──────────────┘     git pull/fetch  └──────────────┘          ║
║                                                                  ║
╚══════════════════════════════════════════════════════════════════╝
```

---

## ✅ QUICK REVISION TABLE

| Topic | Key Command | Purpose |
|-------|-------------|---------|
| Create file | `touch file.txt` | Creates empty file |
| List files | `ls -la` | Shows all files with details |
| Navigate | `cd folder` / `cd ..` | Enter/exit folders |
| Init repo | `git init` | Start tracking with Git |
| Status | `git status` | See file states (red/green) |
| Stage | `git add .` / `git add -A` | Move to staging |
| Commit | `git commit -m "msg"` | Save snapshot |
| Branch | `git branch name` | Create branch |
| Switch | `git checkout -b name` | Create + switch branch |
| Merge | `git merge branch` | Combine branches |
| Push | `git push origin main` | Upload to GitHub |
| Pull | `git pull` | Download + merge |
| Fetch | `git fetch` | Download only |
| Clone | `git clone <url>` | Copy remote repo |
| Reset | `git reset HEAD~1` | Undo last commit |
| Restore | `git restore file` | Discard changes |
| Config | `git config --global` | Set name/email |
| Log | `git log --oneline` | View history |

---

> 📌 **Golden Rule:** `git fetch` is SAFE, `git pull` may cause CONFLICTS, `git push --force` is DANGEROUS.

> 📌 **Remember:** RED = Working Directory, GREEN = Staged, Committed = Clean.

---

*Notes prepared for complete Git mastery — From terminal basics to Pull Requests.* ✅
