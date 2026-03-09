# Module 4: AI Coding Tools

## Overview

AI coding tools have transformed software development. In 2026, AI writes 50-80% of boilerplate code, catches bugs before they ship, and can autonomously implement entire features. This module covers every major AI coding tool -- from IDE integrations to terminal agents -- with setup guides, workflows, and honest comparisons.

**By the end of this module, you will:**
- Set up and master 7+ AI coding environments
- Know which tool to use for which coding task
- Build efficient AI-assisted development workflows
- Understand the strengths and limitations of each tool
- Combine multiple tools for maximum productivity

---

## 1. Cursor -- The AI-First IDE

### 1.1 What Is Cursor?

Cursor is a VS Code fork rebuilt from the ground up for AI-assisted coding. It's the most popular AI coding IDE in 2026.

**Installation:**
```bash
# Download from cursor.com
# Available for macOS, Windows, Linux
# Imports all VS Code extensions, settings, and themes
```

**Pricing (March 2026):**
| Tier | Price | Features |
|------|-------|----------|
| Hobby | Free | 2000 completions, 50 slow premium requests/mo |
| Pro | $20/mo | Unlimited completions, 500 fast premium requests/mo |
| Business | $40/user/mo | Everything + admin, SSO, team features |

### 1.2 Cursor Key Features

**Tab Completion (Copilot++):**
- Predicts your next edit across multiple lines
- Understands your editing patterns (not just line completion)
- Works with your codebase context
- Press Tab to accept, Esc to reject

**Cmd+K (Inline Editing):**
```
# Select code, press Cmd+K, type instruction:
"Add error handling for network timeout and retry 3 times"
"Convert this to async/await"
"Add TypeScript types to all parameters"
"Make this function pure (no side effects)"
```

**Cmd+L (Chat Panel):**
- Full conversation with AI about your code
- Automatically includes relevant files as context
- Can edit files directly from chat responses
- Supports image input (paste screenshots of errors/designs)

**Composer (Multi-File Agent):**
```
Composer is Cursor's most powerful feature -- an AI agent that can:
- Create and edit multiple files simultaneously
- Understand your entire project structure
- Run terminal commands
- Install dependencies
- Create tests, documentation, and configs

Trigger: Cmd+Shift+I or click "Composer" in sidebar

Example:
"Add a user authentication system with JWT tokens. 
Create the auth routes, middleware, database models, 
and tests. Use the existing Express setup in server.js."
→ Composer creates/edits 5-8 files with working auth system
```

**Agent Mode (2026):**
```
Cursor Agent can:
- Plan multi-step implementations
- Execute terminal commands (npm install, git, etc.)
- Run tests and fix failures autonomously
- Iterate until the task is complete
- Use MCP tools to connect to external services

Enable: Settings > Features > Agent Mode
Trigger: Use Composer with agent mode toggle ON

Example:
"Set up a CI/CD pipeline for this project. 
Create GitHub Actions workflow for testing, 
linting, and deployment to AWS Lambda."
→ Agent creates workflow files, tests them, fixes issues
```

### 1.3 Cursor Pro Tips

```
1. Use .cursorrules file in project root:
   - Define coding standards, frameworks, and preferences
   - Cursor reads this file for every request
   
   Example .cursorrules:
   ---
   You are an expert Python developer.
   - Use Python 3.12+ features
   - Follow PEP 8 strictly
   - Use type hints everywhere
   - Prefer dataclasses over dicts
   - Write pytest tests for every function
   - Use httpx instead of requests
   ---

2. @ References in chat:
   - @file -- reference specific files
   - @folder -- reference entire directories
   - @codebase -- search entire project
   - @web -- search the web for latest docs
   - @docs -- reference documentation sites
   - @git -- reference git history

3. Model selection:
   - Use Claude Opus 4.6 for complex refactoring
   - Use GPT-5.3-Codex for fast coding tasks
   - Use Gemini 3 for large codebase understanding
   - Switch models per request based on task

4. Context management:
   - Pin important files in chat (they stay in context)
   - Use @codebase for project-wide questions
   - Add README and architecture docs to context for better results
```

---

## 2. Windsurf (Codeium) -- The Flow State IDE

### 2.1 What Is Windsurf?

Windsurf (formerly Codeium) is an AI IDE focused on keeping developers in a "flow state" with deep codebase awareness and autonomous capabilities.

