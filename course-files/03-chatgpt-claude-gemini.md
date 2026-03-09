# Module 3: Mastering ChatGPT, Claude & Gemini

## Overview

The three dominant AI platforms each have unique features, workflows, and strengths that most users never discover. This module goes beyond basic chatting to unlock the full power of each platform -- from Canvas and Artifacts to Deep Research and Projects.

**By the end of this module, you will:**
- Master advanced features of ChatGPT, Claude, and Gemini
- Know which platform to use for which task
- Build persistent workflows using Projects, Gems, and custom instructions
- Use Deep Research features for comprehensive analysis
- Leverage memory and context features for ongoing work

---

## 1. ChatGPT (OpenAI) -- Deep Dive

### 1.1 Subscription Tiers (March 2026)

| Tier | Price | Models | Key Features |
|------|-------|--------|-------------|
| Free | $0 | GPT-4o (limited) | Basic chat, limited messages |
| Go | $8/mo | GPT-5.2 Instant | 10x free messages, faster |
| Plus | $20/mo | GPT-5.4, o3, DALL-E, Sora | Canvas, Deep Research, voice |
| Pro | $200/mo | Unlimited GPT-5.4, o3-pro | Unlimited everything, priority |
| Team | $25/user/mo | All Plus models | Shared workspace, admin controls |
| Enterprise | Custom | All models + fine-tuning | SSO, compliance, dedicated support |

### 1.2 ChatGPT Canvas

Canvas is ChatGPT's side-by-side editing environment for documents and code. It launched in late 2024 and has been significantly improved.

**How to Use Canvas:**
1. Start a conversation about writing or coding
2. ChatGPT automatically opens Canvas when appropriate, or you can say "Open in Canvas"
3. Edit directly in the canvas while chatting on the left
4. Use inline suggestions, highlighting, and revision history

**Canvas Writing Features:**
- **Adjust Length** -- Make text shorter or longer while preserving meaning
- **Reading Level** -- Rewrite for different audiences (graduate school to kindergarten)
- **Add Polish** -- Grammar, clarity, and style improvements
- **Add Emojis** -- Context-appropriate emoji insertion
- **Suggest Edits** -- Inline tracked changes you can accept/reject

**Canvas Coding Features:**
- **Fix Bugs** -- Automatic bug detection and fixing
- **Add Logs** -- Insert console.log/print statements for debugging
- **Add Comments** -- Generate documentation comments
- **Port Language** -- Convert code between languages
- **Code Review** -- Inline suggestions for improvements

**Pro Tips for Canvas:**
```
- Highlight specific text and ask for changes ("make this section more technical")
- Use "Undo" to go back through revision history
- Canvas persists across conversation turns -- build documents iteratively
- Say "Start a new canvas" to begin fresh while keeping chat context
- Export canvas content directly to files
```

### 1.3 ChatGPT Deep Research

Deep Research is ChatGPT's autonomous research agent. It browses the web, reads multiple sources, and produces comprehensive reports.

**How to Trigger Deep Research:**
- Click the "Deep Research" button in Plus/Pro
- Or ask: "Do deep research on [topic]"
- Specify scope: "Research this topic. Check at least 20 sources."

**What Deep Research Does:**
1. Creates a research plan (you can review/modify)
2. Searches the web across multiple queries
3. Reads and analyzes dozens of pages
4. Synthesizes findings into a structured report
5. Includes citations and source links

**Best Practices:**
```
- Be specific about what you want to learn
- Tell it your expertise level so it calibrates depth
- Ask for comparison tables when evaluating options
- Request it check for conflicting information between sources
- Use it for market research, competitive analysis, literature reviews
```

**Example Deep Research Prompt:**
```
Do deep research on the current state of AI agent frameworks in 2026.

I need:
1. A comparison of the top 5 frameworks (features, performance, community)
2. Pricing/licensing details for each
3. Real-world production use cases
4. Known limitations and gotchas
5. Your recommendation for a startup building a customer service agent

Check at least 15 sources. Include recent benchmarks if available.
```

### 1.4 ChatGPT Memory

ChatGPT remembers facts about you across conversations.

**How Memory Works:**
- ChatGPT automatically saves relevant facts ("User is a security engineer")
- You can explicitly say: "Remember that I prefer Python over JavaScript"
- View/delete memories: Settings > Personalization > Memory
- Memories persist across all conversations

