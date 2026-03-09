# Module 7: Free AI APIs & Where to Get Them

## Overview

You don't need to pay hundreds of dollars to build with AI. In 2026, there are numerous free API providers offering access to state-of-the-art models with generous rate limits. This module shows you exactly where to get free API access, how to set it up, and includes working code examples for every provider.

**By the end of this module, you will:**
- Have free API access to 10+ AI providers
- Run working code for text, image, audio, and embedding APIs
- Build a multi-provider fallback system for reliability
- Understand rate limits and how to stay within free tiers
- Know how to use OpenRouter as a universal API gateway

---

## 1. Free API Provider Overview

| Provider | Free Tier | Models Available | Rate Limit | Credit Card Required? |
|----------|----------|-----------------|------------|----------------------|
| **Google AI Studio** | Generous free tier | Gemini 3 Flash/Pro | 15 RPM (Flash), 2 RPM (Pro) | No |
| **Groq** | Free tier | Llama 4, DeepSeek, Gemma 3 | 30 RPM, 15K tokens/min | No |
| **Hugging Face** | Free inference API | 200K+ open models | Varies by model | No |
| **OpenRouter** | Free models available | Selected open models | Varies | No (for free models) |
| **Cerebras** | Free tier | Llama 4, DeepSeek R1 | Rate-limited | No |
| **DeepSeek** | Free credits | DeepSeek V3.2, R1 | Generous | No |
| **Mistral** | Free tier (La Plateforme) | Mistral Small, open models | 1 RPM free | No |
| **Cohere** | Free trial API | Command R+ | 20 RPM, 1000 calls/mo | No |
| **Together AI** | $5 free credits | 200+ open models | 60 RPM | Yes (for credits) |
| **Fireworks AI** | Free tier | Popular open models | Rate-limited | No |
| **Cloudflare Workers AI** | Free tier | Popular open models | 10K tokens/day | No |
| **Ollama (local)** | Unlimited free | Any open model | Your hardware | No |

---

## 2. Google AI Studio -- Best Free API

### 2.1 Getting Your Key

```
1. Go to aistudio.google.com
2. Sign in with Google account
3. Click "Get API Key" in the left sidebar
4. Click "Create API Key"
5. Copy your key -- that's it! No credit card, no approval.
```

### 2.2 Free Tier Limits (March 2026)

```
Gemini 3 Flash:
  - 15 requests per minute (RPM)
  - 1 million tokens per minute (TPM)
  - 1,500 requests per day (RPD)
  - Context: 1M tokens
  - FREE

Gemini 3 Pro:
  - 2 requests per minute
  - 32K tokens per minute
  - 50 requests per day
  - Context: 2M tokens
  - FREE
```

### 2.3 Python Code Example

```python
import google.generativeai as genai
import os

# Setup
genai.configure(api_key=os.environ.get("GOOGLE_AI_KEY"))

# --- Text Generation ---
model = genai.GenerativeModel('gemini-3-flash')
response = model.generate_content("Explain SQL injection in 3 sentences")
print(response.text)

# --- With System Prompt ---
model = genai.GenerativeModel(
    'gemini-3-flash',
    system_instruction="You are a cybersecurity expert. Be concise and technical."
)
response = model.generate_content("What are the top 5 web vulnerabilities in 2026?")
print(response.text)

# --- Multi-turn Chat ---
chat = model.start_chat()
response = chat.send_message("What is XSS?")
print(response.text)
response = chat.send_message("Show me a prevention example in Python")
print(response.text)

# --- Multimodal (Image Analysis) ---
from PIL import Image
image = Image.open("network_diagram.png")
response = model.generate_content([
    "Analyze this network diagram for security vulnerabilities",
    image
])
print(response.text)

# --- Streaming ---
response = model.generate_content("Write a Python port scanner", stream=True)
for chunk in response:
    print(chunk.text, end="", flush=True)

# --- JSON Mode ---
import json
model_json = genai.GenerativeModel(
    'gemini-3-flash',
    generation_config={"response_mime_type": "application/json"}
)
response = model_json.generate_content(
    "List the OWASP Top 10 as JSON: [{name, description, severity}]"
)
data = json.loads(response.text)
print(json.dumps(data, indent=2))
```

