# Cognition Loop: Autonomous ReAct Agents & Market Research Planner

> **Engineering active, autonomous agentic systems capable of reasoning, real-time web interaction, state persistence, and multi-step task execution.**

**Author:** Saket Vispute  
**Core Technologies:** Python 3.11+, Groq LPU (Llama 3.3 70B), Playwright Browser Automation, Google Gemini API, Yahoo Finance.

---

## 🌟 Overview

Traditional consumer LLMs are passive: they wait for a prompt, generate text, and halt. **Cognition Loop** bridges the gap between basic chatbots and active, autonomous agentic systems.

Through this project, I built a modular orchestration engine implementing the **ReAct (Reason + Act)** framework from scratch. The agent dynamically reasons about missing data, invokes browser automation tools to interact with live websites, updates a disk-backed state machine, and recovers from errors autonomously.

```
                    ┌───────────────────────────────┐
                    │   User Prompt / Objective     │
                    └───────────────┬───────────────┘
                                    │
                      ┌─────────────▼─────────────┐
          ┌───────────┤   ORCHESTRATION ENGINE    ├───────────┐
          │           │   (plan.json State Machine)│          │
          │           └─────────────┬─────────────┘           │
          │                         │                         │
   1. Sends Prompt           2. Routes Tool            3. Appends Result
      & Tool Schemas            Execution                 to Memory
          │                         │                         │
          ▼                         ▼                         ▼
  ┌───────────────┐         ┌───────────────┐         ┌───────────────┐
  │   Groq LLM    │         │  Playwright   │         │   Groq LLM    │
  │ (Llama 3.3)   │         │(Browser Tool) │         │ (Synthesis)   │
  └───────────────┘         └───────────────┘         └───────────────┘
```

---

## 📂 Project Structure

```
Cognition-Loop/
├── README.md               <-- Master documentation & project guide (this file)
├── JOURNEY.md              <-- Architecture retrospective & capstone design notes
├── capstone.py             <-- Final Capstone: State-Driven Research Planner Agent
├── plan.json               <-- Live disk-backed state machine tracking task progress
├── requirements.txt        <-- Dependencies
├── .env.example            <-- Environment variables template
├── .gitignore              <-- Git ignore rules
│
├── Week1/                  <-- Infrastructure & Deterministic Control (Gemini API)
│   ├── basic_call.py       <-- Basic programmatic Gemini API integration
│   ├── rate_limit_handler.py<-- Exponential sleep-backoff retry logic (HTTP 429)
│   ├── persona_call.py     <-- System instruction persona steering (Victorian Butler)
│   └── json_extractor.py   <-- Deterministic JSON output schema enforcement
│
├── Week2/                  <-- Web Automation & Function Calling (Groq + Playwright)
│   ├── basic_tool.py       <-- Groq function calling with keyless Open-Meteo weather API
│   ├── browser_test.py     <-- Playwright headless scraper for live Hacker News stories
│   └── youtube_autoplay.py <-- Browser automation: search, cookie handling & ad-skipping
│
├── Week3/                  <-- Autonomous ReAct Loops & Tool Chaining
│   ├── research_agent.py   <-- Autonomous ReAct search agent via DuckDuckGo HTML
│   └── chat_agent.py       <-- Two-tier loop: persistent memory + multi-tool chaining
│
├── Week4/                  <-- Long-Term Memory & Goal-Driven Agent
│   ├── my_assistant.py     <-- "Alpha" Market Analyst chat agent
│   ├── memory.json         <-- Persistent cross-session user memory
│   └── goals.json          <-- Persistent multi-day research goal tracker
│
└── reports/                <-- Dossiers generated autonomously by the Capstone Agent
    ├── salesforce_analysis_report.md
    ├── cognizant_india_report.md
    └── infosys_india_report.md
```

---

## 🚀 Capstone Project: State-Driven Market Research Planner Agent

The flagship capstone ([`capstone.py`](capstone.py)) integrates all agentic design patterns into a resilient **State Machine Orchestrator**:

### Key Architectural Highlights:
1. **Disk-Backed State Machine (`plan.json`)**:
   Instead of keeping progress in volatile process memory, the agent creates, reads, and updates a structured plan on disk. Each step tracks its `id`, `task`, `tool_to_use`, and status (`pending`, `in_progress`, `completed`).
2. **Crash Resilience & Resumability (`Ctrl+C` Recovery)**:
   If the script is terminated midway (e.g. while executing step 2), re-launching the agent automatically inspects `plan.json`, recognizes that step 1 is finished, and resumes directly from step 2 without re-spending API tokens.
3. **Hardened API Backoff**:
   Wrapped all Groq calls in an exponential backoff decorator that catches HTTP 429 rate limits and falls back across models (`llama-3.3-70b-versatile` $\rightarrow$ `llama-3.1-8b-instant`).
4. **Context Window Guardrails**:
   Webpage body text is scraped and truncated to ~3,000 characters to prevent context-window explosion and keep token costs minimal.
5. **Integrated Tool Suite**:
   * `search_the_web(query)`: Live DuckDuckGo search via Playwright.
   * `open_page(url)`: Deep article reader.
   * `get_stock_price(ticker)`: Real-time price and exchange metrics from Yahoo Finance.
   * `write_report_file(filename, content)`: Saves finalized dossiers to disk.

---

## 🛠️ Quickstart & Execution

### 1. Installation
Clone the repository and install the dependencies:
```bash
git clone https://github.com/SAKETVISPUTE/Cognition-Loop_-SOC.git
cd Cognition-Loop_-SOC

pip install -r requirements.txt
playwright install chromium
```

### 2. Configure Environment
Copy `.env.example` to `.env` and fill in your keys:
```bash
cp .env.example .env
```
Edit `.env`:
```env
GEMINI_API_KEY=your_gemini_api_key
GROQ_API_KEY=your_groq_api_key
```

### 3. Running the Agents

* **Run the Final Capstone Planner Agent:**
  ```bash
  python capstone.py
  ```
  *Enter a target like `Analyze Salesforce (CRM) financials and AI roadmap`.*

* **Run Week 4 Persistent Market Analyst (Alpha):**
  ```bash
  python Week4/my_assistant.py
  ```

* **Run Week 3 Interactive Chat Agent with Memory:**
  ```bash
  python Week3/chat_agent.py
  ```

---

## 🎓 Weekly Learning Milestones

* **Week 1 (Infrastructure & Control):** Mastered programmatic API calls, error backoff retry routines, system instructions, and schema-constrained JSON extraction using Google GenAI SDK.
* **Week 2 (Tool Use & Browser Automation):** Connected Python functions to Groq's LPU inference (`llama-3.3-70b`), executed Playwright web scraping, and automated YouTube video interactions.
* **Week 3 (The ReAct Loop):** Built the ReAct framework from scratch in native Python without frameworks. Implemented two-tier nested loops separating conversational memory from inner reasoning cycles.
* **Week 4 (Memory & Persistence):** Created long-term memory systems (`memory.json`, `goals.json`) to persist facts and multi-day research objectives across CLI restarts.
* **Capstone (Autonomous Orchestration):** Engineered a fully autonomous, fault-tolerant, disk-state-driven market research agent that plans, researches, fact-checks, and writes comprehensive equity reports.
