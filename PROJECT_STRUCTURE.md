# 📁 Project Structure

This document explains the organization of the AI Coding Course repository.

## 🗂️ Directory Structure

```
ai-coding-course/
│
├── README.md                    # Main course overview and introduction
├── QUICK_START.md              # Get started in 10 minutes
├── COURSE_OUTLINE.md           # Complete module breakdown
├── RESOURCES.md                # Curated learning resources
├── CONTRIBUTING.md             # How to contribute to the course
├── LICENSE                     # MIT License
│
├── guides/                     # Setup and reference guides
│   ├── github-setup.md        # Git & GitHub tutorial
│   ├── api-keys.md            # Managing API keys (TODO)
│   └── troubleshooting.md     # Common issues (TODO)
│
├── modules/                    # Course modules
│   │
│   ├── 00-environment-setup/  # Week 1: Getting started
│   │   ├── README.md          # Setup instructions
│   │   ├── examples/          # Example scripts
│   │   └── lab/               # Hands-on exercises
│   │
│   ├── 01-python-basics/      # Weeks 1-2: Python fundamentals
│   │   ├── README.md          # Python crash course
│   │   ├── examples/          # Code examples
│   │   │   ├── 01_variables.py
│   │   │   ├── 02_strings.py
│   │   │   └── ...
│   │   └── lab/               # Lab exercises
│   │       └── prompt_manager.py
│   │
│   ├── 02-understanding-ai/   # Week 3: AI concepts
│   │   ├── README.md          # (TODO)
│   │   └── ...
│   │
│   ├── 03-prompt-engineering/ # Week 4: Prompting
│   │   ├── README.md          # (TODO)
│   │   └── ...
│   │
│   ├── 04-api-basics/         # Week 5: API integration
│   │   ├── README.md          # Working with APIs
│   │   ├── examples/
│   │   │   ├── 01_claude_basic.py
│   │   │   ├── 02_openai_basic.py
│   │   │   └── ai_client.py
│   │   └── lab/
│   │       └── chat_cli.py
│   │
│   ├── 05-first-ai-app/       # Week 6: Building apps
│   │   ├── README.md          # (TODO)
│   │   └── ...
│   │
│   ├── 06-intro-to-agents/    # Week 7: Agent basics
│   │   ├── README.md          # Introduction to agents
│   │   ├── examples/
│   │   │   ├── calculator_agent.py
│   │   │   └── research_agent.py
│   │   └── lab/
│   │       └── task_agent.py
│   │
│   ├── 07-simple-agents/      # Week 8: Building agents
│   │   ├── README.md          # (TODO)
│   │   └── ...
│   │
│   ├── 08-agentic-workflows/  # Week 9: Multi-step
│   │   ├── README.md          # (TODO)
│   │   └── ...
│   │
│   ├── 09-tool-use/           # Week 10: Tools & functions
│   │   ├── README.md          # (TODO)
│   │   └── ...
│   │
│   ├── 10-advanced-agents/    # Week 11: Advanced patterns
│   │   ├── README.md          # (TODO)
│   │   └── ...
│   │
│   ├── 11-rag-memory/         # Week 12: RAG systems
│   │   ├── README.md          # (TODO)
│   │   └── ...
│   │
│   ├── 12-deployment/         # Week 13: Production
│   │   ├── README.md          # (TODO)
│   │   └── ...
│   │
│   └── 13-capstone-projects/  # Weeks 14-16: Final projects
│       ├── README.md          # (TODO)
│       └── project-templates/
│
├── projects/                   # Your projects (create as you go)
│   ├── .gitkeep
│   └── README.md              # Project showcase
│
├── resources/                  # Additional materials
│   ├── cheatsheets/
│   │   ├── python.md
│   │   ├── git.md
│   │   └── apis.md
│   └── templates/
│       ├── agent_template.py
│       └── project_structure/
│
└── .gitignore                 # Files to ignore in git
```

## 📝 File Naming Conventions

### Modules
- Format: `XX-module-name/`
- Example: `06-intro-to-agents/`
- Use lowercase, hyphens for spaces

### Example Code
- Numbered for order: `01_topic.py`
- Descriptive names: `calculator_agent.py`
- Python files: `.py` extension

### Documentation
- Main content: `README.md`
- Additional: `topic-name.md`
- All lowercase with hyphens

## 🎯 Module Structure

Each module follows this template:

```
XX-module-name/
├── README.md              # Main content
│   ├── Module Goals
│   ├── Theory/Concepts
│   ├── Examples (inline)
│   ├── Labs
│   ├── Completion Checklist
│   └── Resources
│
├── examples/              # Working code examples
│   ├── 01_basic.py       # Simple example
│   ├── 02_intermediate.py
│   └── 03_advanced.py
│
├── lab/                   # Hands-on exercises
│   ├── instructions.md   # Lab description
│   ├── starter.py        # Starting point
│   └── solution.py       # Complete solution
│
└── resources.md          # Links to videos, articles, docs
```

## 🔑 Key Files Explained

### Root Level

**README.md**
- Course overview
- What you'll learn
- How to use the course
- Getting started

**QUICK_START.md**
- 10-minute setup guide
- First steps
- Quick wins
- Motivation

**COURSE_OUTLINE.md**
- All modules detailed
- Time estimates
- Prerequisites
- Learning paths

**RESOURCES.md**
- Curated videos
- Courses
- Books
- Communities
- Tools

**CONTRIBUTING.md**
- How to contribute
- Style guide
- Pull request process

**LICENSE**
- MIT License
- Free to use and modify

### Guides

**github-setup.md**
- Git basics
- GitHub workflow
- Repository management
- Common issues

### Modules

**README.md** (in each module)
- Learning objectives
- Theory and concepts
- Code examples
- Labs/exercises
- Resources
- Next steps

**examples/** (in each module)
- Fully working code
- Well-commented
- Progressively harder
- Can be run independently

**lab/** (in each module)
- Hands-on practice
- Starter code provided
- Solutions available
- Real-world applications

## 📦 What's Included vs TODO

### ✅ Completed

- [x] Main README
- [x] Quick Start Guide
- [x] Complete Course Outline
- [x] Learning Resources
- [x] GitHub Setup Guide
- [x] Module 0: Environment Setup
- [x] Module 1: Python Basics
- [x] Module 4: API Basics
- [x] Module 6: Intro to Agents
- [x] Contributing Guide
- [x] License
- [x] Project Structure

### 📝 To Be Added

- [ ] Module 2: Understanding AI
- [ ] Module 3: Prompt Engineering
- [ ] Module 5: First AI App
- [ ] Module 7: Simple Agents
- [ ] Module 8: Agentic Workflows
- [ ] Module 9: Tool Use
- [ ] Module 10: Advanced Agents
- [ ] Module 11: RAG & Memory
- [ ] Module 12: Deployment
- [ ] Module 13: Capstone Projects
- [ ] Example projects
- [ ] Cheat sheets
- [ ] Code templates

## 🚀 How to Use This Repository

### As a Learner

1. **Start with QUICK_START.md**
2. **Follow modules sequentially**
3. **Do all labs**
4. **Build projects**
5. **Share your work**

### As a Contributor

1. **Fork the repository**
2. **Pick a TODO item**
3. **Follow CONTRIBUTING.md**
4. **Submit pull request**

### As a Teacher

1. **Fork for your class**
2. **Customize as needed**
3. **Add your own examples**
4. **Share improvements back**

## 📊 Progress Tracking

Create a `PROGRESS.md` in your fork:

```markdown
# My Progress

## Completed
- [x] Module 0: Setup
- [x] Module 1: Python Basics

## In Progress
- [ ] Module 2: Understanding AI (50%)

## To Do
- [ ] Module 3: Prompt Engineering
- ...

## Projects
- [x] Prompt Manager
- [ ] Chat CLI (started)
```

## 🎓 Certificate-Ready

When you've completed:
- All modules (0-13)
- All labs
- 1 capstone project
- Contributed 1 PR

You can:
- Add to your resume
- Share on LinkedIn
- Include in portfolio
- Help others learn!

## 📁 Your Projects Folder

Create projects as you learn:

```
projects/
├── week-2-prompt-manager/
├── week-6-chat-app/
├── week-10-agent/
└── capstone-research-agent/
```

Each project should have:
- README.md (what it does)
- Source code
- Requirements.txt
- Example usage
- Screenshots/demo

## 🔄 Keeping Updated

Stay current with:

```bash
# Add upstream remote
git remote add upstream https://github.com/original/ai-coding-course.git

# Fetch updates
git fetch upstream

# Merge updates
git merge upstream/main
```

## 🎯 Next Steps

1. **Read QUICK_START.md** - Get started fast
2. **Read COURSE_OUTLINE.md** - See the full journey  
3. **Start Module 0** - Begin learning!

---

**Questions about the structure?** Open an issue or discussion!

**Want to improve it?** Read CONTRIBUTING.md and submit a PR!

---

**Happy Learning!** 🚀