### 2.4 JavaScript/Node.js Example

```javascript
import { GoogleGenerativeAI } from "@google/generative-ai";

const genAI = new GoogleGenerativeAI(process.env.GOOGLE_AI_KEY);
const model = genAI.getGenerativeModel({ model: "gemini-3-flash" });

// Simple generation
const result = await model.generateContent("Explain buffer overflows");
console.log(result.response.text());

// Streaming
const streamResult = await model.generateContentStream("Write a REST API in Express");
for await (const chunk of streamResult.stream) {
    process.stdout.write(chunk.text());
}
```

---

## 3. Groq -- Fastest Free Inference

### 3.1 Getting Your Key

```
1. Go to console.groq.com
2. Sign up (Google/GitHub auth)
3. Go to API Keys section
4. Create new key
5. Copy -- no credit card needed!
```

### 3.2 Free Tier Limits

```
Groq runs on custom LPU chips -- 10x faster than GPU inference:

Llama 4 Scout (17B active):
  - 30 RPM, 15,000 tokens/min, 14,400 RPD
  
DeepSeek R1 (distilled):
  - 30 RPM, 15,000 tokens/min

Gemma 3 27B:
  - 30 RPM, 15,000 tokens/min

Llama 3.3 70B:
  - 30 RPM, 6,000 tokens/min

Speed: 500-1000+ tokens/second (vs ~50-100 on typical GPU)
```

### 3.3 Python Code Example

```python
from groq import Groq
import os

client = Groq(api_key=os.environ.get("GROQ_API_KEY"))

# Basic completion (OpenAI-compatible API)
response = client.chat.completions.create(
    model="llama-4-scout-17b-16e-instruct",
    messages=[
        {"role": "system", "content": "You are a helpful security assistant."},
        {"role": "user", "content": "Write a Python script to check for open ports"}
    ],
    temperature=0.3,
    max_tokens=2048
)
print(response.choices[0].message.content)

# Streaming
stream = client.chat.completions.create(
    model="llama-4-scout-17b-16e-instruct",
    messages=[{"role": "user", "content": "Explain JWT token attacks"}],
    stream=True
)
for chunk in stream:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="")

# Using Groq with OpenAI SDK (drop-in replacement)
from openai import OpenAI

client = OpenAI(
    api_key=os.environ.get("GROQ_API_KEY"),
    base_url="https://api.groq.com/openai/v1"
)

response = client.chat.completions.create(
    model="llama-4-scout-17b-16e-instruct",
    messages=[{"role": "user", "content": "Hello from Groq!"}]
)
print(response.choices[0].message.content)
```

---

## 4. Hugging Face -- Largest Open Model Hub

### 4.1 Getting Your Token

```
1. Go to huggingface.co
2. Sign up (free)
3. Settings > Access Tokens
4. Create new token (Read access is enough for inference)
```

### 4.2 Free Inference API

```python
import requests
import os

API_URL = "https://api-inference.huggingface.co/models/"
HEADERS = {"Authorization": f"Bearer {os.environ.get('HF_TOKEN')}"}

# --- Text Generation (any model on HF) ---
def query_model(model_id, prompt):
    response = requests.post(
        f"{API_URL}{model_id}",
        headers=HEADERS,
        json={"inputs": prompt}
    )
    return response.json()

# Use DeepSeek Coder
result = query_model(
    "deepseek-ai/deepseek-coder-33b-instruct",
    "Write a Python function to detect SQL injection patterns"
)
print(result[0]["generated_text"])

# --- Embeddings (free, great for RAG) ---
result = query_model(
    "sentence-transformers/all-MiniLM-L6-v2",
    "What is cross-site scripting?"
)
# Returns 384-dimensional embedding vector

# --- Image Classification ---
import base64
with open("screenshot.png", "rb") as f:
    data = f.read()
result = requests.post(
    f"{API_URL}google/vit-base-patch16-224",
    headers=HEADERS,
    data=data
).json()
print(result)  # [{"label": "web page", "score": 0.95}]

# --- Hugging Face Hub (download models for local use) ---
from huggingface_hub import snapshot_download

# Download a model for local inference
snapshot_download(
    repo_id="microsoft/phi-4",
    local_dir="./phi-4"
)
```

