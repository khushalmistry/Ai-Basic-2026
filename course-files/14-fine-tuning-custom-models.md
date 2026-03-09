# Module 14: Fine-Tuning & Custom Models

## Overview

Fine-tuning takes a pre-trained AI model and specializes it on your own data. Instead of relying on generic models, you can create AI that understands your domain, speaks your brand voice, and excels at your specific tasks. In 2026, fine-tuning has become more accessible than ever -- you can customize state-of-the-art models with as few as 50 examples.

**By the end of this module, you will:**
- Understand when fine-tuning beats prompting and RAG
- Fine-tune models using OpenAI, together.ai, and Hugging Face
- Prepare high-quality training datasets
- Use LoRA and QLoRA for efficient fine-tuning on consumer GPUs
- Evaluate and deploy custom models
- Understand RLHF and preference tuning

---

## 1. When to Fine-Tune

### Decision Framework

```
Start with prompting (zero cost, instant)
    ↓ Not good enough?
Try few-shot prompting (still zero cost)
    ↓ Not good enough?
Try RAG (add your data as context)
    ↓ Not good enough?
Fine-tune (costs money and time, but maximum control)
```

### Comparison

| Approach | Cost | Effort | Best For |
|----------|------|--------|----------|
| **Prompting** | Free | Minutes | Most tasks |
| **Few-shot** | Free | Minutes | Pattern-following tasks |
| **RAG** | Low | Hours | Knowledge-heavy tasks |
| **Fine-tuning** | Medium | Days | Style, format, domain expertise |
| **Pre-training** | Very High | Weeks+ | Entirely new capabilities |

### Fine-Tune When You Need:

- **Consistent style/tone** that's hard to prompt for
- **Specific output format** that must be exact every time
- **Domain expertise** beyond what's in the base model
- **Reduced latency** (fine-tuned models can be smaller)
- **Lower inference cost** (shorter prompts, no RAG retrieval)
- **Behavior modification** (safety, personality, constraints)

### Don't Fine-Tune When:

- Good prompting achieves your goal
- You need factual knowledge (use RAG instead)
- Your data changes frequently (RAG is easier to update)
- You have fewer than 50 quality examples
- The task is generic (base models already excel)

---

## 2. Preparing Training Data

### Data Quality Is Everything

```
Golden Rule: Your fine-tuned model will be as good as your data.

- 50 excellent examples > 5,000 mediocre ones
- Every example should be one you'd rate 5/5
- Diversity matters: cover edge cases and variations
- Consistency matters: format should be identical across examples
```

### Data Format (OpenAI Style)

```jsonl
{"messages": [{"role": "system", "content": "You are a medical coding assistant that converts clinical notes to ICD-10 codes."}, {"role": "user", "content": "Patient presents with acute bronchitis with productive cough for 5 days."}, {"role": "assistant", "content": "J20.9 - Acute bronchitis, unspecified"}]}
{"messages": [{"role": "system", "content": "You are a medical coding assistant that converts clinical notes to ICD-10 codes."}, {"role": "user", "content": "Type 2 diabetes with diabetic chronic kidney disease, stage 3."}, {"role": "assistant", "content": "E11.22 - Type 2 diabetes mellitus with diabetic chronic kidney disease\nN18.3 - Chronic kidney disease, stage 3"}]}
```

### Creating Training Data

**Method 1: From Existing Data**
```python
import json

# Convert your existing Q&A pairs, support tickets, etc.
raw_data = [
    {"question": "How do I reset my password?", 
     "answer": "Go to Settings > Security > Reset Password..."},
    # ... more examples
]

training_data = []
for item in raw_data:
    training_data.append({
        "messages": [
            {"role": "system", "content": "You are a helpful support agent for Acme Corp."},
            {"role": "user", "content": item["question"]},
            {"role": "assistant", "content": item["answer"]}
        ]
    })

# Save as JSONL
with open("training_data.jsonl", "w") as f:
    for item in training_data:
        f.write(json.dumps(item) + "\n")

print(f"Created {len(training_data)} training examples")
```

