# Module 3: Introduction to AI APIs

> **Goal**: Make your first API calls to Claude and understand how to communicate with AI models.

**Time**: 3-4 hours  
**Difficulty**: Beginner  
**Prerequisites**: Modules 0-2 (Python basics + Files/APIs)

---

## 🎯 What You'll Learn

- What LLMs (Large Language Models) are and how they work
- How to get API keys from Anthropic and OpenAI
- Making your first AI API call
- Prompt engineering basics
- Streaming responses
- Handling conversation history
- Cost management and best practices

---

## 📝 Part 1: Understanding AI APIs

### What is an LLM?

An LLM (Large Language Model) is an AI that:
- Understands and generates human-like text
- Has been trained on vast amounts of text data
- Can answer questions, write code, analyze data, and more

**Popular LLMs**:
- **Claude** (by Anthropic) - What you're talking to right now!
- **GPT-4** (by OpenAI) - ChatGPT's underlying model
- **Gemini** (by Google)
- **Llama** (by Meta)

### What is an API?

**API** = Application Programming Interface

Think of it as a menu at a restaurant:
- You (your code) are the customer
- The API is the menu of available services
- The kitchen (AI model) prepares your request
- The waiter (API) brings back the response

### Claude vs GPT-4: When to Use Which?

**Claude** (Anthropic):
- ✅ Longer context windows (remember more)
- ✅ Better at following instructions precisely
- ✅ More thoughtful and nuanced responses
- ✅ Excellent for analysis and reasoning
- 💰 Generally more cost-effective

**GPT-4** (OpenAI):
- ✅ Broader general knowledge
- ✅ Better for creative writing
- ✅ More tools (DALL-E, Whisper integration)
- ✅ Larger ecosystem and community
- 💰 More expensive

**For this course**: We'll start with Claude (free tier available), then add OpenAI later.

---

## 🔑 Part 2: Getting Your API Keys

### Anthropic (Claude) API Key

**Step 1: Create Account**
1. Go to https://console.anthropic.com
2. Sign up with your email
3. Verify your email

**Step 2: Get API Key**
1. Go to API Keys section
2. Click "Create Key"
3. Name it (e.g., "Learning Project")
4. Copy the key (starts with `sk-ant-`)
5. **Save it immediately** - you won't see it again!

**Step 3: Add Credits** (Optional for now)
- New accounts get $5 free credits
- After that, it's pay-as-you-go
- Typical costs: $0.01-0.10 per conversation

**Step 4: Secure Your Key**

**NEVER** put API keys directly in code! Use environment variables:

```bash
# On Mac, add to ~/.zshrc
echo 'export ANTHROPIC_API_KEY="your-key-here"' >> ~/.zshrc
source ~/.zshrc

# Verify it works
echo $ANTHROPIC_API_KEY
```

### OpenAI API Key (For Later)

We'll set this up in Module 6, but here's how:
1. Go to https://platform.openai.com
2. Sign up and verify
3. Add payment method ($5 minimum)
4. Create API key
5. Add to environment:

```bash
echo 'export OPENAI_API_KEY="your-key-here"' >> ~/.zshrc
source ~/.zshrc
```

---

## 💻 Part 3: Your First AI API Call

### Install the Anthropic SDK

```bash
pip install anthropic --break-system-packages
```

### Hello, Claude!

Create `hello_claude.py`:

```python
import anthropic
import os

# Initialize the client
client = anthropic.Anthropic(
    api_key=os.environ.get("ANTHROPIC_API_KEY")
)

# Make your first API call
message = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=1024,
    messages=[
        {"role": "user", "content": "Hello! Who are you?"}
    ]
)

# Print the response
print(message.content[0].text)
```

**Run it**:
```bash
python3 hello_claude.py
```

You should see Claude introduce itself!

---

### Understanding the API Call

Let's break down what just happened:

```python
message = client.messages.create(
    model="claude-sonnet-4-20250514",  # Which model to use
    max_tokens=1024,                    # Maximum response length
    messages=[                          # Your conversation
        {"role": "user", "content": "Hello!"}
    ]
)
```

**Parameters explained**:
- `model`: Which version of Claude (Sonnet is balanced, Opus is most capable)
- `max_tokens`: Rough limit on response length (1 token ≈ 0.75 words)
- `messages`: List of conversation messages
- `role`: Either "user" (you) or "assistant" (Claude)
- `content`: The actual text

