# 🎙️ Local-First Voice AI Agent

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=flat-square&logo=python)](https://www.python.org/)
[![Gradio](https://img.shields.io/badge/UI-Gradio-orange?style=flat-square&logo=gradio)](https://gradio.app/)
[![Ollama](https://img.shields.io/badge/LLM-Ollama-white?style=flat-square&logo=ollama)](https://ollama.ai/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=flat-square)](https://opensource.org/licenses/MIT)

A privacy-preserving, local-first AI voice agent capable of understanding and executing complex, compound commands. Unlike standard chatbots, this agent can simultaneously write software, manage local files, generate architectural summaries, and hold a conversation—all from a single voice prompt.

Built with a strong focus on data sovereignty, this system ensures that your voice data and code generation never leave your controlled environment.

---

## 🚀 Features

* **Compound Command Orchestration:** Ask the agent to do three completely different things at once (e.g., *"Create a Java banking system, summarize it, and tell me a joke"*), and it will route them perfectly.
* **Lightning-Fast STT:** Uses `faster-whisper` running on CUDA for sub-second, highly accurate audio transcription.
* **Decoupled Architecture:** Bypasses the common "JSON Crash" hallucination found in local LLMs by separating the Intent Planner from the Code Generator.
* **Dynamic Context Injection:** Utilizes a custom `CODE_CONTEXT` flag to share memory between independent LLM tool calls.
* **100% Local Under the Hood:** No proprietary cloud APIs (OpenAI, Anthropic, etc.). Powered by **Llama 3.1 (8B)** via Ollama.

---

## 📸 Demo

[Video Demonstration](https://youtu.be/pUsEdgVNIR8)
*The agent successfully parsing a compound command into a strict JSON intent array and executing the tools.*

---

## 🧠 System Architecture

This project is divided into three distinct layers:

1. **The Ears (STT):** `faster-whisper` intercepts microphone audio and converts it to text, forcing English translation to prevent phonetic hallucinations.
2. **The Brain (Intent Planner):** Llama 3.1 analyzes the text and maps it to a strict JSON schema. It extracts multiple distinct intents from a single sentence.
3. **The Hands (Tool Execution):** A Python execution loop that triggers specialized functions based on the parsed JSON (e.g., `generate_code()`, `safe_create_file()`, `summarize_text()`).

### Overcoming JSON Hallucinations
A major engineering challenge in this project was forcing an LLM to output massive code blocks inside strict JSON arrays, which often resulted in syntax crashes or lazy pseudo-code. 

This was solved by **decoupling**. The Intent Parser only generates the *instructions* (e.g., `{"intent": "write_code", "instructions": "simulate a banking system"}`). A secondary, unconstrained LLM call then generates the actual raw code, completely bypassing JSON structural limits.

---

## 🛠️ Installation & Setup

### Prerequisites
* Python 3.8+
* [Ollama](https://ollama.com/) installed on your machine or Colab instance.
* NVIDIA GPU (Recommended for STT speed, though CPU fallback is possible).


### 1. Clone the Repository
```bash
git clone [https://github.com/](https://github.com/)[YOUR_USERNAME]/local-voice-agent.git
cd local-voice-agent
````

### 2\. Install Dependencies

```bash
pip install gradio requests faster-whisper
```

### 3\. Pull the Local LLM

Ensure Ollama is running, then pull the Llama 3.1 model:

```bash
ollama pull llama3.1
```

### 4\. Run the Agent

```bash
python agent.py
```

The Gradio UI will launch automatically in your browser at `http://127.0.0.1:7860`.

-----

## 💻 Usage Example

Press the record button in the UI and speak a compound command:

> *"Create a Python file called fibonacci.py that prints the first 10 numbers, summarize how the code works in detail, and tell me a joke about programmers."*

The agent will:

1.  Safely generate and save `fibonacci.py` to the `/agent_output` directory.
2.  Pass the generated code to the summarization tool to explain the logic.
3.  Print the summary and the joke directly into the chat interface.

-----

## 📝 Read the Full Technical Write-up

For a deep dive into how I solved the token cut-off issues, engineered the `CODE_CONTEXT` flag, and optimized this pipeline to run effectively even on lower-end hardware, check out my full article:

👉 **[Read the article on Medium here](https://medium.com/@mohdfahamb/how-i-built-a-multi-intent-voice-agent-and-stopped-my-llm-from-crashing-778ec6e122d7)**

-----

## 👨‍💻 Author

**Faham**

  * Undergraduate Software Engineer
  * Passionate about Full-Stack Development, Artificial Intelligence, and Cybersecurity.
  * [LinkedIn](https://www.linkedin.com/in/mohammed-faham-956116318)