**Method 2: AI-Assisted Data Generation**
```python
from openai import OpenAI
import json

client = OpenAI()

def generate_training_examples(topic, n=20):
    """Use GPT-4 to generate training examples, then human-review."""
    response = client.chat.completions.create(
        model="gpt-4o",
        messages=[{
            "role": "user",
            "content": f"""Generate {n} diverse training examples for a 
            fine-tuned model that {topic}.
            
            Format each as a JSON object with:
            - "input": the user's message
            - "output": the ideal assistant response
            
            Make examples diverse, covering edge cases.
            Return as a JSON array."""
        }],
        response_format={"type": "json_object"}
    )
    
    examples = json.loads(response.choices[0].message.content)
    return examples["examples"]

# Generate, then MANUALLY REVIEW and edit
examples = generate_training_examples(
    "converts natural language to SQL queries for an e-commerce database"
)

# Human review step is critical!
print("Review each example and edit as needed:")
for i, ex in enumerate(examples):
    print(f"\n--- Example {i+1} ---")
    print(f"Input: {ex['input']}")
    print(f"Output: {ex['output']}")
```

### Data Validation

```python
import json
from collections import Counter

def validate_training_data(filepath):
    """Validate JSONL training data."""
    issues = []
    examples = []
    
    with open(filepath) as f:
        for i, line in enumerate(f, 1):
            try:
                data = json.loads(line)
            except json.JSONDecodeError:
                issues.append(f"Line {i}: Invalid JSON")
                continue
            
            if "messages" not in data:
                issues.append(f"Line {i}: Missing 'messages' key")
                continue
            
            messages = data["messages"]
            roles = [m["role"] for m in messages]
            
            if "assistant" not in roles:
                issues.append(f"Line {i}: No assistant message")
            
            if "user" not in roles:
                issues.append(f"Line {i}: No user message")
            
            # Check for empty content
            for m in messages:
                if not m.get("content", "").strip():
                    issues.append(f"Line {i}: Empty content for {m['role']}")
            
            examples.append(data)
    
    # Statistics
    print(f"Total examples: {len(examples)}")
    print(f"Issues found: {len(issues)}")
    for issue in issues[:10]:
        print(f"  - {issue}")
    
    # Token length distribution
    lengths = [sum(len(m["content"]) for m in ex["messages"]) for ex in examples]
    print(f"\nCharacter lengths:")
    print(f"  Min: {min(lengths)}, Max: {max(lengths)}, Avg: {sum(lengths)//len(lengths)}")
    
    return len(issues) == 0

validate_training_data("training_data.jsonl")
```

---

## 3. Fine-Tuning with OpenAI

### The Simplest Path

OpenAI makes fine-tuning straightforward:

```python
from openai import OpenAI

client = OpenAI()

# Step 1: Upload training data
training_file = client.files.create(
    file=open("training_data.jsonl", "rb"),
    purpose="fine-tune"
)
print(f"File ID: {training_file.id}")

# Step 2: Create fine-tuning job
job = client.fine_tuning.jobs.create(
    training_file=training_file.id,
    model="gpt-4o-mini-2024-07-18",  # Base model
    hyperparameters={
        "n_epochs": 3,
        "batch_size": "auto",
        "learning_rate_multiplier": "auto"
    },
    suffix="my-custom-model"  # Model name suffix
)
print(f"Job ID: {job.id}")

# Step 3: Monitor progress
import time

while True:
    job_status = client.fine_tuning.jobs.retrieve(job.id)
    print(f"Status: {job_status.status}")
    
    if job_status.status in ["succeeded", "failed", "cancelled"]:
        break
    time.sleep(60)

# Step 4: Use your fine-tuned model
if job_status.status == "succeeded":
    model_name = job_status.fine_tuned_model
    print(f"Model: {model_name}")
    
    response = client.chat.completions.create(
        model=model_name,
        messages=[{"role": "user", "content": "Your test prompt here"}]
    )
    print(response.choices[0].message.content)
```

