# Cognition Loop: Building Autonomous ReAct Agents

Welcome to the Cognition Loop. This project bridges the gap between basic conversational chatbots and autonomous, action-driven AI systems.

## The Final Vision

Current consumer LLMs are passive systems: they require a user prompt, generate text, and halt. This project shifts that paradigm. Over the next eight weeks, you will engineer an active, autonomous agent capable of reasoning through multi-step problems and executing actions in the real world.

By the end of this project, you will build an orchestration engine capable of executing complex instructions such as:
> *"Query the internal documentation database for troubleshooting steps regarding error code 404, summarize the fix, and then open a web browser to order a replacement ethernet cable from Amazon."*

To achieve this, the agent will independently:
1. Formulate a plan.
2. Utilize a Retrieval-Augmented Generation (RAG) tool to search internal documents.
3. Utilize browser automation tools (Selenium/Playwright) to interact with live websites.
4. Process the observations and complete the objective without human intervention.

##  Week 1: Required Reading & Technical Resources

Before writing the control logic, it is critical to understand the underlying architecture of modern AI. Review these resources before starting the tasks.

### 1. Conceptual Foundations
* **The Mechanics of LLMs:** [[1hr Talk] Intro to Large Language Models by Andrej Karpathy](https://www.youtube.com/watch?v=zjkBMFhNj_g) — Demystifies how standard generative models are trained and why they hallucinate. Understand the training pipeline, tokenization, and the fundamental limitations of standard generative models.
* **The Shift to Agentic AI:** [Andrew Ng Explores The Rise Of AI Agents](https://www.youtube.com/watch?v=KrRD7r7y7NY) — Explains the conceptual shift from direct prompting to systems that utilize reflection, tool use, and planning.
* **The ReAct Framework:** [Prompt Engineering Guide: ReAct](https://www.promptingguide.ai/techniques/react) — Differentiate between standard prompting, Chain of Thought, and the ReAct methodology.
* **4Data Serialization (JSON & APIs)** - Understand why autonomous agents rely on strictly structured JSON data rather than conversational text to trigger external code.
### 2. Core API Documentation
* **Google GenAI SDK (Python):** [Official Documentation](https://ai.google.dev/api/python/google/genai) — Bookmark this. You will need it for understanding how to pass System Instructions, force JSON schemas, and handle rate limits.



## Week 1 Tasks: Infrastructure and Control

This week focuses on establishing your local development environment and using the Google Gemini API to return predictable data formats.


---


### Task 1: Environment Configuration
1. Open your terminal and initialize a Python virtual environment:
   `python -m venv venv`
2. Activate your virtual environment:
   * Windows: `venv\Scripts\activate`
   * Mac/Linux: `source venv/bin/activate`
3. Install the specific Google Gemini SDK and the environment manager:
   `pip install google-genai python-dotenv`
4. Obtain a free API Key from Google AI Studio.
5. Create a file named `.env` in the root of your project directory and add your key:
   `GEMINI_API_KEY=your_actual_api_key_here`

### Task 2: Programmatic Execution (basic_call.py)
Create a script named `basic_call.py` that connects to the Gemini API and prints a simple response.
* Use `python-dotenv` to import your key securely into your script.
* Initialize the Gemini client using the `gemini-2.5-flash` model.
* Pass a basic prompt (e.g., "Explain Newton's 2nd law in one sentence") and print the raw text response to your terminal.

### Task 3: Managing Rate Limits (rate_limit_handler.py)
The free tier for `gemini-2.5-flash` limits you to 10 requests per minute. Since autonomous agents run loops, they can hit this limit instantly and crash your script. 
* Write a script named `rate_limit_handler.py`.
* Create a simple `for` loop that attempts to call the API 15 times rapidly. 
* Use a `try...except` block to catch the rate limit exception. When caught, your script should use `time.sleep()` to pause execution for a few seconds before automatically trying again, rather than crashing entirely.

### Task 4: System Instruction Manipulation (persona_call.py)
Create a script named `persona_call.py` to change the behavior of the model.
* When initializing the model, use the `system_instruction` parameter.
* Set a ruleset directing the model to act as a highly specific persona (e.g., a formal 19th-century butler or a vintage computer terminal from the 1980s).
* Send a generic prompt like "How is the weather today?" and print the response to prove the persona constraints are strictly followed.

### Task 5: Strict Data Extraction (json_extractor.py)
To trigger external code later, our engine requires clean data structures, not conversational text. Create a script named `json_extractor.py`.
* Copy this exact unstructured block of text into your script as a variable:
    "We interviewed Alex Mercer today. He is 24 years old and works as a Junior Data Analyst. His technical toolkit consists of Python, SQL, and Tableau."
* Set a `system_instruction` directing the model to act as a strict data parser that outputs ONLY raw JSON, with no conversational text and no markdown formatting wrappers.
* The target JSON schema must look like this:
    `{"name": "string", "age": integer, "role": "string", "skills": ["string", "string"]}`
* Inside your script, pass the model's raw string output directly into Python's native `json.loads()` function. Extract and print just the skills list to verify the conversion worked without syntax errors.

---

## Expected Outputs for Week 1

Upon executing your code locally, your scripts must produce the following terminal outcomes:

* **basic_call.py**: A standard paragraph or sentence answering your query cleanly.
* **rate_limit_handler.py**: Your terminal should show the first requests succeeding, a pause when the limit is hit, and then the remaining requests completing successfully.
* **persona_call.py**: A text response written entirely within the voice and vocabulary of your chosen system instruction ruleset.
* **json_extractor.py**: The terminal must display a native Python list (e.g., `['Python', 'SQL', 'Tableau']`). If your script encounters a `JSONDecodeError` or displays markdown characters, your system prompt is not strict enough.

---

## Submission Protocol

1. Fork this repository to your personal GitHub account.
2. Navigate to the `/mentee_submissions` directory and create a new directory titled `Firstname_Lastname`.
3. Place your four Python scripts within this directory.
4. Open a Pull Request (PR) against the main repository prior to the Week 1 deadline.

---

### A Note Before You Begin
This might feel like we are just writing basic API scripts right now, but you are actually laying the foundation for something massive. The string parsing, error handling, and system instructions you are mastering this week are the exact mechanisms that will act as the "brain" of your autonomous agent. Every complex software system starts with a successful baseline. Read the documentation, debug the errors, and have fun building!
