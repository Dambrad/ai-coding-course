# ai-learning-journey

---

# 🎓 The AI-Native Engineer: Zero-to-Hero

**Project:** `ai-learning-journey`

**Focus:** Absolute Beginner, Zero-Cost AI, and Visual GitHub Management.

---

## 🛠 Phase 1: The "Command Center" (Physical Setup)

*Think of this like setting up your desk. We do this once and never again.*

### 1.1 The "Magic" Command

1. Press **Cmd + Space**, type `Terminal`, and hit Enter.
2. Copy and paste this exactly:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

```


*(If it asks for your Mac password, type it. You won't see the letters moving—that's normal! Just hit Enter.)*
3. Once that finishes, paste this:
```bash
brew install python git ollama

```



### 1.2 The Two Main Apps

1. **Download [Cursor](https://cursor.com):** This is where you write code. It’s like a "Smart Word" for coding.
2. **Download [GitHub Desktop](https://desktop.github.com):** This is how you "save" your work to the internet without using scary commands.

---

## 🌐 Phase 2: Connecting to the Cloud (The GitHub Mix-up)

*Here is the difference: **GitHub Web** is the "Cloud Storage" (like Google Drive). **GitHub Desktop** is the "Sync App" (like the Dropbox folder on your Mac).*

### Step-by-Step Sync:

1. **Go to GitHub.com (Web):** Log in and click the green **New** button to create a repository named `ai-learning-journey`.
* *Crucial:* Check the box that says **"Add a README file."**


2. **Open GitHub Desktop (App):**
* Go to **File > Clone Repository**.
* You will see your `ai-learning-journey` listed there. Click it and hit **Clone**.
* **Where did it go?** It just created a folder on your Mac (usually in `Documents/GitHub`). **This is now your home base.**



---

## 🤖 Phase 3: Setting Up Your FREE AI (Ollama)

*You don't need to pay for Claude's API. We will run the AI directly on your M3 chip.*

1. **Start Ollama:** Open the Ollama app you installed in Phase 1. You’ll see a little llama icon in your top Mac menu bar.
2. **Download the "Brain":** Open your Terminal and type:
```bash
ollama run qwen2.5-coder:7b

```


*(Wait for it to download. This is a free, high-end coding AI that lives only on your Mac.)*
3. **Link to Cursor:**
* Open **Cursor**. Press **Cmd + Shift + X** (the Extensions view).
* Search for and install **"Continue"**.
* Click the **Continue** icon (a little "C" on the far left bar).
* Change the model provider to **Ollama**.
* **Result:** You can now highlight code and ask questions for **free**.



---

## 🐍 Phase 4: Python Fluency (Spoon-Fed Version)

*You only need to know 3 things to build AI tools:*

1. **Variables:** Giving a name to a piece of information.
* `my_car = "Subaru"` (You just saved text into a label).


2. **Lists:** A shopping list for code.
* `kids_ages = [2.5, 4.5]`


3. **The "Ask" (API):** Telling the AI what to do.
* `ai.ask("How do I fix this car?")`



### Lab 4.1: Your First Script

1. In Cursor, click **File > Open Folder** and pick your `ai-learning-journey` folder.
2. Right-click the sidebar and click **New File**. Name it `hello.py`.
3. Type `print("Hello Levon, I'm coding!")`
4. At the bottom of Cursor, you’ll see a **Terminal**. Type: `python3 hello.py`.
5. **Vibe Code it:** Highlight that line, press **Cmd + K**, and type: *"Change this to a script that counts from 1 to 10."* Watch the AI write it for you.

---

## 🤖 Phase 5: Building Agents (The Goal)

*An Agent is just a Python script that has "Rules" and "Tools."*

* **The Rule:** "You are a Cloud Consulting Assistant."
* **The Tool:** "You are allowed to read files in this folder."
* **The Task:** "Read my `README.md` and tell me what I should learn next."

---

## 🚀 How to Save Your Work (The Visual Workflow)

*Do this every time you finish a session:*

1. Open the **GitHub Desktop** app.
2. You will see a list of changes on the left.
3. At the bottom left, there’s a box. Type: **"Finished my first Python script"**.
4. Click the blue **Commit to main** button. (This saves it to your Mac's history).
5. Click **Push Origin** at the top. (This sends it to the GitHub website).

---

### Your First Homework

1. Open **Cursor**.
2. Open your **`ai-learning-journey`** folder.
3. Create a file called `Syllabus.md` and paste this entire message into it.
4. Use **GitHub Desktop** to "Commit" and "Push" it to the cloud.

---

*Note: To customize how I talk to you or format these lessons in the future, you can always update your preferences [here](https://gemini.google.com/saved-info).*

## 📖 Recommended Resources

### 📺 Video & Visuals

* **[Andrej Karpathy: Intro to LLMs](https://www.youtube.com/watch?v=zjkBMFhNj_g):** Best high-level overview.
* **[Cursor's Official Tutorials](https://www.google.com/search?q=https://cursor.com/how-to-use):** Master the "Cmd+K" and "Cmd+L" shortcuts.
* ** Watch: Andrej Karpathy's "Vibe Coding" Philosophy https://www.youtube.com/watch?v=zjkBMFhNj_g 
* ** Course: DeepLearning.AI - AI Agents in Python https://www.google.com/search?q=https://www.deeplearning.ai/short-courses/ai-agents-in-python/
* ** Tutorial: GitHub Hello World Guide https://docs.github.com/en/get-started/quickstart/hello-world

### 📜 Documentation

* **[Anthropic's Guide to Agents](https://www.google.com/search?q=https://www.anthropic.com/news/building-effective-agents):** Professional strategies for agentic design.
* **[GitHub Skills](https://skills.github.com/):** Interactive labs for version control.

---
