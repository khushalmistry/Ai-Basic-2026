# Module 1: The AI Landscape in 2026

## Overview

The AI industry has undergone a seismic shift since 2023. What was once a two-horse race (OpenAI vs Google) is now a multi-polar landscape with open-source models rivaling closed-source ones, specialized AI chips reshaping inference costs, and AI agents becoming the dominant paradigm. This module maps the entire terrain so you can make informed decisions about which tools, models, and platforms to invest your time in.

**By the end of this module, you will:**
- Understand every major AI provider and their latest models
- Know the difference between open-source and closed-source AI ecosystems
- Grasp the economics of AI (pricing, tokens, rate limits)
- Identify which models are best for which tasks
- Understand the agent paradigm shift happening right now

---

## 1. The Big Picture: AI in March 2026

### 1.1 Where We Are

```
Timeline of Major AI Milestones:

2022: ChatGPT launches (GPT-3.5) -- AI enters mainstream
2023: GPT-4, Claude 2, Gemini 1.0 -- "Foundation model" era
2024: GPT-4o, Claude 3.5 Sonnet, Gemini 1.5 Pro -- Multimodal + long context
2025: GPT-5, Claude Opus 4, Gemini 2.5 Pro -- Reasoning + agents emerge
2026: GPT-5.4, Claude Opus 4.6, Gemini 3 -- Agents become mainstream
      DeepSeek V3.2, Qwen3.5, Llama 4 -- Open-source reaches parity
```

### 1.2 Key Trends Defining 2026

1. **Agents Over Chatbots** -- AI that takes actions, not just generates text
2. **Open-Source Parity** -- DeepSeek, Qwen, and Llama match proprietary models on many benchmarks
3. **Inference Cost Collapse** -- Groq, Cerebras, and custom silicon make AI 100x cheaper than 2023
4. **Multimodal by Default** -- Every major model handles text, images, audio, and video
5. **Tool Use is Standard** -- Models natively call APIs, browse the web, and execute code
6. **Context Windows Explode** -- 1M+ token contexts are common (Claude, Gemini)
7. **Specialization** -- Models optimized for coding, math, creative writing, and specific domains

---

## 2. Closed-Source / Proprietary Models

### 2.1 OpenAI (GPT Family)

**Company:** OpenAI (San Francisco) | **Valuation:** ~$300B+ | **API:** api.openai.com

| Model | Released | Key Strengths | Context | Pricing (Input/Output per 1M tokens) |
|-------|----------|---------------|---------|--------------------------------------|
| GPT-5.4 | Mar 2026 | Best enterprise model, 33% fewer hallucinations, agentic | 256K | $3.00 / $15.00 |
| GPT-5.3-Codex | Feb 2026 | State-of-the-art coding, autonomous app building | 256K | $2.50 / $12.50 |
| GPT-5.3-Codex-Spark | Feb 2026 | Ultra-fast coding (1000+ tok/s), Cerebras partnership | 128K | $1.50 / $7.50 |
| GPT-5.2 | Dec 2025 | Professional knowledge work, GDPval SOTA (70.9%) | 256K | $2.00 / $10.00 |
| GPT-5 | Aug 2025 | Unified quick + deep reasoning | 128K | $1.50 / $7.50 |
| GPT-4o | May 2024 | Fast multimodal, still great value | 128K | $0.50 / $1.50 |
| o3 | Jan 2026 | Deep reasoning, math, science | 200K | $2.00 / $8.00 |
| o4-mini | Feb 2026 | Fast reasoning, excellent cost/perf | 200K | $0.55 / $2.20 |

**Key Products:**
- **ChatGPT** -- Consumer app (Free, Plus $20/mo, Pro $200/mo)
- **ChatGPT Go** -- $8/mo tier with GPT-5.2 Instant, 10x free messages
- **ChatGPT Team** -- $25/user/mo for businesses
- **ChatGPT Enterprise** -- Custom pricing, advanced security
- **API Platform** -- Pay-per-token access to all models
- **Operator** -- AI agent that browses the web and takes actions for you

**When to Use OpenAI:**
- Enterprise applications requiring reliability and low hallucination rates
- Coding tasks (GPT-5.3-Codex is best-in-class for autonomous coding)
- When you need the broadest ecosystem (plugins, integrations, fine-tuning)
- Real-time applications (Spark model for ultra-fast inference)

