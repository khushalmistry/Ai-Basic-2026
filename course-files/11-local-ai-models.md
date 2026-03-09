# Module 11: Running AI Models Locally

## Overview

Running AI models on your own hardware gives you complete privacy, zero API costs, offline access, and unlimited usage. In 2026, local models have closed the gap with cloud APIs dramatically -- open-source models like Llama 4, Qwen 3.5, and DeepSeek V3.2 rival GPT-4 on many tasks. This module teaches you everything about running AI locally.

**By the end of this module, you will:**
- Run 10+ models locally using Ollama, llama.cpp, and vLLM
- Choose the right model size for your hardware
- Optimize performance with quantization and GPU offloading
- Build local AI applications with API-compatible servers
- Set up a private AI development environment

---

## 1. Why Run AI Locally?

### Benefits

| Benefit | Details |
|---------|----------|
| **Privacy** | Data never leaves your machine |
| **Cost** | Zero per-token charges after hardware investment |
| **Offline** | Works without internet |
| **Speed** | No network latency for small models |
| **Customization** | Fine-tune, modify, no restrictions |
| **Compliance** | Meet data residency requirements |

### When Local Makes Sense

- Processing sensitive documents (medical, legal, financial)
- High-volume tasks where API costs would be excessive
- Development and testing (save money while iterating)
- Air-gapped environments
- Custom fine-tuned models

### When Cloud Is Better

- Need cutting-edge reasoning (GPT-4.5, Claude Opus 4.6)
- Limited hardware (no GPU)
- Occasional use (API is cheaper than buying hardware)
- Need multimodal capabilities beyond what local models offer

---

## 2. Hardware Requirements

### GPU Memory Guide

| Model Size | VRAM Needed (Q4) | VRAM Needed (FP16) | Example Models |
|-----------|-------------------|---------------------|----------------|
| 1-3B | 2-3 GB | 4-6 GB | Llama 3.2 1B/3B, Phi-3 Mini |
| 7-8B | 4-6 GB | 14-16 GB | Llama 3.1 8B, Mistral 7B, Qwen 2.5 7B |
| 13-14B | 8-10 GB | 26-28 GB | Llama 2 13B, Qwen 2.5 14B |
| 32-34B | 18-22 GB | 64-68 GB | Qwen 2.5 32B, CodeLlama 34B |
| 70-72B | 36-42 GB | 140+ GB | Llama 3.1 70B, Qwen 2.5 72B |
| 405B | 200+ GB | 810+ GB | Llama 3.1 405B (multi-GPU) |

### Recommended Hardware Setups

**Budget ($300-500): CPU-Only**
- Any modern CPU with 32GB+ RAM
- Run 7B models at ~10 tokens/sec
- Good for: experimentation, light tasks

**Mid-Range ($800-1500): Single GPU**
- NVIDIA RTX 4060 Ti 16GB or RTX 4070 Ti 12GB
- Run 7B-13B models at 30-60 tokens/sec
- Good for: development, moderate workloads

**High-End ($2000-4000): Enthusiast GPU**
- NVIDIA RTX 4090 24GB or RTX 5080 16GB
- Run 13B-34B models at 40-80 tokens/sec
- Good for: production local inference, large models

**Professional ($5000+): Multi-GPU**
- 2x RTX 4090 or NVIDIA A6000 48GB
- Run 70B+ models at good speeds
- Good for: serving teams, running the largest open models

**Apple Silicon**
- M1/M2/M3 with 16GB: 7B-13B models
- M2/M3 Pro with 32GB: Up to 34B models
- M2/M3 Max with 64GB: Up to 70B models
- M2/M3 Ultra with 192GB: 405B models
- Unified memory is a huge advantage for LLMs

---

## 3. Ollama -- The Easiest Way to Run Local Models

### Installation

```bash
# macOS / Linux
curl -fsSL https://ollama.com/install.sh | sh

# Windows
# Download from https://ollama.com/download

# Verify installation
ollama --version
```

### Basic Usage

```bash
# Pull and run a model (downloads automatically)
ollama run llama3.1

# Run specific sizes
ollama run llama3.1:8b
ollama run llama3.1:70b

# Run other popular models
ollama run mistral
ollama run codellama
ollama run phi3
ollama run gemma2
ollama run qwen2.5
ollama run deepseek-v3

# List downloaded models
ollama list

# Remove a model
ollama rm llama3.1:70b

# Show model info
ollama show llama3.1
```

### Popular Models on Ollama (2026)

