
# 💬 Emotion-Aware Chatbot (Offline, Local AI)

A full-featured offline **Emotion-Aware Chatbot** built with:

* 🔥 Local LLMs via **Ollama** (LLaMA 2 / Mistral)
* 🧠 Emotion detection using Hugging Face's `roberta-base-go_emotions`
* 🖥️ Beautiful, responsive **frontend** with mood tracking, emoji stats, and debugging

This project allows users to **chat in real-time**, see how they emotionally feel over time, and get **empathetic responses** — all **fully offline**, privacy-respecting, and customizable.

---

## 🔥 Why This Workflow Is Great

✅ **100% offline-compatible**: No need for OpenAI API or cloud services
✅ **Emotion + Language split**: Best-practice microservice design with modular APIs
✅ **LLM fallback handling**: Primary/fallback model switching for resiliency
✅ **Cached responses**: Faster interactions and reduced latency
✅ **Frontend is clean, intuitive, and mobile-ready**
✅ **Chart.js integration**: Provides mood trend over time
✅ **Debug panel**: Makes it easy to track API communication during development

This architecture is scalable and extensible: you can plug in sentiment-aware features, mood journaling, or personalized wellness interventions with ease.

---

## 📁 Repository Structure

| File / Folder      | Description                                                      |
| ------------------ | ---------------------------------------------------------------- |
| `app02.py`         | Flask API for emotion detection using Hugging Face               |
| `ollama.py`        | Flask API for chat completion using Ollama models                |
| `page.html`        | Frontend UI with integrated Chart.js, emotion tracking, and chat |
| `.env`             | Config file for model behavior (you create this)                 |
| `requirements.txt` | Python dependencies (you create this)                            |

---

## 🚀 Full Setup Guide

### 🧱 Prerequisites

