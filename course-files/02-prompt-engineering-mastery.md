# Module 2: Prompt Engineering Mastery

## Overview

Prompt engineering is the skill of communicating effectively with AI models to get exactly the output you need. In 2026, with models becoming more capable, the gap between a mediocre prompt and an expert prompt can mean the difference between useless output and production-ready results.

This module teaches you every major prompting technique -- from basic to advanced -- with real examples you can use immediately.

**By the end of this module, you will:**
- Master 15+ prompting techniques with practical examples
- Understand system prompts, temperature, and model parameters
- Build reusable prompt templates for common tasks
- Chain prompts together for complex workflows
- Know which techniques work best with which models

---

## 1. Prompting Fundamentals

### 1.1 Anatomy of a Prompt

Every prompt has implicit or explicit components:

```
┌─────────────────────────────────┐
│  SYSTEM PROMPT (Role/Context)   │  ← Who the AI should be
├─────────────────────────────────┤
│  CONTEXT (Background Info)      │  ← What the AI needs to know
├─────────────────────────────────┤
│  INSTRUCTION (The Task)         │  ← What you want done
├─────────────────────────────────┤
│  INPUT DATA (If applicable)     │  ← Data to process
├─────────────────────────────────┤
│  OUTPUT FORMAT (Structure)      │  ← How you want the answer
├─────────────────────────────────┤
│  CONSTRAINTS (Rules/Limits)     │  ← Boundaries and requirements
└─────────────────────────────────┘
```

### 1.2 The Golden Rules of Prompting

1. **Be specific** -- "Write a 500-word blog post about X" beats "Write about X"
2. **Provide context** -- The model doesn't know your situation unless you tell it
3. **Show, don't just tell** -- Examples are worth 1000 words of instruction
4. **Specify format** -- JSON, markdown, bullet points, table -- say what you want
5. **Iterate** -- Your first prompt is a draft; refine based on output
6. **Use the right model** -- A $0.10/M token model might not need the same prompting as a $5/M one

---

## 2. Core Prompting Techniques

### 2.1 Zero-Shot Prompting

Give the model a task with no examples. Works best with capable models (GPT-5+, Claude Opus, Gemini Ultra).

```
# Zero-shot (just the instruction)
Classify the following customer review as Positive, Negative, or Neutral:

"The product arrived on time but the packaging was damaged. The item itself works fine."

Classification:
```

**When to use:** Simple, well-defined tasks. The model's training data includes enough examples.

**Pro tip:** Adding "Think step by step" to a zero-shot prompt often improves accuracy (this is called zero-shot chain-of-thought).

### 2.2 Few-Shot Prompting

Provide 2-5 examples before your actual query. This "teaches" the model your expected format and reasoning.

```
# Few-shot with examples
Classify these customer reviews:

Review: "Absolutely love this! Best purchase ever."
Classification: Positive
Confidence: 95%

Review: "Terrible quality. Broke after one day."
Classification: Negative
Confidence: 90%

Review: "It's okay. Does what it says but nothing special."
Classification: Neutral
Confidence: 75%

Now classify this review:
Review: "The product arrived on time but the packaging was damaged. The item itself works fine."
Classification:
Confidence:
```

**When to use:** When you need specific formatting, when the task is nuanced, or when zero-shot gives inconsistent results.

**Pro tips:**
- Use diverse examples (cover edge cases)
- Order matters -- put the most representative examples first
- 3-5 examples is the sweet spot; more rarely helps

### 2.3 Chain-of-Thought (CoT) Prompting

Ask the model to show its reasoning step by step before giving the final answer. This dramatically improves accuracy on math, logic, and complex reasoning.

```
# Chain-of-Thought
A store has 45 apples. They sell 12 in the morning and receive a shipment 
of 30 in the afternoon. A customer then buys 1/3 of the total. 
How many apples remain?

Let's solve this step by step:

1. Starting apples: 45
2. After morning sales: 45 - 12 = 33
3. After afternoon shipment: 33 + 30 = 63
4. Customer buys 1/3: 63 / 3 = 21
5. Remaining: 63 - 21 = 42

Answer: 42 apples remain.
```

**When to use:** Math problems, logic puzzles, multi-step reasoning, coding problems, analysis tasks.