| Model | Sizes | Best For |
|-------|-------|----------|
| `llama3.1` | 8B, 70B, 405B | General purpose |
| `llama3.2` | 1B, 3B | Mobile/edge, lightweight |
| `llama4` | 8B, 70B | Latest Llama generation |
| `qwen2.5` | 0.5B-72B | Multilingual, coding |
| `qwen3.5` | 7B-72B | Latest Qwen, strong reasoning |
| `deepseek-v3` | 7B, 67B | Coding, math |
| `mistral` | 7B | Fast, efficient |
| `mixtral` | 8x7B, 8x22B | MoE, good quality/speed |
| `phi3` | 3.8B, 14B | Microsoft, small but capable |
| `gemma2` | 2B, 9B, 27B | Google, instruction following |
| `codellama` | 7B, 13B, 34B | Code generation |
| `llava` | 7B, 13B, 34B | Vision + language |
| `nomic-embed-text` | 137M | Text embeddings |

### Ollama API (OpenAI Compatible)

Ollama runs an API server on port 11434 that's compatible with OpenAI's format:

```bash
# Chat completion
curl http://localhost:11434/api/chat -d '{
  "model": "llama3.1",
  "messages": [
    {"role": "user", "content": "Explain quantum computing in simple terms"}
  ]
}'

# OpenAI-compatible endpoint
curl http://localhost:11434/v1/chat/completions -d '{
  "model": "llama3.1",
  "messages": [
    {"role": "user", "content": "Hello!"}
  ]
}'
```

### Using Ollama with Python

```python
# Option 1: Official Ollama library
import ollama

response = ollama.chat(
    model='llama3.1',
    messages=[
        {'role': 'user', 'content': 'Write a haiku about coding'}
    ]
)
print(response['message']['content'])

# Streaming
for chunk in ollama.chat(
    model='llama3.1',
    messages=[{'role': 'user', 'content': 'Tell me a story'}],
    stream=True
):
    print(chunk['message']['content'], end='', flush=True)

# Option 2: OpenAI SDK (since Ollama is compatible)
from openai import OpenAI

client = OpenAI(
    base_url='http://localhost:11434/v1',
    api_key='ollama'  # Any string works
)

response = client.chat.completions.create(
    model='llama3.1',
    messages=[{'role': 'user', 'content': 'Hello!'}]
)
print(response.choices[0].message.content)
```

### Custom Models (Modelfile)

```dockerfile
# Modelfile
FROM llama3.1

# Set system prompt
SYSTEM """You are a senior Python developer. You write clean, 
well-documented code with type hints. You always include error 
handling and tests."""

# Set parameters
PARAMETER temperature 0.3
PARAMETER top_p 0.9
PARAMETER num_ctx 8192
```

```bash
# Create custom model
ollama create python-expert -f Modelfile

# Run it
ollama run python-expert
```

---

## 4. llama.cpp -- Maximum Performance

### What is llama.cpp

llama.cpp is the foundational C++ inference engine that most local AI tools (including Ollama) are built on. Use it directly for maximum control and performance.

### Installation

```bash
# Clone and build
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp

# Build with CUDA (NVIDIA GPU)
make GGML_CUDA=1

# Build with Metal (Apple Silicon)
make GGML_METAL=1

# Build CPU-only
make

# Or use CMake
mkdir build && cd build
cmake .. -DGGML_CUDA=ON
cmake --build . --config Release
```

### Running Models

```bash
# Download a GGUF model from HuggingFace
# Example: https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGUF

# Interactive chat
./llama-cli -m models/llama-3.1-8b-instruct-q4_k_m.gguf \
  -n 512 \
  --interactive-first \
  -ngl 99 \
  --chat-template llama3

# Parameters:
# -m: model path
# -n: max tokens to generate
# -ngl: number of GPU layers (99 = all)
# -c: context length
# -t: number of CPU threads
```

### llama.cpp Server (API)

```bash
# Start OpenAI-compatible API server
./llama-server \
  -m models/llama-3.1-8b-instruct-q4_k_m.gguf \
  -c 4096 \
  -ngl 99 \
  --host 0.0.0.0 \
  --port 8080

# Use with any OpenAI-compatible client
curl http://localhost:8080/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "llama-3.1-8b",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
```

### Understanding Quantization (GGUF Formats)

| Format | Bits | Size (7B) | Quality | Speed |
|--------|------|-----------|---------|-------|
| Q2_K | 2-bit | ~2.7 GB | Poor | Fastest |
| Q3_K_M | 3-bit | ~3.3 GB | Acceptable | Very Fast |
| Q4_K_M | 4-bit | ~4.1 GB | Good | Fast |
| Q5_K_M | 5-bit | ~4.8 GB | Very Good | Moderate |
| Q6_K | 6-bit | ~5.5 GB | Excellent | Slower |
| Q8_0 | 8-bit | ~7.2 GB | Near-perfect | Slow |
| F16 | 16-bit | ~14.0 GB | Perfect | Slowest |