### OpenAI Fine-Tuning Pricing (2026)

| Model | Training Cost | Inference Cost |
|-------|--------------|----------------|
| gpt-4o-mini | $3.00 / 1M tokens | $0.30 / 1M input, $1.20 / 1M output |
| gpt-4o | $25.00 / 1M tokens | $3.75 / 1M input, $15.00 / 1M output |

### Tips for OpenAI Fine-Tuning

```
1. Start with gpt-4o-mini (cheaper, faster iteration)
2. Use 50-100 examples for initial testing
3. Scale to 500-1000 for production quality
4. Always include a validation set (10-20% of data)
5. Run 2-3 epochs typically (more can overfit)
6. Compare fine-tuned model against base + good prompt
7. Iterate on data quality, not just quantity
```

---

## 4. Fine-Tuning with LoRA (Local/Open Source)

### What is LoRA?

LoRA (Low-Rank Adaptation) fine-tunes only a small fraction of model parameters:
- **Full fine-tuning:** Updates all billions of parameters (needs huge GPU)
- **LoRA:** Adds small trainable matrices (1-10% of parameters)
- **QLoRA:** LoRA on quantized models (runs on consumer GPUs)

```
Memory Requirements Comparison (7B model):
- Full fine-tuning: ~60 GB VRAM
- LoRA: ~16 GB VRAM
- QLoRA (4-bit): ~6 GB VRAM  ← Fits on RTX 4060!
```

### Fine-Tuning with Hugging Face + PEFT

```python
# Install dependencies
# pip install transformers peft trl datasets bitsandbytes accelerate

import torch
from transformers import (
    AutoTokenizer, 
    AutoModelForCausalLM, 
    BitsAndBytesConfig,
    TrainingArguments
)
from peft import LoraConfig, get_peft_model, prepare_model_for_kbit_training
from trl import SFTTrainer
from datasets import load_dataset

# 1. Load model with 4-bit quantization (QLoRA)
bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_quant_type="nf4",
    bnb_4bit_compute_dtype=torch.bfloat16,
    bnb_4bit_use_double_quant=True
)

model_name = "meta-llama/Llama-3.1-8B-Instruct"

tokenizer = AutoTokenizer.from_pretrained(model_name)
tokenizer.pad_token = tokenizer.eos_token

model = AutoModelForCausalLM.from_pretrained(
    model_name,
    quantization_config=bnb_config,
    device_map="auto"
)

# 2. Configure LoRA
lora_config = LoraConfig(
    r=16,                    # Rank (higher = more capacity, more VRAM)
    lora_alpha=32,           # Scaling factor
    target_modules=[         # Which layers to adapt
        "q_proj", "k_proj", "v_proj", "o_proj",
        "gate_proj", "up_proj", "down_proj"
    ],
    lora_dropout=0.05,
    bias="none",
    task_type="CAUSAL_LM"
)

model = prepare_model_for_kbit_training(model)
model = get_peft_model(model, lora_config)

# Print trainable parameters
trainable = sum(p.numel() for p in model.parameters() if p.requires_grad)
total = sum(p.numel() for p in model.parameters())
print(f"Trainable: {trainable:,} / {total:,} ({100*trainable/total:.2f}%)")
# Output: ~0.5-2% of total parameters

# 3. Load and format dataset
dataset = load_dataset("json", data_files="training_data.jsonl", split="train")

def format_chat(example):
    """Format messages into chat template."""
    text = tokenizer.apply_chat_template(
        example["messages"], 
        tokenize=False, 
        add_generation_prompt=False
    )
    return {"text": text}

dataset = dataset.map(format_chat)

# 4. Training configuration
training_args = TrainingArguments(
    output_dir="./fine-tuned-model",
    num_train_epochs=3,
    per_device_train_batch_size=4,
    gradient_accumulation_steps=4,
    learning_rate=2e-4,
    logging_steps=10,
    save_strategy="epoch",
    bf16=True,
    warmup_ratio=0.1,
    lr_scheduler_type="cosine",
    optim="paged_adamw_8bit"
)

# 5. Train
trainer = SFTTrainer(
    model=model,
    train_dataset=dataset,
    tokenizer=tokenizer,
    args=training_args,
    max_seq_length=2048,
    dataset_text_field="text"
)

trainer.train()

# 6. Save the LoRA adapter
model.save_pretrained("./fine-tuned-model/adapter")
tokenizer.save_pretrained("./fine-tuned-model/adapter")

print("Fine-tuning complete!")
```

