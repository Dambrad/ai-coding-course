# Module 1: Python Crash Course

> Learn just enough Python to build AI applications — no fluff, just what you need.

## 🎯 Module Goals

By the end of this module, you'll understand:
- Variables, data types, and basic operations
- Lists, dictionaries, and data structures
- Functions and how to write reusable code
- Loops and conditional logic
- Reading and writing files
- Installing and using Python packages

**Philosophy**: We're not becoming Python experts. We're learning enough to build AI apps. You'll learn more advanced Python as you need it.

## ⏱️ Estimated Time: 5-7 hours (spread over 1-2 weeks)

## 📚 Table of Contents

1. [Variables & Data Types](#1-variables--data-types)
2. [Strings & Text Manipulation](#2-strings--text-manipulation)
3. [Lists & Dictionaries](#3-lists--dictionaries)
4. [Functions](#4-functions)
5. [Conditionals & Loops](#5-conditionals--loops)
6. [Files & Packages](#6-files--packages)
7. [Error Handling](#7-error-handling)

---

## 1. Variables & Data Types

### What is a Variable?

A variable is a name that stores a value. Think of it as a labeled box.

```python
# Creating variables
name = "Claude"           # String (text)
age = 2                   # Integer (whole number)
temperature = 98.6        # Float (decimal number)
is_ai = True             # Boolean (True/False)

# Using variables
print(name)              # Output: Claude
print(age + 5)          # Output: 7
```

### Key Data Types

```python
# String - text data
greeting = "Hello, World!"
api_key = "sk-1234567890"

# Integer - whole numbers
count = 42
year = 2026

# Float - decimal numbers
price = 19.99
temperature = -5.5

# Boolean - True or False
is_ready = True
has_api_key = False

# None - represents "nothing" or "empty"
result = None
```

### Type Checking

```python
# Check what type something is
print(type(name))        # <class 'str'>
print(type(age))         # <class 'int'>
print(type(temperature)) # <class 'float'>

# Convert between types
number_as_string = "42"
number_as_int = int(number_as_string)  # Convert to integer
print(number_as_int + 8)  # Output: 50
```

### 🧪 Mini Lab: Variables

Create `01_variables.py`:

```python
# Your personal info (use fake data if you prefer)
first_name = "Your Name"
favorite_number = 7
hours_per_week = 3.5
ready_to_learn = True

# Print a message
print(f"Hi, I'm {first_name}!")
print(f"My favorite number is {favorite_number}")
print(f"I can dedicate {hours_per_week} hours per week")
print(f"Am I ready? {ready_to_learn}")

# Do some math
weeks_in_course = 16
total_hours = hours_per_week * weeks_in_course
print(f"\nI'll spend {total_hours} hours total on this course!")
```

---

## 2. Strings & Text Manipulation

Strings are critical for AI work - prompts, responses, and data are all text.

### Creating Strings

```python
# Different ways to create strings
single_quotes = 'Hello'
double_quotes = "World"
multi_line = """This is a
multi-line
string"""

# String concatenation
full_name = first_name + " " + last_name

# F-strings (modern, preferred way)
greeting = f"Hello, {first_name}! You are {age} years old."
```

### Common String Operations

```python
text = "  AI is Amazing  "

# Length
print(len(text))              # 16

# Remove whitespace
print(text.strip())           # "AI is Amazing"

# Change case
print(text.upper())           # "  AI IS AMAZING  "
print(text.lower())           # "  ai is amazing  "

# Replace text
print(text.replace("AI", "Artificial Intelligence"))

# Split into list
words = text.split()          # ["AI", "is", "Amazing"]

# Check if text contains something
print("AI" in text)           # True
print("Python" in text)       # False
```

### String Slicing

```python
message = "Hello, AI World!"

# Get specific characters
print(message[0])       # "H" (first character)
print(message[-1])      # "!" (last character)
print(message[0:5])     # "Hello" (characters 0-4)
print(message[7:])      # "AI World!" (from 7 to end)
```

### 🧪 Mini Lab: Strings

Create `02_strings.py`:

```python
# AI prompt manipulation
prompt = "   Explain machine learning in simple terms   "

# Clean and prepare the prompt
clean_prompt = prompt.strip()
print(f"Original length: {len(prompt)}")
print(f"Clean length: {len(clean_prompt)}")

# Create a formatted prompt
formatted_prompt = f"[SYSTEM]: You are a helpful AI assistant.\n[USER]: {clean_prompt}"
print("\n" + formatted_prompt)

# Extract keywords
words = clean_prompt.lower().split()
print(f"\nKeywords found: {words}")
```

---

## 3. Lists & Dictionaries

Lists and dictionaries are how you organize data in Python.

### Lists - Ordered Collections

```python
# Create a list
ai_models = ["GPT-4", "Claude", "Gemini", "Llama"]

# Access items (0-indexed)
print(ai_models[0])     # "GPT-4"
print(ai_models[-1])    # "Llama" (last item)

# Add items
ai_models.append("Mistral")
ai_models.insert(0, "GPT-3.5")  # Insert at position 0

# Remove items
ai_models.remove("GPT-3.5")
last_model = ai_models.pop()    # Remove and return last item

# Useful list operations
print(len(ai_models))           # Length
print("Claude" in ai_models)    # Check if exists
sorted_models = sorted(ai_models)  # Sort alphabetically
```

### List Slicing

```python
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

print(numbers[0:5])    # [0, 1, 2, 3, 4]
print(numbers[5:])     # [5, 6, 7, 8, 9]
print(numbers[::2])    # [0, 2, 4, 6, 8] (every 2nd item)
print(numbers[::-1])   # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0] (reversed)
```

### Dictionaries - Key-Value Pairs

Dictionaries are like real dictionaries: you look up a key to get a value.

```python
# Create a dictionary
ai_info = {
    "name": "Claude",
    "company": "Anthropic",
    "version": "Sonnet 4.5",
    "year": 2024
}

# Access values
print(ai_info["name"])          # "Claude"
print(ai_info.get("company"))   # "Anthropic" (safer)

# Add or update
ai_info["capabilities"] = ["coding", "analysis", "writing"]
ai_info["year"] = 2025  # Update existing

# Remove
del ai_info["version"]

# Check if key exists
if "name" in ai_info:
    print(f"Model name: {ai_info['name']}")

# Get all keys and values
print(ai_info.keys())    # dict_keys(['name', 'company', ...])
print(ai_info.values())  # dict_values(['Claude', 'Anthropic', ...])
```

### Nested Structures

```python
# Dictionary of lists
api_config = {
    "anthropic": {
        "models": ["claude-3-opus", "claude-3-sonnet"],
        "api_key": "sk-xxx",
        "max_tokens": 4096
    },
    "openai": {
        "models": ["gpt-4", "gpt-3.5-turbo"],
        "api_key": "sk-yyy",
        "max_tokens": 8192
    }
}

# Access nested data
claude_models = api_config["anthropic"]["models"]
print(claude_models[0])  # "claude-3-opus"
```

### 🧪 Mini Lab: Data Structures

Create `03_data_structures.py`:

```python
# Build a simple AI agent configuration
agent_config = {
    "name": "ResearchAgent",
    "model": "claude-sonnet-4.5",
    "temperature": 0.7,
    "tools": ["web_search", "calculator", "code_executor"],
    "system_prompt": "You are a helpful research assistant."
}

# Add a new capability
agent_config["tools"].append("file_reader")

# Create a list of agents
agents = [
    agent_config,
    {
        "name": "WritingAgent",
        "model": "gpt-4",
        "temperature": 0.9,
        "tools": ["grammar_check", "style_guide"],
        "system_prompt": "You are a creative writing assistant."
    }
]

# Print all agent names
for agent in agents:
    print(f"Agent: {agent['name']} using {agent['model']}")
```

---

## 4. Functions

Functions are reusable blocks of code. They're essential for organizing AI applications.

### Basic Functions

```python
# Define a function
def greet(name):
    return f"Hello, {name}!"

# Call the function
message = greet("Alice")
print(message)  # "Hello, Alice!"

# Function with multiple parameters
def create_prompt(role, task, context=""):
    prompt = f"You are a {role}. {task}"
    if context:
        prompt += f"\n\nContext: {context}"
    return prompt

# Use it
prompt = create_prompt("teacher", "Explain Python functions", "For beginners")
print(prompt)
```

### Default Arguments

```python
def call_ai(prompt, model="claude-sonnet-4.5", temperature=0.7):
    return f"Calling {model} with temp={temperature}: {prompt}"

# Different ways to call
result1 = call_ai("Hello!")                    # Uses defaults
result2 = call_ai("Hello!", model="gpt-4")    # Override model
result3 = call_ai("Hello!", temperature=0.9)  # Override temperature
```

### Return Multiple Values

```python
def analyze_text(text):
    word_count = len(text.split())
    char_count = len(text)
    return word_count, char_count

# Unpack multiple return values
words, chars = analyze_text("Hello AI World")
print(f"Words: {words}, Characters: {chars}")
```

### 🧪 Mini Lab: Functions

Create `04_functions.py`:

```python
def format_prompt(user_message, system_message="You are a helpful AI assistant.", temperature=0.7):
    """
    Format a prompt for an AI API call.
    
    Args:
        user_message: The user's input
        system_message: System instructions for the AI
        temperature: Creativity level (0-1)
    
    Returns:
        Formatted prompt dictionary
    """
    return {
        "system": system_message,
        "user": user_message,
        "temperature": temperature
    }

# Test the function
prompt1 = format_prompt("What is machine learning?")
prompt2 = format_prompt(
    "Write a haiku about AI",
    system_message="You are a creative poet.",
    temperature=0.9
)

print("Prompt 1:", prompt1)
print("Prompt 2:", prompt2)
```

---

## 5. Conditionals & Loops

Control flow lets your code make decisions and repeat actions.

### If/Elif/Else

```python
temperature = 0.8

if temperature < 0.3:
    creativity = "low"
elif temperature < 0.7:
    creativity = "medium"
else:
    creativity = "high"

print(f"Creativity level: {creativity}")

# One-liner (ternary)
status = "ready" if has_api_key else "not ready"
```

### For Loops

```python
# Loop through a list
models = ["gpt-4", "claude-sonnet-4.5", "gemini-pro"]

for model in models:
    print(f"Testing {model}...")

# Loop with index
for i, model in enumerate(models):
    print(f"{i+1}. {model}")

# Loop through dictionary
api_config = {"model": "claude", "temp": 0.7}
for key, value in api_config.items():
    print(f"{key}: {value}")

# Range loops
for i in range(5):
    print(f"Iteration {i}")  # 0, 1, 2, 3, 4
```

### While Loops

```python
# Loop until condition is false
attempts = 0
max_attempts = 3

while attempts < max_attempts:
    print(f"Attempt {attempts + 1}")
    attempts += 1
    
print("Done!")
```

### List Comprehensions

A powerful Python feature for creating lists in one line:

```python
# Traditional way
numbers = [1, 2, 3, 4, 5]
squared = []
for n in numbers:
    squared.append(n ** 2)

# List comprehension way (preferred)
squared = [n ** 2 for n in numbers]

# With condition
even_squared = [n ** 2 for n in numbers if n % 2 == 0]

# Real AI example
prompts = ["Explain AI", "Write code", "Summarize text"]
formatted = [f"[USER]: {p}" for p in prompts]
```

### 🧪 Mini Lab: Control Flow

Create `05_control_flow.py`:

```python
# Simulate processing multiple AI requests
requests = [
    {"prompt": "Hello", "priority": "low"},
    {"prompt": "Emergency help!", "priority": "high"},
    {"prompt": "Explain Python", "priority": "medium"},
    {"prompt": "URGENT: System down", "priority": "high"},
]

# Process high priority first
print("Processing HIGH priority requests:")
for req in requests:
    if req["priority"] == "high":
        print(f"  -> {req['prompt']}")

print("\nProcessing MEDIUM priority requests:")
for req in requests:
    if req["priority"] == "medium":
        print(f"  -> {req['prompt']}")

# Count by priority
high_count = len([r for r in requests if r["priority"] == "high"])
print(f"\nTotal high priority: {high_count}")
```

---

## 6. Files & Packages

### Reading Files

```python
# Read entire file
with open("prompt.txt", "r") as file:
    content = file.read()
    print(content)

# Read line by line
with open("prompts.txt", "r") as file:
    for line in file:
        print(line.strip())
```

### Writing Files

```python
# Write to file
with open("output.txt", "w") as file:
    file.write("Hello from Python!\n")
    file.write("This is line 2\n")

# Append to file
with open("output.txt", "a") as file:
    file.write("This is appended\n")
```

### Working with JSON

JSON is the standard format for API communication:

```python
import json

# Python dict to JSON
data = {
    "model": "claude-sonnet-4.5",
    "messages": [{"role": "user", "content": "Hello!"}]
}

# Convert to JSON string
json_string = json.dumps(data, indent=2)
print(json_string)

# Write JSON to file
with open("config.json", "w") as file:
    json.dump(data, file, indent=2)

# Read JSON from file
with open("config.json", "r") as file:
    loaded_data = json.load(file)
    print(loaded_data["model"])
```

### Installing Packages

```python
# In terminal:
# pip3 install requests
# pip3 install anthropic

# In your Python code:
import requests
from anthropic import Anthropic

# Now you can use these libraries!
```

### 🧪 Mini Lab: Files

Create `06_files.py`:

```python
import json

# Create a configuration file
config = {
    "api_provider": "anthropic",
    "model": "claude-sonnet-4.5",
    "settings": {
        "temperature": 0.7,
        "max_tokens": 1024
    },
    "prompts": [
        "Explain this code",
        "Find bugs",
        "Suggest improvements"
    ]
}

# Save to JSON file
with open("ai_config.json", "w") as file:
    json.dump(config, file, indent=2)
    print("✓ Configuration saved!")

# Read it back
with open("ai_config.json", "r") as file:
    loaded_config = json.load(file)
    print(f"✓ Loaded config for {loaded_config['model']}")
    print(f"  Temperature: {loaded_config['settings']['temperature']}")
```

---

## 7. Error Handling

Errors happen. Handle them gracefully.

### Try/Except

```python
# Basic error handling
try:
    number = int("not a number")
except ValueError:
    print("That's not a valid number!")

# Handle multiple error types
try:
    result = risky_operation()
except FileNotFoundError:
    print("File not found!")
except PermissionError:
    print("Permission denied!")
except Exception as e:
    print(f"Unexpected error: {e}")

# Finally block (always runs)
try:
    file = open("data.txt")
    data = file.read()
except FileNotFoundError:
    print("File doesn't exist")
finally:
    print("This always runs")
```

### Practical AI Example

```python
def call_ai_safe(prompt, api_key=None):
    """Safely call AI API with error handling"""
    try:
        if not api_key:
            raise ValueError("API key is required")
        
        # Simulate API call
        if len(prompt) == 0:
            raise ValueError("Prompt cannot be empty")
        
        return f"Response to: {prompt}"
    
    except ValueError as e:
        print(f"❌ Validation error: {e}")
        return None
    except Exception as e:
        print(f"❌ Unexpected error: {e}")
        return None

# Test it
result1 = call_ai_safe("Hello")      # Works
result2 = call_ai_safe("")           # Validation error
result3 = call_ai_safe("Hi", "key")  # Works
```

---

## 🎯 Main Lab: Build a Simple AI Prompt Manager

Create `ai_prompt_manager.py`:

```python
import json
from datetime import datetime

class PromptManager:
    """Manage and organize AI prompts"""
    
    def __init__(self, filename="prompts.json"):
        self.filename = filename
        self.prompts = self.load_prompts()
    
    def load_prompts(self):
        """Load prompts from file"""
        try:
            with open(self.filename, "r") as file:
                return json.load(file)
        except FileNotFoundError:
            return []
    
    def save_prompts(self):
        """Save prompts to file"""
        with open(self.filename, "w") as file:
            json.dump(self.prompts, file, indent=2)
    
    def add_prompt(self, category, text, tags=None):
        """Add a new prompt"""
        prompt = {
            "id": len(self.prompts) + 1,
            "category": category,
            "text": text,
            "tags": tags or [],
            "created": datetime.now().isoformat()
        }
        self.prompts.append(prompt)
        self.save_prompts()
        print(f"✓ Added prompt #{prompt['id']}")
    
    def search_prompts(self, keyword):
        """Search prompts by keyword"""
        results = []
        for prompt in self.prompts:
            if keyword.lower() in prompt["text"].lower():
                results.append(prompt)
        return results
    
    def list_by_category(self, category):
        """List prompts in a category"""
        return [p for p in self.prompts if p["category"] == category]

# Example usage
if __name__ == "__main__":
    manager = PromptManager()
    
    # Add some prompts
    manager.add_prompt(
        "coding",
        "Explain this Python code and suggest improvements",
        ["python", "code-review"]
    )
    
    manager.add_prompt(
        "writing",
        "Write a professional email based on these bullet points",
        ["email", "professional"]
    )
    
    # Search
    results = manager.search_prompts("python")
    print(f"\nFound {len(results)} prompts about Python:")
    for prompt in results:
        print(f"  - {prompt['text']}")
```

**Run it**:
```bash
python3 ai_prompt_manager.py
```

---

## ✅ Module Completion Checklist

Before moving to Module 2, ensure you can:
- [ ] Create and manipulate variables
- [ ] Work with strings, lists, and dictionaries
- [ ] Write and call functions
- [ ] Use if/else and loops
- [ ] Read and write files
- [ ] Handle errors with try/except
- [ ] Successfully run the AI Prompt Manager lab

## 🎯 Next Steps

You now know enough Python to build AI applications! 

Move to **[Module 2: Understanding AI Models](../02-understanding-ai/README.md)**

## 📚 Additional Resources

### Videos
- [Python in 100 Seconds](https://www.youtube.com/watch?v=x7X9w_GIm1s) - Quick overview
- [Python for Beginners - Full Course](https://www.youtube.com/watch?v=rfscVS0vtbw) - 4.5 hours
- [Automate the Boring Stuff with Python](https://www.youtube.com/playlist?list=PL0-84-yl1fUnRuXGFe_F7qSH1LEnn9LkW)

### Interactive Learning
- [Python Tutor](https://pythontutor.com/) - Visualize code execution
- [Exercism Python Track](https://exercism.org/tracks/python) - Practice exercises

### Documentation
- [Official Python Tutorial](https://docs.python.org/3/tutorial/)
- [Real Python Tutorials](https://realpython.com/)

---

**Estimated Time**: 5-7 hours  
**Difficulty**: ⭐⭐☆☆☆ (Easy-Medium)  
**Prerequisites**: Module 0

**Next**: [Module 2: Understanding AI Models →](../02-understanding-ai/README.md)