**Strategic Memory Usage:**
```
Tell ChatGPT:
- Your role and expertise level
- Your preferred programming languages and frameworks
- Your writing style preferences
- Your project names and contexts
- Your company/team conventions
- Your timezone and location (for relevant recommendations)

Example:
"Remember: I'm a senior pentester specializing in web and API security.
I use Burp Suite, Python, and Kali Linux. I prefer concise, technical
responses with code examples. My reports follow OWASP format."
```

### 1.5 Custom GPTs

Build specialized ChatGPT instances for specific tasks.

**Creating a Custom GPT:**
1. Go to ChatGPT > Explore GPTs > Create
2. Use the GPT Builder (conversational) or Configure (manual)
3. Define: Name, description, instructions, conversation starters
4. Add capabilities: Web browsing, DALL-E, Code Interpreter
5. Upload knowledge files (PDFs, docs, CSVs)
6. Optionally add API actions (connect to external services)

**Custom GPT Ideas for Security Professionals:**
```
- "CVE Analyzer" -- Upload NIST CVE database, analyze vulnerabilities
- "Pentest Reporter" -- Generate professional pentest reports from notes
- "Code Auditor" -- Specialized security code review with OWASP rules
- "Threat Modeler" -- STRIDE/DREAD threat modeling assistant
- "Compliance Checker" -- SOC2/ISO27001/PCI-DSS compliance analysis
```

### 1.6 ChatGPT Operator

Operator is ChatGPT's web automation agent (Pro tier).

**What Operator Can Do:**
- Browse any website and take actions
- Fill forms, click buttons, navigate pages
- Book restaurants, order products, manage accounts
- Extract data from web pages
- Multi-step workflows across multiple sites

**Using Operator:**
```
"Go to GitHub and create a new repository called 'my-project' 
with a Python .gitignore and MIT license"

"Search Amazon for USB-C hubs with 4K HDMI under $50, 
compare the top 3 by reviews, and show me a comparison table"

"Log into my Jira and list all critical bugs assigned to me"
```

---

## 2. Claude (Anthropic) -- Deep Dive

### 2.1 Subscription Tiers (March 2026)

| Tier | Price | Models | Key Features |
|------|-------|--------|-------------|
| Free | $0 | Sonnet 4.5 (limited) | Basic chat, limited messages |
| Pro | $20/mo | Opus 4.6, Sonnet 4.6 | Projects, Artifacts, more messages |
| Team | $25/user/mo | All Pro models | Shared workspace, admin |
| Enterprise | Custom | All + fine-tuning | SSO, SCIM, audit logs |
| API | Pay-per-token | All models | Full programmatic access |

### 2.2 Claude Artifacts

Artifacts are Claude's interactive content creation feature. They create standalone, runnable content in a side panel.

**Types of Artifacts:**
- **Code** -- Syntax-highlighted, copyable code blocks
- **HTML/React** -- Live-rendering web pages and React components
- **SVG** -- Vector graphics and diagrams
- **Mermaid** -- Flowcharts, sequence diagrams, architecture diagrams
- **Markdown** -- Formatted documents

**Artifact Superpowers:**
```
# Create an interactive dashboard
"Build an interactive React dashboard that visualizes network traffic data.
Include charts for bandwidth, top talkers, and protocol distribution.
Use dummy data but make it realistic."
→ Claude creates a working React app in the artifact panel
→ You can interact with it, see it render live
→ Download the code or iterate on it

# Create a diagram
"Draw a network architecture diagram showing a DMZ with:
- External firewall
- Web servers in DMZ
- Application servers in internal network
- Database servers in restricted zone
- IDS/IPS at each boundary
Use Mermaid syntax."
→ Claude creates a rendered Mermaid diagram

# Create a working tool
"Build an HTML page with a CVSS 3.1 calculator. It should have dropdowns
for each metric, calculate the score in real-time, and show the severity
rating with color coding."
→ Claude creates a fully working CVSS calculator you can use immediately
```

### 2.3 Claude Projects

Projects give Claude persistent context and knowledge for ongoing work.

**Setting Up a Project:**
1. Go to Claude.ai > Projects > New Project
2. Add a project description (becomes the system prompt)
3. Upload knowledge files (up to 200K tokens of context)
4. Start conversations within the project

