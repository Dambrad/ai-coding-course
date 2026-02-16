# GitHub Setup & Hosting Guide

> Learn Git and GitHub to store and share your code.

**Time**: 1-2 hours  
**Difficulty**: Beginner

---

## 🎯 What You'll Learn

- What Git and GitHub are
- Creating repositories
- Basic Git workflow (add, commit, push)
- Hosting your code online

---

## 📝 Understanding Git & GitHub

### What is Git?
**Git** = Version control system (like "Track Changes" for code)

**Why you need it**:
- Never lose your work
- Try new things without breaking what works
- See your entire project history

### What is GitHub?
**GitHub** = Website that hosts Git repositories online

**Why GitHub**:
- Backup your code in the cloud
- Share code with the world
- Build a portfolio

---

## 🛠️ Setup

### 1. Create GitHub Account
1. Go to https://github.com
2. Sign up (choose a professional username!)
3. Verify your email

### 2. Configure Git
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Verify
git config --global user.name
```

### 3. Set Up SSH Keys (Recommended)

**Generate key**:
```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
# Press Enter for default location
# Enter passphrase (or leave blank)

# Start SSH agent
eval "$(ssh-agent -s)"

# Add key
ssh-add ~/.ssh/id_ed25519

# Copy public key
cat ~/.ssh/id_ed25519.pub
```

**Add to GitHub**:
1. GitHub → Settings → SSH and GPG keys
2. "New SSH key"
3. Paste your public key
4. Save

**Test**:
```bash
ssh -T git@github.com
# Should see: "Hi username! You've successfully authenticated"
```

---

## 💻 Creating Your First Repository

### Method 1: Start on GitHub (Easiest)

**On GitHub**:
1. Click + → New repository
2. Name: `my-first-repo`
3. Description: "Learning Git"
4. Public, Add README
5. Create

**On Your Computer**:
```bash
cd ~/coding-projects
git clone git@github.com:YOUR-USERNAME/my-first-repo.git
cd my-first-repo
```

### Method 2: Start Locally

```bash
# Create folder
mkdir my-project
cd my-project

# Initialize Git
git init

# Create README
echo "# My Project" > README.md

# First commit
git add README.md
git commit -m "Initial commit"

# Create repo on GitHub, then:
git remote add origin git@github.com:YOUR-USERNAME/my-project.git
git branch -M main
git push -u origin main
```

---

## 🔄 Basic Git Workflow

```bash
# 1. Check status
git status

# 2. Add files
git add filename.py    # One file
git add .              # All files

# 3. Commit
git commit -m "Describe what changed"

# 4. Push to GitHub
git push

# 5. Get latest from GitHub
git pull
```

### Complete Example

```bash
# Create a file
echo "print('Hello!')" > hello.py

# Check what changed
git status

# Add it
git add hello.py

# Commit
git commit -m "Add hello script"

# Push to GitHub
git push

# Check GitHub - you'll see your file!
```

---

## 🚫 .gitignore - What NOT to Commit

Create `.gitignore`:

```
# Python
__pycache__/
*.pyc
.Python
venv/

# Secrets - NEVER COMMIT THESE!
.env
*.key
secrets.txt

# OS
.DS_Store

# IDE
.vscode/
.idea/
```

**Critical**: Never commit API keys or passwords!

---

## 📚 Hosting This Course on GitHub

### Option 1: Create Your Own Repository

```bash
# 1. Create repo on GitHub: "ai-coding-course"

# 2. Upload this folder
cd path/to/ai-coding-course
git init
git add .
git commit -m "Initial course setup"
git remote add origin git@github.com:YOUR-USERNAME/ai-coding-course.git
git branch -M main
git push -u origin main
```

### Recommended Structure

```
ai-coding-course/
├── README.md
├── modules/
│   ├── 00-setup.md
│   └── ...
├── labs/          # Your solutions
├── projects/      # Your projects
└── notes/         # Your notes
```

---

## 🔧 Common Git Commands

```bash
# View history
git log
git log --oneline

# Undo changes (before add)
git checkout -- filename.py

# Unstage file (after add)
git reset HEAD filename.py

# Undo last commit (keep changes)
git reset --soft HEAD~1

# See remote
git remote -v
```

---

## 🛠️ Troubleshooting

**"Permission denied (publickey)"**
→ Set up SSH keys (see Setup section)

**"not a git repository"**
→ Run `git init` or `cd` to correct folder

**"Your branch is ahead"**
→ Run `git push`

**"Your branch is behind"**
→ Run `git pull`

---

## 📚 Resources

- [GitHub Guides](https://guides.github.com/)
- [Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)
- [Learn Git Branching](https://learngitbranching.js.org/) - Interactive

---

## ✅ Checklist

- [ ] Create GitHub account
- [ ] Set up SSH keys
- [ ] Create a repository
- [ ] Make changes and commit
- [ ] Push to GitHub
- [ ] Create .gitignore

---

## Quick Reference

```bash
git status          # What's changed?
git add .           # Stage all
git commit -m "msg" # Save changes
git push            # Send to GitHub
git pull            # Get from GitHub
git log             # View history
```

---

**Great work!** You now know Git & GitHub - essential for developers! 🎉