---

### 🔬 Lab 3.1: Interactive Chat

Create `chat.py`:

```python
import anthropic
import os

client = anthropic.Anthropic(api_key=os.environ.get("ANTHROPIC_API_KEY"))

def ask_claude(question):
    """Send a question to Claude and get a response"""
    message = client.messages.create(
        model="claude-sonnet-4-20250514",
        max_tokens=1024,
        messages=[
            {"role": "user", "content": question}
        ]
    )
    return message.content[0].text

# Interactive loop
print("=== Chat with Claude ===")
print("(Type 'quit' to exit)\n")

while True:
    user_input = input("You: ")
    
    if user_input.lower() == 'quit':
        print("Goodbye!")
        break
    
    response = ask_claude(user_input)
    print(f"\nClaude: {response}\n")
```

**Try asking**:
- "Explain quantum computing in simple terms"
- "Write a haiku about Python"
- "What's the capital of France?"

---

## 💻 Part 4: Conversation History

The previous example doesn't remember context. Let's fix that!

### Chat with Memory

Create `chat_with_memory.py`:

```python
import anthropic
import os

client = anthropic.Anthropic(api_key=os.environ.get("ANTHROPIC_API_KEY"))

# Store conversation history
conversation = []

def chat(user_message):
    """Send message and maintain conversation history"""
    # Add user message to history
    conversation.append({
        "role": "user",
        "content": user_message
    })
    
    # Get Claude's response
    message = client.messages.create(
        model="claude-sonnet-4-20250514",
        max_tokens=1024,
        messages=conversation
    )
    
    # Add Claude's response to history
    assistant_message = message.content[0].text
    conversation.append({
        "role": "assistant",
        "content": assistant_message
    })
    
    return assistant_message

# Test the memory
print("=== Chat with Memory ===\n")

print("You: My name is Alice")
response = chat("My name is Alice")
print(f"Claude: {response}\n")

print("You: What's my name?")
response = chat("What's my name?")
print(f"Claude: {response}\n")

print("You: Tell me a joke about my name")
response = chat("Tell me a joke about my name")
print(f"Claude: {response}\n")
```

**Notice**: Claude remembers your name across messages!

---

## 💻 Part 5: Prompt Engineering Basics

The way you phrase your request (the "prompt") dramatically affects the response.

### Principle 1: Be Specific

**Bad**:
```python
response = chat("Write about dogs")
```

**Good**:
```python
response = chat("""Write a 100-word paragraph about Golden Retrievers, 
focusing on why they make great family pets. Use simple language 
suitable for children.""")
```

### Principle 2: Provide Context

**Bad**:
```python
response = chat("Explain this code")
```

**Good**:
```python
code = '''
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
'''

response = chat(f"""I'm a beginner learning Python. 
Can you explain this code in simple terms?

{code}

Please explain:
1. What it does
2. How it works step-by-step
3. What 'recursion' means""")
```

### Principle 3: Use System Prompts

System prompts set Claude's behavior for the entire conversation:

```python
message = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=1024,
    system="You are a helpful Python tutor. Always explain concepts simply with examples.",
    messages=[
        {"role": "user", "content": "What is a function?"}
    ]
)
```

### Principle 4: Few-Shot Learning

Show examples of what you want:

```python
prompt = """Convert these sentences to pirate speak:

Example 1:
Input: "Hello, how are you?"
Output: "Ahoy, how be ye?"

Example 2:
Input: "I'm going to the store"
Output: "I be sailin' to the market"

Now convert this:
Input: "I love learning to code"
Output:"""

response = chat(prompt)
```

---

### 🔬 Lab 3.2: Prompt Engineering Practice

Create `prompt_practice.py`:

```python
import anthropic
import os

client = anthropic.Anthropic(api_key=os.environ.get("ANTHROPIC_API_KEY"))

def ask(prompt, system=None):
    """Helper function for quick prompts"""
    kwargs = {
        "model": "claude-sonnet-4-20250514",
        "max_tokens=1024,
        "messages": [{"role": "user", "content": prompt}]
    }
    if system:
        kwargs["system"] = system
    
    message = client.messages.create(**kwargs)
    return message.content[0].text

# Experiment 1: Different tones
print("=== EXPERIMENT 1: System Prompts ===\n")

response1 = ask(
    "Explain what Python is",
    system="You are a formal computer science professor"
)
print("Professor:", response1[:200], "...\n")

response2 = ask(
    "Explain what Python is",
    system="You are a friendly coding buddy who uses lots of emojis"
)
print("Buddy:", response2[:200], "...\n")

# Experiment 2: Output format
print("=== EXPERIMENT 2: Structured Output ===\n")

response = ask("""List 3 benefits of learning Python.
Format your response as JSON:
{
  "benefits": [
    {"title": "...", "description": "..."},
    ...
  ]
}""")
print(response)
```

---

## 💻 Part 6: Streaming Responses

For long responses, streaming shows text as it's generated (like ChatGPT):

```python
import anthropic
import os

client = anthropic.Anthropic(api_key=os.environ.get("ANTHROPIC_API_KEY"))

def stream_response(prompt):
    """Stream Claude's response in real-time"""
    with client.messages.stream(
        model="claude-sonnet-4-20250514",
        max_tokens=1024,
        messages=[{"role": "user", "content": prompt}]
    ) as stream:
        for text in stream.text_stream:
            print(text, end="", flush=True)
    print()  # New line at end

# Try it
print("Claude: ", end="")
stream_response("Write a short story about a robot learning to cook")
```

---

## 💻 Part 7: Error Handling

Things can go wrong (network issues, invalid keys, rate limits). Always handle errors:

```python
import anthropic
import os
from anthropic import APIError, RateLimitError

client = anthropic.Anthropic(api_key=os.environ.get("ANTHROPIC_API_KEY"))

def safe_ask(question, max_retries=3):
    """Ask Claude with error handling and retries"""
    for attempt in range(max_retries):
        try:
            message = client.messages.create(
                model="claude-sonnet-4-20250514",
                max_tokens=1024,
                messages=[{"role": "user", "content": question}]
            )
            return message.content[0].text
        
        except RateLimitError:
            print("Rate limit hit, waiting 60 seconds...")
            import time
            time.sleep(60)
        
        except APIError as e:
            print(f"API Error: {e}")
            if attempt == max_retries - 1:
                return "Sorry, I couldn't get a response."
        
        except Exception as e:
            print(f"Unexpected error: {e}")
            return None

# Use it
response = safe_ask("Hello!")
if response:
    print(response)
```

---

## 🎯 Project: AI Study Buddy

Let's build a complete AI-powered study assistant!

Create `study_buddy.py`:

```python
import anthropic
import os
import json
from datetime import datetime

client = anthropic.Anthropic(api_key=os.environ.get("ANTHROPIC_API_KEY"))

class StudyBuddy:
    def __init__(self):
        self.conversation = []
        self.system_prompt = """You are a patient and encouraging study buddy. 
        Help students learn by:
        - Breaking down complex topics
        - Providing examples
        - Asking clarifying questions
        - Encouraging their progress
        Keep responses concise (2-3 paragraphs max)."""
    
    def chat(self, user_message):
        """Send message and get response"""
        self.conversation.append({
            "role": "user",
            "content": user_message
        })
        
        message = client.messages.create(
            model="claude-sonnet-4-20250514",
            max_tokens=500,
            system=self.system_prompt,
            messages=self.conversation
        )
        
        response = message.content[0].text
        self.conversation.append({
            "role": "assistant",
            "content": response
        })
        
        return response
    
    def save_session(self):
        """Save conversation to file"""
        filename = f"study_session_{datetime.now().strftime('%Y%m%d_%H%M%S')}.json"
        with open(filename, 'w') as f:
            json.dump(self.conversation, f, indent=2)
        print(f"\nSession saved to {filename}")

def main():
    buddy = StudyBuddy()
    
    print("=== AI Study Buddy ===")
    print("I'm here to help you learn! Ask me anything.")
    print("Commands: 'save' to save session, 'quit' to exit\n")
    
    while True:
        user_input = input("You: ").strip()
        
        if not user_input:
            continue
        
        if user_input.lower() == 'quit':
            buddy.save_session()
            print("Happy studying! 📚")
            break
        
        if user_input.lower() == 'save':
            buddy.save_session()
            continue
        
        response = buddy.chat(user_input)
        print(f"\nStudy Buddy: {response}\n")

if __name__ == "__main__":
    main()
```