### 2.2 Anthropic (Claude Family)

**Company:** Anthropic (San Francisco) | **Valuation:** ~$60B+ | **API:** api.anthropic.com

| Model | Released | Key Strengths | Context | Pricing (Input/Output per 1M tokens) |
|-------|----------|---------------|---------|--------------------------------------|
| Claude Opus 4.6 | Feb 2026 | Best overall (coding, agents, enterprise), 1M context beta | 200K (1M beta) | $5.00 / $25.00 |
| Claude Sonnet 4.6 | Feb 2026 | Near-Opus quality at fraction of cost | 200K | $1.50 / $7.50 |
| Claude Sonnet 4.5 | Oct 2025 | Extended thinking, computer use | 200K | $1.50 / $7.50 |
| Claude Haiku 4 | Mar 2025 | Ultra-fast, cheapest Claude | 200K | $0.25 / $1.25 |

**Key Products:**
- **Claude.ai** -- Consumer/Pro app (Free, Pro $20/mo, Team $25/mo)
- **Claude Code** -- Terminal-based AI coding agent (agentic coding from CLI)
- **Claude for Enterprise** -- With admin controls, SSO, audit logs
- **API Platform** -- Pay-per-token, prompt caching, batch API
- **Computer Use** -- Claude can control your desktop (mouse, keyboard, screen)
- **MCP (Model Context Protocol)** -- Open standard for connecting AI to tools

**When to Use Claude:**
- Long document analysis (1M token context is unmatched for comprehension)
- Coding and code review (Claude Code is the best terminal coding agent)
- When you need careful, nuanced reasoning with fewer hallucinations
- Computer use / desktop automation tasks
- Building tool-connected agents via MCP

### 2.3 Google DeepMind (Gemini Family)

**Company:** Google DeepMind | **API:** ai.google.dev (AI Studio) / cloud.google.com (Vertex AI)

| Model | Released | Key Strengths | Context | Pricing |
|-------|----------|---------------|---------|--------|
| Gemini 3 Ultra | Feb 2026 | Multimodal reasoning, 2M context, native Google integration | 2M | $3.50 / $10.50 |
| Gemini 3 Pro | Feb 2026 | Balanced performance, great for most tasks | 2M | $0.63 / $2.50 |
| Gemini 3 Flash | Feb 2026 | Ultra-fast, adaptive thinking | 1M | $0.10 / $0.40 |
| Gemini 2.5 Pro | Mar 2025 | Deep Think, strong on coding/math | 1M | $1.25 / $5.00 |

**Key Products:**
- **Gemini App** -- Consumer AI assistant (Free, Advanced $20/mo)
- **Google AI Studio** -- Free API playground and development environment
- **Vertex AI** -- Enterprise platform with fine-tuning, deployment, monitoring
- **NotebookLM** -- AI-powered research and note-taking tool
- **Project Astra** -- Multimodal AI agent (vision, audio, real-time)

**When to Use Gemini:**
- Massive context tasks (2M tokens = entire codebases, book collections)
- Multimodal analysis (best native image/video/audio understanding)
- Budget-conscious projects (Flash models are incredibly cheap)
- Google ecosystem integration (Workspace, Search, Cloud)
- Free tier usage via Google AI Studio (generous free limits)

### 2.4 xAI (Grok Family)

**Company:** xAI (Elon Musk) | **API:** api.x.ai

| Model | Released | Key Strengths | Context | Pricing |
|-------|----------|---------------|---------|--------|
| Grok 3 | Feb 2025 | Strong reasoning, real-time X/Twitter data | 128K | $3.00 / $15.00 |
| Grok 3 Mini | Feb 2025 | Fast, efficient reasoning | 128K | $0.30 / $0.50 |

**When to Use Grok:**
- Real-time social media analysis (native X/Twitter integration)
- When you want an "unfiltered" model with fewer content restrictions
- Real-time news and trending topic analysis

---

## 3. Open-Source / Open-Weight Models

The open-source AI ecosystem reached a tipping point in 2025-2026. These models run locally, can be fine-tuned, and have no API costs.

### 3.1 DeepSeek (China)