**Project Use Cases:**
```
Project: "Security Audit - Client X"
- Description: "You're helping me audit Client X's web application..."
- Files: Architecture docs, API specs, previous audit reports
- All conversations in this project have access to these files

Project: "Python Library Development"
- Description: "You're helping me develop a Python security scanning library..."
- Files: Current codebase, API design docs, test files
- Claude remembers your coding conventions and architecture decisions

Project: "Learning Rust"
- Description: "You're my Rust programming tutor. I know Python well..."
- Files: Rust book chapters, my practice exercises
- Progressive learning with persistent context
```

### 2.4 Claude Code (Terminal Agent)

Claude Code is Anthropic's terminal-based AI coding agent. It's the most powerful coding assistant for developers who live in the terminal.

**Installation and Setup:**
```bash
# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Authenticate
claude login

# Start in your project directory
cd /path/to/your/project
claude
```

**What Claude Code Can Do:**
- Read and understand your entire codebase
- Edit multiple files simultaneously
- Run tests and fix failures
- Execute shell commands
- Create new files and directories
- Git operations (commit, branch, push)
- Install dependencies
- Debug runtime errors

**Claude Code Workflows:**
```bash
# Code review
claude "Review the last 3 commits for security issues"

# Bug fixing
claude "The login endpoint returns 500 when email contains a plus sign. Fix it."

# Feature development
claude "Add rate limiting to all API endpoints. Use Redis. Add tests."

# Refactoring
claude "Refactor the authentication module to use the repository pattern"

# Documentation
claude "Generate API documentation for all endpoints in /routes/"
```

**Claude Code with MCP:**
```bash
# Connect to databases, APIs, and other tools via MCP
claude mcp add postgres -- npx @anthropic-ai/mcp-postgres postgresql://localhost/mydb
claude mcp add github -- npx @anthropic-ai/mcp-github

# Now Claude Code can query your database and access GitHub directly
claude "Check if there are any users with admin role who haven't logged in for 90 days"
```

### 2.5 Claude Computer Use

Claude can control your computer -- moving the mouse, clicking, typing, and reading the screen.

**Use Cases:**
- Automated testing (navigate app like a real user)
- Data entry across legacy systems
- Automated screenshots and documentation
- Testing accessibility of web applications
- Workflow automation on desktop apps

### 2.6 Extended Thinking

Claude Opus and Sonnet 4.6 support "extended thinking" -- the model reasons internally before responding.

```python
# API: Enable extended thinking
response = client.messages.create(
    model="claude-opus-4-6-20260205",
    max_tokens=16000,
    thinking={
        "type": "enabled",
        "budget_tokens": 10000  # How much "thinking" to allow
    },
    messages=[{"role": "user", "content": "Analyze this code for race conditions..."}]
)

# Access the thinking process
for block in response.content:
    if block.type == "thinking":
        print("Thinking:", block.thinking)  # See Claude's reasoning
    elif block.type == "text":
        print("Answer:", block.text)
```

**When to Use Extended Thinking:**
- Complex security analysis
- Mathematical proofs or calculations
- Multi-step logic problems
- Architecture design decisions
- Code that requires careful reasoning about edge cases

---

## 3. Google Gemini -- Deep Dive

### 3.1 Subscription Tiers (March 2026)

| Tier | Price | Models | Key Features |
|------|-------|--------|-------------|
| Free | $0 | Gemini 3 Flash | Basic chat, limited features |
| Advanced | $20/mo | Gemini 3 Ultra | 2M context, Deep Research, Gems |
| Google One AI Premium | $20/mo | Same as Advanced | Bundled with 2TB storage |
| Workspace | Included | Gemini in Docs/Sheets/etc. | AI in Google apps |
| AI Studio | Free | All models via API | Developer playground |
| Vertex AI | Pay-per-use | Enterprise models | Enterprise platform |

### 3.2 Gemini's Unique Strengths

**2M Token Context Window:**
Gemini 3's massive context window lets you process entire codebases, book collections, or video transcripts in a single conversation.

```
Practical Uses of 2M Context:
- Upload an entire GitHub repository and ask questions about it
- Analyze hours of meeting transcripts at once
- Compare multiple research papers simultaneously
- Process entire legal contracts for review
- Feed complete API documentation for integration planning
```

**Native Multimodal Understanding:**
```
Gemini natively processes:
- Images (photos, screenshots, diagrams, charts)
- Video (upload clips, YouTube analysis)
- Audio (transcription, analysis, music)
- Documents (PDFs with layout understanding)
- Code (with execution in Colab)

Example:
"Here's a 45-minute conference talk video. Summarize the key points,
list all tools mentioned, and create action items from the Q&A section."
```

