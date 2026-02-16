# 🤝 Contributing to AI Coding Course

Thank you for your interest in improving this course! Whether you're fixing a typo, adding examples, or creating new content, your contributions are welcome.

## 📋 Ways to Contribute

1. **Fix Errors**: Typos, broken links, incorrect code
2. **Add Examples**: More working code examples
3. **Improve Explanations**: Clearer wording, better analogies
4. **Add Resources**: Helpful videos, articles, tools
5. **Create Labs**: New hands-on exercises
6. **Share Projects**: Your capstone projects
7. **Report Issues**: Found a bug or unclear section?

## 🚀 How to Contribute

### Quick Fixes (Typos, Links)

1. Click "Edit" on the file in GitHub
2. Make your changes
3. Click "Propose changes"
4. Submit pull request

### Larger Changes (New Content)

1. **Fork the repository**
   ```bash
   # On GitHub, click "Fork"
   ```

2. **Clone your fork**
   ```bash
   git clone https://github.com/YOUR_USERNAME/ai-coding-course.git
   cd ai-coding-course
   ```

3. **Create a branch**
   ```bash
   git checkout -b feature/your-improvement
   ```

4. **Make your changes**
   - Add code examples
   - Create new labs
   - Improve documentation

5. **Test your changes**
   - Run code examples
   - Check formatting
   - Test links

6. **Commit your changes**
   ```bash
   git add .
   git commit -m "Add: detailed description of your changes"
   ```

7. **Push to GitHub**
   ```bash
   git push origin feature/your-improvement
   ```

8. **Create Pull Request**
   - Go to your fork on GitHub
   - Click "Pull Request"
   - Describe your changes

## 📝 Contribution Guidelines

### Code Examples
- Must be tested and working
- Include comments explaining what it does
- Follow Python style guidelines (PEP 8)
- Keep it simple and beginner-friendly

### Documentation
- Use clear, simple language
- Avoid jargon (or explain it)
- Include examples
- Check spelling and grammar

### Labs & Exercises
- Clear instructions
- Expected output shown
- Difficulty level indicated
- Solution provided (in separate file)

## 🎨 Style Guide

### Code
```python
# Good: Clear, commented, simple
def greet(name):
    """Return a greeting message."""
    return f"Hello, {name}!"

# Avoid: Complex, uncommented
def g(n): return f"Hello, {n}!"
```

### Markdown
- Use headers appropriately (H1 for title, H2 for sections)
- Include code blocks with language specification
- Use emoji sparingly for visual interest
- Keep line length reasonable (~80-100 chars)

### File Structure
```
modules/XX-module-name/
├── README.md          # Main content
├── examples/          # Working code examples
│   ├── 01_basic.py
│   └── 02_advanced.py
├── lab/               # Hands-on exercises
│   ├── instructions.md
│   ├── starter_code.py
│   └── solution.py
└── resources.md       # Additional links
```

## 🐛 Reporting Issues

Found a problem? [Open an issue](https://github.com/yourusername/ai-coding-course/issues)

Include:
- **Module/File**: Where is the issue?
- **Description**: What's wrong?
- **Expected**: What should happen?
- **Actual**: What actually happens?
- **Screenshots**: If relevant

## 💡 Suggesting Features

Have an idea? [Open an issue](https://github.com/yourusername/ai-coding-course/issues) with:
- **Feature**: What do you want to add?
- **Motivation**: Why would this help learners?
- **Implementation**: How might it work?

## ✅ Checklist Before Submitting

- [ ] Code examples run without errors
- [ ] Documentation is clear and beginner-friendly
- [ ] No broken links
- [ ] Proper formatting (Markdown, code)
- [ ] Commit messages are descriptive
- [ ] Changes are focused (one improvement per PR)

## 🙏 Recognition

Contributors will be:
- Listed in the main README
- Credited in commits
- Mentioned in release notes (for major contributions)

## 📜 Code of Conduct

### Our Standards
- Be respectful and inclusive
- Welcome newcomers
- Provide constructive feedback
- Focus on learning and growth

### Not Acceptable
- Harassment or discrimination
- Disrespectful comments
- Spam or self-promotion
- Sharing others' code without credit

## 📬 Questions?

- **General**: Open a discussion
- **Issues**: Open an issue
- **Direct contact**: [your-email@example.com]

---

**Thank you for helping make this course better for everyone!** 🎉

Remember: No contribution is too small. Even fixing a single typo helps learners!
