# Module 6: Introduction to Agents

> From simple chatbots to autonomous agents — understanding the paradigm shift in AI.

## 🎯 Module Goals

By the end of this module, you'll understand:
- What makes an agent different from a chatbot
- The core components of agentic systems
- Tool use and function calling
- Building your first autonomous agent
- Agent decision-making and loops

## ⏱️ Estimated Time: 6-8 hours

## 🤖 What is an Agent?

### Chatbot vs Agent

**Chatbot** (Stateless):
```
You: "What's the weather?"
AI: "I don't have access to real-time weather data..."
[End of interaction]
```

**Agent** (Can Take Actions):
```
You: "What's the weather?"
Agent: [Decides to use weather tool]
Agent: [Calls weather API with your location]
Agent: "It's 72°F and sunny in San Francisco"
[Agent acted autonomously to get the answer]
```

### Key Differences

| Feature | Chatbot | Agent |
|---------|---------|-------|
| **Interaction** | Single turn | Multi-step |
| **Tools** | None | Can use tools |
| **Autonomy** | Responds only | Takes actions |
| **Memory** | Conversation only | Can remember & learn |
| **Decision Making** | Limited | Plans and reasons |

---

## 🧩 Anatomy of an Agent

Every agent has these components:

```
┌─────────────────────────────────────┐
│           USER INPUT                │
└────────────┬────────────────────────┘
             │
             ▼
┌─────────────────────────────────────┐
│     LLM (Brain/Reasoning)           │
│   "What should I do?"               │
└────────────┬────────────────────────┘
             │
             ▼
┌─────────────────────────────────────┐
│      DECISION LAYER                 │
│   "I need to use the weather tool"  │
└────────────┬────────────────────────┘
             │
             ▼
┌─────────────────────────────────────┐
│         TOOLS/ACTIONS               │
│   [Weather API, Calculator, etc.]   │
└────────────┬────────────────────────┘
             │
             ▼
┌─────────────────────────────────────┐
│      RESPONSE TO USER               │
└─────────────────────────────────────┘
```

### 1. The Brain (LLM)
The AI model that reasons and makes decisions.

### 2. Tools
Functions the agent can call (APIs, calculators, databases, etc.)

### 3. Memory
Short-term (conversation) and long-term (database, files)

### 4. Agent Loop
The cycle of: Think → Act → Observe → Repeat

---

## 🛠️ Tool Use: The Foundation of Agents

### What are Tools?

Tools are Python functions that your agent can call. Examples:
- `get_weather(city)` - Get weather data
- `search_web(query)` - Search the internet
- `calculate(expression)` - Do math
- `send_email(to, subject, body)` - Send emails

### Simple Example: Calculator Tool

```python
import os
from anthropic import Anthropic

# Define a tool
def calculator(expression):
    """Safely evaluate a math expression"""
    try:
        result = eval(expression, {"__builtins__": {}})
        return str(result)
    except Exception as e:
        return f"Error: {str(e)}"

# Tool definition for Claude
tools = [
    {
        "name": "calculator",
        "description": "Performs mathematical calculations. Input should be a valid Python expression like '2+2' or '10*5'.",
        "input_schema": {
            "type": "object",
            "properties": {
                "expression": {
                    "type": "string",
                    "description": "The mathematical expression to evaluate"
                }
            },
            "required": ["expression"]
        }
    }
]

# Create client
client = Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))

# User asks a math question
user_message = "What is 1234 multiplied by 5678?"

# Call Claude with tools
response = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=1024,
    tools=tools,
    messages=[{"role": "user", "content": user_message}]
)

# Check if Claude wants to use a tool
if response.stop_reason == "tool_use":
    # Extract tool use
    tool_use = next(block for block in response.content if block.type == "tool_use")
    
    print(f"Claude wants to use: {tool_use.name}")
    print(f"With input: {tool_use.input}")
    
    # Execute the tool
    result = calculator(tool_use.input["expression"])
    print(f"Calculator result: {result}")
    
    # Send result back to Claude
    final_response = client.messages.create(
        model="claude-sonnet-4-20250514",
        max_tokens=1024,
        tools=tools,
        messages=[
            {"role": "user", "content": user_message},
            {"role": "assistant", "content": response.content},
            {
                "role": "user",
                "content": [
                    {
                        "type": "tool_result",
                        "tool_use_id": tool_use.id,
                        "content": result
                    }
                ]
            }
        ]
    )
    
    print(f"\nFinal answer: {final_response.content[0].text}")
```

**What just happened?**

1. User asks: "What is 1234 × 5678?"
2. Claude decides: "I should use the calculator tool"
3. We execute: `calculator("1234*5678")`
4. We return result: "7006652"
5. Claude responds: "The answer is 7,006,652"

