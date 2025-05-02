

# 💬 Emotion-Aware Chat App

A full-stack emotion-aware chatbot using:

* 🤖 **Ollama** (local LLMs like LLaMA2 or Mistral) for chatbot responses
* 🧠 **Hugging Face** emotion model (`roberta-base-go_emotions`)
* 🌐 A modern **responsive web UI** with charts, offline chat history, debug tools, and emotion tracking

---

## 📦 Features

✅ Local AI chatbot using LLMs
✅ Real-time emotion detection & empathy-based response
✅ Responsive chat UI with mood tracking (Chart.js)
✅ Smart fallback to secondary model if primary fails
✅ Debug mode & error handling
✅ Works **fully offline** after initial setup

---

## 🧰 Tech Stack

* Python 3 + Flask (APIs)
* Hugging Face Transformers
* Ollama for LLMs
* HTML + CSS + JS (Frontend)
* Chart.js for mood tracking

---

## 📁 Files Included

| File               | Purpose                                    |
| ------------------ | ------------------------------------------ |
| `app02.py`         | Emotion Detection API (Hugging Face model) |
| `ollama.py`        | Chat Completion API (Ollama LLMs)          |
| `page.html`        | Frontend UI (open in browser)              |
| `.env`             | Ollama config (you'll create this)         |
| `requirements.txt` | Python dependencies (you'll create this)   |

---

## 🚀 Step-by-Step Setup

### ✅ 1. Install Ollama

Download and install Ollama for your system from [https://ollama.com](https://ollama.com):

```bash
# On macOS
brew install ollama

# On Ubuntu / Debian
curl -fsSL https://ollama.com/install.sh | sh
```

Start the Ollama server:

```bash
ollama serve
```

---

### 🤖 2. Pull Required LLMs

Pull two models:

* Primary: `llama2`
* Fallback: `mistral`

```bash
ollama pull llama2
ollama pull mistral
```

Test if they're working:

```bash
ollama run llama2
```

---

### 📦 3. Clone the Repository

```bash
git clone https://github.com/your-username/emotion-aware-chat.git
cd emotion-aware-chat
```

---

### 🐍 4. Set Up Python Environment

```bash
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

Create `requirements.txt` file:

```txt
Flask
flask-cors
transformers
requests
python-dotenv
```

Then install:

```bash
pip install -r requirements.txt
```

---

### ⚙️ 5. Create `.env` File

Create a `.env` in the root:

```ini
# .env file for ollama.py
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

## 🔧 Run the Services

### 🧠 Emotion Detection API (Port 5002)

```bash
python app02.py
```

* Loads `roberta-base-go_emotions` from Hugging Face
* Detects top 3 emotions and gives a helpful response

---

### 💬 Chat API (Port 5003)

Make sure Ollama is running (`ollama serve`) and models are pulled.

```bash
python ollama.py
```

* Uses Ollama LLMs to generate responses
* Smart fallback between models

---

## 🌐 Open the Frontend

Just double-click or open `page.html` in your browser.

If using a mobile device or want LAN access:

```bash
# Optionally serve via Python:
python -m http.server 8080
# Visit http://localhost:8080/page.html
```

---

## 🖥️ Frontend Features

* Clean responsive chat interface
* Tracks mood/emotion trends with a live chart
* Offline history with `localStorage`
* Emojis for each detected emotion
* Debug panel to view raw API responses
* Auto-retry on API errors

---

## 🌈 API Summary

### POST `/detect-emotion` — Emotion Detection (Port 5002)

```json
{ "text": "I am feeling hopeful today" }
```

Returns:

```json
{
  "primary_emotion": "optimism",
  "primary_score": 0.97,
  "secondary_emotions": [...],
  "message": "I appreciate your positive outlook."
}
```

---

### POST `/chat` — Chatbot Response (Port 5003)

```json
{
  "text": "Tell me a fun fact!",
  "max_tokens": 150,
  "temperature": 0.7,
  "stream": false
}
```

Returns:

```json
{
  "model": "llama2",
  "response": "Did you know honey never spoils?"
}
```

---

### GET `/health` (on both APIs)

Check if services and models are ready:

```json
{
  "status": "ready",
  "model": "SamLowe/roberta-base-go_emotions",
  "timestamp": 1714661183.65
}
```

---

## 📸 Preview Screenshot

> *(Add screenshot here)*
> ![Preview](https://via.placeholder.com/900x400?text=Emotion-Aware+Chat+App)

---

## 🧠 Supported Emotions (Sample)

The model detects 20+ emotions, such as:

* joy, sadness, fear, anger
* curiosity, gratitude, love
* confusion, optimism, embarrassment
* surprise, grief, desire, admiration, etc.

---

## 📌 Troubleshooting

| Problem                        | Solution                                                             |
| ------------------------------ | -------------------------------------------------------------------- |
| `Model is still loading...`    | Wait 5–15 seconds after starting Flask apps                          |
| `Ollama server not responding` | Run `ollama serve` and ensure port 11434 is free                     |
| Model not found                | Run `ollama pull llama2` or `mistral` manually                       |
| Frontend can’t connect         | Make sure both APIs are running (`http://localhost:5002` and `5003`) |

---

## 🙏 Acknowledgements

* 🤗 [Hugging Face](https://huggingface.co/SamLowe/roberta-base-go_emotions) for the emotion model
* 🦙 [Ollama](https://ollama.com) for local LLM deployment
* 📊 [Chart.js](https://www.chartjs.org/) for visualization

---

project by manoj L