**Pricing (March 2026):**
| Tier | Price | Features |
|------|-------|----------|
| Free | $0 | Unlimited autocomplete, 5 agent actions/day |
| Pro | $15/mo | Unlimited agent actions, all models |
| Enterprise | Custom | SSO, audit logs, custom models |

### 2.2 Windsurf Key Features

**Cascade (AI Agent):**
```
Cascade is Windsurf's agentic coding assistant:
- Deep codebase understanding (indexes your entire project)
- Multi-file editing with automatic dependency tracking
- Terminal command execution
- Automatic context gathering (finds relevant files for you)
- Iterative bug fixing (runs code, sees errors, fixes them)

Trigger: Open Cascade panel > type your request

Example:
"The payment processing is failing for subscriptions. 
Find the bug and fix it."
→ Cascade searches codebase, finds the issue, edits the correct files
```

**Supercomplete:**
```
More than autocomplete -- Windsurf predicts your INTENT:
- Suggests entire function implementations
- Predicts multi-line edits based on what you're doing
- Learns from your coding patterns in the current session
- Adapts to your project's coding style
```

**Windsurf vs Cursor:**
```
Windsurf advantages:
- Better free tier (unlimited autocomplete)
- Deeper automatic context gathering
- Smoother "flow state" experience
- Better at understanding intent without explicit instructions

Cursor advantages:
- More model choices (OpenAI, Anthropic, Google, custom)
- Larger community and ecosystem
- More powerful Composer for complex tasks
- Better MCP integration
- More mature extension support
```

---

## 3. GitHub Copilot -- The Ecosystem Leader

### 3.1 What Is GitHub Copilot?

GitHub Copilot is the most widely adopted AI coding assistant, deeply integrated with GitHub's ecosystem.

**Pricing (March 2026):**
| Tier | Price | Features |
|------|-------|----------|
| Free | $0 | 2000 completions/mo, 50 chat messages/mo |
| Pro | $10/mo | Unlimited completions and chat |
| Business | $19/user/mo | Organization management, policy controls |
| Enterprise | $39/user/mo | Fine-tuned models, knowledge bases |

### 3.2 Copilot Key Features

**Copilot Chat:**
```
Available in VS Code, JetBrains, Neovim, and GitHub.com:
- Ask questions about code in context
- Generate code from natural language
- Explain complex code
- Fix errors and bugs
- Write tests and documentation

Commands:
/explain  -- Explain selected code
/fix      -- Fix bugs in selected code
/tests    -- Generate unit tests
/doc      -- Add documentation
/optimize -- Suggest performance improvements
```

**Copilot Workspace:**
```
GitHub's AI-native development environment:
1. Start from a GitHub Issue
2. Copilot creates a plan (files to create/modify)
3. Review and adjust the plan
4. Copilot implements changes across multiple files
5. Review, test, and create a PR

Perfect for:
- Issue-driven development
- Open-source contributions
- Team workflows where issues are well-documented
```

**Copilot in the CLI:**
```bash
# Install GitHub Copilot CLI
gh extension install github/gh-copilot

# Ask for command help
gh copilot suggest "find all Python files modified in the last 7 days"
# → find . -name "*.py" -mtime -7

gh copilot explain "awk '{sum+=$3} END {print sum}' data.csv"
# → Explains what the awk command does
```

**Copilot Code Review:**
```
Automatic AI code review on pull requests:
- Detects bugs, security issues, and style violations
- Suggests improvements inline on the PR
- Learns from your team's coding patterns
- Integrates with existing CI/CD checks

Enable: Repository Settings > Code Review > Copilot
```

### 3.3 Copilot Pro Tips

```
1. Use comments as prompts:
   # Function to validate email addresses using RFC 5322 regex
   # Returns True if valid, raises ValueError with specific reason if not
   → Copilot generates the full function

2. Open relevant files as tabs:
   - Copilot uses open files as context
   - Open interfaces, types, and related files for better suggestions

3. Use Copilot Chat for exploration:
   @workspace "How does the authentication flow work in this project?"
   @workspace "Find all places where we handle user input without sanitization"

4. Custom instructions file (.github/copilot-instructions.md):
   - Define project-specific coding standards
   - Copilot reads this for every suggestion
```

---

## 4. Claude Code -- The Terminal Coding Agent

### 4.1 What Is Claude Code?

Claude Code is Anthropic's agentic coding tool that runs in your terminal. Unlike IDE integrations, it's a standalone agent that can independently navigate, understand, and modify your codebase.

**Installation:**
```bash
npm install -g @anthropic-ai/claude-code
claude login
```

