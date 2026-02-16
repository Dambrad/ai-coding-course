# GitHub Setup & Hosting Guide

> Learn how to host this course on GitHub and manage your code like a pro.

## 🎯 What You'll Learn

- Creating a GitHub repository
- Pushing code to GitHub
- Understanding Git basics
- Managing your course repository
- Sharing your work

## ⏱️ Estimated Time: 30-45 minutes

---

## 📚 Git vs GitHub - What's the Difference?

**Git**: Version control software (runs on your computer)
- Tracks changes to your code
- Like "Save" but for entire project states
- Works offline

**GitHub**: Website that hosts Git repositories
- Stores your code online
- Share code with others
- Collaborate on projects

**Analogy**: Git is like Microsoft Word (tracks changes), GitHub is like Google Drive (stores and shares documents).

---

## 🚀 Step 1: Create Your First Repository

### Option A: From GitHub Website (Easiest)

1. **Go to GitHub.com** and log in

2. **Click the "+" icon** (top right) → "New repository"

3. **Fill in repository details**:
   - **Repository name**: `ai-coding-course`
   - **Description**: "My journey learning AI coding and agent development"
   - **Public** (so you can share) or **Private** (just for you)
   - ✅ Check "Add a README file"
   - **Add .gitignore**: Python
   - **License**: MIT (optional)

4. **Click "Create repository"**

5. **Clone to your computer**:
   ```bash
   cd ~/Developer
   git clone https://github.com/YOUR_USERNAME/ai-coding-course.git
   cd ai-coding-course
   ```

### Option B: From Terminal (More Control)

```bash
# Navigate to your project
cd ~/Developer/ai-coding-course

# Initialize git
git init

# Create README
echo "# AI Coding Course" > README.md

# Add all files
git add .

# Make first commit
git commit -m "Initial commit"

# Create repository on GitHub (you'll need GitHub CLI)
# Or create it manually on github.com

# Link local to GitHub
git remote add origin https://github.com/YOUR_USERNAME/ai-coding-course.git

# Push to GitHub
git branch -M main
git push -u origin main
```

---

## 📖 Understanding Git Basics

### The Git Workflow

```
Working Directory  →  Staging Area  →  Repository
(Your files)          (git add)        (git commit)
```

### Essential Git Commands

```bash
# Check status of files
git status

# Add files to staging
git add filename.py          # Add specific file
git add .                    # Add all files
git add *.py                 # Add all Python files

# Commit changes
git commit -m "Your message describing what you did"

# Push to GitHub
git push

# Pull latest changes
git pull

# View commit history
git log
git log --oneline            # Shorter version

# See what changed
git diff                     # Changes not yet staged
git diff --staged            # Changes staged for commit
```

---

## 🌿 Working with Branches

Branches let you work on features without affecting the main code.

```bash
# Create a new branch
git checkout -b feature/new-agent

# List all branches
git branch

# Switch branches
git checkout main

# Merge a branch into current branch
git checkout main
git merge feature/new-agent

# Delete a branch
git branch -d feature/new-agent
```

**When to use branches**:
- Experimenting with new features
- Fixing bugs
- Working on modules independently

---

## 📦 Typical Workflow: Adding New Module

Let's say you just completed Module 3:

```bash
# 1. Check current status
git status

# 2. Add the new module
git add modules/03-prompt-engineering/

# 3. Commit with descriptive message
git commit -m "Complete Module 3: Prompt Engineering

- Added comprehensive guide on prompt engineering
- Created examples for few-shot learning
- Built prompt template system lab
- Added resources and best practices"

# 4. Push to GitHub
git push
```

### Good Commit Messages

```bash
# ❌ Bad
git commit -m "stuff"
git commit -m "fixed bug"
git commit -m "updates"

# ✅ Good
git commit -m "Add calculator tool to agent framework"
git commit -m "Fix API rate limiting in chat client"
git commit -m "Complete Module 6 lab exercises"
```

---

## 📝 Creating a .gitignore File

Some files shouldn't be in Git (API keys, temp files, etc.).

Create `.gitignore` in your repository root:

```bash
# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
env/
venv/
.venv/

# Environment variables
.env
.env.local
*.env

# API Keys (NEVER commit these!)
*api_key*
*secret*
config/secrets.json

# IDEs
.vscode/
.idea/
*.swp
*.swo
*~

# OS
.DS_Store
Thumbs.db

# Project specific
tasks.json
notes.txt
scratch/
temp/
test_outputs/

# Data files (can be large)
*.csv
*.xlsx
data/
datasets/
```

**Add and commit it**:
```bash
git add .gitignore
git commit -m "Add .gitignore for Python project"
```

---