### 4.3 Hugging Face Spaces (Free GPU)

```
Hugging Face Spaces gives you free GPU to host AI apps:

1. Create a new Space (huggingface.co/new-space)
2. Choose Gradio or Streamlit template
3. Upload your code
4. Get a free hosted URL with GPU inference

Great for:
- Hosting demos and prototypes
- Running models without local GPU
- Sharing AI tools with others
```

---

## 5. OpenRouter -- Universal API Gateway

### 5.1 What Is OpenRouter?

OpenRouter is a unified API that gives you access to 200+ models from all providers through a single API endpoint. Some models are completely free.

### 5.2 Getting Started

```
1. Go to openrouter.ai
2. Sign up
3. Go to Keys > Create Key
4. Free models require no payment
5. For paid models, add credits ($5 minimum)
```

### 5.3 Free Models on OpenRouter

```
Free (as of March 2026):
- google/gemini-3-flash (free tier)
- meta-llama/llama-4-scout (free tier)
- deepseek/deepseek-v3.2 (free tier)
- mistral/mistral-small (free tier)
- google/gemma-3-27b (free tier)

Rate limits apply but are generous for development.
```

### 5.4 Code Example

```python
from openai import OpenAI
import os

# OpenRouter uses OpenAI-compatible API!
client = OpenAI(
    api_key=os.environ.get("OPENROUTER_API_KEY"),
    base_url="https://openrouter.ai/api/v1"
)

# Use any model with the same code
response = client.chat.completions.create(
    model="google/gemini-3-flash",  # Free!
    messages=[
        {"role": "system", "content": "You are a security analyst."},
        {"role": "user", "content": "Analyze this HTTP header for security issues: X-Powered-By: PHP/7.4"}
    ]
)
print(response.choices[0].message.content)

# Switch models by changing one string
models = [
    "google/gemini-3-flash",       # Free
    "meta-llama/llama-4-scout",    # Free
    "deepseek/deepseek-v3.2",      # Free
    "anthropic/claude-sonnet-4.6", # Paid ($1.50/M input)
    "openai/gpt-5.4",              # Paid ($3/M input)
]

for model in models:
    try:
        response = client.chat.completions.create(
            model=model,
            messages=[{"role": "user", "content": "Say hello in one sentence"}],
            max_tokens=50
        )
        print(f"{model}: {response.choices[0].message.content}")
    except Exception as e:
        print(f"{model}: Error - {e}")
```

---

## 6. Cerebras -- Ultra-Fast Free Inference

### 6.1 Setup

```
1. Go to cloud.cerebras.ai
2. Sign up
3. Create API key
4. Powers the fastest inference available (1000+ tok/s)
```

### 6.2 Code Example

```python
from openai import OpenAI
import os

# Cerebras uses OpenAI-compatible API
client = OpenAI(
    api_key=os.environ.get("CEREBRAS_API_KEY"),
    base_url="https://api.cerebras.ai/v1"
)

response = client.chat.completions.create(
    model="llama-4-scout-17b",
    messages=[
        {"role": "system", "content": "You are a fast coding assistant."},
        {"role": "user", "content": "Write a Python async web scraper using httpx"}
    ],
    max_tokens=2048
)
print(response.choices[0].message.content)
# Response arrives in < 1 second for most queries!
```

---

## 7. DeepSeek -- Best Free Chinese AI

### 7.1 Setup