**Pricing:** Uses your Claude API credits (Opus 4.6 by default)

### 4.2 Claude Code Workflows

**Explore a New Codebase:**
```bash
cd /path/to/new-project
claude
> "Give me a high-level overview of this project. 
   What does it do, what's the tech stack, and 
   what's the directory structure?"
```

**Implement a Feature:**
```bash
claude "Add WebSocket support for real-time notifications. 
       Use the existing auth middleware. Create a notifications 
       service, WebSocket handler, and update the client SDK."

# Claude will:
# 1. Read relevant existing code
# 2. Plan the implementation
# 3. Create new files
# 4. Modify existing files
# 5. Run tests to verify
```

**Debug a Problem:**
```bash
claude "The API returns 500 when creating a user with a 
       unicode name. Find and fix the issue."

# Claude will:
# 1. Search for the user creation endpoint
# 2. Trace the code path
# 3. Identify the encoding issue
# 4. Fix it and add a test
```

**Git Workflow:**
```bash
claude "Review all changes since the last commit. 
       Create a well-formatted commit message 
       following conventional commits."

claude "Create a PR with a description explaining 
       all the changes in the current branch."
```

### 4.3 Claude Code with MCP Tools

```bash
# Add MCP servers for extended capabilities
claude mcp add postgres -- npx @anthropic-ai/mcp-postgres $DATABASE_URL
claude mcp add github -- npx @anthropic-ai/mcp-github
claude mcp add linear -- npx @anthropic-ai/mcp-linear
claude mcp add sentry -- npx @anthropic-ai/mcp-sentry

# Now Claude Code can:
claude "Check Sentry for the top 5 unresolved errors this week. 
       Find the related code and fix the most critical one."

claude "Create a Linear ticket for the bug we just fixed, 
       then push the fix and create a PR linking to the ticket."
```

### 4.4 Claude Code vs Cursor/Copilot

```
Claude Code is BEST for:
- Terminal-first developers (no GUI needed)
- Complex multi-file refactoring
- CI/CD integration (scriptable, headless)
- Large codebase exploration
- MCP tool integration
- Autonomous long-running tasks

Claude Code is LESS ideal for:
- Visual diff review (no side-by-side GUI)
- Quick inline completions (Tab-complete)
- Real-time pair programming style
- Beginners who need visual feedback
```

---

## 5. Cline -- The Open-Source AI Coding Agent

### 5.1 What Is Cline?

Cline (formerly Claude Dev) is an open-source VS Code extension that turns any LLM into an autonomous coding agent. It supports multiple AI providers and gives you full control over every action.

**Installation:**
```
VS Code > Extensions > Search "Cline" > Install
Configure: API key for OpenAI, Anthropic, Google, or any OpenAI-compatible API
```

**Pricing:** Free extension + your own API costs

### 5.2 Cline Key Features

```
Cline can:
- Read and write files in your project
- Execute terminal commands
- Use a browser (for testing web apps)
- Use MCP tools
- Ask you for permission before each action (configurable)

Unique Cline advantages:
- Open source (full transparency, community plugins)
- Use ANY model (local models via Ollama, any API)
- Human-in-the-loop (approve/reject each action)
- Fully configurable model settings
- Free (you only pay for API calls)
```

**Using Cline:**
```
1. Open Cline panel in VS Code sidebar
2. Type: "Add input validation to all API endpoints using Zod schemas"
3. Cline proposes changes to each file
4. You approve/reject each change
5. Cline continues until the task is complete
```

### 5.3 Cline with Local Models

```bash
# Run a local model with Ollama
ollama pull deepseek-coder-v3:33b

# Configure Cline to use it:
# Settings > Cline > API Provider: "OpenAI Compatible"
# Base URL: http://localhost:11434/v1
# Model: deepseek-coder-v3:33b

# Now you have a FREE, PRIVATE AI coding agent!
```

---

## 6. Aider -- Git-Aware AI Pair Programming

### 6.1 What Is Aider?

Aider is a terminal-based AI coding tool that works directly with your git repo. Every AI change is a proper git commit, making it easy to review, revert, or modify.

**Installation:**
```bash
pip install aider-chat

# Use with any provider:
export OPENAI_API_KEY=sk-...
# OR
export ANTHROPIC_API_KEY=sk-ant-...
```

### 6.2 Aider Workflows