## 🔐 Protecting Your API Keys

**CRITICAL**: Never commit API keys to Git!

### Method 1: Environment Variables (Recommended)

```bash
# In your ~/.zshrc or ~/.bash_profile
export ANTHROPIC_API_KEY="your-key-here"
export OPENAI_API_KEY="your-key-here"
```

In your code:
```python
import os
api_key = os.getenv("ANTHROPIC_API_KEY")
```

### Method 2: .env Files

```bash
# Install python-dotenv
pip3 install python-dotenv

# Create .env file (make sure it's in .gitignore!)
echo "ANTHROPIC_API_KEY=your-key" > .env
echo "OPENAI_API_KEY=your-key" >> .env
```

In your code:
```python
from dotenv import load_dotenv
import os

load_dotenv()  # Load .env file
api_key = os.getenv("ANTHROPIC_API_KEY")
```

### Method 3: Config File

Create `config/secrets.json` (in .gitignore):
```json
{
  "anthropic_api_key": "your-key",
  "openai_api_key": "your-key"
}
```

Create `config/secrets.example.json` (safe to commit):
```json
{
  "anthropic_api_key": "your-anthropic-api-key-here",
  "openai_api_key": "your-openai-api-key-here"
}
```

---

## 📂 Recommended Repository Structure

```
ai-coding-course/
├── README.md                 # Main course overview
├── .gitignore               # Files to ignore
├── LICENSE                  # Optional: MIT, Apache, etc.
│
├── modules/                 # Course modules
│   ├── 00-environment-setup/
│   ├── 01-python-basics/
│   ├── 02-understanding-ai/
│   └── ...
│
├── projects/                # Your projects
│   ├── project-1-chatbot/
│   ├── project-2-agent/
│   └── ...
│
├── resources/              # Additional resources
│   ├── cheatsheets/
│   ├── videos.md
│   └── books.md
│
├── notes/                  # Your personal notes
│   └── .gitkeep            # Keep empty folder in git
│
└── config/                 # Configuration
    ├── secrets.example.json
    └── .gitkeep
```

---

## 🚀 Publishing Your Course Repository

### Making it Public

1. Go to your repository on GitHub
2. Click **Settings**
3. Scroll to "Danger Zone"
4. Click **Change visibility** → **Make public**

### Adding a Good README

Your README is the first thing people see. Include:

```markdown
# AI Coding Course

My personal learning journey through AI coding and agent development.

## 📚 What's Inside

- Complete modules from zero to building AI agents
- Hands-on labs and projects
- Working code examples
- Personal notes and insights

## 🎯 Progress

- [x] Module 0: Environment Setup
- [x] Module 1: Python Basics
- [x] Module 2: Understanding AI
- [ ] Module 3: Prompt Engineering (in progress)
- [ ] ... more modules

## 🛠️ Projects

- [x] Simple Chatbot
- [x] Calculator Agent
- [ ] Research Assistant (in progress)

## 📖 Resources

Links to helpful tutorials, videos, and documentation I found useful.

## 🤝 Contributing

Feel free to fork this repository for your own learning journey!

## 📜 License

MIT License - feel free to use for your own learning.
```

---

## 🔄 Daily Workflow

Here's a typical day of working on the course:

```bash
# Morning: Start work
cd ~/Developer/ai-coding-course
git pull                    # Get latest changes

# Work on Module 4
code modules/04-api-basics/

# ... do some coding ...

# Save your progress
git add modules/04-api-basics/
git commit -m "Work on Module 4: API basics examples"
git push

# ... continue working ...

# Complete the module
git add modules/04-api-basics/
git commit -m "Complete Module 4: API Basics

- Finished all examples
- Completed lab exercises
- Added API client wrapper
- Tested with both Claude and GPT-4"
git push

# Evening: View your progress
git log --oneline
```

---

## 🐛 Troubleshooting Common Issues

### Issue: "Permission denied (publickey)"

**Problem**: SSH authentication not set up

**Fix**:
```bash
# Generate SSH key (if you haven't)
ssh-keygen -t ed25519 -C "your.email@example.com"

# Copy public key
cat ~/.ssh/id_ed25519.pub

# Add to GitHub: Settings → SSH Keys → New SSH Key
```

### Issue: "Failed to push some refs"

**Problem**: Remote has changes you don't have locally

**Fix**:
```bash
git pull --rebase origin main
git push
```

### Issue: Accidentally committed sensitive data

**Problem**: API key in commit history

**Fix** (if just committed, not pushed):
```bash
git reset HEAD~1            # Undo last commit
# Remove the sensitive data
git add .
git commit -m "..."
```