**Try it out**:
```bash
python3 study_buddy.py
```

**Example conversation**:
```
You: Explain what APIs are
Study Buddy: [explains]

You: Can you give me an example?
Study Buddy: [gives example]

You: How would I use an API in Python?
Study Buddy: [shows code example]

You: save
Session saved to study_session_20250115_143022.json
```

---

## 💰 Part 8: Cost Management

### Understanding Pricing

**Claude Sonnet 4** (as of Jan 2025):
- Input: ~$3 per million tokens
- Output: ~$15 per million tokens

**What does this mean?**
- 1,000 tokens ≈ 750 words
- A typical conversation: ~500-2000 tokens
- Cost per conversation: $0.001 - $0.05

**Tips to save money**:
1. **Use appropriate models**: Haiku for simple tasks, Sonnet for most work, Opus only when needed
2. **Limit max_tokens**: Don't request 4000 tokens if 500 will do
3. **Cache repeated content**: System prompts can be cached
4. **Batch requests**: Combine multiple questions when possible

### Tracking Usage

```python
def chat_with_tracking(prompt):
    """Track token usage"""
    message = client.messages.create(
        model="claude-sonnet-4-20250514",
        max_tokens=1024,
        messages=[{"role": "user", "content": prompt}]
    )
    
    # Get usage stats
    usage = message.usage
    print(f"\nTokens - Input: {usage.input_tokens}, Output: {usage.output_tokens}")
    
    # Rough cost estimate (Sonnet pricing)
    cost = (usage.input_tokens * 0.000003 + 
            usage.output_tokens * 0.000015)
    print(f"Estimated cost: ${cost:.6f}")
    
    return message.content[0].text
```

---

## 📚 Best Practices

### 1. Security
```python
# ✅ DO: Use environment variables
api_key = os.environ.get("ANTHROPIC_API_KEY")

# ❌ DON'T: Hardcode keys
api_key = "sk-ant-1234..."  # NEVER DO THIS
```

### 2. Error Handling
Always wrap API calls in try/except blocks.

### 3. Rate Limiting
Don't spam the API - add delays between requests if making many:
```python
import time
time.sleep(1)  # Wait 1 second between calls
```

### 4. Conversation Management
For long conversations, consider:
- Summarizing earlier messages
- Keeping only recent messages
- Saving to files for later

### 5. Testing
Test with small token limits first, then increase:
```python
max_tokens=50  # Test with small responses
max_tokens=1024  # Use in production
```

---

## 🎓 Resources

### Documentation
- [Anthropic API Reference](https://docs.anthropic.com/claude/reference/getting-started-with-the-api)
- [Claude Prompt Engineering Guide](https://docs.anthropic.com/claude/docs/prompt-engineering)
- [Model Pricing](https://www.anthropic.com/pricing)

### Videos
- [Building with Claude API](https://www.youtube.com/watch?v=example) - Anthropic Official
- [Prompt Engineering Crash Course](https://www.youtube.com/watch?v=example)

### Tools
- [Anthropic Workbench](https://console.anthropic.com/workbench) - Test prompts in the browser
- [Token Counter](https://platform.openai.com/tokenizer) - Count tokens in text

---

## ✅ Module Checklist

Before moving to Module 4, make sure you can:
- [ ] Get and secure an API key
- [ ] Make basic API calls to Claude
- [ ] Maintain conversation history
- [ ] Write effective prompts
- [ ] Handle errors gracefully
- [ ] Understand token usage and costs
- [ ] Build a simple chatbot

---

## 🎯 Next Steps

Ready for **[Module 4: Building with Claude API](04-claude-api.md)**!

In Module 4, you'll learn:
- Advanced conversation management
- Function/tool calling
- Streaming for better UX
- Building real applications

---

## 💡 Challenge Projects

1. **Quiz Bot**: Create a bot that asks programming quiz questions
2. **Code Explainer**: Upload code and get explanations
3. **Language Tutor**: Practice conversations in another language
4. **Journal Buddy**: Daily journaling with AI feedback
5. **Debug Helper**: Paste errors and get solutions

---

**Great work!** 🎉 You can now communicate with AI via code. This is a superpower - use it wisely!
