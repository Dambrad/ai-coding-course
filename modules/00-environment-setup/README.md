# Module 0: Environment Setup

> Setting up your M3 MacBook Air for AI development — from zero to ready-to-code in 60 minutes.

## 🎯 Module Goals

By the end of this module, you'll have:
- A fully configured development environment
- Python 3.11+ installed and working
- VS Code set up with essential extensions
- Git and GitHub configured
- Your first "Hello, AI World!" program running

## ⏱️ Estimated Time: 60-90 minutes

## 📚 What We're Installing

1. **Homebrew**: macOS package manager (makes installing everything else easy)
2. **Python 3.11+**: The programming language we'll use
3. **VS Code**: Your code editor (where you'll write code)
4. **Git**: Version control (tracks changes to your code)
5. **GitHub Account**: Where you'll store and share your code

## 🚀 Step-by-Step Setup

### Step 1: Install Homebrew (10 min)

Homebrew is like an App Store for developer tools. It makes installing everything else painless.

1. **Open Terminal**
   - Press `Cmd + Space`, type "Terminal", press Enter
   - You'll see a window with text and a cursor

2. **Install Homebrew**
   - Copy and paste this entire command into Terminal:
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
   - Press Enter
   - Enter your Mac password when prompted (you won't see it as you type - that's normal!)
   - Wait 5-10 minutes for installation

3. **Verify Homebrew**
   - Type this and press Enter:
   ```bash
   brew --version
   ```
   - You should see something like `Homebrew 4.x.x`

**Troubleshooting**: If `brew` command isn't found, you may need to add it to your PATH:
```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

### Step 2: Install Python (10 min)

Your Mac comes with Python, but it's an old version. We need the latest.

1. **Install Python 3.11**
   ```bash
   brew install python@3.11
   ```

2. **Verify Python**
   ```bash
   python3 --version
   ```
   - Should show `Python 3.11.x` or higher

3. **Set up pip (Python's package manager)**
   ```bash
   pip3 --version
   ```
   - Should show `pip 23.x.x` or similar

**What just happened?** You installed Python, the programming language that powers most AI applications. pip is Python's package manager - it installs libraries (pre-written code) that we'll use later.

### Step 3: Install VS Code (15 min)

VS Code is where you'll write code. Think of it as Microsoft Word, but for programmers.

1. **Install VS Code**
   ```bash
   brew install --cask visual-studio-code
   ```

2. **Launch VS Code**
   - Press `Cmd + Space`, type "Visual Studio Code", press Enter
   - You should see VS Code open

3. **Install Essential Extensions**
   - Click the Extensions icon on the left sidebar (looks like 4 squares)
   - Search for and install these:
     - **Python** (by Microsoft) - Python support
     - **Pylance** (by Microsoft) - Smart Python features
     - **autoDocstring** - Auto-generates documentation
     - **Error Lens** - Shows errors inline
     - **GitHub Copilot** (optional, paid) - AI coding assistant

4. **Configure VS Code for Python**
   - Press `Cmd + Shift + P` (opens command palette)
   - Type "Python: Select Interpreter"
   - Choose Python 3.11.x

**Pro Tip**: VS Code has tons of keyboard shortcuts. Press `Cmd + Shift + P` anytime to search for commands.

### Step 4: Install Git (10 min)

Git tracks changes to your code. GitHub hosts your code online.

1. **Install Git**
   ```bash
   brew install git
   ```

2. **Configure Git**
   - Set your name (replace with your actual name):
   ```bash
   git config --global user.name "Your Name"
   ```
   
   - Set your email:
   ```bash
   git config --global user.email "your.email@example.com"
   ```

3. **Verify Git**
   ```bash
   git --version
   ```

### Step 5: Set Up GitHub (15 min)

GitHub is where you'll store your code and share it with others.

1. **Create GitHub Account**
   - Go to [github.com](https://github.com)
   - Click "Sign up"
   - Follow the prompts (use the same email as your git config)

2. **Set Up GitHub Authentication**
   
   **Option A: SSH (Recommended)**
   ```bash
   # Generate SSH key
   ssh-keygen -t ed25519 -C "your.email@example.com"
   
   # Press Enter 3 times (accept defaults)
   
   # Copy your public key
   cat ~/.ssh/id_ed25519.pub
   ```
   
   - Go to GitHub → Settings → SSH and GPG keys → New SSH key
   - Paste the key you just copied
   - Give it a title like "M3 MacBook Air"
   - Click "Add SSH key"

   **Option B: Personal Access Token (Simpler)**
   - GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
   - Generate new token
   - Select scopes: `repo`, `workflow`
   - Copy the token (save it somewhere safe!)
   - Use this token as your password when pushing to GitHub

3. **Test GitHub Connection**
   ```bash
   ssh -T git@github.com
   ```
   - You should see: "Hi username! You've successfully authenticated..."

### Step 6: Create Your Development Folder (5 min)

Let's organize your projects.

1. **Create a dedicated folder**
   ```bash
   mkdir -p ~/Developer/ai-projects
   cd ~/Developer/ai-projects
   ```

2. **This is your workspace**
   - All your AI projects will live here
   - Easy to find, easy to backup

### Step 7: Your First Python Program (10 min)

Let's make sure everything works!

1. **Create a test file**
   ```bash
   cd ~/Developer/ai-projects
   mkdir hello-ai
   cd hello-ai
   code .
   ```
   - This opens VS Code in your new folder

2. **Create `hello.py`**
   - In VS Code, click File → New File
   - Name it `hello.py`
   - Type this code:
   ```python
   # My first Python program!
   print("Hello, AI World!")
   print("Python version:", __import__('sys').version)
   print("I'm ready to build AI agents! 🚀")
   ```

3. **Run the program**
   - In VS Code, press `Ctrl + ~` to open the terminal
   - Type:
   ```bash
   python3 hello.py
   ```
   - You should see your messages printed!

**🎉 Congratulations!** You just wrote and ran your first Python program!

## 🧪 Lab: Environment Verification

Complete this lab to ensure everything is set up correctly.

### Lab Checklist

Create a new file called `environment_check.py` and run each check:

```python
#!/usr/bin/env python3
"""Environment verification script"""

import sys
import subprocess

def check_python_version():
    version = sys.version_info
    print(f"✓ Python {version.major}.{version.minor}.{version.micro}")
    assert version.major == 3 and version.minor >= 11, "Python 3.11+ required"

def check_pip():
    result = subprocess.run(['pip3', '--version'], capture_output=True, text=True)
    print(f"✓ pip installed: {result.stdout.strip()}")

def check_git():
    result = subprocess.run(['git', '--version'], capture_output=True, text=True)
    print(f"✓ Git installed: {result.stdout.strip()}")

if __name__ == "__main__":
    print("🔍 Checking your environment...\n")
    check_python_version()
    check_pip()
    check_git()
    print("\n✅ All checks passed! You're ready to code!")
```

**Run it**:
```bash
python3 environment_check.py
```

**Expected output**:
```
🔍 Checking your environment...

✓ Python 3.11.x
✓ pip installed: pip 23.x.x
✓ Git installed: git version 2.x.x

✅ All checks passed! You're ready to code!
```

## 📦 Optional: Install Additional Tools

These aren't required but are helpful:

### iTerm2 (Better Terminal)
```bash
brew install --cask iterm2
```

### Postman (API Testing)
```bash
brew install --cask postman
```

### Rectangle (Window Management)
```bash
brew install --cask rectangle
```

## 🎓 Understanding Your New Tools

### What is Terminal?
A text-based way to control your computer. Instead of clicking, you type commands. More powerful and faster once you learn it.

### What is Python?
A programming language that's easy to read and write. It's the #1 language for AI and data science.

### What is VS Code?
A code editor made by Microsoft. Free, powerful, and has extensions for everything.

### What is Git?
Version control software. It saves "snapshots" of your code so you can go back in time if something breaks.

### What is GitHub?
A website that hosts Git repositories. Think of it as Google Drive for code.

## 🐛 Common Issues & Fixes

### Issue: "command not found: brew"
**Fix**: Run the PATH command from Step 1.3

### Issue: "Python 2.7" shows instead of 3.11
**Fix**: Use `python3` instead of `python` in all commands

### Issue: VS Code can't find Python
**Fix**: 
1. `Cmd + Shift + P`
2. Type "Python: Select Interpreter"
3. Choose Python 3.11

### Issue: Git asks for password every time
**Fix**: Set up SSH keys (Step 5, Option A)

## ✅ Module Completion Checklist

Before moving to Module 1, ensure you can:
- [X] Open Terminal and run commands
- [X] Check Python version with `python3 --version`
- [X] Open VS Code from Terminal with `code .`
- [X] Create and run a Python file
- [X] Successfully run `environment_check.py`
- [X] Have a GitHub account and can authenticate

## 🎯 Next Steps

Once you complete this module:
1. Take a 5-minute break (seriously!)
2. Keep VS Code and Terminal open
3. Move to **[Module 1: Python Crash Course](../01-python-basics/README.md)**

## 📚 Additional Resources

### Videos
- [VS Code Setup for Python](https://www.youtube.com/watch?v=W--_EOzdTHk) - 15 min
- [Terminal Basics for Beginners](https://www.youtube.com/watch?v=aKRYQsKR46I) - 20 min
- [Git and GitHub for Beginners](https://www.youtube.com/watch?v=RGOj5yH7evk) - 1 hour

### Documentation
- [Homebrew Documentation](https://docs.brew.sh/)
- [Python Official Tutorial](https://docs.python.org/3/tutorial/)
- [VS Code Python Guide](https://code.visualstudio.com/docs/python/python-tutorial)

### Community
- [r/learnpython](https://reddit.com/r/learnpython) - Beginner-friendly subreddit
- [Python Discord](https://discord.gg/python) - Real-time help

---

**Estimated Time**: 60-90 minutes  
**Difficulty**: ⭐☆☆☆☆ (Beginner)  
**Prerequisites**: None

**Next**: [Module 1: Python Crash Course →](../01-python-basics/README.md)
