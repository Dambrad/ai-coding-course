# Module 8: Introduction to Agents

> **Goal**: Build your first autonomous AI agent that can reason, plan, and execute tasks.

**Time**: 4-5 hours  
**Difficulty**: Intermediate  
**Prerequisites**: Modules 0-7 (Python, APIs, Claude basics)

---

## 🎯 What You'll Learn

- What autonomous agents are and how they differ from chatbots
- The ReAct pattern (Reasoning + Acting)
- Building tools for agents to use
- Creating a simple task-planning agent
- Agent frameworks overview (LangChain, CrewAI, AutoGen)
- Building your first agent from scratch

---

## 📝 Part 1: Understanding Agents

### What is an AI Agent?

**Chatbot** (what you've built so far):
- Responds to single queries
- No memory of goals
- User drives the interaction
- Example: "What's the weather?" → Answer

**Agent** (what we'll build now):
- Given a goal, figures out how to achieve it
- Can use tools (search web, run code, etc.)
- Makes decisions autonomously
- Example: "Plan a trip to Tokyo" → Researches flights, hotels, creates itinerary

### Key Components of an Agent

```
┌─────────────────────────────────────┐
│           AI AGENT                  │
├─────────────────────────────────────┤
│ 1. LLM (Brain)                      │
│    - Thinks and reasons             │
│    - Makes decisions                │
│                                     │
│ 2. Tools (Hands)                    │
│    - Web search                     │
│    - Calculator                     │
│    - File system                    │
│    - APIs                           │
│                                     │
│ 3. Memory (Mind)                    │
│    - Short-term (current task)      │
│    - Long-term (learned knowledge)  │
│                                     │
│ 4. Planning (Strategy)              │
│    - Break down goals               │
│    - Sequence actions               │
└─────────────────────────────────────┘
```

### The ReAct Pattern

**ReAct** = Reasoning + Acting

The agent follows this loop:
1. **Thought**: "What should I do next?"
2. **Action**: Use a tool or take an action
3. **Observation**: See the result
4. **Repeat** until goal is achieved

**Example**:
```
User: "What's the weather in San Francisco and should I bring an umbrella?"

Thought: I need to check the weather in San Francisco
Action: search_weather("San Francisco")
Observation: 68°F, Sunny, 0% chance of rain

Thought: It's sunny with no rain, so no umbrella needed
Action: respond_to_user
Answer: "The weather in San Francisco is 68°F and sunny. No umbrella needed!"
```

---

## 💻 Part 2: Building Your First Agent

### Simple Agent Architecture

Create `simple_agent.py`:

```python
import anthropic
import os
import json

client = anthropic.Anthropic(api_key=os.environ.get("ANTHROPIC_API_KEY"))

class SimpleAgent:
    def __init__(self):
        self.conversation = []
        self.tools = {
            "calculator": self.calculator,
            "web_search": self.web_search,
        }
    
    def calculator(self, expression):
        """Evaluate a math expression"""
        try:
            result = eval(expression)
            return f"Result: {result}"
        except Exception as e:
            return f"Error: {e}"
    
    def web_search(self, query):
        """Simulate web search (we'll make this real later)"""
        # For now, just return a mock response
        mock_results = {
            "weather": "Sunny, 72°F",
            "time": "3:45 PM PST",
            "news": "Latest tech headlines..."
        }
        return mock_results.get(query.lower(), "No results found")
    
    def run(self, goal):
        """Main agent loop"""
        print(f"\n🎯 Goal: {goal}\n")
        
        max_iterations = 5
        for i in range(max_iterations):
            print(f"--- Iteration {i+1} ---")
            
            # Ask Claude to think and choose an action
            thought_response = self.think(goal)
            print(f"💭 Thought: {thought_response['thought']}")
            
            if thought_response['action'] == 'DONE':
                print(f"\n✅ Final Answer: {thought_response['answer']}")
                return thought_response['answer']
            
            # Execute the action
            action = thought_response['action']
            action_input = thought_response['action_input']
            print(f"🛠️  Action: {action}({action_input})")
            
            # Use the tool
            if action in self.tools:
                observation = self.tools[action](action_input)
                print(f"👀 Observation: {observation}\n")
            else:
                observation = "Tool not found"
                print(f"❌ Error: {observation}\n")
    
    def think(self, goal):
        """Ask Claude to think about what to do next"""
        prompt = f"""You are an autonomous agent trying to achieve this goal:

GOAL: {goal}

You have these tools available:
- calculator(expression): Evaluate math expressions
- web_search(query): Search for information

Think step by step:
1. What do I know so far?
2. What do I need to find out?
3. Which tool should I use?
4. What input should I give it?

Respond in this JSON format:
{{
  "thought": "your reasoning here",
  "action": "tool_name or DONE",
  "action_input": "input for the tool",
  "answer": "final answer if done, otherwise empty"
}}

If you can answer the goal without more tools, use action: "DONE"."""

        message = client.messages.create(
            model="claude-sonnet-4-20250514",
            max_tokens=500,
            messages=[{"role": "user", "content": prompt}]
        )
        
        # Parse Claude's response
        response_text = message.content[0].text
        
        # Extract JSON (Claude might wrap it in markdown)
        if "```json" in response_text:
            response_text = response_text.split("```json")[1].split("```")[0]
        elif "```" in response_text:
            response_text = response_text.split("```")[1].split("```")[0]
        
        try:
            return json.loads(response_text.strip())
        except:
            # Fallback if parsing fails
            return {
                "thought": "I'm having trouble parsing my thoughts",
                "action": "DONE",
                "action_input": "",
                "answer": "I encountered an error"
            }

# Test it
if __name__ == "__main__":
    agent = SimpleAgent()
    
    # Try different goals
    agent.run("What is 15 * 37 + 42?")
    agent.run("What's the weather like?")
```

**Run it**:
```bash
python3 simple_agent.py
```

---

### 🔬 Lab 8.1: Understanding the Flow

**Exercise**: Add a `logger` tool to the agent:

```python
def logger(self, message):
    """Log information to a file"""
    with open("agent_log.txt", "a") as f:
        from datetime import datetime
        f.write(f"[{datetime.now()}] {message}\n")
    return "Logged successfully"
```

Don't forget to add it to `self.tools`!

**Test**: Run the agent with goal "Log today's date and time"

---

## 💻 Part 3: ReAct Agent with Real Tools

Let's build a more sophisticated agent with actual tools.

### Install Dependencies

```bash
pip install requests beautifulsoup4 --break-system-packages
```

### Create `react_agent.py`:

```python
import anthropic
import os
import json
import requests
from datetime import datetime

client = anthropic.Anthropic(api_key=os.environ.get("ANTHROPIC_API_KEY"))

class ReActAgent:
    def __init__(self):
        self.conversation_history = []
        self.max_iterations = 10
    
    # ===== TOOLS =====
    
    def calculator(self, expression):
        """Safely evaluate math expressions"""
        try:
            # Only allow safe math operations
            allowed_chars = set("0123456789+-*/(). ")
            if not all(c in allowed_chars for c in expression):
                return "Error: Invalid characters in expression"
            
            result = eval(expression)
            return str(result)
        except Exception as e:
            return f"Error: {str(e)}"
    
    def get_current_time(self, timezone="UTC"):
        """Get current time"""
        now = datetime.now()
        return now.strftime("%Y-%m-%d %H:%M:%S")
    
    def web_search_simulation(self, query):
        """Simulated web search - returns mock data"""
        # In Module 9, we'll make this real!
        mock_data = {
            "python": "Python is a programming language known for simplicity and readability.",
            "weather": "Current weather: 72°F, Partly cloudy",
            "anthropic": "Anthropic is an AI safety company that created Claude."
        }
        
        for key in mock_data:
            if key in query.lower():
                return mock_data[key]
        
        return f"No information found for: {query}"
    
    def file_read(self, filename):
        """Read a file"""
        try:
            with open(filename, 'r') as f:
                return f.read()
        except FileNotFoundError:
            return f"Error: File '{filename}' not found"
        except Exception as e:
            return f"Error: {str(e)}"
    
    def file_write(self, filename, content):
        """Write to a file"""
        try:
            with open(filename, 'w') as f:
                f.write(content)
            return f"Successfully wrote to {filename}"
        except Exception as e:
            return f"Error: {str(e)}"
    
    # ===== AGENT LOGIC =====
    
    def get_tool_descriptions(self):
        """Generate tool descriptions for Claude"""
        return """
Available tools:

1. calculator(expression: str) -> str
   Evaluates mathematical expressions
   Example: calculator("25 * 4 + 10")

2. get_current_time(timezone: str = "UTC") -> str
   Returns current date and time
   Example: get_current_time()

3. web_search_simulation(query: str) -> str
   Searches for information (simulated for now)
   Example: web_search_simulation("python programming")

4. file_read(filename: str) -> str
   Reads content from a file
   Example: file_read("notes.txt")

5. file_write(filename: str, content: str) -> str
   Writes content to a file
   Example: file_write("output.txt", "Hello World")
"""
    
    def execute_tool(self, tool_name, tool_input):
        """Execute a tool and return the result"""
        tools = {
            "calculator": self.calculator,
            "get_current_time": self.get_current_time,
            "web_search_simulation": self.web_search_simulation,
            "file_read": self.file_read,
            "file_write": self.file_write,
        }
        
        if tool_name not in tools:
            return f"Error: Unknown tool '{tool_name}'"
        
        try:
            # Parse tool input if it's a JSON string
            if isinstance(tool_input, str) and tool_input.startswith('{'):
                tool_input = json.loads(tool_input)
            
            # Call the tool
            if isinstance(tool_input, dict):
                result = tools[tool_name](**tool_input)
            else:
                result = tools[tool_name](tool_input)
            
            return result
        except Exception as e:
            return f"Error executing {tool_name}: {str(e)}"
    
    def run(self, task):
        """Run the agent on a task"""
        print(f"\n{'='*60}")
        print(f"🎯 Task: {task}")
        print(f"{'='*60}\n")
        
        # Build the system prompt
        system_prompt = f"""You are an autonomous agent that accomplishes tasks by reasoning and using tools.

{self.get_tool_descriptions()}

For each step, think about:
1. What information do I have?
2. What do I need to find out?
3. Which tool should I use?
4. What should I ask it?

Respond in this exact JSON format:
{{
  "thought": "Your reasoning about what to do next",
  "action": "tool_name",
  "action_input": "input for the tool",
  "final_answer": null
}}

When you have completed the task, respond with:
{{
  "thought": "I have all the information needed",
  "action": "finished",
  "action_input": "",
  "final_answer": "Your complete answer to the task"
}}

IMPORTANT: 
- Only use tools that are listed above
- Be specific with tool inputs
- Think step by step
"""
        
        # Initialize conversation
        messages = [
            {"role": "user", "content": f"Task: {task}\n\nWhat's your first step?"}
        ]
        
        # Agent loop
        for iteration in range(self.max_iterations):
            print(f"\n--- Iteration {iteration + 1} ---")
            
            # Get agent's next action
            response = client.messages.create(
                model="claude-sonnet-4-20250514",
                max_tokens=1000,
                system=system_prompt,
                messages=messages
            )
            
            # Parse response
            response_text = response.content[0].text
            
            # Extract JSON
            if "```json" in response_text:
                json_text = response_text.split("```json")[1].split("```")[0]
            elif "```" in response_text:
                json_text = response_text.split("```")[1].split("```")[0]
            else:
                json_text = response_text
            
            try:
                action_data = json.loads(json_text.strip())
            except json.JSONDecodeError:
                print("❌ Failed to parse agent response")
                print(response_text)
                break
            
            # Display thought
            print(f"💭 Thought: {action_data.get('thought', 'No thought provided')}")
            
            # Check if finished
            if action_data.get('action') == 'finished' or action_data.get('final_answer'):
                print(f"\n{'='*60}")
                print(f"✅ Final Answer:")
                print(f"{'='*60}")
                print(action_data.get('final_answer', 'Task completed'))
                return action_data.get('final_answer')
            
            # Execute action
            action = action_data.get('action')
            action_input = action_data.get('action_input', '')
            
            print(f"🛠️  Action: {action}")
            print(f"📝 Input: {action_input}")
            
            # Use the tool
            observation = self.execute_tool(action, action_input)
            print(f"👀 Observation: {observation}")
            
            # Add to conversation
            messages.append({"role": "assistant", "content": json.dumps(action_data)})
            messages.append({
                "role": "user",
                "content": f"Observation: {observation}\n\nWhat's your next step?"
            })
        
        print("\n⚠️  Max iterations reached")
        return "Task incomplete - max iterations exceeded"

# Test the agent
if __name__ == "__main__":
    agent = ReActAgent()
    
    # Test 1: Simple calculation
    print("\n" + "="*60)
    print("TEST 1: Math Problem")
    agent.run("Calculate the result of (123 + 456) * 2, then tell me the answer")
    
    # Test 2: Multi-step task
    print("\n" + "="*60)
    print("TEST 2: File Operations")
    agent.run("Write 'Hello from the agent!' to a file called 'greeting.txt', then read it back and confirm the content")
    
    # Test 3: Information gathering
    print("\n" + "="*60)
    print("TEST 3: Information Gathering")
    agent.run("Search for information about Python programming and summarize it in one sentence")
```

**Run it**:
```bash
python3 react_agent.py
```

---

### 🔬 Lab 8.2: Extending the Agent

**Challenge**: Add these tools to the agent:

1. **Word Counter**:
```python
def count_words(self, text):
    """Count words in text"""
    return str(len(text.split()))
```

2. **Temperature Converter**:
```python
def celsius_to_fahrenheit(self, celsius):
    """Convert Celsius to Fahrenheit"""
    try:
        c = float(celsius)
        f = (c * 9/5) + 32
        return f"{c}°C = {f}°F"
    except:
        return "Error: Invalid temperature"
```

**Test task**: "Convert 25 Celsius to Fahrenheit, then write the result to a file"

---

## 💻 Part 4: Agent Frameworks Overview

Building agents from scratch teaches you how they work, but frameworks make it easier.

### Popular Frameworks

#### 1. **LangChain** (Most Popular)
```python
# Example (don't run yet - just showing concept)
from langchain.agents import initialize_agent, AgentType
from langchain.llms import Anthropic
from langchain.tools import Tool

tools = [
    Tool(
        name="Calculator",
        func=calculator,
        description="Useful for math"
    )
]

agent = initialize_agent(
    tools=tools,
    llm=Anthropic(),
    agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION
)

agent.run("What is 25 * 17?")
```

**Pros**: Huge ecosystem, many integrations  
**Cons**: Complex, steep learning curve

#### 2. **CrewAI** (Best for Multi-Agent)
```python
from crewai import Agent, Task, Crew

researcher = Agent(
    role="Researcher",
    goal="Find information",
    tools=[search_tool]
)

writer = Agent(
    role="Writer",
    goal="Write articles",
    tools=[write_tool]
)

crew = Crew(agents=[researcher, writer])
crew.kickoff()
```

**Pros**: Great for teams of agents  
**Cons**: Newer, less documentation

#### 3. **AutoGen** (Microsoft)
```python
from autogen import AssistantAgent, UserProxyAgent

assistant = AssistantAgent("assistant")
user_proxy = UserProxyAgent("user")

user_proxy.initiate_chat(assistant, message="Build me a website")
```

**Pros**: Powerful conversation flows  
**Cons**: Requires more setup

### When to Use What?

- **From scratch** (what we're doing): Learning, full control
- **LangChain**: Production apps, need lots of integrations
- **CrewAI**: Multiple agents working together
- **AutoGen**: Complex multi-turn conversations

---

## 🎯 Project: Research Assistant Agent

Let's build a practical agent that can research topics!

Create `research_agent.py`:

```python
import anthropic
import os
import json
from datetime import datetime

client = anthropic.Anthropic(api_key=os.environ.get("ANTHROPIC_API_KEY"))

class ResearchAgent:
    def __init__(self):
        self.notes = []
        self.sources = []
    
    def take_note(self, note):
        """Store a research note"""
        self.notes.append({
            "content": note,
            "timestamp": datetime.now().isoformat()
        })
        return f"Note saved: {note}"
    
    def search_topic(self, topic):
        """Simulate searching for a topic"""
        # Mock data - in Module 9 we'll use real search
        knowledge_base = {
            "machine learning": "Machine learning is a subset of AI that enables systems to learn from data...",
            "neural networks": "Neural networks are computing systems inspired by biological neural networks...",
            "python": "Python is a high-level programming language known for readability...",
        }
        
        for key in knowledge_base:
            if key in topic.lower():
                self.sources.append(f"Source: {key}")
                return knowledge_base[key]
        
        return f"No specific information found for: {topic}. Using general knowledge."
    
    def generate_report(self, topic):
        """Generate final report"""
        report = f"""
# Research Report: {topic}

Generated: {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}

## Key Findings

"""
        for i, note in enumerate(self.notes, 1):
            report += f"\n{i}. {note['content']}"
        
        report += "\n\n## Sources\n"
        for source in set(self.sources):
            report += f"\n- {source}"
        
        return report
    
    def research(self, topic):
        """Conduct research on a topic"""
        print(f"\n🔍 Researching: {topic}\n")
        
        system_prompt = f"""You are a research assistant. Your goal is to research the topic: "{topic}"

You have these tools:
1. search_topic(topic: str) - Search for information
2. take_note(note: str) - Save an important finding
3. generate_report(topic: str) - Create final report

Process:
1. Search for information about the topic
2. Take notes on key findings (aim for 3-5 notes)
3. Search related subtopics if needed
4. Generate the final report

Respond in JSON:
{{
  "thought": "what you're thinking",
  "action": "tool name",
  "action_input": "input",
  "is_finished": false
}}

When done researching, set is_finished to true and use generate_report."""
        
        messages = [{"role": "user", "content": f"Research this topic: {topic}"}]
        
        for i in range(15):  # Max 15 steps
            response = client.messages.create(
                model="claude-sonnet-4-20250514",
                max_tokens=800,
                system=system_prompt,
                messages=messages
            )
            
            # Parse response
            text = response.content[0].text
            if "```json" in text:
                text = text.split("```json")[1].split("```")[0]
            
            try:
                step = json.loads(text.strip())
            except:
                print("❌ Parse error")
                break
            
            print(f"💭 {step.get('thought', '')}")
            
            # Check if finished
            if step.get('is_finished'):
                print("\n" + "="*60)
                print("📊 FINAL REPORT")
                print("="*60)
                report = self.generate_report(topic)
                print(report)
                return report
            
            # Execute action
            action = step.get('action')
            action_input = step.get('action_input', '')
            
            print(f"🛠️  {action}({action_input})")
            
            # Call the appropriate method
            if action == "search_topic":
                result = self.search_topic(action_input)
            elif action == "take_note":
                result = self.take_note(action_input)
            elif action == "generate_report":
                result = self.generate_report(action_input)
            else:
                result = "Unknown action"
            
            print(f"👀 {result}\n")
            
            # Continue conversation
            messages.append({"role": "assistant", "content": text})
            messages.append({"role": "user", "content": f"Result: {result}"})
        
        return "Research incomplete"

# Run it
if __name__ == "__main__":
    agent = ResearchAgent()
    agent.research("machine learning and neural networks")
```

**Try it**:
```bash
python3 research_agent.py
```

---

## 📚 Best Practices for Agents

### 1. Clear Tool Descriptions
```python
def search(query):
    """
    Search for information on the web.
    
    Args:
        query (str): Search query (2-5 words work best)
    
    Returns:
        str: Search results summary
    
    Example:
        search("weather in Tokyo")
    """
    pass
```

### 2. Error Handling
Always return strings from tools, not exceptions:
```python
try:
    result = risky_operation()
    return f"Success: {result}"
except Exception as e:
    return f"Error: {str(e)}"
```

### 3. Limit Iterations
Prevent infinite loops:
```python
max_iterations = 10  # Prevent runaway agents
```

### 4. Logging
Track what agents do:
```python
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

logger.info(f"Agent action: {action}")
```

### 5. Cost Monitoring
Track API costs:
```python
total_tokens = 0
for iteration in agent_loop:
    response = api_call()
    total_tokens += response.usage.total_tokens
    print(f"Total tokens used: {total_tokens}")
```

---

## 🎓 Resources

### Documentation
- [Anthropic Tool Use Guide](https://docs.anthropic.com/claude/docs/tool-use)
- [ReAct Paper](https://arxiv.org/abs/2210.03629) - The research behind this pattern
- [LangChain Agents](https://python.langchain.com/docs/modules/agents/)

### Videos
- [Building AI Agents from Scratch](https://www.youtube.com/watch?v=example)
- [LangChain Agent Tutorial](https://www.youtube.com/watch?v=example)

### Articles
- [What Are AI Agents?](https://www.anthropic.com/index/what-are-ai-agents)
- [Agent Design Patterns](https://example.com/agent-patterns)

---

## ✅ Module Checklist

Before moving to Module 9, make sure you can:
- [ ] Explain what an agent is vs a chatbot
- [ ] Understand the ReAct pattern
- [ ] Build tools for agents to use
- [ ] Create a simple agent loop
- [ ] Handle agent errors gracefully
- [ ] Track agent iterations and costs
- [ ] Build a multi-step reasoning agent

---

## 🎯 Next Steps

Ready for **[Module 9: Multi-Step Agents](09-multi-step-agents.md)**!

In Module 9, you'll learn:
- Real web search integration
- Complex planning strategies
- Memory and state management
- Building production-ready agents

---

## 💡 Challenge Projects

1. **Personal Assistant**: Schedule tasks, set reminders
2. **Code Reviewer**: Analyze code and suggest improvements
3. **Data Analyst**: Load CSV, analyze, create charts
4. **Shopping Helper**: Compare prices, find deals
5. **Study Buddy**: Create quizzes, explain concepts

---

**Fantastic progress!** 🎉 You've now built autonomous agents that can think and act. This is cutting-edge AI engineering!