```
1. Go to platform.deepseek.com
2. Sign up
3. Get free API credits (usually $5+ on signup)
4. Create API key
```

### 7.2 Code Example

```python
from openai import OpenAI
import os

client = OpenAI(
    api_key=os.environ.get("DEEPSEEK_API_KEY"),
    base_url="https://api.deepseek.com"
)

# DeepSeek V3.2 (general purpose)
response = client.chat.completions.create(
    model="deepseek-chat",
    messages=[
        {"role": "system", "content": "You are a security researcher."},
        {"role": "user", "content": "Explain how DNS tunneling works and how to detect it"}
    ],
    temperature=0.3
)
print(response.choices[0].message.content)

# DeepSeek R1 (reasoning model)
response = client.chat.completions.create(
    model="deepseek-reasoner",
    messages=[
        {"role": "user", "content": "Solve: A server has 3 vulnerabilities. Each can be "
         "exploited independently with probability 0.3, 0.5, and 0.7. "
         "What's the probability at least one is exploited?"}
    ]
)
# R1 shows chain-of-thought reasoning
print(response.choices[0].message.content)
```

---

## 8. Mistral -- European AI API

### 8.1 Setup

```
1. Go to console.mistral.ai
2. Sign up
3. Create API key (La Plateforme)
4. Free tier: 1 RPM on select models
```

### 8.2 Code Example

```python
from mistralai import Mistral
import os

client = Mistral(api_key=os.environ.get("MISTRAL_API_KEY"))

response = client.chat.complete(
    model="mistral-small-latest",
    messages=[
        {"role": "system", "content": "You are a network security expert."},
        {"role": "user", "content": "Write Wireshark display filters for detecting common attacks"}
    ]
)
print(response.choices[0].message.content)

# Codestral (coding model)
response = client.chat.complete(
    model="codestral-latest",
    messages=[
        {"role": "user", "content": "Write a Python SOCKS5 proxy server"}
    ]
)
print(response.choices[0].message.content)
```

---

## 9. Cohere -- Enterprise-Grade Free Tier

### 9.1 Setup

```
1. Go to dashboard.cohere.com
2. Sign up
3. Get trial API key (no credit card)
4. 20 RPM, 1000 calls/month free
```

### 9.2 Code Example

```python
import cohere
import os

co = cohere.ClientV2(api_key=os.environ.get("COHERE_API_KEY"))

# Chat (Command R+)
response = co.chat(
    model="command-r-plus-08-2024",
    messages=[
        {"role": "user", "content": "Explain zero-trust architecture"}
    ]
)
print(response.message.content[0].text)

# Embeddings (excellent for RAG -- free!)
response = co.embed(
    texts=["SQL injection", "Cross-site scripting", "Buffer overflow"],
    model="embed-english-v3.0",
    input_type="search_document",
    embedding_types=["float"]
)
# Returns high-quality embeddings for semantic search

# Rerank (improve search results -- free!)
response = co.rerank(
    model="rerank-english-v3.0",
    query="How to prevent SQL injection",
    documents=[
        "Use parameterized queries to prevent SQL injection.",
        "SQL databases store data in tables.",
        "Input validation helps prevent injection attacks.",
        "SQL stands for Structured Query Language."
    ],
    top_n=2
)
for result in response.results:
    print(f"Score: {result.relevance_score:.3f} | {result.document.text}")
```

---

## 10. Ollama -- Unlimited Free Local AI

### 10.1 Setup

```bash
# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Pull models
ollama pull llama4-scout          # 17B active params
ollama pull deepseek-v3.2:14b     # Quantized for consumer GPU
ollama pull qwen3.5:7b            # Small, fast
ollama pull phi-4                 # Microsoft's best small model
ollama pull codestral             # Coding model

# Chat directly
ollama run deepseek-v3.2:14b
```

### 10.2 Using Ollama as an API