**Trigger phrases:**
- "Let's think step by step"
- "Show your reasoning"
- "Walk me through your thought process"
- "Break this down into steps"

### 2.4 Tree-of-Thought (ToT) Prompting

Explore multiple reasoning paths simultaneously, then evaluate which path leads to the best answer. Think of it as brainstorming before deciding.

```
# Tree-of-Thought
I need to design a login system for a mobile app. 
Explore 3 different approaches, evaluate each, then recommend the best one.

Approach 1: [Traditional email/password]
- Pros: ...
- Cons: ...
- Security score: X/10

Approach 2: [Passwordless magic link]
- Pros: ...
- Cons: ...
- Security score: X/10

Approach 3: [Biometric + passkey]
- Pros: ...
- Cons: ...
- Security score: X/10

Evaluation: Compare all three on security, UX, and implementation cost.
Recommendation: [Best approach with justification]
```

**When to use:** Design decisions, strategy planning, creative problem-solving, when you want to avoid the model "locking in" on its first idea.

### 2.5 Self-Consistency Prompting

Ask the model to solve a problem multiple times using different approaches, then pick the most common answer.

```
# Self-Consistency
Solve this problem 3 different ways and verify your answer:

If a train travels at 60 mph for 2.5 hours, then 80 mph for 1.5 hours, 
what is the total distance?

Method 1 (Direct calculation): ...
Method 2 (Unit conversion): ...
Method 3 (Estimation then precise): ...

Consensus answer: [agreed answer from all methods]
```

**When to use:** Math, data analysis, and any task where you want high confidence in accuracy.

---

## 3. Advanced Prompting Techniques

### 3.1 System Prompts (Role Prompting)

System prompts define the AI's persona, expertise, and behavioral rules. They're the most powerful lever for consistent, high-quality output.

```
# System Prompt Example (for an API)
{
  "role": "system",
  "content": "You are a senior security engineer with 15 years of experience 
  in penetration testing and vulnerability assessment. You specialize in 
  web application security and OWASP Top 10 vulnerabilities.
  
  Rules:
  - Always provide actionable remediation steps
  - Rate vulnerabilities using CVSS 3.1 scoring
  - Include code examples for fixes when applicable
  - Never suggest illegal activities
  - Be direct and technical -- the audience is other security professionals"
}
```

**System Prompt Templates for Common Roles:**

```
# Code Reviewer
You are a principal software engineer conducting a code review. Focus on:
- Security vulnerabilities
- Performance bottlenecks
- Code maintainability
- Edge cases and error handling
Provide specific line-by-line feedback with severity ratings.

# Technical Writer
You are a senior technical writer. Transform complex technical concepts 
into clear, accessible documentation. Use:
- Short paragraphs (3-4 sentences max)
- Code examples for every concept
- Progressive disclosure (simple → complex)
- Consistent terminology throughout

# Data Analyst
You are a senior data analyst. When given data:
1. First summarize the key patterns
2. Identify anomalies or outliers
3. Provide statistical insights
4. Recommend actionable next steps
Always show your calculations and cite which data points support your conclusions.
```

### 3.2 Structured Output Prompting

Force the model to respond in a specific machine-readable format.

```
# JSON Output
Analyze this security vulnerability and respond in this exact JSON format:

{
  "vulnerability_name": "string",
  "cvss_score": number,
  "severity": "Critical|High|Medium|Low|Info",
  "affected_component": "string",
  "description": "string (max 200 words)",
  "remediation": ["step 1", "step 2", "step 3"],
  "references": ["url1", "url2"]
}

Vulnerability: SQL injection in the login form at /api/auth/login
```

**API-Level Structured Output (2026):**

OpenAI and Anthropic now support native structured output:

```python
# OpenAI Structured Output (2026)
from openai import OpenAI
from pydantic import BaseModel

class VulnReport(BaseModel):
    name: str
    cvss: float
    severity: str
    remediation: list[str]

client = OpenAI()
response = client.responses.create(
    model="gpt-5.4",
    input="Analyze: SQL injection in /api/auth/login",
    text={"format": {"type": "json_schema", "schema": VulnReport.model_json_schema()}}
)
```