* ✅ Python 3.7+
* ✅ Git
* ✅ [Ollama](https://ollama.com) installed locally
* ✅ `llama2` and `mistral` pulled locally using `ollama pull`

---

### 1. Clone the Repository

```bash
git clone https://github.com/manojgit9945/hackthon-emotion-aware-chat-bot.git
cd hackthon-emotion-aware-chat-bot
```

---

### 2. Install & Launch Ollama

#### Install Ollama:

* macOS:

  ```bash
  brew install ollama
  ```
* Linux:

  ```bash
  curl -fsSL https://ollama.com/install.sh | sh
  ```

#### Pull models:

```bash
ollama pull llama2
ollama pull mistral
```

#### Start Ollama server:

```bash
ollama serve
```

Test it:

```bash
ollama run llama2
```

---

### 3. Set Up Python Backend

Create a virtual environment and activate it:

```bash
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
```

Create `requirements.txt`:

```txt
Flask
flask-cors
transformers
requests
python-dotenv
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

### 4. Create `.env` File

Create a `.env` in the root directory:

```ini
# Ollama model config
PRIMARY_MODEL=llama2
FALLBACK_MODEL=mistral
OLLAMA_API_BASE=http://localhost:11434
DEFAULT_MAX_TOKENS=512
DEFAULT_TEMPERATURE=0.7
RESPONSE_TIMEOUT=10
ENABLE_CACHE=True
CACHE_SIZE=100
PORT=5003
```

---

### 5. Run the Backends

#### Emotion Detection API:

```bash
python app02.py
# → Runs on http://localhost:5002
```

#### Ollama Chat API:

```bash
python ollama.py
# → Runs on http://localhost:5003
```

---

## 🌐 Run the Frontend

Simply open `page.html` in your browser.

📱 To run it on phone or LAN:

```bash
python -m http.server 8080
# Then access http://<your-ip>:8080/page.html
```

---

## 🔍 API Details

### 📮 `POST /chat`

(Local LLM chat)

```json
{
  "text": "Tell me a joke",
  "max_tokens": 150,
  "temperature": 0.7,
  "stream": false
}
```

Returns:

```json
{
  "model": "llama2",
  "response": "Why did the AI cross the road? Because it learned from millions of chickens!"
}
```

---

### 📮 `POST /detect-emotion`

(Hugging Face Emotion Classifier)

```json
{ "text": "I am so happy and thankful today!" }
```

Returns:

```json
{
  "primary_emotion": "gratitude",
  "primary_score": 0.95,
  "secondary_emotions": [...],
  "message": "Your gratitude is heartwarming."
}
```

---

### ✅ `GET /health`

Returns model loading or readiness info.

```json
{
  "status": "ready",
  "service": "Emotion Detection",
  "model": "roberta-base-go_emotions"
}
```

---

## 🧠 Emotions Supported

The model detects 25+ nuanced emotions:

* joy, sadness, anger, grief, surprise, fear
* curiosity, optimism, desire, admiration, confusion, pride
* ... and more!

---

## 📊 Frontend Features

| Feature                      | Description                                |
| ---------------------------- | ------------------------------------------ |
| 🎨 Emotion-aware messages    | Bot replies change tone based on emotion   |
| 📈 Live emotion trend chart  | Using Chart.js (with colored emotion dots) |
| 💾 Offline chat history      | Saved in localStorage                      |
| 🐛 Debug mode                | Toggle to view raw API responses           |
| 🎭 Emoji-based emotion stats | With color-coded emotion bars              |
| 🧠 Emotion labeling          | Each message shows inferred emotion        |

---

## 🧪 Developer Tips

* Use `debug=True` in both scripts during development.
* All frontend values (like API URLs) are easily configurable in `page.html`.
* You can even extend this to store logs in MongoDB or add login sessions.

---


---

## 🙏 Acknowledgments

* 🤗 Hugging Face — `roberta-base-go_emotions`
* 🦙 Ollama — For local LLM inference
* 📊 Chart.js — Emotion charting
* 💻 Built with love by [@manojgit9945](https://github.com/manojgit9945)

---

Here’s a **detailed explanation of the entire system architecture and workflow**, showing how **HTML, Python (Flask), Hugging Face, and Ollama** communicate together in your [repo](https://github.com/manojgit9945/hackthon-emotion-aware-chat-bot):

---

## 📡 High-Level Workflow Overview

```
User (browser) ↔️ Frontend (page.html)
        ↕
 Emotion API (app02.py, Hugging Face) ⬅️
        ↕
   Chat API (ollama.py) ↔️ Ollama (Local LLM)
```

---

## 🧠 Detailed Workflow Breakdown

### 1. **Frontend (`page.html`)**

* This is the user-facing chat interface.
* It's a single HTML file with:

  * A **message input box**
  * A **real-time chat window**
  * **Charts** showing emotion trends
  * A **debug panel** for raw API results

✅ It does **two jobs** for every user message:

#### A. Send message to **Chat API**

```js
fetch("http://localhost:5003/chat", { method: "POST", body: { text: "Hi" } })
```

#### B. Send same message to **Emotion API**

```js
fetch("http://localhost:5002/detect-emotion", { method: "POST", body: { text: "Hi" } })
```

#### Then:

* Displays bot’s reply from Chat API
* Detects & displays emotion and emoji
* Updates emotion chart and stats
* Stores everything in `localStorage`

---

### 2. **Chat API – `ollama.py` (Flask)**

✅ This is the Flask server that talks to your **local Ollama LLM models**.

#### Key Components:

* Loads env variables from `.env`
* Sends `/chat` requests to:

  ```
  http://localhost:11434/api/chat
  ```
* If `llama2` fails or times out → switches to `mistral`
* Can stream or send full replies
* Caches recent conversations (if `ENABLE_CACHE=True`)
* Has `/health` and `/models` endpoints

#### API Flow:

```text
Frontend  ──(POST /chat)──▶  ollama.py  ──▶ Ollama model (e.g., llama2)
```

✅ Formats the message as Ollama expects:

```json
{
  "model": "llama2",
  "messages": [{"role": "user", "content": "hello"}],
  "options": { "temperature": 0.7, "num_predict": 150 },
  "stream": false
}
```

✅ Response:

```json
{ "model": "llama2", "response": "Hello! How can I help you today?" }
```

---

### 3. **Emotion Detection API – `app02.py` (Flask + Hugging Face)**

✅ This server loads a **Hugging Face transformer pipeline** using:

```python
pipeline("text-classification", model="SamLowe/roberta-base-go_emotions")
```

#### What It Does:

* Receives a message like: `"I'm feeling hopeful today."`
* Classifies emotions like:

  ```json
  [
    {"label": "optimism", "score": 0.87},
    {"label": "joy", "score": 0.55},
    {"label": "gratitude", "score": 0.34}
  ]
  ```
* Responds with:

  * Primary emotion
  * Top 3 scores
  * Custom message for empathy

#### API Flow:

```text
Frontend  ──(POST /detect-emotion)──▶  app02.py  ──▶ Hugging Face pipeline
```

✅ Response:

```json
{
  "primary_emotion": "optimism",
  "message": "I appreciate your positive outlook.",
  "secondary_emotions": [...],
  "primary_score": 0.87
}
```

---

## 🔄 End-to-End Message Journey

Here’s what happens when a user types “I feel sad but hopeful” in the chat box:

1. **User types** in `page.html`
2. **Frontend JS** sends text to:

   * `http://localhost:5003/chat` ➝ Gets **AI reply**
   * `http://localhost:5002/detect-emotion` ➝ Gets **emotional analysis**
3. **Flask servers**:

   * `ollama.py` calls Ollama and returns a LLM-generated message
   * `app02.py` uses Hugging Face to return emotion data
4. **Frontend**:

   * Displays both chat and emotion
   * Adds emoji for detected emotion
   * Updates mood chart and stores history

---

## 🧩 How the Technologies Connect

| Layer            | Technology                        | Purpose                           |
| ---------------- | --------------------------------- | --------------------------------- |
| 💬 Chat LLM      | Ollama + Flask (`ollama.py`)      | Local LLM-based responses         |
| 🧠 Emotion AI    | Hugging Face + Flask (`app02.py`) | Emotion classification            |
| 🌐 Frontend UI   | HTML + CSS + JS (`page.html`)     | User interface, chart, logic      |
| 🔄 Communication | REST APIs (Fetch / POST)          | JS ↔ Flask ↔ Ollama ↔ HF Pipeline |

---

## 📌 TL;DR

* **Frontend** collects user input, sends it to **two Flask APIs**
* **Chat API** (`ollama.py`) generates smart responses using Ollama
* **Emotion API** (`app02.py`) analyzes emotions using a Hugging Face model
* **Frontend** combines the results, shows them beautifully, and tracks mood over time

Absolutely. Here's a **simple and crystal-clear explanation** of how your Emotion-Aware Chatbot project works — written so **anyone**, even without technical knowledge, can understand it:

---

## 🧠 What Is This Project?

This is a **chatbot that understands emotions**.

When you type a message like:

> “I’m feeling broken but I’m trying to stay strong.”

It will:

* 💬 **Reply** to you like a smart chatbot
* 🧠 **Detect how you’re feeling** (sad, proud, confused, etc.)
* 📈 **Show your emotional trend** over time with charts
* 😊 Use emojis and colors to represent your mood

And the best part? **It runs 100% offline on your computer.** No internet needed after setup.

---

## 🧩 How Does It Work? (Simple Breakdown)

This app is made up of **3 main parts**:

---

### 1️⃣ **The Web Page (`page.html`)**

This is what you see and interact with in your browser.

It:

* Lets you type messages
* Shows chatbot replies
* Detects your mood
* Displays charts and emojis

👉 But it can’t think by itself. So it sends your message to 2 helpers (APIs)...

---

### 2️⃣ **Emotion Detector (`app02.py`)**

This is like a psychologist.

It looks at what you wrote and figures out **how you feel** using a brainy tool from Hugging Face (called `roberta-base-go_emotions`).

For example:

> You type: “I feel alone.”

It says:

> “You’re feeling *sadness*. Do you want to talk about it?”

✅ It also returns secondary emotions like *grief*, *disappointment*, etc.

---

### 3️⃣ **Chatbot Brain (`ollama.py`)**

This is like your AI friend.

It uses **local AI models** (like LLaMA 2 or Mistral) to **respond** to your message in a smart and helpful way.

It runs on a tool called **Ollama**, which brings powerful AI to your computer — no internet or OpenAI key needed!

Example:

> You: “Tell me something interesting.”

It might say:

> “Did you know octopuses have three hearts?”

✅ It also has a backup brain (fallback model) in case the main one fails.

---

## 🔁 The Full Flow (in Human Language)

Here’s what happens when you chat:

1. You type a message in the browser.
2. The web page sends your message to:

   * The **Emotion Detector** → to find your feelings
   * The **Chatbot Brain** → to get a smart reply
3. Both helpers reply in seconds.
4. The web page:

   * Shows the bot’s answer
   * Shows your detected emotion (with emoji & color)
   * Updates a **chart** showing how your feelings change over time
   * Saves your conversation privately in your browser

---

## 💡 Example

Let’s say you typed:

> “I’m feeling broken but hopeful.”

### The app does:

| Step                | Tool Used                 | Result                                             |
| ------------------- | ------------------------- | -------------------------------------------------- |
| 🧠 Detects emotions | `app02.py` → Hugging Face | Primary: **Sadness**, Secondary: **Optimism**      |
| 💬 Responds to you  | `ollama.py` → LLaMA 2     | “I’m here for you. It’s brave to hold on to hope.” |
| 🎨 Visualizes mood  | `page.html` + Chart.js    | Shows sadness in blue, hope in green               |
| 💾 Saves it         | `localStorage`            | Keeps your chat history for later                  |

---

## 🔧 How All the Pieces Talk to Each Other

Think of it like a team:

```
You 🧍
 ↳ sends message to →
     🌐 Frontend (page.html)
         ↳ asks →
            🧠 Emotion API (app02.py using Hugging Face)
            💬 Chat API (ollama.py using Ollama LLMs)
         ↳ gets replies back
         ↳ shows in browser
```

---

## ✅ Why This Project Is Awesome

* 🔒 100% Private — runs completely on your computer
* 💬 Empathetic — understands how you feel
* 🧠 Smart — powered by modern LLMs
* 📈 Insightful — lets you track your mood over time
* ⚡ Fast & Offline — no waiting or loading issues
* 🧰 Fully Customizable — you can change themes, models, etc.

## 📌 Final Thoughts

This workflow is **well-structured, modular, scalable, and privacy-first** — ideal for:

* Emotion-aware assistants
* Therapy/chat journaling tools
* AI companions
* Hackathon projects

It’s production-ready with just a few additions like database logging or session management. 🔥

---

Would you like me to export this as a `README.md` file ready to upload to your repo?