**Fix** (if already pushed):
```bash
# 1. Remove from latest commit
git rm --cached path/to/file
git commit --amend

# 2. Force push (DANGER: only if no one else uses repo)
git push --force

# 3. Rotate your API keys immediately!
```

### Issue: Merge conflicts

**Problem**: Same file edited in different places

**Fix**:
```bash
# Open the conflicted file
# Look for markers:
# <<<<<<< HEAD
# Your changes
# =======
# Other changes
# >>>>>>>

# Edit to resolve
# Remove markers
git add resolved_file.py
git commit -m "Resolve merge conflict"
```

---

## 📊 Viewing Your Repository

### On GitHub

Your repository will be at: `https://github.com/YOUR_USERNAME/ai-coding-course`

Features:
- **Code tab**: Browse all files
- **Issues**: Track bugs or todos
- **Projects**: Organize work in boards
- **Wiki**: Create documentation
- **Insights**: See contribution graphs

### Locally

```bash
# View in browser (if you have GitHub CLI)
gh repo view --web

# Or use VS Code's Git interface
code .
# Click Git icon in sidebar
```

---

## 🎓 Advanced: GitHub Features

### GitHub Issues (To-Do List)

Create issues for:
- Modules you want to complete
- Bugs you find
- Ideas for projects

```bash
# Via GitHub CLI
gh issue create --title "Complete Module 7" --body "Finish agent examples"
```

### GitHub Projects (Kanban Board)

1. Go to "Projects" tab
2. Create "Course Progress" board
3. Add columns: "To Do", "In Progress", "Done"
4. Add modules as cards
5. Move them as you progress!

### GitHub Actions (Automation)

Automatically run tests when you push code:

Create `.github/workflows/test.yml`:
```yaml
name: Run Tests

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-python@v2
        with:
          python-version: '3.11'
      - run: pip install -r requirements.txt
      - run: python -m pytest tests/
```

---

## 🎯 Course-Specific Workflow

### When Starting a New Module

```bash
# Create feature branch
git checkout -b module/04-api-basics

# Work on module
code modules/04-api-basics/

# Commit as you go
git add modules/04-api-basics/examples/
git commit -m "Add API examples for Module 4"

# When complete, merge to main
git checkout main
git merge module/04-api-basics
git push

# Delete feature branch
git branch -d module/04-api-basics
```

### When Completing a Project

```bash
# Add project
git add projects/chatbot-cli/
git commit -m "Complete chatbot CLI project

Features:
- Multi-turn conversations
- Conversation history
- Multiple AI providers
- Error handling"

git push

# Tag the milestone
git tag -a v1.0-chatbot -m "First working chatbot"
git push --tags
```

---

## ✅ Checklist: Is Your Repository Ready?

Before sharing your repository, ensure:

- [ ] No API keys or secrets in code
- [ ] .gitignore properly set up
- [ ] README has clear description
- [ ] Code is organized in modules/
- [ ] Examples actually run
- [ ] Requirements.txt lists dependencies
- [ ] License file (if making public)

---

## 🎯 Next Steps

Now that you know Git and GitHub:

1. **Commit regularly**: After each work session
2. **Write good commit messages**: Describe what and why
3. **Use branches**: For experimental features
4. **Keep secrets secret**: Never commit API keys
5. **Push often**: Don't lose your work!

---

## 📚 Additional Resources

### Interactive Tutorials
- [GitHub Skills](https://skills.github.com/) - Interactive Git/GitHub lessons
- [Git Immersion](https://gitimmersion.com/) - Step-by-step Git tutorial
- [Learn Git Branching](https://learngitbranching.js.org/) - Visual Git tutorial

### Videos
- [Git & GitHub for Beginners](https://www.youtube.com/watch?v=RGOj5yH7evk) - 1 hour crash course
- [Git Tutorial for Beginners](https://www.youtube.com/watch?v=8JJ101D3knE) - Detailed walkthrough

### Cheat Sheets
- [GitHub Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)
- [Atlassian Git Cheat Sheet](https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet)

### Documentation
- [Official Git Documentation](https://git-scm.com/doc)
- [GitHub Docs](https://docs.github.com)

---

## 🎓 Pro Tips

1. **Commit often**: Better to have many small commits than one huge one
2. **Pull before push**: Always `git pull` before `git push` to avoid conflicts
3. **Branch for experiments**: Don't break your working code
4. **Read the diff**: Always `git diff` before committing
5. **Use VS Code**: Built-in Git tools make it easier

---

**Need Help?**

- `git help <command>` - Built-in help
- `git status` - When in doubt, check status
- GitHub's [Community Forum](https://github.community/)

---

**Happy Coding!** 🚀

Remember: Everyone makes Git mistakes. The pros just know how to fix them faster.