| Model | Released | Parameters | Key Strengths |
|-------|----------|-----------|---------------|
| DeepSeek V3.2 | Feb 2026 | 685B MoE | Matches GPT-5 on many benchmarks, MIT license |
| DeepSeek R1 | Jan 2025 | 671B MoE | First open reasoning model, chain-of-thought |
| DeepSeek Coder V3 | 2025 | 33B | Excellent code generation, fine-tunable |

**Why DeepSeek Matters:**
- Trained for ~$6M (vs billions for GPT-5) -- proved efficient training is possible
- Fully open weights under MIT license -- use commercially without restrictions
- Mixture of Experts (MoE) architecture -- only activates ~37B parameters per query
- Consistently beats models costing 100x more to train
- Available on Hugging Face, Ollama, and every major inference platform

### 3.2 Alibaba Qwen (China)

| Model | Released | Parameters | Key Strengths |
|-------|----------|-----------|---------------|
| Qwen3.5 | Feb 2026 | 72B / 14B / 7B | Multilingual, strong coding, Apache 2.0 |
| Qwen2.5-Coder | 2025 | 32B | Best open-source coding model at its size |
| QwQ | 2025 | 32B | Open reasoning model, competitive with o1 |

**Why Qwen Matters:**
- Apache 2.0 license -- truly open for commercial use
- Excellent multilingual support (Chinese, English, and 27+ languages)
- Strong coding capabilities rivaling proprietary models
- Multiple sizes from 0.5B to 72B for different hardware

### 3.3 Meta Llama (USA)

| Model | Released | Parameters | Key Strengths |
|-------|----------|-----------|---------------|
| Llama 4 Maverick | Apr 2025 | 400B MoE (17B active) | Multimodal, beats GPT-4o on benchmarks |
| Llama 4 Scout | Apr 2025 | 109B MoE (17B active) | 10M token context window |
| Llama 3.3 | Dec 2024 | 70B | Excellent general-purpose, widely supported |

**Why Llama Matters:**
- Meta's massive investment ensures continued development
- Llama 4 Scout's 10M token context is the largest in open-source
- Huge ecosystem of fine-tunes, adapters, and tooling
- Llama license allows commercial use (with conditions for 700M+ MAU apps)

### 3.4 Mistral (France/EU)

| Model | Released | Parameters | Key Strengths |
|-------|----------|-----------|---------------|
| Mistral Large 2 | 2025 | 123B | Best European model, strong multilingual |
| Mistral Medium | 2025 | ~70B | Good balance of speed and quality |
| Mistral Small | 2025 | ~22B | Fast, efficient, great for edge deployment |
| Codestral | 2025 | 22B | Specialized coding model |

### 3.5 Others Worth Knowing

| Model | Org | Size | Notable For |
|-------|-----|------|-------------|
| Phi-4 | Microsoft | 14B | Best small model for reasoning |
| Gemma 3 | Google | 27B / 12B / 4B / 1B | Open model from Google, strong for its size |
| Command R+ | Cohere | 104B | Best for RAG and enterprise search |
| DBRX | Databricks | 132B MoE | Optimized for data/analytics tasks |
| Falcon 3 | TII (UAE) | 40B / 10B / 1B | Open under Apache 2.0, strong multilingual |
| Yi-Lightning | 01.AI | 200B+ | Fast inference, competitive benchmarks |

---

## 4. The Model Selection Framework

### 4.1 Decision Matrix

```
What's your primary use case?

├── General Assistant / Chat
│   ├── Budget: Free → Gemini 3 Flash (AI Studio) or DeepSeek V3.2 (local)
│   ├── Budget: $20/mo → ChatGPT Plus or Claude Pro
│   └── Budget: Enterprise → GPT-5.4 or Claude Opus 4.6
│
├── Coding
│   ├── IDE Integration → Cursor (multi-model) or GitHub Copilot
│   ├── Terminal Agent → Claude Code
│   ├── API Coding → GPT-5.3-Codex or Claude Opus 4.6
│   └── Local/Free → DeepSeek Coder V3 or Qwen2.5-Coder
│
├── Long Documents / Research
│   ├── Up to 200K → Claude Opus 4.6
│   ├── Up to 1M → Claude (beta) or Gemini 3
│   └── Up to 2M → Gemini 3 Ultra
│
├── Image/Video Understanding
│   ├── Best Quality → Gemini 3 Ultra or GPT-5.4
│   └── Free → Gemini 3 Flash or Llama 4 Maverick (local)
│
├── Reasoning / Math / Science
│   ├── Best → o3 or Claude Opus 4.6 (extended thinking)
│   ├── Fast → o4-mini
│   └── Free/Local → DeepSeek R1 or QwQ
│
└── Running Locally (Privacy / Offline)
    ├── High-end GPU (24GB+) → DeepSeek V3.2 (quantized) or Llama 4
    ├── Mid-range GPU (8-16GB) → Qwen3.5 14B or Llama 3.3 70B (quantized)
    ├── CPU Only → Phi-4 14B or Gemma 3 4B
    └── Mobile → Gemma 3 1B or SmolLM2
```