---

## 🔄 The Agent Loop

An agent doesn't just use tools once — it can loop until the task is complete.

```python
class SimpleAgent:
    """Basic agent that can use tools"""
    
    def __init__(self, client, tools):
        self.client = client
        self.tools = tools
        self.tool_functions = {}
    
    def register_tool(self, name, function):
        """Register a Python function as a tool"""
        self.tool_functions[name] = function
    
    def run(self, user_message, max_iterations=10):
        """Run the agent loop"""
        messages = [{"role": "user", "content": user_message}]
        
        for iteration in range(max_iterations):
            print(f"\n--- Iteration {iteration + 1} ---")
            
            # Get response from Claude
            response = self.client.messages.create(
                model="claude-sonnet-4-20250514",
                max_tokens=1024,
                tools=self.tools,
                messages=messages
            )
            
            # Add assistant response to messages
            messages.append({"role": "assistant", "content": response.content})
            
            # Check stop reason
            if response.stop_reason == "end_turn":
                # Agent is done!
                final_text = next(
                    (block.text for block in response.content if hasattr(block, "text")),
                    None
                )
                return final_text
            
            elif response.stop_reason == "tool_use":
                # Agent wants to use a tool
                tool_results = []
                
                for block in response.content:
                    if block.type == "tool_use":
                        print(f"Using tool: {block.name}")
                        print(f"Input: {block.input}")
                        
                        # Execute the tool
                        tool_func = self.tool_functions[block.name]
                        result = tool_func(**block.input)
                        
                        print(f"Result: {result}")
                        
                        tool_results.append({
                            "type": "tool_result",
                            "tool_use_id": block.id,
                            "content": str(result)
                        })
                
                # Add tool results to messages
                messages.append({"role": "user", "content": tool_results})
            
            else:
                return "Agent stopped unexpectedly"
        
        return "Max iterations reached"


# Example usage
if __name__ == "__main__":
    from anthropic import Anthropic
    
    client = Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))
    
    # Define tools
    tools = [
        {
            "name": "calculator",
            "description": "Performs mathematical calculations",
            "input_schema": {
                "type": "object",
                "properties": {
                    "expression": {"type": "string"}
                },
                "required": ["expression"]
            }
        }
    ]
    
    # Create agent
    agent = SimpleAgent(client, tools)
    agent.register_tool("calculator", calculator)
    
    # Run agent
    result = agent.run("What is (123 + 456) * 789?")
    print(f"\n\nFinal Answer: {result}")
```

---

## 🧰 Common Agent Tools

### 1. Web Search Tool

```python
import requests

def web_search(query):
    """Search the web (simplified example)"""
    # In reality, use a proper API like SerpAPI or Google Custom Search
    api_key = os.getenv("SERP_API_KEY")
    response = requests.get(
        "https://serpapi.com/search",
        params={"q": query, "api_key": api_key}
    )
    return response.json()["organic_results"][0]["snippet"]
```

### 2. File Reader Tool

```python
def read_file(filepath):
    """Read contents of a file"""
    try:
        with open(filepath, "r") as f:
            return f.read()
    except FileNotFoundError:
        return f"Error: File {filepath} not found"
    except Exception as e:
        return f"Error: {str(e)}"
```

### 3. Current Time Tool

```python
from datetime import datetime

def get_current_time(timezone="UTC"):
    """Get current time"""
    return datetime.now().strftime("%Y-%m-%d %H:%M:%S")
```

### 4. API Call Tool

```python
def call_api(url, method="GET", data=None):
    """Make HTTP API calls"""
    try:
        if method == "GET":
            response = requests.get(url)
        elif method == "POST":
            response = requests.post(url, json=data)
        return response.json()
    except Exception as e:
        return f"Error: {str(e)}"
```

---

## 🎯 Building Your First Real Agent

Let's build a **Research Assistant Agent** that can:
- Search the web
- Read files
- Take notes
- Answer questions

Create `research_agent.py`:

```python
import os
from anthropic import Anthropic
from datetime import datetime

class ResearchAgent:
    """An agent that can research topics using tools"""
    
    def __init__(self):
        self.client = Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))
        self.notes = []
        
        # Define available tools
        self.tools = [
            {
                "name": "take_note",
                "description": "Save important information to notes",
                "input_schema": {
                    "type": "object",
                    "properties": {
                        "note": {
                            "type": "string",
                            "description": "The note to save"
                        }
                    },
                    "required": ["note"]
                }
            },
            {
                "name": "get_current_time",
                "description": "Get the current date and time",
                "input_schema": {
                    "type": "object",
                    "properties": {},
                    "required": []
                }
            },
            {
                "name": "read_notes",
                "description": "Read all saved notes",
                "input_schema": {
                    "type": "object",
                    "properties": {},
                    "required": []
                }
            }
        ]
    
    def take_note(self, note):
        """Save a note"""
        timestamp = datetime.now().strftime("%H:%M:%S")
        self.notes.append(f"[{timestamp}] {note}")
        return f"Note saved: {note}"
    
    def get_current_time(self):
        """Get current time"""
        return datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    
    def read_notes(self):
        """Read all notes"""
        if not self.notes:
            return "No notes saved yet"
        return "\n".join(self.notes)
    
    def execute_tool(self, tool_name, tool_input):
        """Execute a tool by name"""
        if tool_name == "take_note":
            return self.take_note(tool_input["note"])
        elif tool_name == "get_current_time":
            return self.get_current_time()
        elif tool_name == "read_notes":
            return self.read_notes()
        else:
            return f"Unknown tool: {tool_name}"
    
    def research(self, query, max_iterations=10):
        """Main research method"""
        print(f"🔍 Researching: {query}\n")
        
        messages = [{"role": "user", "content": query}]
        
        for iteration in range(max_iterations):
            # Get response
            response = self.client.messages.create(
                model="claude-sonnet-4-20250514",
                max_tokens=2048,
                tools=self.tools,
                messages=messages
            )
            
            # Add to conversation
            messages.append({"role": "assistant", "content": response.content})
            
            # Check if done
            if response.stop_reason == "end_turn":
                # Extract final answer
                for block in response.content:
                    if hasattr(block, "text"):
                        return block.text
            
            # Process tool uses
            elif response.stop_reason == "tool_use":
                tool_results = []
                
                for block in response.content:
                    if block.type == "tool_use":
                        print(f"🔧 Using tool: {block.name}")
                        
                        # Execute tool
                        result = self.execute_tool(block.name, block.input)
                        print(f"   Result: {result}\n")
                        
                        tool_results.append({
                            "type": "tool_result",
                            "tool_use_id": block.id,
                            "content": str(result)
                        })
                
                # Send results back
                messages.append({"role": "user", "content": tool_results})
        
        return "Research completed (max iterations reached)"


# Example usage
if __name__ == "__main__":
    agent = ResearchAgent()
    
    # Test the agent
    result = agent.research("""
    I need to research Python agents. Please:
    1. Take a note about what time you started
    2. Take a note summarizing what an agent is
    3. Take a note about why agents are useful
    4. Then read all your notes and give me a summary
    """)
    
    print("\n" + "="*50)
    print("FINAL RESULT:")
    print(result)
    print("="*50)
```

**Run it**:
```bash
python3 research_agent.py
```

**What you'll see**:
```
🔍 Researching: I need to research Python agents...

🔧 Using tool: get_current_time
   Result: 2026-02-14 15:30:45

🔧 Using tool: take_note
   Result: Note saved: Started research at 2026-02-14 15:30:45

🔧 Using tool: take_note
   Result: Note saved: An agent is an AI system that can autonomously...

🔧 Using tool: take_note
   Result: Note saved: Agents are useful because they can...

🔧 Using tool: read_notes
   Result: [15:30:45] Started research at...

==================================================
FINAL RESULT:
Based on my research notes, here's a summary...
==================================================
```

---

## 🧪 Lab: Build a Task Agent

Create an agent that can manage a to-do list:

```python
# task_agent.py
import os
import json
from anthropic import Anthropic
from datetime import datetime

class TaskAgent:
    """Agent that manages tasks"""
    
    def __init__(self, filename="tasks.json"):
        self.client = Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))
        self.filename = filename
        self.tasks = self.load_tasks()
        
        self.tools = [
            {
                "name": "add_task",
                "description": "Add a new task to the to-do list",
                "input_schema": {
                    "type": "object",
                    "properties": {
                        "task": {"type": "string", "description": "The task description"},
                        "priority": {"type": "string", "enum": ["low", "medium", "high"]}
                    },
                    "required": ["task"]
                }
            },
            {
                "name": "list_tasks",
                "description": "List all tasks",
                "input_schema": {"type": "object", "properties": {}}
            },
            {
                "name": "complete_task",
                "description": "Mark a task as complete",
                "input_schema": {
                    "type": "object",
                    "properties": {
                        "task_id": {"type": "integer"}
                    },
                    "required": ["task_id"]
                }
            }
        ]
    
    def load_tasks(self):
        """Load tasks from file"""
        try:
            with open(self.filename, "r") as f:
                return json.load(f)
        except FileNotFoundError:
            return []
    
    def save_tasks(self):
        """Save tasks to file"""
        with open(self.filename, "w") as f:
            json.dump(self.tasks, f, indent=2)
    
    def add_task(self, task, priority="medium"):
        """Add a task"""
        new_task = {
            "id": len(self.tasks) + 1,
            "task": task,
            "priority": priority,
            "completed": False,
            "created": datetime.now().isoformat()
        }
        self.tasks.append(new_task)
        self.save_tasks()
        return f"Added task #{new_task['id']}: {task}"
    
    def list_tasks(self):
        """List all tasks"""
        if not self.tasks:
            return "No tasks found"
        
        active = [t for t in self.tasks if not t["completed"]]
        completed = [t for t in self.tasks if t["completed"]]
        
        result = "ACTIVE TASKS:\n"
        for task in active:
            result += f"  #{task['id']} [{task['priority']}] {task['task']}\n"
        
        if completed:
            result += "\nCOMPLETED:\n"
            for task in completed:
                result += f"  #{task['id']} ✓ {task['task']}\n"
        
        return result
    
    def complete_task(self, task_id):
        """Mark task as complete"""
        for task in self.tasks:
            if task["id"] == task_id:
                task["completed"] = True
                self.save_tasks()
                return f"Completed task #{task_id}: {task['task']}"
        return f"Task #{task_id} not found"
    
    def execute_tool(self, tool_name, tool_input):
        """Execute a tool"""
        if tool_name == "add_task":
            return self.add_task(**tool_input)
        elif tool_name == "list_tasks":
            return self.list_tasks()
        elif tool_name == "complete_task":
            return self.complete_task(**tool_input)
    
    def run(self, user_input):
        """Process user input"""
        messages = [{"role": "user", "content": user_input}]
        
        for _ in range(5):
            response = self.client.messages.create(
                model="claude-sonnet-4-20250514",
                max_tokens=1024,
                tools=self.tools,
                messages=messages
            )
            
            messages.append({"role": "assistant", "content": response.content})
            
            if response.stop_reason == "end_turn":
                for block in response.content:
                    if hasattr(block, "text"):
                        return block.text
            
            elif response.stop_reason == "tool_use":
                tool_results = []
                
                for block in response.content:
                    if block.type == "tool_use":
                        result = self.execute_tool(block.name, block.input)
                        tool_results.append({
                            "type": "tool_result",
                            "tool_use_id": block.id,
                            "content": result
                        })
                
                messages.append({"role": "user", "content": tool_results})

# Example usage
if __name__ == "__main__":
    agent = TaskAgent()
    
    # Test commands
    print(agent.run("Add a task to learn about Python agents, high priority"))
    print("\n" + agent.run("Add a task to practice coding"))
    print("\n" + agent.run("Show me all my tasks"))
    print("\n" + agent.run("Mark task 1 as done"))
    print("\n" + agent.run("What tasks are left?"))
```

---

## 📊 Agent Design Patterns

### Pattern 1: ReAct (Reason + Act)

The agent reasons about what to do, then acts:

```
Thought: I need to find the current weather
Action: Use weather_tool(city="San Francisco")
Observation: It's 72°F and sunny
Thought: Now I can answer the user
Final Answer: The weather in SF is 72°F and sunny
```

### Pattern 2: Chain of Tools

Use multiple tools in sequence:

```
1. search_web("Python tutorials")
2. read_url(first_result)
3. summarize(content)
4. save_to_file(summary)
```

### Pattern 3: Parallel Tool Use

Use multiple tools at once:

```
Simultaneously:
- get_weather("NYC")
- get_weather("LA")  
- get_weather("Chicago")

Then compare results
```

---

## ✅ Module Completion Checklist

Before moving forward, ensure you can:
- [ ] Explain the difference between chatbots and agents
- [ ] Define tools for Claude to use
- [ ] Build a simple agent loop
- [ ] Create an agent with multiple tools
- [ ] Understand when agents should vs shouldn't use tools

## 🎯 Next Steps

You've built your first agents! Next, we'll make them more sophisticated.

Move to **[Module 7: Building Simple Agents](../07-simple-agents/README.md)**

## 📚 Additional Resources

### Papers
- [ReAct: Reasoning and Acting](https://arxiv.org/abs/2210.03629)
- [Anthropic's Tool Use Guide](https://docs.anthropic.com/en/docs/build-with-claude/tool-use)

### Videos
- [AI Agents Explained](https://www.youtube.com/watch?v=F8NKVhkZZWI)
- [Building Production Agents](https://www.youtube.com/watch?v=eBhh4hjRuPs)

---

**Estimated Time**: 6-8 hours  
**Difficulty**: ⭐⭐⭐⭐☆ (Medium-Hard)  
**Prerequisites**: Modules 0-5

**Next**: [Module 7: Building Simple Agents →](../07-simple-agents/README.md)
