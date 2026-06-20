# Git-Integrated Interactive Generative Interface
 
## Description
Gigi is a smart coding assistant that runs right inside your terminal. It acts like a helper who sits next to you while you code, helping you understand, write, edit, and review your files. Under the hood, it is built with Rust, making it super fast, lightweight, and easy to run anywhere. It connects to powerful AI brains (both in the cloud and running locally on your own computer) and runs a loop where it can think, execute commands, read files, and search information to solve coding tasks for you.

---

## Installation

You can install Gigi directly from [Crates.io](https://crates.io/crates/gigi-cli) using Cargo:

```bash
cargo install gigi-cli
```

*Ensure that your Cargo binary directory (typically `~/.cargo/bin`) is in your system's `PATH` environment variable.*

## Quick Start

To start a new interactive coding session in your current directory:

```bash
gigi
```

Inside the interactive REPL, you can ask questions, ask Gigi to edit code, or use slash commands such as:
* `/help` — Display help and list all commands.
* `/commit` — Generate a commit message and perform git commits.
* `/jobs` — Monitor, view logs of, or terminate background processes.
* `/exit` — Exit the REPL.

---

## How It Works
Gigi works as a continuous conversation loop between you and an AI helper:
1. **Starting Up**: When you launch Gigi, it checks if you have configured it before. If it is your first time, it guides you through an onboarding wizard to set up your AI connections.
2. **Context Gathering**: Before Gigi sends your messages to the AI, it automatically gathers information about your computer and project. It detects your operating system, active files, Git repository status, recent commits, and any custom guidelines you have written in your project folders. It bundles all of this up as context.
3. **The Thinking Loop**: When you ask a question or give a task, Gigi sends the request and context to the AI. If the AI needs to do something to answer you (like reading a file or running a test), it decides to use a "tool".
4. **Tool Execution**: Gigi intercepts the AI's request to use a tool, runs the action safely on your machine (making sure it doesn't escape your project folder), and sends the results back to the AI.
5. **Finishing the Turn**: The AI looks at the tool results, decides if it needs to run more tools, or gives you its final response. 

---

## Features and Capabilities
- **Onboarding Wizard**: A simple terminal configuration flow that helps you set up API keys and server endpoints on your first run.
- **Smart Directory Boundaries**: Safety guards that prevent Gigi from reading or editing files outside your active project directory.
- **Interactive Recovery Options**: If your internet drops or an API fails, Gigi displays a menu allowing you to retry the request, switch to a local offline model, switch to another cloud provider, or save your session progress and exit.
- **Rate-Limiting Protection**: Gigi detects rate limit errors from AI providers and automatically pauses and retries with a backoff delay, while also throttling rapid consecutive requests to prevent hitting limits in the first place.
- **Workspace Rules Scanning**: Automatically crawls up your directory structure to find project guidelines and rules, ensuring Gigi follows your exact preferences.
- **Git Integration and Commands**: Provides commands that automatically create commit messages by analyzing git diffs, prompt for confirmation, and execute the actual git commit directly. It also provides commands to review code changes and manage persistent memory notes.
- **Background Task Tracker**: Supports running shell commands in the background without orphaning them. It logs background output to task files and provides a `/jobs` slash command to inspect logs, view status, and kill active background tasks.
- **Integrated Search & Cognitive Harness (Agentic RAG)**: Includes both web search fetching and connection to a custom local document search engine. Gigi enforces a strict cognitive search harness in its system prompt instructions: it is guided to analyze task context, decide which search/retrieval tools to run (such as file globbing, text grepping, or document query search), execute those tools, and only then formulate a plan to edit or write files based on real facts rather than speculative assumptions.

---

## Comparison with Claude Code

Here is a checklist comparison between Claude Code and Gigi:

| Feature / Parameter | Claude Code | Gigi |
| :--- | :--- | :--- |
| **Core Programming Language** | TypeScript / Node.js | Rust (much faster startup, single binary) |
| **Model Providers Supported** | Anthropic Claude only | Anthropic Claude, Groq, Google Gemini, Ollama, LM Studio, llama.cpp, and Custom OpenAI-compatible endpoints |
| **Offline / Local Execution** | No (requires active internet and paid Anthropic API keys) | Yes (can run fully offline using local runtimes like Ollama) |
| **Interactive Configuration** | Yes (auth login prompt) | Yes (onboarding wizard for cloud keys and local models) |
| **Session Resuming** | Yes | Yes (automatically detects your last session and prompts you to resume) |
| **Connection Failures** | Fails immediately on network disconnect | Interactive menu to retry, switch models, or save and quit |
| **System Prompt Builder** | Deeply layered (git context, environment, custom rules) | Direct parity (builds git status, commits, OS info, and rule file scanning) |
| **Memory Management** | Persistent notes system | Persistent notes saved to JSON files |
| **Rate Limit Handling** | Basic retry logic | Auto-retry with backoff and client-side query throttling |
| **File Access Control** | Interactive prompts for execution | Hard directory boundary verification checks |
| **Terminal Tool (`bash`)** | Runs commands with timeout | Runs commands with timeout and full background task tracking (`/jobs`) |
| **Git Committing (`/commit`)** | Generates commit message and commits | Generates commit message, prompts for confirmation, and commits |
| **File Reading Tool (`read_file`)** | Reads text files with line windows | Reads text files with line windows |
| **File Writing Tool (`write_file`)** | Writes new files and edits | Writes new files and edits |
| **File Editing Tool (`edit_file`)** | Precise search-and-replace | Precise search-and-replace |
| **Glob File Search Tool (`glob_search`)** | File pattern matching | File pattern matching |
| **Grep Text Search Tool (`grep_search`)** | Substring content matching | Substring content matching |
| **Web Search Tool (`web_search`)** | Web queries | Web queries via DuckDuckGo (no API keys needed) |
| **Web Page Fetching Tool (`web_fetch`)** | Scrapes URL content | Scrapes URL content |
| **Semantic Documentation Tool (`tech_query`)** | Not available | Custom PyTorch document search engine connector |