### 3.3 Gemini Gems

Gems are Gemini's version of custom AI assistants (similar to Custom GPTs).

**Creating a Gem:**
1. Go to Gemini > Gems > Create Gem
2. Define the Gem's role and instructions
3. Add conversation starters
4. Save and access from your Gem library

**Gem Examples:**
```
Gem: "Security Brief Writer"
Instructions: "You help write daily security briefings for executive 
leadership. Keep language non-technical but accurate. Always include:
risk level, business impact, and recommended actions. Format as a
one-page brief with bullet points."

Gem: "Architecture Reviewer"
Instructions: "Review system architectures for scalability, security, 
and cost optimization. Use the AWS Well-Architected Framework. 
Always provide a severity-ranked list of findings."
```

### 3.4 Google AI Studio (Free Developer Platform)

AI Studio is Google's free playground for Gemini models -- and it's one of the best free AI resources available.

**What You Get Free:**
- Access to Gemini 3 Flash and Pro models
- API key generation (no credit card required)
- System prompt testing
- Structured output testing
- Token counting and cost estimation
- Code generation in multiple languages

**AI Studio as a Development Tool:**
```
1. Go to aistudio.google.com
2. Click "Get API Key" (instant, free)
3. Test prompts in the playground
4. Export working prompts as Python/JavaScript/cURL code
5. Use the free API tier for development and prototyping
```

### 3.5 NotebookLM

Google's AI research and note-taking tool. Upload sources and have AI discussions grounded in your documents.

**NotebookLM Features:**
- Upload PDFs, Google Docs, websites, YouTube videos as sources
- AI answers are grounded in your sources (with citations)
- Audio Overview -- generates a podcast-style discussion of your sources
- Collaborative notebooks with team members
- Mind map generation from source material

**Use Cases:**
```
- Upload security standards (NIST, ISO 27001) and query specific requirements
- Feed it RFCs and ask for implementation guidance
- Upload meeting recordings and generate structured minutes
- Research paper analysis with cross-referencing
- Study guide generation from course materials
```

### 3.6 Gemini in Google Workspace

Gemini is integrated across Google's productivity suite:

```
Gmail:
- "Draft a reply to this email" (context-aware)
- "Summarize this email thread"
- Smart compose with AI suggestions

Google Docs:
- "Help me write" sidebar
- Summarize, rewrite, or expand selected text
- Generate entire documents from prompts

Google Sheets:
- "Help me organize" for data structuring
- Formula generation from natural language
- Data analysis and visualization suggestions

Google Slides:
- Generate presentations from prompts
- Create speaker notes
- Design suggestions

Google Meet:
- Real-time captions and translation
- Meeting summaries and action items
- "Take notes for me" feature
```

---

## 4. Head-to-Head Comparison

### 4.1 Feature Matrix

| Feature | ChatGPT | Claude | Gemini |
|---------|---------|--------|--------|
| Max Context | 256K | 200K (1M beta) | 2M |
| Code Execution | Code Interpreter | Claude Code (terminal) | Colab integration |
| Image Generation | DALL-E 3, Sora | No (planned) | Imagen 4 |
| Image Understanding | Yes | Yes | Yes (best) |
| Video Understanding | Limited | No | Yes (native) |
| Audio Understanding | Whisper/voice | No | Yes (native) |
| Web Browsing | Yes (built-in) | No (API tools) | Yes (with Search grounding) |
| File Upload | Yes (many formats) | Yes (many formats) | Yes (many formats) |
| Memory | Cross-conversation | Per-project | Cross-conversation |
| Desktop Control | Operator | Computer Use | No |
| IDE Integration | Via Copilot | Claude Code + MCP | Via AI Studio |
| Custom Bots | Custom GPTs | Projects | Gems |
| Deep Research | Yes (built-in) | No (planned) | Yes (built-in) |
| Free API | Limited | Limited | Generous (AI Studio) |
| Structured Output | Native JSON mode | Tool use schema | JSON mode |
| Extended Thinking | o3/o4 models | Built-in (Opus/Sonnet) | Gemini 3 (adaptive) |

### 4.2 Best Platform by Task