**Recommendation:** Q4_K_M offers the best balance of quality and speed for most users.

---

## 5. vLLM -- Production Serving

### What is vLLM

vLLM is optimized for high-throughput model serving:
- PagedAttention for efficient memory management
- Continuous batching for maximum GPU utilization
- Tensor parallelism for multi-GPU setups
- OpenAI-compatible API server

### Installation

```bash
pip install vllm
```

### Running a Server

```bash
# Start serving a model
python -m vllm.entrypoints.openai.api_server \
  --model meta-llama/Llama-3.1-8B-Instruct \
  --port 8000 \
  --max-model-len 8192

# Multi-GPU
python -m vllm.entrypoints.openai.api_server \
  --model meta-llama/Llama-3.1-70B-Instruct \
  --tensor-parallel-size 2 \
  --port 8000

# With quantization
python -m vllm.entrypoints.openai.api_server \
  --model meta-llama/Llama-3.1-70B-Instruct \
  --quantization awq \
  --port 8000
```

### Using vLLM

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:8000/v1",
    api_key="not-needed"
)

# Works exactly like OpenAI API
response = client.chat.completions.create(
    model="meta-llama/Llama-3.1-8B-Instruct",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "What is machine learning?"}
    ],
    temperature=0.7,
    max_tokens=500
)
print(response.choices[0].message.content)
```

### vLLM vs Ollama vs llama.cpp

| Feature | Ollama | llama.cpp | vLLM |
|---------|--------|-----------|------|
| Ease of use | Easiest | Moderate | Moderate |
| Performance | Good | Very Good | Best (batched) |
| Multi-GPU | Limited | Yes | Best |
| Quantization | GGUF | GGUF | AWQ, GPTQ, SqueezeLLM |
| Batching | No | Limited | Continuous |
| Best for | Personal use | Single-user perf | Production serving |
| Platform | Mac, Linux, Windows | All | Linux (GPU required) |

---

## 6. LM Studio -- GUI for Local Models

### What is LM Studio

LM Studio provides a polished desktop application for running local models:
- Download models from HuggingFace with one click
- Chat interface with conversation history
- Built-in API server (OpenAI compatible)
- Model parameter tuning UI
- Available on macOS, Windows, and Linux

### Getting Started

1. Download from [lmstudio.ai](https://lmstudio.ai)
2. Search for a model (e.g., "Llama 3.1 8B")
3. Click Download (auto-selects best quantization for your hardware)
4. Click "Chat" to start talking
5. Click "Server" to start the API (port 1234)

### Using LM Studio API

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:1234/v1",
    api_key="lm-studio"
)

response = client.chat.completions.create(
    model="local-model",  # LM Studio uses this identifier
    messages=[{"role": "user", "content": "Hello!"}]
)
```

---

## 7. Open WebUI -- ChatGPT-Like Interface for Local Models

### Installation

```bash
# Docker (connects to Ollama automatically)
docker run -d -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui \
  ghcr.io/open-webui/open-webui:main

# Access at http://localhost:3000
```

### Features

- ChatGPT-like interface for any Ollama/OpenAI model
- Multi-user support with authentication
- Conversation history and search
- Document upload and RAG
- Model switching mid-conversation
- Custom system prompts and presets
- Voice input and TTS output
- Plugin system for extensions

---

## 8. Running Embeddings Locally

### Why Local Embeddings

- Zero cost for high-volume embedding tasks
- Privacy for sensitive documents
- No rate limits

### With Ollama

```bash
# Pull an embedding model
ollama pull nomic-embed-text
ollama pull mxbai-embed-large
```

```python
import ollama

# Generate embeddings
response = ollama.embeddings(
    model='nomic-embed-text',
    prompt='What is machine learning?'
)
embedding = response['embedding']  # 768-dimensional vector
print(f"Embedding dimension: {len(embedding)}")
```

### With Sentence Transformers

```python
from sentence_transformers import SentenceTransformer

# Download and load model (runs locally)
model = SentenceTransformer('all-MiniLM-L6-v2')

# Generate embeddings
sentences = [
    "What is machine learning?",
    "How does AI work?",
    "Best pizza recipe"
]
embeddings = model.encode(sentences)
print(f"Shape: {embeddings.shape}")  # (3, 384)

# Compute similarity
from sentence_transformers.util import cos_sim
similarity = cos_sim(embeddings[0], embeddings[1])
print(f"ML vs AI similarity: {similarity.item():.4f}")  # ~0.75
```

---

## 9. Local RAG Setup

Build a complete private RAG system:

```python
# local_rag.py - Complete local RAG with Ollama + ChromaDB
import ollama
import chromadb
from pathlib import Path

# Initialize ChromaDB (local vector store)
client = chromadb.PersistentClient(path="./chroma_db")
collection = client.get_or_create_collection(
    name="documents",
    metadata={"hnsw:space": "cosine"}
)


def add_documents(texts: list[str], ids: list[str]):
    """Embed and store documents locally."""
    embeddings = []
    for text in texts:
        response = ollama.embeddings(
            model='nomic-embed-text',
            prompt=text
        )
        embeddings.append(response['embedding'])
    
    collection.add(
        documents=texts,
        embeddings=embeddings,
        ids=ids
    )
    print(f"Added {len(texts)} documents")


def query(question: str, n_results: int = 3) -> str:
    """Query documents and generate answer locally."""
    # Embed the question
    q_embedding = ollama.embeddings(
        model='nomic-embed-text',
        prompt=question
    )['embedding']
    
    # Search for relevant documents
    results = collection.query(
        query_embeddings=[q_embedding],
        n_results=n_results
    )
    
    # Build context from results
    context = "\n\n".join(results['documents'][0])
    
    # Generate answer with local LLM
    response = ollama.chat(
        model='llama3.1',
        messages=[
            {
                'role': 'system',
                'content': f"""Answer the question using ONLY the provided context.
If the answer isn't in the context, say "I don't have enough information."

Context:
{context}"""
            },
            {'role': 'user', 'content': question}
        ]
    )
    
    return response['message']['content']


# Example usage
if __name__ == "__main__":
    # Add some documents
    docs = [
        "Our return policy allows returns within 30 days of purchase.",
        "Shipping is free for orders over $50 in the continental US.",
        "We accept Visa, Mastercard, and PayPal as payment methods.",
        "Customer support is available Monday-Friday, 9am-5pm EST.",
        "Premium members get 20% off all purchases and priority shipping."
    ]
    add_documents(docs, [f"doc_{i}" for i in range(len(docs))])
    
    # Query
    answer = query("What is the return policy?")
    print(f"Answer: {answer}")
    
    answer = query("How can I contact support?")
    print(f"Answer: {answer}")
```

---

## 10. Performance Optimization

### Ollama Optimization

```bash
# Set GPU layers (more = faster, uses more VRAM)
OLLAMA_NUM_GPU=99 ollama serve

# Increase context window
ollama run llama3.1 --num-ctx 32768

# Keep model in memory (faster subsequent requests)
ollama run llama3.1 --keepalive 60m

# Set number of CPU threads
OLLAMA_NUM_THREADS=8 ollama serve
```

### llama.cpp Optimization

```bash
# Flash attention (faster, less memory)
./llama-cli -m model.gguf -ngl 99 --flash-attn

# Adjust batch size for throughput
./llama-cli -m model.gguf -ngl 99 -b 512

# Use mmap for faster loading
./llama-cli -m model.gguf --mmap

# Specify GPU split for multi-GPU
./llama-cli -m model.gguf -ngl 99 --tensor-split 0.5,0.5
```

### Benchmarking

```bash
# Ollama benchmark
time ollama run llama3.1 "Write a 500-word essay about AI" --verbose

# llama.cpp built-in benchmark
./llama-bench -m model.gguf -ngl 99
```

### Speed Reference (RTX 4090, Q4_K_M)

| Model | Prompt Processing | Generation |
|-------|-------------------|------------|
| Llama 3.1 8B | ~3000 tok/s | ~80 tok/s |
| Llama 3.1 70B | ~400 tok/s | ~15 tok/s |
| Qwen 2.5 32B | ~800 tok/s | ~35 tok/s |
| Mistral 7B | ~3500 tok/s | ~90 tok/s |
| Phi-3 14B | ~1500 tok/s | ~50 tok/s |

---

## Summary

| Tool | Best For | Ease | Performance |
|------|----------|------|------------|
| **Ollama** | Getting started, personal use | Easiest | Good |
| **LM Studio** | GUI users, experimentation | Very Easy | Good |
| **llama.cpp** | Maximum control, benchmarking | Moderate | Very Good |
| **vLLM** | Production serving, teams | Moderate | Best |
| **Open WebUI** | Team ChatGPT replacement | Easy | Depends on backend |

**Getting Started Path:**
1. Install Ollama
2. Run `ollama run llama3.1`
3. Try different models for your use case
4. Set up Open WebUI for a nice chat interface
5. Build a local RAG system for your documents
6. Graduate to vLLM when you need production serving

**Next Module:** [Module 12: AI for Business & Startups](./12-ai-for-business.md) -- Leverage AI to build and grow your business.