```bash
# Start Aider in your project
cd /path/to/project
aider

# Add files to context
/add src/auth/login.py src/auth/middleware.py

# Make changes
> "Add rate limiting to the login endpoint. Max 5 attempts per IP per minute. Use Redis."

# Aider edits the files and creates a git commit
# Commit: "feat: Add Redis-based rate limiting to login endpoint"

# Continue with more changes
> "Now add tests for the rate limiting"

# Each change is a separate commit
# Commit: "test: Add rate limiting tests"

# Don't like a change? Just:
/undo
# Git revert, clean and simple
```

**Aider Unique Features:**
```
- Every change is a git commit (easy to review/revert)
- Architect mode: Use expensive model for planning, cheap model for coding
- Map of entire repo for context
- Linting integration (auto-fix lint errors)
- Voice input support
- Benchmark leader on SWE-bench
```

**Aider Architect Mode:**
```bash
# Use Opus for planning, Sonnet for coding (saves money)
aider --architect claude-opus-4-6 --editor claude-sonnet-4-6

# Opus plans the changes, Sonnet implements them
# Best of both worlds: quality planning + fast implementation
```

---

## 7. Replit Agent -- The Cloud-Native Builder

### 7.1 What Is Replit Agent?

Replit Agent is an AI that builds complete web applications from natural language descriptions. It handles everything: code, deployment, databases, and hosting.

**Pricing:**
| Tier | Price | Features |
|------|-------|----------|
| Free | $0 | Limited agent usage |
| Replit Core | $25/mo | Full agent access, deployments, GPU |
| Teams | Custom | Collaboration, custom domains |

### 7.2 Replit Agent Workflow

```
Step 1: Describe your app
"Build a project management tool with:
- User authentication (email + Google OAuth)
- Project boards with drag-and-drop tasks
- Team member management
- File uploads
- Real-time updates
Use React, Node.js, and PostgreSQL."

Step 2: Agent creates a plan
→ Shows you the architecture, file structure, and tech stack
→ You can approve or modify the plan

Step 3: Agent builds
→ Creates all files (frontend, backend, database schema)
→ Installs dependencies
→ Configures environment
→ Sets up database
→ Deploys to Replit hosting

Step 4: Iterate
"Add a notification system with email and in-app alerts"
→ Agent modifies existing code and adds new features
```

**Best For:**
```
- Rapid prototyping (idea to deployed app in minutes)
- Non-developers building tools
- Students learning to code
- MVPs and proof-of-concepts
- Hackathons
```

---

## 8. Other Notable AI Coding Tools

### 8.1 Quick Reference

| Tool | Type | Best For | Price |
|------|------|----------|-------|
| **Bolt.new** | Web builder | Full-stack apps from prompt | Free tier + $20/mo |
| **v0 (Vercel)** | UI generator | React/Next.js components | Free tier + $20/mo |
| **Lovable** | App builder | Beautiful apps from description | $20/mo |
| **Devin** | AI SWE | Autonomous software engineering | $500/mo (enterprise) |
| **Amazon Q Developer** | IDE + CLI | AWS-integrated development | Free + Pro $19/mo |
| **Tabnine** | Completion | Enterprise privacy-focused | $12/user/mo |
| **Sourcegraph Cody** | Code search + AI | Large codebase navigation | Free + $9/mo |
| **JetBrains AI** | IDE integration | JetBrains IDE users | $10/mo |
| **Continue** | Open source | VS Code/JetBrains, any model | Free |
| **Zed AI** | Editor | Ultra-fast editor with AI | Free |

### 8.2 Bolt.new and v0 (Vercel)

```
Bolt.new:
- Browser-based AI app builder
- Type what you want, get a full working app
- Supports React, Next.js, Vue, Svelte
- Includes backend, database, and deployment
- Great for rapid prototyping

v0 (Vercel):
- AI-powered UI component generator
- Specializes in React and Next.js
- Generates production-ready, accessible components
- Direct deployment to Vercel
- Best for frontend-focused projects

Example v0 prompt:
"Create a responsive dashboard with a sidebar navigation, 
top bar with user avatar and notifications, main content 
area with data cards, and a footer. Use shadcn/ui components."
→ v0 generates complete, styled, accessible React code
```

---

## 9. Comparison Matrix

### 9.1 Feature Comparison

