# Module 4: Working with APIs

> Learn to communicate with AI models through APIs — the foundation of every AI application.

## 🎯 Module Goals

By the end of this module, you'll:
- Understand what APIs are and how they work
- Make your first API call to Claude and GPT-4
- Handle API responses and errors
- Build a reusable AI client
- Understand costs, rate limits, and best practices

## ⏱️ Estimated Time: 4-6 hours

## 📚 What is an API?

**API** = Application Programming Interface

Think of it as a restaurant:
- **You (your code)**: The customer
- **API**: The waiter
- **AI Model**: The kitchen

You tell the waiter what you want (send a request), they tell the kitchen (AI model), the kitchen prepares it (processes your prompt), and the waiter brings it back (returns the response).

### Real World Example

```
You: "Hey Claude, explain quantum computing in simple terms"
  ↓
Your Code: POST request to api.anthropic.com
  ↓
Claude API: Processes your request
  ↓
Your Code: Receives JSON response with answer
  ↓
You: See the explanation!
```

---

## 🔑 Getting Your API Keys

### Anthropic (Claude)

1. **Go to**: https://console.anthropic.com
2. **Sign up** or log in
3. **Click "API Keys"** in sidebar
4. **Create a new key**
5. **Copy and save it** (you won't see it again!)

**Free Tier**: $5 in free credits to start

### OpenAI (GPT-4)

1. **Go to**: https://platform.openai.com
2. **Sign up** or log in  
3. **Navigate to API Keys**
4. **Create new secret key**
5. **Copy and save it**

**Free Tier**: Limited, but enough to learn

### 🔒 Keeping API Keys Safe

**NEVER** put API keys directly in your code!

```python
# ❌ WRONG - Never do this!
api_key = "sk-ant-1234567890"

# ✅ CORRECT - Use environment variables
import os
api_key = os.getenv("ANTHROPIC_API_KEY")
```

**Setup**:
```bash
# Add to your ~/.zshrc or ~/.bash_profile
echo 'export ANTHROPIC_API_KEY="your-key-here"' >> ~/.zshrc
echo 'export OPENAI_API_KEY="your-key-here"' >> ~/.zshrc

# Reload
source ~/.zshrc
```

---

## 🚀 Your First API Call

### Install Required Packages

```bash
pip3 install anthropic openai python-dotenv requests
```

### Example 1: Claude API

Create `01_claude_basic.py`:

```python
import os
from anthropic import Anthropic

# Initialize client
client = Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))

# Make a request
response = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=1024,
    messages=[
        {"role": "user", "content": "Explain what an API is in one sentence."}
    ]
)

# Print response
print(response.content[0].text)
```

**Run it**:
```bash
python3 01_claude_basic.py
```

### Example 2: OpenAI API

Create `02_openai_basic.py`:

```python
import os
from openai import OpenAI

# Initialize client
client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

# Make a request
response = client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "user", "content": "Explain what an API is in one sentence."}
    ]
)

# Print response
print(response.choices[0].message.content)
```

### 🎉 You Just Called an AI API!

Let's break down what happened:

```python
response = client.messages.create(
    model="claude-sonnet-4-20250514",    # Which AI model to use
    max_tokens=1024,                      # Max response length
    messages=[                            # Your conversation
        {
            "role": "user",               # Who's speaking
            "content": "Your prompt"      # What you're asking
        }
    ]
)
```

---

## 🔧 Building a Reusable AI Client

Create `ai_client.py`:

```python
import os
from anthropic import Anthropic
from openai import OpenAI

class AIClient:
    """Unified client for multiple AI providers"""
    
    def __init__(self, provider="anthropic"):
        self.provider = provider
        
        if provider == "anthropic":
            self.client = Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))
            self.model = "claude-sonnet-4-20250514"
        elif provider == "openai":
            self.client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))
            self.model = "gpt-4"
        else:
            raise ValueError(f"Unknown provider: {provider}")
    
    def chat(self, prompt, system_message=None, temperature=0.7):
        """
        Send a chat message and get response
        
        Args:
            prompt: User's message
            system_message: Optional system instructions
            temperature: Creativity level (0-1)
        
        Returns:
            AI response as string
        """
        try:
            if self.provider == "anthropic":
                return self._chat_anthropic(prompt, system_message, temperature)
            elif self.provider == "openai":
                return self._chat_openai(prompt, system_message, temperature)
        except Exception as e:
            return f"Error: {str(e)}"
    
    def _chat_anthropic(self, prompt, system_message, temperature):
        """Call Anthropic API"""
        kwargs = {
            "model": self.model,
            "max_tokens": 1024,
            "temperature": temperature,
            "messages": [{"role": "user", "content": prompt}]
        }
        
        if system_message:
            kwargs["system"] = system_message
        
        response = self.client.messages.create(**kwargs)
        return response.content[0].text
    
    def _chat_openai(self, prompt, system_message, temperature):
        """Call OpenAI API"""
        messages = []
        
        if system_message:
            messages.append({"role": "system", "content": system_message})
        
        messages.append({"role": "user", "content": prompt})
        
        response = self.client.chat.completions.create(
            model=self.model,
            messages=messages,
            temperature=temperature
        )
        
        return response.choices[0].message.content


# Example usage
if __name__ == "__main__":
    # Test both providers
    claude = AIClient("anthropic")
    gpt = AIClient("openai")
    
    prompt = "What is 2+2? Answer in one word."
    
    print("Claude says:", claude.chat(prompt))
    print("GPT-4 says:", gpt.chat(prompt))
```

---

## 💬 Advanced: Streaming Responses

For long responses, stream them word-by-word (like ChatGPT):

```python
import os
from anthropic import Anthropic

client = Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))

# Stream the response
with client.messages.stream(
    model="claude-sonnet-4-20250514",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Write a short story about AI"}]
) as stream:
    for text in stream.text_stream:
        print(text, end="", flush=True)

print()  # New line at end
```

---

## 📊 Understanding API Responses

### Claude Response Structure

```python
{
    "id": "msg_123",
    "type": "message",
    "role": "assistant",
    "content": [
        {
            "type": "text",
            "text": "This is the actual response text"
        }
    ],
    "model": "claude-sonnet-4-20250514",
    "usage": {
        "input_tokens": 15,
        "output_tokens": 127
    }
}
```

### Extracting Information

```python
response = client.messages.create(...)

# Get the text
text = response.content[0].text

# Get token usage (for cost estimation)
input_tokens = response.usage.input_tokens
output_tokens = response.usage.output_tokens
total_tokens = input_tokens + output_tokens

print(f"Used {total_tokens} tokens")
```

---

## 💰 Understanding Costs

### Pricing (as of 2026)

**Claude Sonnet 4.5**:
- Input: $3 per million tokens (~750k words)
- Output: $15 per million tokens

**GPT-4**:
- Input: $30 per million tokens
- Output: $60 per million tokens

### Estimating Costs

```python
def estimate_cost(input_tokens, output_tokens, model="claude-sonnet"):
    """Estimate API call cost in USD"""
    
    prices = {
        "claude-sonnet": {"input": 3/1_000_000, "output": 15/1_000_000},
        "gpt-4": {"input": 30/1_000_000, "output": 60/1_000_000}
    }
    
    if model not in prices:
        return None
    
    input_cost = input_tokens * prices[model]["input"]
    output_cost = output_tokens * prices[model]["output"]
    
    return input_cost + output_cost

# Example
cost = estimate_cost(100, 500, "claude-sonnet")
print(f"Cost: ${cost:.6f}")  # Very cheap for small requests!
```

**Pro Tip**: For learning, you'll spend pennies. A typical learning month: $1-5.

---

## ⚡ Rate Limits & Best Practices

### Rate Limits

- **Claude**: 50 requests/min (free tier)
- **OpenAI**: 3 requests/min (free tier)

### Handling Rate Limits

```python
import time
from anthropic import RateLimitError

def chat_with_retry(client, prompt, max_retries=3):
    """Make API call with automatic retry on rate limit"""
    
    for attempt in range(max_retries):
        try:
            response = client.messages.create(
                model="claude-sonnet-4-20250514",
                max_tokens=1024,
                messages=[{"role": "user", "content": prompt}]
            )
            return response.content[0].text
        
        except RateLimitError:
            if attempt < max_retries - 1:
                wait_time = (attempt + 1) * 2  # 2, 4, 6 seconds
                print(f"Rate limited. Waiting {wait_time}s...")
                time.sleep(wait_time)
            else:
                return "Error: Rate limit exceeded"
        
        except Exception as e:
            return f"Error: {str(e)}"
```

### Best Practices

1. **Cache responses**: Don't re-ask the same question
2. **Batch requests**: Process multiple items in one call
3. **Use appropriate models**: Don't use GPT-4 for simple tasks
4. **Set max_tokens**: Prevent unexpectedly long (expensive) responses
5. **Monitor usage**: Track your API costs

---

## 🧪 Lab: Build an AI Chat CLI

Create `ai_chat_cli.py`:

```python
import os
from anthropic import Anthropic

class ChatCLI:
    """Simple command-line chat interface"""
    
    def __init__(self):
        self.client = Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))
        self.conversation = []
    
    def chat(self, user_message):
        """Send a message and get response"""
        # Add user message to conversation
        self.conversation.append({
            "role": "user",
            "content": user_message
        })
        
        # Call API
        response = self.client.messages.create(
            model="claude-sonnet-4-20250514",
            max_tokens=2048,
            messages=self.conversation
        )
        
        # Extract response
        assistant_message = response.content[0].text
        
        # Add to conversation history
        self.conversation.append({
            "role": "assistant",
            "content": assistant_message
        })
        
        return assistant_message
    
    def run(self):
        """Start the chat interface"""
        print("🤖 AI Chat CLI")
        print("Type 'quit' to exit\n")
        
        while True:
            # Get user input
            user_input = input("You: ").strip()
            
            if user_input.lower() in ['quit', 'exit', 'q']:
                print("Goodbye!")
                break
            
            if not user_input:
                continue
            
            # Get AI response
            response = self.chat(user_input)
            print(f"\nAI: {response}\n")

if __name__ == "__main__":
    chat = ChatCLI()
    chat.run()
```

**Run it**:
```bash
python3 ai_chat_cli.py
```

**Try these prompts**:
- "Explain APIs in simple terms"
- "What did we just talk about?" (tests conversation memory)
- "Write a haiku about coding"

---

## 🔍 Debugging API Calls

### Print Full Response

```python
import json

response = client.messages.create(...)

# Pretty print the entire response
print(json.dumps(response.model_dump(), indent=2))
```

### Common Errors

**1. Authentication Error**
```
Error: 401 Unauthorized
```
**Fix**: Check your API key is correct and set in environment

**2. Invalid Request**
```
Error: 400 Bad Request
```
**Fix**: Check your request format (messages structure, model name)

**3. Rate Limit**
```
Error: 429 Too Many Requests
```
**Fix**: Wait a moment, then retry (or implement retry logic)

**4. Token Limit**
```
Error: Maximum context length exceeded
```
**Fix**: Reduce your prompt or max_tokens

---

## 🎯 Main Lab: AI Writing Assistant

Create `writing_assistant.py`:

```python
import os
from anthropic import Anthropic

class WritingAssistant:
    """AI-powered writing helper"""
    
    def __init__(self):
        self.client = Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))
    
    def improve_text(self, text):
        """Improve grammar and clarity"""
        prompt = f"""Please improve this text for grammar, clarity, and professionalism:

{text}

Return only the improved version, no explanation."""
        
        return self._call_api(prompt)
    
    def summarize(self, text, max_sentences=3):
        """Summarize text"""
        prompt = f"""Summarize this in {max_sentences} sentences or less:

{text}"""
        
        return self._call_api(prompt)
    
    def change_tone(self, text, tone="professional"):
        """Rewrite in different tone"""
        prompt = f"""Rewrite this text in a {tone} tone:

{text}

Return only the rewritten version."""
        
        return self._call_api(prompt)
    
    def _call_api(self, prompt):
        """Make API call"""
        try:
            response = self.client.messages.create(
                model="claude-sonnet-4-20250514",
                max_tokens=1024,
                messages=[{"role": "user", "content": prompt}]
            )
            return response.content[0].text
        except Exception as e:
            return f"Error: {str(e)}"


# Example usage
if __name__ == "__main__":
    assistant = WritingAssistant()
    
    # Test text
    original = """hey can u help me with this thing i need to 
    write a email to my boss about the project being delayed
    its because of some technical issues"""
    
    print("ORIGINAL:")
    print(original)
    print("\n" + "="*50 + "\n")
    
    # Improve it
    improved = assistant.improve_text(original)
    print("IMPROVED:")
    print(improved)
    print("\n" + "="*50 + "\n")
    
    # Make it more casual
    casual = assistant.change_tone(improved, "friendly and casual")
    print("CASUAL VERSION:")
    print(casual)
```

---

## ✅ Module Completion Checklist

Before moving to Module 5, ensure you can:
- [ ] Successfully call Claude API
- [ ] Successfully call OpenAI API (if you have access)
- [ ] Store and use API keys securely
- [ ] Handle API responses and errors
- [ ] Build a simple chat interface
- [ ] Understand token usage and costs

## 🎯 Next Steps

You can now communicate with AI models programmatically! This is the foundation of every AI app.

Move to **[Module 5: Your First AI App](../05-first-ai-app/README.md)**

## 📚 Additional Resources

### Documentation
- [Anthropic API Docs](https://docs.anthropic.com)
- [OpenAI API Docs](https://platform.openai.com/docs)

### Videos
- [What is an API? (In 5 Minutes)](https://www.youtube.com/watch?v=s7wmiS2mSXY)
- [Building with Claude API](https://www.youtube.com/watch?v=Jq89DB_KiEE)

### Tools
- [Postman](https://www.postman.com/) - Test APIs visually
- [HTTP Toolkit](https://httptoolkit.tech/) - Debug API calls

---

**Estimated Time**: 4-6 hours  
**Difficulty**: ⭐⭐⭐☆☆ (Medium)  
**Prerequisites**: Modules 0-3

**Next**: [Module 5: Your First AI App →](../05-first-ai-app/README.md)