```python
# Anthropic Structured Output (2026)
import anthropic

client = anthropic.Anthropic()
response = client.messages.create(
    model="claude-sonnet-4-6-20260217",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Analyze: SQL injection in /api/auth/login"}],
    # Claude uses tool_use for structured output
    tools=[{
        "name": "vuln_report",
        "description": "Output vulnerability report",
        "input_schema": {
            "type": "object",
            "properties": {
                "name": {"type": "string"},
                "cvss": {"type": "number"},
                "severity": {"type": "string"},
                "remediation": {"type": "array", "items": {"type": "string"}}
            },
            "required": ["name", "cvss", "severity", "remediation"]
        }
    }],
    tool_choice={"type": "tool", "name": "vuln_report"}
)
```

### 3.3 Prompt Chaining

Break complex tasks into a sequence of simpler prompts, where each step's output feeds into the next.

```
# Chain Example: Blog Post Pipeline

Step 1 (Research): "List the top 5 trends in AI security for 2026 with brief descriptions"
    ↓ (output feeds into step 2)
Step 2 (Outline): "Create a blog post outline from these 5 trends. Include intro, 
    one section per trend, and conclusion"
    ↓ (output feeds into step 3)
Step 3 (Draft): "Write the full blog post from this outline. Target 1500 words, 
    professional but accessible tone"
    ↓ (output feeds into step 4)
Step 4 (Edit): "Review this blog post for: factual accuracy, readability, 
    SEO optimization. Suggest specific improvements."
    ↓ (output feeds into step 5)
Step 5 (Final): "Apply these edits and produce the final version. Add a 
    compelling title and meta description."
```

**Why chaining works better than one giant prompt:**
- Each step gets focused attention
- You can inspect and course-correct between steps
- Models have a "working memory" limit -- simpler tasks = better output
- You can use different models for different steps (cheap model for research, expensive for writing)

### 3.4 Meta-Prompting

Use AI to generate better prompts. This is genuinely one of the most powerful techniques.

```
# Meta-Prompt: Generate a Better Prompt
I want to use AI to [describe your goal]. 

Generate an optimized prompt that will produce the best possible results. 
The prompt should include:
1. A clear system/role definition
2. Specific instructions with constraints
3. Output format specification
4. 2-3 few-shot examples
5. Error handling instructions

Target model: [GPT-5.4 / Claude Opus 4.6 / etc.]
Target audience: [who will read the output]
```

**Real example:**

```
Input: "I want to use AI to review my Python code for security issues"

Generated prompt:
---
You are a senior application security engineer specializing in Python. 
Review the following code for security vulnerabilities.

For each issue found, provide:
1. **Line number(s)** affected
2. **Vulnerability type** (e.g., SQL Injection, XSS, SSRF, Path Traversal)
3. **Severity** (Critical/High/Medium/Low)
4. **Explanation** of why it's dangerous
5. **Fixed code** showing the secure implementation
6. **CWE reference** number

If no vulnerabilities are found, explicitly state "No security issues detected" 
and suggest defensive improvements.

Code to review:
[paste code here]
---
```

### 3.5 Constraint-Based Prompting

Define what the AI should NOT do, in addition to what it should do.

```
# Constraint-Based Prompt
Write a product description for a smart home camera.

DO:
- Focus on security and privacy features
- Use specific technical specs
- Include a clear call-to-action
- Keep it under 200 words

DO NOT:
- Make claims about being "unhackable" or "100% secure"
- Use fear-based marketing language
- Include pricing (this changes frequently)
- Compare to competitor products by name
```

### 3.6 Recursive Refinement

Ask the model to critique and improve its own output in the same conversation.

```
# Step 1: Generate
Write a Python function that validates email addresses.

# Step 2: Critique
Now review the function you just wrote. Identify:
- Edge cases it misses
- Security concerns
- Performance issues
- Standards compliance (RFC 5322)

# Step 3: Improve
Rewrite the function addressing all the issues you identified.

# Step 4: Test
Generate 10 test cases (5 valid, 5 invalid) and trace through your 
function to verify it handles each correctly.
```

---

## 4. Model-Specific Prompting Tips

### 4.1 OpenAI GPT-5.x Tips

