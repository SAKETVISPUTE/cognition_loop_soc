# Cognition Loop: Building Autonomous ReAct Agents

Welcome to the Cognition Loop. This project bridges the gap between basic conversational chatbots and autonomous, action-driven AI systems.

## The Final Vision

Current consumer LLMs are passive systems: they require a user prompt, generate text, and halt. This project shifts that paradigm. Over the next eight weeks, you will engineer an active, autonomous agent capable of reasoning through multi-step problems and executing actions in the real world.

By the end of this project, you will build an orchestration engine capable of executing tasks such as:
> *"Query the internal documentation database to find the user's preferred music genre, then open a web browser, navigate to YouTube or Spotify, and autonomously search for and play a playlist matching that mood."*

To achieve this, the agent will independently:
1. Formulate a plan.
2. Utilize a Retrieval-Augmented Generation (RAG) tool to search internal documents.
3. Utilize browser automation tools to interact with live websites.
4. Process the observations and complete the objective without human intervention.

##  Week 1: Reading & Technical Resources

Before writing the control logic, it is critical to understand the underlying architecture of modern AI. Review these resources before starting the tasks.

### 1. Conceptual Foundations
* **The Mechanics of LLMs:** [[1hr Talk] Intro to Large Language Models by Andrej Karpathy](https://www.youtube.com/watch?v=zjkBMFhNj_g) — Demystifies how standard generative models are trained and why they hallucinate. Understand the training pipeline, tokenization, and the fundamental limitations of standard generative models.
* **The Shift to Agentic AI:** [Andrew Ng Explores The Rise Of AI Agents](https://www.youtube.com/watch?v=KrRD7r7y7NY) — Explains the conceptual shift from direct prompting to systems that utilize reflection, tool use, and planning.
* **The ReAct Framework:** [Prompt Engineering Guide: ReAct](https://www.promptingguide.ai/techniques/react) — Differentiate between standard prompting, Chain of Thought, and the ReAct methodology.
* **4Data Serialization (JSON & APIs)** - Understand why autonomous agents rely on strictly structured JSON data rather than conversational text to trigger external code.
### 2. Core API Documentation
* **Google GenAI SDK (Python):** [Official Documentation](https://ai.google.dev/api/python/google/genai) — Bookmark this. You will need it for understanding how to pass System Instructions, force JSON schemas, and handle rate limits.

## Week 2: Learning Resources

Read this before you start coding. It covers the ideas behind the three tasks so you understand what you are building. You do not need to finish every link so, pick what fills your gaps.

### A note on the model we use (please read)

In Week 1 you used **Gemini 2.5 Flash**. It works, but it burns through a lot of tokens quickly, so the free quota runs out fast and it can get slow or rate-limited during practice. From this week on, **do not use Gemini 2.5 Flash**. Use the **Groq API** instead — it is fast, has a generous free tier. All AI tasks below assume Groq.

### Groq API key setup

1. Go to https://console.groq.com/keys
2. Sign in and click **Create API Key**. Copy the key (you only see it once).
3. In the project root, open the `.env` file and add this line:
   ```
   GROQ_API_KEY=paste-your-key-here
   ```
4. In your code, load it like this (never hardcode the key):
   ```python
   import os
   from dotenv import load_dotenv
   from groq import Groq

   load_dotenv()
   client = Groq(api_key=os.environ["GROQ_API_KEY"])
   ```

Do not share this key or push it to GitHub. Keep your API key in `.env`, never in your code. Make sure `.env` is written inside your `.gitignore` file.



- Groq docs (just for reference): https://console.groq.com/docs/overview

### Tool / function calling with Groq

An LLM can do more than chat: you can give it tools (real Python functions). The model decides when a tool is needed, your code runs it and sends the result back, and the model uses that result to write its final answer. This is exactly the flow you build in the first task.

- Groq tool use docs (read this first): https://console.groq.com/docs/tool-use
- Groq local tool calling — full code walkthrough (docs): https://console.groq.com/docs/tool-use/local-tool-calling
- Build an AI function agent with Groq in Python (video): https://www.youtube.com/watch?v=FZJf4yaiVts

### Browser automation with Playwright

Playwright lets your code open a real browser, visit pages, type, click, and read what is on screen. You use this to open a website and to search and play a video.

- Playwright for Python — getting started (docs): https://playwright.dev/python/docs/intro
- Locators: how to find elements on a page (docs): https://playwright.dev/python/docs/locators
- Playwright Python browser automation crash course (video): https://www.youtube.com/watch?v=cO997sPYZ9U