### Using Your Fine-Tuned Model

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
from peft import PeftModel
import torch

# Load base model + LoRA adapter
base_model = AutoModelForCausalLM.from_pretrained(
    "meta-llama/Llama-3.1-8B-Instruct",
    torch_dtype=torch.bfloat16,
    device_map="auto"
)

model = PeftModel.from_pretrained(base_model, "./fine-tuned-model/adapter")
tokenizer = AutoTokenizer.from_pretrained("./fine-tuned-model/adapter")

# Generate
messages = [
    {"role": "user", "content": "Your prompt here"}
]

inputs = tokenizer.apply_chat_template(
    messages, return_tensors="pt", add_generation_prompt=True
).to(model.device)

outputs = model.generate(
    inputs, 
    max_new_tokens=512, 
    temperature=0.7,
    do_sample=True
)

response = tokenizer.decode(outputs[0][inputs.shape[1]:], skip_special_tokens=True)
print(response)
```

### Merging LoRA into Base Model

```python
# Merge adapter into base model for faster inference
merged_model = model.merge_and_unload()
merged_model.save_pretrained("./fine-tuned-model/merged")
tokenizer.save_pretrained("./fine-tuned-model/merged")

# Convert to GGUF for Ollama/llama.cpp
# Use llama.cpp's convert script:
# python convert_hf_to_gguf.py ./fine-tuned-model/merged --outtype q4_k_m
```

---

## 5. Fine-Tuning Platforms

### Together.ai

```python
import together

# Upload data
file_id = together.Files.upload("training_data.jsonl")

# Start fine-tuning
response = together.Finetune.create(
    training_file=file_id,
    model="meta-llama/Llama-3.1-8B-Instruct",
    n_epochs=3,
    batch_size=4,
    learning_rate=1e-5,
    suffix="my-model"
)

# Use the model
output = together.Complete.create(
    model=response.output_name,
    prompt="Your prompt here"
)
```

### Platform Comparison

| Platform | Models Available | GPU Required | Cost | Best For |
|----------|-----------------|-------------|------|----------|
| **OpenAI** | GPT-4o, GPT-4o-mini | No (cloud) | $$  | Easiest, production |
| **Together.ai** | Llama, Mistral, + more | No (cloud) | $$ | Open models, fast |
| **Hugging Face** | Any HF model | Yes (or Spaces) | $ | Full control |
| **Unsloth** | Any HF model | Yes (local) | Free | 2x faster training |
| **Axolotl** | Any HF model | Yes (local) | Free | Config-driven |
| **Modal** | Any model | No (cloud GPUs) | $$ | Serverless GPUs |
| **Lambda Labs** | Any model | Rented GPUs | $$ | GPU cloud |

---

## 6. Unsloth -- 2x Faster Fine-Tuning

Unsloth optimizes the training loop for significantly faster fine-tuning:

```python
# pip install unsloth

from unsloth import FastLanguageModel
import torch

# Load model (Unsloth optimized)
model, tokenizer = FastLanguageModel.from_pretrained(
    model_name="unsloth/Llama-3.1-8B-Instruct",
    max_seq_length=2048,
    dtype=None,  # Auto-detect
    load_in_4bit=True
)