| Feature | Cursor | Windsurf | Copilot | Claude Code | Cline | Aider |
|---------|--------|----------|---------|-------------|-------|-------|
| IDE | Fork of VS Code | Custom IDE | VS Code/JetBrains | Terminal | VS Code ext | Terminal |
| Autocomplete | Excellent | Excellent | Good | No | No | No |
| Chat | Yes | Yes | Yes | Yes (terminal) | Yes | Yes |
| Multi-file agent | Composer | Cascade | Workspace | Native | Yes | Yes |
| Terminal commands | Yes | Yes | Limited | Native | Yes | Yes |
| MCP support | Yes | Limited | No | Excellent | Yes | No |
| Git integration | Good | Good | Excellent | Good | Basic | Excellent |
| Model choice | Many | Limited | GitHub models | Claude only | Any | Many |
| Local models | Via API | No | No | No | Yes (Ollama) | Yes |
| Free tier | Limited | Generous | Limited | No (API costs) | Free ext | Free tool |
| Open source | No | No | No | No | Yes | Yes |
| Best for | All-around IDE | Flow state | GitHub workflow | Terminal devs | Privacy/control | Git workflow |

### 9.2 Recommendation Guide

```
"I want the best all-around AI IDE"
→ Cursor Pro ($20/mo)

"I want free AI coding"
→ Cline + Ollama (local) or Windsurf Free or Copilot Free

"I live in the terminal"
→ Claude Code or Aider

"I want the best autocomplete"
→ Cursor or Windsurf

"I need enterprise/team features"
→ GitHub Copilot Enterprise or Cursor Business

"I want full control and privacy"
→ Cline + local model via Ollama

"I want to build apps fast without deep coding"
→ Replit Agent or Bolt.new

"I want the best Git workflow"
→ Aider (every change = clean commit)

"I want maximum power for complex refactoring"
→ Claude Code with MCP or Cursor Composer Agent Mode
```

---

## 10. Setting Up Your AI Coding Stack

### 10.1 Recommended Setup for 2026

```
Primary IDE:        Cursor Pro (daily coding)
Terminal Agent:     Claude Code (complex tasks, CI/CD)
Git Copilot:        GitHub Copilot Free (PR reviews, GitHub workflow)
Prototyping:        Bolt.new or v0 (quick UI prototypes)
Local/Private:      Cline + Ollama (sensitive projects)
Config:             .cursorrules + .github/copilot-instructions.md

Total cost: ~$30/mo for a professional-grade AI coding setup
```

### 10.2 Essential Configuration Files

```yaml
# .cursorrules (Project root)
You are an expert in [your tech stack].

Code standards:
- [Your standards here]

Architecture:
- [Project architecture notes]

Do not:
- [Things to avoid]
```

```markdown
# .github/copilot-instructions.md
This project uses:
- Framework: [X]
- Database: [Y]
- Testing: [Z]

Coding conventions:
- [Convention 1]
- [Convention 2]
```

---

## 11. Practical Exercises

### Exercise 1: Tool Trial
Pick a small project idea (e.g., a URL shortener). Build it using:
1. Cursor Composer
2. Claude Code
3. Replit Agent

Compare: speed, code quality, and how much manual intervention was needed.

### Exercise 2: Complex Refactoring
Take an existing project and use AI to:
1. Add TypeScript types to an untyped JavaScript project
2. Convert callbacks to async/await
3. Add comprehensive error handling
4. Write tests for untested code

### Exercise 3: AI Pair Programming Session
Spend 2 hours coding with Cursor/Windsurf and track:
- How many suggestions you accepted vs rejected
- Time saved on boilerplate
- Bugs the AI introduced vs caught
- Overall productivity improvement

### Exercise 4: Build with Local AI
Set up Cline + Ollama with DeepSeek Coder and build a REST API entirely with local, free AI. Document the experience compared to cloud-based tools.

---

## 12. Key Takeaways

1. **Cursor is the current king** for all-around AI-assisted development
2. **Claude Code dominates the terminal** for complex, agentic coding tasks
3. **GitHub Copilot wins the ecosystem** with native GitHub integration
4. **Cline + Ollama = free and private** AI coding with full control
5. **Aider's git integration** makes it the safest for production changes
6. **Bolt.new/Replit Agent** are best for rapid prototyping from natural language
7. **Use multiple tools** -- they complement each other perfectly
8. **Configuration files (.cursorrules)** dramatically improve AI output quality
9. **Agent mode is the future** -- AI that plans, codes, tests, and deploys autonomously

---

**Next Module:** [Module 5: AI Image, Video & Audio Tools](05-ai-image-video-audio.md) -- Master the creative AI tools reshaping media production.