```
Coding:           Claude (Opus 4.6 or Claude Code) > ChatGPT (GPT-5.3-Codex) > Gemini
Long Documents:   Gemini (2M) > Claude (1M beta) > ChatGPT (256K)
Creative Writing:  ChatGPT > Claude > Gemini
Research:         ChatGPT (Deep Research) = Gemini (Deep Research) > Claude
Image Generation:  ChatGPT (DALL-E/Sora) > Gemini (Imagen) > Claude (none)
Video Analysis:    Gemini >> ChatGPT > Claude
Security Audit:    Claude (extended thinking) > ChatGPT (o3) > Gemini
Data Analysis:     ChatGPT (Code Interpreter) > Gemini (Colab) > Claude
Free Usage:        Gemini (AI Studio) >> ChatGPT (free tier) > Claude (free tier)
Enterprise:        All three are competitive
Desktop Automation: Claude (Computer Use) > ChatGPT (Operator) > Gemini
```

---

## 5. Power User Workflows

### 5.1 Multi-Platform Research Pipeline

```
Step 1: Gemini (Deep Research)
→ "Research the top 10 AI security startups in 2026. Get detailed info on each."
→ Gemini uses its Search grounding to find current information

Step 2: Claude (Analysis)
→ Paste Gemini's research into a Claude Project
→ "Analyze these 10 companies. Create a SWOT analysis for each.
   Identify which ones are potential acquisition targets."
→ Claude's extended thinking provides deeper analysis

Step 3: ChatGPT (Presentation)
→ "Create a board presentation from this analysis. Include executive
   summary, comparison matrix, and recommendation slides."
→ ChatGPT Canvas creates polished content
```

### 5.2 Code Development Pipeline

```
Step 1: ChatGPT (Architecture)
→ "Design a microservices architecture for a vulnerability scanning platform"
→ Get high-level design, API contracts, tech stack recommendation

Step 2: Claude Code (Implementation)
→ cd /project && claude
→ "Implement the architecture from this design doc. Start with the 
   scanning service. Use FastAPI, PostgreSQL, and Redis."
→ Claude Code creates files, writes code, runs tests

Step 3: Gemini (Review)
→ Upload entire codebase to Gemini (uses 2M context)
→ "Review this entire codebase for security issues, performance 
   bottlenecks, and architecture inconsistencies"
```

### 5.3 Security Assessment Pipeline

```
Step 1: Claude (Threat Model)
→ Upload architecture diagrams and API specs to Claude Project
→ "Create a comprehensive threat model using STRIDE methodology"

Step 2: ChatGPT (Attack Planning)
→ "Based on this threat model, create a penetration test plan.
   Prioritize by risk. Include specific tools and techniques."

Step 3: Claude Code (Automation)
→ "Write Python scripts to automate the reconnaissance phase 
   of this pentest plan. Use nmap, subfinder, and httpx."

Step 4: Gemini (Reporting)
→ Feed all findings into Gemini with report template
→ "Generate a professional penetration test report from these findings"
```

---

## 6. Practical Exercises

### Exercise 1: Platform Comparison
Ask all three platforms the exact same complex question and compare:
- Quality and depth of answer
- Response speed
- Format and readability
- Accuracy (verify facts)

### Exercise 2: Build Custom Assistants
- Create a Custom GPT for a task you do weekly
- Create a Claude Project for an ongoing project
- Create a Gemini Gem for a specialized domain

### Exercise 3: Multi-Platform Workflow
Complete a real work task using all three platforms:
1. Research with one
2. Analyze with another
3. Create deliverable with the third

### Exercise 4: Push the Limits
- Test Gemini's 2M context with a large codebase
- Test Claude's extended thinking on a complex logic problem
- Test ChatGPT's Deep Research on a niche topic

---

## 7. Key Takeaways

1. **No single platform wins everything** -- Use the right tool for each task
2. **ChatGPT excels at** -- Creative content, deep research, DALL-E/Sora, broad ecosystem
3. **Claude excels at** -- Coding (Claude Code), long-form analysis, extended thinking, MCP tools
4. **Gemini excels at** -- Massive context (2M), multimodal (video/audio), free API, Google integration
5. **Custom assistants are underused** -- GPTs, Projects, and Gems dramatically improve quality
6. **Memory/context persistence is a game-changer** -- Set it up once, benefit forever
7. **Multi-platform workflows give the best results** -- Combine strengths for complex tasks

---

**Next Module:** [Module 4: AI Coding Tools](04-ai-coding-tools.md) -- Master Cursor, Windsurf, GitHub Copilot, and other AI coding environments.