```python
# Ollama exposes an OpenAI-compatible API on localhost:11434
from openai import OpenAI

client = OpenAI(
    api_key="ollama",  # Any string works
    base_url="http://localhost:11434/v1"
)

response = client.chat.completions.create(
    model="deepseek-v3.2:14b",
    messages=[
        {"role": "system", "content": "You are a security expert."},
        {"role": "user", "content": "Write a Python script to analyze Apache access logs for suspicious patterns"}
    ]
)
print(response.choices[0].message.content)

# Works with ANY tool that supports the OpenAI API!
# Cursor, Cline, Aider, LangChain, etc.
```

### 10.3 GPU Requirements

```
Model Size → VRAM Needed (approximate):
1B-3B     → 2-4 GB   (Gemma 3 1B, SmolLM2)
7B        → 6-8 GB   (Qwen3.5 7B, Mistral 7B)
14B       → 10-12 GB (Phi-4, Qwen3.5 14B)
33B       → 20-24 GB (DeepSeek Coder 33B)
70B       → 40+ GB   (Llama 3.3 70B -- needs quantization or multi-GPU)

Quantization reduces VRAM by 50-75%:
- Q4_K_M: Good balance of quality and size
- Q8_0: Near-original quality
- Q2_K: Maximum compression, lower quality
```

---

## 11. Free Embeddings & Vector Search

### 11.1 Free Embedding APIs

```python
# --- Cohere Embeddings (free, high quality) ---
import cohere
co = cohere.ClientV2(api_key="your-key")
response = co.embed(
    texts=["cybersecurity", "penetration testing", "network defense"],
    model="embed-english-v3.0",
    input_type="search_document",
    embedding_types=["float"]
)

# --- Hugging Face (free, local option) ---
from sentence_transformers import SentenceTransformer
model = SentenceTransformer('all-MiniLM-L6-v2')  # Free, runs locally
embeddings = model.encode(["SQL injection", "XSS attack", "CSRF"])

# --- Google (free via AI Studio) ---
import google.generativeai as genai
result = genai.embed_content(
    model="models/text-embedding-004",
    content="What is a firewall?"
)
# Returns 768-dimensional embedding

# --- Ollama (free, local) ---
import requests
response = requests.post("http://localhost:11434/api/embeddings", json={
    "model": "nomic-embed-text",
    "prompt": "network security best practices"
})
embedding = response.json()["embedding"]
```

---

## 12. Building a Multi-Provider Fallback System

```python
"""Multi-provider AI client with automatic fallback.

If one provider is down or rate-limited, automatically tries the next.
All providers use OpenAI-compatible API."""

from openai import OpenAI
import os

PROVIDERS = [
    {
        "name": "Groq",
        "base_url": "https://api.groq.com/openai/v1",
        "api_key": os.environ.get("GROQ_API_KEY"),
        "model": "llama-4-scout-17b-16e-instruct"
    },
    {
        "name": "Cerebras",
        "base_url": "https://api.cerebras.ai/v1",
        "api_key": os.environ.get("CEREBRAS_API_KEY"),
        "model": "llama-4-scout-17b"
    },
    {
        "name": "DeepSeek",
        "base_url": "https://api.deepseek.com",
        "api_key": os.environ.get("DEEPSEEK_API_KEY"),
        "model": "deepseek-chat"
    },
    {
        "name": "OpenRouter",
        "base_url": "https://openrouter.ai/api/v1",
        "api_key": os.environ.get("OPENROUTER_API_KEY"),
        "model": "google/gemini-3-flash"
    },
    {
        "name": "Ollama (local)",
        "base_url": "http://localhost:11434/v1",
        "api_key": "ollama",
        "model": "deepseek-v3.2:14b"
    }
]

def chat(messages, temperature=0.7, max_tokens=2048):
    """Try each provider in order until one succeeds."""
    for provider in PROVIDERS:
        if not provider["api_key"]:
            continue
        try:
            client = OpenAI(
                api_key=provider["api_key"],
                base_url=provider["base_url"]
            )
            response = client.chat.completions.create(
                model=provider["model"],
                messages=messages,
                temperature=temperature,
                max_tokens=max_tokens
            )
            print(f"[Using: {provider['name']}]")
            return response.choices[0].message.content
        except Exception as e:
            print(f"[{provider['name']} failed: {e}]")
            continue
    return "All providers failed."

# Usage
result = chat([
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "Explain how HTTPS works"}
])
print(result)
```