# Add LoRA adapters (Unsloth optimized)
model = FastLanguageModel.get_peft_model(
    model,
    r=16,
    target_modules=["q_proj", "k_proj", "v_proj", "o_proj",
                    "gate_proj", "up_proj", "down_proj"],
    lora_alpha=16,
    lora_dropout=0,
    bias="none",
    use_gradient_checkpointing="unsloth"  # 2x faster
)

# Train with standard HuggingFace Trainer
from trl import SFTTrainer
from transformers import TrainingArguments

trainer = SFTTrainer(
    model=model,
    tokenizer=tokenizer,
    train_dataset=dataset,
    max_seq_length=2048,
    args=TrainingArguments(
        output_dir="outputs",
        per_device_train_batch_size=2,
        gradient_accumulation_steps=4,
        num_train_epochs=3,
        learning_rate=2e-4,
        bf16=True,
        logging_steps=10,
        optim="adamw_8bit",
        warmup_steps=5
    )
)

trainer.train()

# Save as GGUF directly (for Ollama)
model.save_pretrained_gguf(
    "my-model-gguf",
    tokenizer,
    quantization_method="q4_k_m"
)
```

---

## 7. RLHF and Preference Tuning

### What is RLHF?

Reinforcement Learning from Human Feedback (RLHF) trains models to prefer responses that humans rate higher:

```
Traditional Fine-Tuning: "Here's the right answer" (supervised)
RLHF: "Response A is better than Response B" (preference)
```

### DPO -- Direct Preference Optimization

DPO is the simpler, more practical alternative to full RLHF:

```python
# DPO training data format
# Each example has a prompt, chosen (good) response, and rejected (bad) response

training_data = [
    {
        "prompt": "Explain quantum computing",
        "chosen": "Quantum computing uses quantum mechanical phenomena like superposition and entanglement to process information. Unlike classical bits (0 or 1), quantum bits (qubits) can exist in multiple states simultaneously, enabling certain computations to be exponentially faster.",
        "rejected": "Quantum computing is like a really fast computer that uses quantum physics. It's complicated but basically it can do things normal computers can't."
    },
    # ... more preference pairs
]
```

```python
# DPO with TRL
from trl import DPOTrainer, DPOConfig

dpo_config = DPOConfig(
    output_dir="dpo-model",
    num_train_epochs=1,
    per_device_train_batch_size=2,
    gradient_accumulation_steps=4,
    learning_rate=5e-7,
    beta=0.1,  # KL penalty coefficient
    bf16=True,
    logging_steps=10
)

dpo_trainer = DPOTrainer(
    model=model,
    ref_model=None,  # Uses implicit reference
    train_dataset=preference_dataset,
    tokenizer=tokenizer,
    args=dpo_config
)

dpo_trainer.train()
```

### When to Use Preference Tuning

| Use Case | Method |
|----------|--------|
| Improve response quality | DPO |
| Reduce harmful outputs | DPO with safety preferences |
| Match specific writing style | SFT first, then DPO |
| Align with company values | DPO with curated preferences |

---

## 8. Evaluation

### Measuring Fine-Tuning Success

```python
def evaluate_model(model, tokenizer, test_data, reference_model=None):
    """Compare fine-tuned model against test set."""
    results = []
    
    for example in test_data:
        # Generate response
        prompt = example["messages"][:-1]  # Everything except assistant response
        expected = example["messages"][-1]["content"]
        
        inputs = tokenizer.apply_chat_template(
            prompt, return_tensors="pt", add_generation_prompt=True
        ).to(model.device)
        
        outputs = model.generate(inputs, max_new_tokens=512, temperature=0.1)
        generated = tokenizer.decode(
            outputs[0][inputs.shape[1]:], skip_special_tokens=True
        )
        
        results.append({
            "prompt": prompt[-1]["content"],
            "expected": expected,
            "generated": generated
        })
    
    return results