### 4.2 Cost Comparison (Per 1 Million Tokens)

```
Cheapest → Most Expensive (Input pricing):

Free Tier:        Gemini 3 Flash (AI Studio), DeepSeek API, Groq (rate-limited)
$0.01-0.10:       Gemini 3 Flash (paid), Haiku 4
$0.10-0.50:       GPT-4o-mini, Gemini 3 Pro
$0.50-1.50:       GPT-4o, Claude Sonnet 4.6
$1.50-3.00:       GPT-5, GPT-5.4, Grok 3
$3.00-5.00:       Claude Opus 4.6, Gemini 3 Ultra
$0.00 (local):    Any open-source model via Ollama (your hardware cost only)
```

---

## 5. Understanding AI Economics

### 5.1 Tokens Explained

```
What is a token?
- Roughly 3/4 of a word in English
- "Hello, how are you?" = 6 tokens
- 1,000 tokens ≈ 750 words
- 1 page of text ≈ 400-500 tokens
- A full novel ≈ 100,000-150,000 tokens

Context Window:
- How many tokens the model can "see" at once
- 128K tokens ≈ a 300-page book
- 1M tokens ≈ ~10 novels
- 2M tokens ≈ a small library
```

### 5.2 API Pricing Models

1. **Pay-per-token** -- Most common (OpenAI, Anthropic, Google)
2. **Rate-limited free** -- Generous free tiers (Google AI Studio, Groq, DeepSeek)
3. **Subscription** -- Fixed monthly fee for consumer apps (ChatGPT Plus, Claude Pro)
4. **Self-hosted** -- Pay for compute only (GPU rental or own hardware)
5. **Credits** -- Some platforms give free credits to start (OpenRouter, Together AI)

### 5.3 Hidden Costs to Watch

- **Output tokens cost more** -- Typically 3-5x input token pricing
- **Reasoning tokens** -- o3/o4 models charge for "thinking" tokens you don't see
- **Image/audio tokens** -- Multimodal inputs consume more tokens than text
- **Prompt caching** -- Claude and OpenAI offer 50-90% discounts on cached prompts
- **Batch API** -- 50% discount for non-real-time processing (OpenAI, Anthropic)

---

## 6. The Agent Paradigm Shift

### 6.1 From Chatbots to Agents

```
2023-2024: Chatbot Era
- User types prompt → Model generates text → User reads
- Models are "text in, text out"
- Integrations require manual copy-paste

2025-2026: Agent Era
- User gives goal → Agent plans steps → Agent uses tools → Agent delivers result
- Models call APIs, browse web, execute code, control computers
- AI works autonomously on multi-step tasks
```

### 6.2 Key Agent Capabilities (2026)

| Capability | Description | Who Does It Best |
|-----------|-------------|------------------|
| Tool Use | Calling external APIs and functions | Claude Opus 4.6, GPT-5.4 |
| Computer Use | Controlling mouse, keyboard, screen | Claude (native), OpenAI Operator |
| Web Browsing | Searching and reading web pages | ChatGPT, Gemini, Perplexity |
| Code Execution | Writing and running code | GPT-5.3-Codex, Claude Code |
| File Processing | Reading/writing documents, CSVs, images | All major models |
| Multi-Agent | Coordinating multiple specialized agents | OpenAI Agents SDK, CrewAI, LangGraph |
| Memory | Remembering across conversations | ChatGPT (native), Claude Projects |

### 6.3 Agent Frameworks Overview