---

## 13. Free Image & Audio APIs

### 13.1 Free Image Generation

```python
# --- Hugging Face (free, Stable Diffusion) ---
import requests
API_URL = "https://api-inference.huggingface.co/models/stabilityai/stable-diffusion-xl-base-1.0"
HEADERS = {"Authorization": f"Bearer {os.environ.get('HF_TOKEN')}"}

response = requests.post(API_URL, headers=HEADERS, json={
    "inputs": "A futuristic cybersecurity command center, neon lights, 4K"
})
with open("generated.png", "wb") as f:
    f.write(response.content)

# --- Pollinations.ai (completely free, no API key) ---
import urllib.parse
prompt = urllib.parse.quote("A hacker in a dark room with multiple screens")
url = f"https://image.pollinations.ai/prompt/{prompt}?width=1024&height=768"
# Just fetch this URL -- returns an image, no API key needed!
```

### 13.2 Free Audio Transcription

```python
# --- Groq Whisper (free, fast) ---
from groq import Groq
client = Groq()

with open("recording.mp3", "rb") as f:
    transcript = client.audio.transcriptions.create(
        model="whisper-large-v3-turbo",
        file=f,
        response_format="verbose_json"
    )
print(transcript.text)
```

---

## 14. Rate Limit Management

```python
import time
import functools

def rate_limit(calls_per_minute=15):
    """Decorator to rate-limit API calls."""
    min_interval = 60.0 / calls_per_minute
    last_called = [0.0]
    
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            elapsed = time.time() - last_called[0]
            if elapsed < min_interval:
                time.sleep(min_interval - elapsed)
            last_called[0] = time.time()
            return func(*args, **kwargs)
        return wrapper
    return decorator

@rate_limit(calls_per_minute=15)  # Google AI Studio free limit
def generate_text(prompt):
    response = model.generate_content(prompt)
    return response.text

# Now this function automatically respects rate limits
for i in range(100):
    result = generate_text(f"Question {i}: Explain concept {i}")
    print(result)
```

---

## 15. Practical Exercises

### Exercise 1: API Key Collection
Sign up for all 5 free providers (Google AI Studio, Groq, Hugging Face, OpenRouter, Cerebras). Store keys in a `.env` file.

### Exercise 2: Multi-Provider Test
Send the same prompt to all 5 providers. Compare:
- Response quality
- Speed (time the requests)
- Rate limits hit

### Exercise 3: Build a Free Chatbot
Using only free APIs, build a command-line chatbot with:
- Multi-turn conversation
- Provider fallback
- Conversation history

### Exercise 4: Free RAG System
Build a simple RAG (Retrieval-Augmented Generation) system using:
- Cohere embeddings (free)
- ChromaDB (free, local vector store)
- Groq for generation (free)

---

## 16. Key Takeaways

1. **Google AI Studio is the best free API** -- generous limits, powerful models, no credit card
2. **Groq is the fastest free option** -- 500+ tokens/sec on LPU hardware
3. **OpenRouter unifies everything** -- one API key for 200+ models
4. **Ollama gives unlimited free local AI** -- no rate limits, complete privacy
5. **Most providers use OpenAI-compatible APIs** -- switch by changing `base_url` and `model`
6. **Build fallback systems** -- never depend on a single provider
7. **Free tiers are enough for development** -- only pay when you go to production
8. **Embeddings are free everywhere** -- Cohere, HuggingFace, Google, Ollama

---

**Next Module:** [Module 8: Building Your Own AI Agents](08-building-ai-agents.md) -- From simple function-calling to multi-agent systems.