```
✅ GPT-5 responds well to:
- Clear system prompts with explicit roles
- Structured output requests (native JSON mode)
- "Be concise" or "Be thorough" directives
- Numbered step instructions
- Temperature 0 for deterministic tasks, 0.7-1.0 for creative

⚠️ Watch out for:
- GPT-5+ can be overly verbose without "be concise" constraint
- May refuse edge-case security/hacking questions without proper framing
- Reasoning tokens in o3/o4 models cost extra -- use wisely
```

```python
# GPT-5.4 API Example with best practices
from openai import OpenAI
client = OpenAI()

response = client.responses.create(
    model="gpt-5.4",
    instructions="You are a concise technical assistant. Answer in markdown.",
    input="Explain the OWASP Top 10 for 2025 in a comparison table",
    temperature=0.3,  # Low temp for factual content
)
print(response.output_text)
```

### 4.2 Anthropic Claude Tips

```
✅ Claude responds well to:
- XML tags for structuring prompts (Claude's favorite format)
- Detailed context in the system prompt
- "Think carefully before answering"
- Extended thinking for complex tasks (Claude Opus/Sonnet)
- Placing instructions AFTER the data (Claude reads in order)

⚠️ Watch out for:
- Claude tends to be more cautious/verbose about ethical caveats
- Use "assistant" prefill to steer response format
- Claude excels at long context -- feed it entire documents
```

```python
# Claude with XML-structured prompt (Claude's preferred format)
import anthropic
client = anthropic.Anthropic()

prompt = """
<context>
You are reviewing a web application's authentication system.
The application uses JWT tokens stored in localStorage.
</context>

<task>
Identify all security vulnerabilities in this authentication approach 
and provide specific remediation steps.
</task>

<format>
For each vulnerability, use this structure:
- **Issue**: [name]
- **Risk**: [High/Medium/Low]
- **Details**: [explanation]
- **Fix**: [specific code/config changes]
</format>
"""

response = client.messages.create(
    model="claude-opus-4-6-20260205",
    max_tokens=4096,
    messages=[{"role": "user", "content": prompt}],
    # Enable extended thinking for deep analysis
    thinking={"type": "enabled", "budget_tokens": 10000}
)
```

### 4.3 Google Gemini Tips

```
✅ Gemini responds well to:
- Multimodal inputs (images + text together)
- Direct, concise instructions
- Google-specific terminology and frameworks
- Very long context inputs (feed entire repos/document collections)
- Grounding with Google Search (enterprise feature)

⚠️ Watch out for:
- Gemini can be more conservative on content policy
- Flash models trade quality for speed -- adjust expectations
- Temperature and safety settings have big impact
```

```python
# Gemini 3 with multimodal input
import google.generativeai as genai
from PIL import Image

genai.configure(api_key="YOUR_KEY")
model = genai.GenerativeModel('gemini-3-flash')

# Analyze a screenshot for security issues
image = Image.open('login_page.png')
response = model.generate_content([
    "Analyze this login page screenshot for security and UX issues. "
    "List each issue with severity and recommendation.",
    image
])
print(response.text)
```

### 4.4 Open-Source Model Tips (DeepSeek, Qwen, Llama)

```
✅ Open-source models respond well to:
- Simpler, more direct prompts (less meta-instruction needed)
- Few-shot examples (helps smaller models significantly)
- Explicit output format specifications
- Temperature tuning (often need lower temp than proprietary models)

⚠️ Watch out for:
- Smaller models may not follow complex multi-step instructions
- May need more explicit examples for structured output
- Quantized models may lose nuance on complex reasoning
- System prompt support varies by model and framework
```

---

## 5. Prompt Templates Library

### 5.1 Code Generation Template

```
Write a [language] function/class that [description].

Requirements:
- [requirement 1]
- [requirement 2]
- [requirement 3]

Constraints:
- [constraint, e.g., "no external libraries"]
- [performance requirement]
- [compatibility requirement]

Include:
- Comprehensive error handling
- Type hints / documentation
- Unit tests (at least 5 test cases)
- Example usage

Code style: [PEP 8 / Google style / etc.]
```

### 5.2 Analysis Template

```
Analyze the following [data/text/code/system]:

[paste content]

Provide:
1. **Summary** (3-5 sentences)
2. **Key Findings** (top 5, ranked by importance)
3. **Potential Issues** (risks, vulnerabilities, problems)
4. **Recommendations** (actionable next steps)
5. **Confidence Level** (how certain are you in this analysis?)

Base your analysis only on the provided content. 
Flag any areas where you would need more information.
```