# Then use an LLM to judge quality
def llm_judge(results):
    """Use GPT-4 to evaluate response quality."""
    scores = []
    for r in results:
        response = client.chat.completions.create(
            model="gpt-4o",
            messages=[{
                "role": "user",
                "content": f"""Rate the generated response on a scale of 1-5.
                
Prompt: {r['prompt']}
Expected: {r['expected']}
Generated: {r['generated']}

Score (1-5) and brief explanation:"""
            }]
        )
        scores.append(response.choices[0].message.content)
    return scores
```

### Evaluation Metrics

| Metric | What It Measures | When to Use |
|--------|-----------------|------------|
| **Perplexity** | How well model predicts text | General quality |
| **BLEU/ROUGE** | Overlap with reference text | Translation, summarization |
| **LLM-as-Judge** | Quality rated by GPT-4 | Open-ended generation |
| **Human evaluation** | Quality rated by humans | Final validation |
| **Task-specific accuracy** | Correctness on your task | Classification, extraction |
| **A/B testing** | User preference in production | Deployed models |

---

## 9. Deploying Fine-Tuned Models

### With Ollama (Easiest)

```bash
# After converting to GGUF
# Create a Modelfile
cat > Modelfile << 'EOF'
FROM ./my-model-q4_k_m.gguf

SYSTEM "You are a specialized assistant for..."

PARAMETER temperature 0.3
PARAMETER num_ctx 4096
EOF

# Create and run
ollama create my-custom-model -f Modelfile
ollama run my-custom-model

# Serve via API
# Already available at http://localhost:11434/v1/chat/completions
```

### With vLLM (Production)

```bash
python -m vllm.entrypoints.openai.api_server \
    --model ./fine-tuned-model/merged \
    --port 8000
```

### With Hugging Face Inference Endpoints

```python
# Push model to Hugging Face Hub
model.push_to_hub("your-username/my-fine-tuned-model")
tokenizer.push_to_hub("your-username/my-fine-tuned-model")

# Create inference endpoint via HF UI or API
# Gets a dedicated API endpoint with auto-scaling
```

---

## 10. Hands-On Projects

### Project 1: Customer Support Model

```
Goal: Fine-tune a model on your support tickets

1. Export 500+ resolved support conversations
2. Format as user/assistant message pairs
3. Fine-tune gpt-4o-mini or Llama 3.1 8B
4. Evaluate on held-out test set
5. Deploy as support chatbot
6. Monitor and collect feedback for next iteration
```

### Project 2: Code Generation Specialist

```
Goal: Fine-tune for your codebase's patterns

1. Collect code examples from your repo
2. Create instruction/response pairs:
   "Write a function that..." → actual code
3. Fine-tune CodeLlama or DeepSeek Coder
4. Evaluate on coding benchmarks + custom tests
5. Integrate with your IDE via API
```

### Project 3: Domain Expert Model

```
Goal: Create a model that's expert in your industry

1. Collect domain-specific Q&A pairs (legal, medical, finance)
2. Include technical terminology and reasoning
3. Fine-tune with QLoRA on Llama 3.1
4. Evaluate with domain expert review
5. Deploy locally for data privacy
```

---

## Summary

| Topic | Key Takeaway |
|-------|-------------|
| When to fine-tune | After prompting and RAG aren't sufficient |
| Data quality | 50 great examples beat 5,000 mediocre ones |
| OpenAI fine-tuning | Easiest path, good for production |
| LoRA/QLoRA | Fine-tune on consumer GPUs (6GB VRAM) |
| Unsloth | 2x faster training, direct GGUF export |
| DPO | Preference tuning without full RLHF complexity |
| Evaluation | LLM-as-Judge + human review + task metrics |
| Deployment | Ollama (personal), vLLM (production) |

**Key principle:** Fine-tuning is a last resort, not a first step. Always try prompting and RAG first. When you do fine-tune, invest heavily in data quality -- it's the single biggest factor in success.

**Next Module:** [Module 15: AI Career Guide](./15-ai-career-guide.md) -- Navigate the AI job market and build your career.