| Framework | Type | Best For |
|-----------|------|----------|
| OpenAI Agents SDK | Code (Python/TS) | Production agents with OpenAI models |
| LangGraph | Code (Python/TS) | Complex stateful agent workflows |
| CrewAI | Code (Python) | Multi-agent team collaboration |
| AutoGen | Code (Python) | Microsoft ecosystem, research |
| Claude Code | CLI Agent | Terminal-based coding agent |
| n8n | No-Code | Visual AI workflow automation |
| Dify | Low-Code | Building AI apps with visual editor |
| Flowise | No-Code | Drag-and-drop LLM chains |

*We'll deep-dive into each of these in Modules 8-10.*

---

## 7. The Infrastructure Layer

### 7.1 AI Inference Providers

These companies provide fast, cheap access to open-source models:

| Provider | Specialty | Free Tier | Notable |
|----------|----------|-----------|----------|
| Groq | Ultra-fast inference (LPU chips) | Yes (rate-limited) | 10x faster than GPU inference |
| Cerebras | Wafer-scale inference | Limited | 1000+ tokens/sec, powers GPT-5.3-Codex-Spark |
| Together AI | Wide model selection | $5 credits | 200+ open models, fine-tuning |
| Fireworks AI | Fast, cheap inference | Free tier | Serverless GPU, great for startups |
| Replicate | Run any model via API | Some free | Easy deployment, pay per second |
| OpenRouter | Model aggregator | Free models | Single API for 200+ models from all providers |
| Hugging Face | Model hub + inference | Free (rate-limited) | Largest open model repository |

### 7.2 GPU Cloud Providers (For Self-Hosting)

| Provider | GPU Options | Starting Price | Best For |
|----------|------------|----------------|----------|
| Lambda | A100, H100, H200 | ~$1.10/hr (A100) | ML training and inference |
| RunPod | A100, H100, RTX 4090 | ~$0.44/hr (RTX 4090) | Budget GPU rental |
| Vast.ai | Community GPUs | ~$0.20/hr (varies) | Cheapest option, peer-to-peer |
| AWS (SageMaker) | Full range | Varies | Enterprise, auto-scaling |
| Google Cloud (Vertex) | TPUs + GPUs | Varies | Google ecosystem |

---

## 8. Practical Exercise: Your First AI Exploration

### Exercise 1: Compare Models Head-to-Head

1. Go to [chat.lmsys.org](https://chat.lmsys.org) (Chatbot Arena)
2. Choose "Battle" mode -- you'll get two anonymous models
3. Ask a complex question and compare responses
4. After 10 battles, check which models you preferred

### Exercise 2: Try Free APIs

1. Get a free API key from Google AI Studio (ai.google.dev)
2. Get a free Groq API key (console.groq.com)
3. Send the same prompt to both and compare speed + quality

```python
# Quick test with Google AI Studio (free)
import google.generativeai as genai

genai.configure(api_key="YOUR_FREE_KEY")
model = genai.GenerativeModel('gemini-3-flash')
response = model.generate_content("Explain quantum computing in 3 sentences")
print(response.text)
```

### Exercise 3: Run a Model Locally

1. Install Ollama: `curl -fsSL https://ollama.com/install.sh | sh`
2. Pull a model: `ollama pull deepseek-v3.2:14b`
3. Chat: `ollama run deepseek-v3.2:14b`
4. You now have a free, private AI running on your machine!

---

## 9. Key Takeaways

1. **No single model is best at everything** -- Learn when to use which model
2. **Open-source has reached parity** -- DeepSeek V3.2 and Qwen3.5 are genuinely competitive
3. **Cost varies 1000x** -- From free (local/Groq) to $25/M tokens (Claude Opus)
4. **Agents are the new paradigm** -- AI that acts, not just responds
5. **The ecosystem matters** -- Choose based on tools, integrations, and community, not just benchmarks
6. **Context windows are a superpower** -- 2M tokens changes what's possible
7. **Speed matters** -- Groq and Cerebras make real-time AI applications viable

---

## 10. Further Reading

- [Chatbot Arena Leaderboard](https://chat.lmsys.org) -- Live model rankings from blind human evaluations
- [Artificial Analysis](https://artificialanalysis.ai) -- Independent benchmarks, pricing, and speed comparisons
- [Hugging Face Open LLM Leaderboard](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard) -- Open model benchmarks
- [OpenRouter Models](https://openrouter.ai/models) -- Compare pricing across all providers

---

**Next Module:** [Module 2: Prompt Engineering Mastery](02-prompt-engineering-mastery.md) -- Learn to get 10x better results from any AI model.