### 5.3 Debugging Template

```
I'm getting this error in my [language] code:

Error message:
```
[paste error]
```

Relevant code:
```
[paste code]
```

Context:
- [framework/library versions]
- [what you were trying to do]
- [what you've already tried]

Please:
1. Explain what's causing the error
2. Provide the exact fix
3. Explain why the fix works
4. Suggest how to prevent similar issues
```

### 5.4 Security Audit Template

```
Perform a security audit on the following [code/config/architecture]:

[paste content]

Audit scope:
- [ ] Authentication & Authorization
- [ ] Input Validation & Sanitization
- [ ] Cryptography & Data Protection
- [ ] Error Handling & Logging
- [ ] Configuration & Secrets Management
- [ ] Dependency Vulnerabilities

For each finding:
- **ID**: SEC-001, SEC-002, etc.
- **Severity**: Critical / High / Medium / Low / Informational
- **OWASP Category**: [relevant OWASP Top 10 category]
- **CWE**: [CWE number]
- **Description**: What's vulnerable and why
- **Proof of Concept**: How an attacker would exploit this
- **Remediation**: Specific code changes to fix
- **Priority**: Fix immediately / Next sprint / Backlog
```

### 5.5 Learning / Explanation Template

```
Explain [concept] at three levels:

1. **ELI5 (Explain Like I'm 5):** Simple analogy, no jargon
2. **Intermediate:** For someone who knows basic [field]
3. **Expert:** Technical depth with precise terminology

Then provide:
- A practical real-world example
- Common misconceptions about this concept
- How it connects to [related concept]
- Resources for learning more
```

---

## 6. Advanced Patterns for 2026

### 6.1 Agentic Prompting (Tool Use)

Modern models can call functions/tools. Your prompt defines what tools are available:

```python
# OpenAI Agents SDK - Tool-Using Agent
from agents import Agent, Runner, function_tool

@function_tool
def check_vulnerability(target_url: str, check_type: str) -> str:
    """Check a URL for a specific vulnerability type.
    Args:
        target_url: The URL to scan
        check_type: Type of check (xss, sqli, ssrf, open_redirect)
    """
    # Your scanning logic here
    return f"Scan results for {target_url}: ..."

@function_tool
def lookup_cve(cve_id: str) -> str:
    """Look up details about a CVE vulnerability."""
    # CVE database lookup
    return f"CVE details for {cve_id}: ..."

agent = Agent(
    name="Security Scanner",
    instructions="""You are a web security scanner assistant. 
    When given a target, plan and execute appropriate security checks.
    Always start with reconnaissance before testing.
    Report findings in a structured format.""",
    tools=[check_vulnerability, lookup_cve]
)

result = await Runner.run(agent, "Scan https://example.com for common web vulnerabilities")
```

### 6.2 Multi-Turn Strategy Prompting

For complex tasks, plan the conversation across multiple turns:

```
Turn 1: "Here's a large codebase. First, just list all the files and give 
         me a high-level architecture overview. Don't analyze for bugs yet."

Turn 2: "Good overview. Now analyze just the authentication module 
         (files X, Y, Z) for security issues."

Turn 3: "Found 3 issues. For issue #2 (the JWT vulnerability), write 
         a detailed fix with before/after code."

Turn 4: "Now write unit tests that would catch this vulnerability."
```

**Why this works better than one massive prompt:**
- You validate understanding before deep analysis
- Each turn gets the model's full attention budget
- You can redirect if the model misunderstands

### 6.3 Persona Stacking

Combine multiple expert perspectives in one prompt:

```
Review this system architecture from three perspectives:

1. As a **Security Architect**: Identify attack surfaces and threat vectors
2. As a **Performance Engineer**: Find bottlenecks and scaling issues  
3. As a **DevOps Engineer**: Evaluate deployment, monitoring, and resilience

For each perspective, provide:
- Top 3 concerns (ranked by severity)
- Specific recommendations
- Estimated effort to fix (hours/days/weeks)

Then synthesize: What are the top 5 changes that would have the most 
impact across all three dimensions?
```

### 6.4 Adversarial Prompting (Red Team Your Own Prompts)

Test your prompts by trying to break them:

```
# Step 1: Create your prompt
[your prompt here]

# Step 2: Ask the model to attack it
"You are a prompt engineer testing for robustness. 
Find 5 ways a user could make this prompt produce 
unintended, incorrect, or harmful output. Then suggest 
fixes for each."

# Step 3: Harden based on findings
[updated prompt with guardrails]
```

---

## 7. Temperature and Parameter Guide

### 7.1 Temperature

```
Temperature controls randomness in output:

0.0  → Deterministic (same input = same output)
       Best for: Code generation, factual Q&A, data extraction

0.3  → Low creativity, high consistency
       Best for: Technical writing, analysis, summarization

0.7  → Balanced (default for most models)
       Best for: General conversation, explanations, brainstorming

1.0  → High creativity, more varied
       Best for: Creative writing, idea generation, marketing copy

1.5+ → Very random (may produce incoherent output)
       Best for: Experimental, surreal creative work
```

### 7.2 Other Key Parameters

| Parameter | What It Does | Recommended Setting |
|-----------|-------------|--------------------|
| `max_tokens` | Maximum response length | Set explicitly to avoid cutoff |
| `top_p` | Nucleus sampling (diversity) | 0.9-1.0 for most tasks |
| `frequency_penalty` | Reduce word repetition | 0.1-0.5 for long outputs |
| `presence_penalty` | Encourage topic diversity | 0.1-0.5 for brainstorming |
| `stop` | Stop sequences | Use to control output boundaries |
| `seed` | Reproducibility | Set for consistent outputs |

---

## 8. Common Prompting Mistakes

### What Not to Do

```
❌ Mistake 1: Vague instructions
"Write something about cybersecurity"

✅ Fix:
"Write a 1000-word guide explaining the top 5 cybersecurity threats 
facing small businesses in 2026, with specific mitigation steps for 
each. Target audience: non-technical business owners."

---

❌ Mistake 2: Overloading a single prompt
"Analyze this code, fix all bugs, add tests, optimize performance, 
add documentation, refactor for clean code, and deploy to production"

✅ Fix: Break into a prompt chain (see Section 3.3)

---

❌ Mistake 3: Not specifying output format
"List some programming languages"

✅ Fix:
"List the top 10 programming languages by 2026 job demand in a table with:
| Rank | Language | Primary Use Case | Avg Salary (US) | Trend |"

---

❌ Mistake 4: Ignoring the system prompt
[Putting everything in the user message]

✅ Fix: Use system prompts for persistent instructions, user messages for specific tasks

---

❌ Mistake 5: Not iterating
[Accepting first output as final]

✅ Fix: "Good start. Now improve it by: [specific feedback]"
```

---

## 9. Practical Exercises

### Exercise 1: Prompt Battle
Take any task and write 3 different prompts for it:
1. A basic zero-shot prompt
2. A few-shot prompt with examples
3. A chain-of-thought prompt with system role

Compare the outputs. Which technique worked best?

### Exercise 2: Meta-Prompt Generator
Use AI to generate an optimized prompt for:
- Reviewing a pull request for security issues
- Writing API documentation from code
- Creating a test plan for a web application

### Exercise 3: Build a Prompt Library
Create a personal collection of 10 reusable prompt templates for tasks you do frequently. Test each with at least 3 different inputs.

### Exercise 4: Adversarial Testing
Take your best prompt template and try to break it:
- Feed it unexpected input formats
- Try to make it ignore its instructions
- Test with edge cases (empty input, extremely long input, multilingual)

---

## 10. Key Takeaways

1. **Prompting is a skill** -- It improves with practice and experimentation
2. **Structure beats length** -- A well-structured 5-line prompt beats a rambling 50-line one
3. **Chain-of-thought is your most valuable technique** -- Use it for anything requiring reasoning
4. **System prompts are underused** -- They're the single biggest lever for output quality
5. **Different models need different approaches** -- XML for Claude, JSON for GPT, direct for Gemini
6. **Meta-prompting saves time** -- Let AI help you write better prompts
7. **Always iterate** -- Prompt engineering is an iterative design process
8. **Templates are productivity multipliers** -- Build a personal library

---

**Next Module:** [Module 3: Mastering ChatGPT, Claude & Gemini](03-chatgpt-claude-gemini.md) -- Deep-dive into getting maximum value from each platform.
