# Module 8: Building Your Own AI Agents

## Overview

AI agents are the defining paradigm of 2026 -- autonomous systems that plan, use tools, and complete multi-step tasks. This module teaches you to build agents from scratch using every major framework, with complete working code.

**By the end of this module, you will:**
- Build agents with 5 different frameworks
- Understand agent architectures (ReAct, plan-and-execute, multi-agent)
- Create agents that use tools, browse the web, and execute code
- Build multi-agent teams that collaborate on complex tasks
- Deploy agents to production

---

## 1. Agent Fundamentals

### 1.1 What Is an AI Agent?

```
Chatbot:  User asks → Model responds → Done
Agent:    User sets goal → Model plans → Uses tools → Iterates → Delivers result

Key differences:
- Agents have TOOLS (functions they can call)
- Agents have MEMORY (remember across steps)
- Agents PLAN (break goals into steps)
- Agents ITERATE (try, evaluate, retry)
- Agents can DELEGATE (hand off to other agents)
```

### 1.2 Agent Architecture Patterns

```
1. ReAct (Reason + Act)
   Think → Act → Observe → Think → Act → ... → Done
   Most common pattern. Model reasons about what to do, takes an action,
   observes the result, then decides next step.

2. Plan-and-Execute
   Plan all steps → Execute step 1 → Execute step 2 → ... → Done
   Creates a full plan first, then executes sequentially.
   Better for predictable, multi-step workflows.

3. Multi-Agent
   Orchestrator → Specialist Agent 1 → Specialist Agent 2 → Combine results
   Different agents with different tools/expertise collaborate.
   Best for complex tasks requiring diverse capabilities.

4. Reflexion
   Act → Evaluate → Reflect on mistakes → Retry with improvements
   Self-improving agents that learn from errors within a session.
```

---

## 2. OpenAI Agents SDK

The official framework from OpenAI for building production agents.

### 2.1 Installation

```bash
pip install openai-agents
export OPENAI_API_KEY=sk-...
```

### 2.2 Your First Agent

```python
from agents import Agent, Runner
import asyncio

# Define a simple agent
agent = Agent(
    name="Security Advisor",
    instructions="""You are a cybersecurity expert. 
    Provide clear, actionable security advice. 
    Always mention specific tools or techniques."""
)

# Run it
async def main():
    result = await Runner.run(agent, "How do I secure a new Express.js API?")
    print(result.final_output)

asyncio.run(main())
```

### 2.3 Agent with Custom Tools

```python
from agents import Agent, Runner, function_tool
import asyncio
import subprocess
import json

@function_tool
def scan_ports(target: str, ports: str = "1-1000") -> str:
    """Scan a target for open ports using nmap.
    Args:
        target: IP address or hostname to scan
        ports: Port range to scan (default: 1-1000)
    """
    try:
        result = subprocess.run(
            ["nmap", "-p", ports, "--open", "-oG", "-", target],
            capture_output=True, text=True, timeout=60
        )
        return result.stdout
    except Exception as e:
        return f"Scan error: {str(e)}"

@function_tool
def check_ssl(domain: str) -> str:
    """Check SSL/TLS certificate details for a domain."""
    try:
        result = subprocess.run(
            ["openssl", "s_client", "-connect", f"{domain}:443", "-brief"],
            capture_output=True, text=True, timeout=10,
            input=""
        )
        return result.stdout + result.stderr
    except Exception as e:
        return f"SSL check error: {str(e)}"

@function_tool
def lookup_cve(keyword: str) -> str:
    """Search for recent CVEs related to a keyword."""
    import httpx
    response = httpx.get(
        f"https://services.nvd.nist.gov/rest/json/cves/2.0",
        params={"keywordSearch": keyword, "resultsPerPage": 5}
    )
    if response.status_code == 200:
        cves = response.json().get("vulnerabilities", [])
        results = []
        for cve in cves:
            info = cve["cve"]
            results.append({
                "id": info["id"],
                "description": info["descriptions"][0]["value"][:200]
            })
        return json.dumps(results, indent=2)
    return "CVE lookup failed"

# Create agent with tools
security_scanner = Agent(
    name="Security Scanner",
    instructions="""You are an automated security scanner. 
    When given a target, perform appropriate security checks:
    1. First scan for open ports
    2. Check SSL if port 443 is open
    3. Look up relevant CVEs based on discovered services
    4. Provide a summary report with findings and recommendations.""",
    tools=[scan_ports, check_ssl, lookup_cve]
)

async def main():
    result = await Runner.run(
        security_scanner, 
        "Perform a security assessment of example.com"
    )
    print(result.final_output)

asyncio.run(main())
```

### 2.4 Agent Handoffs (Multi-Agent)

```python
from agents import Agent, Runner
import asyncio

# Specialist agents
recon_agent = Agent(
    name="Recon Specialist",
    instructions="You specialize in reconnaissance and information gathering. "
                 "Enumerate subdomains, technologies, and exposed services.",
    tools=[scan_ports]  # recon-specific tools
)

vuln_agent = Agent(
    name="Vulnerability Analyst",
    instructions="You analyze systems for vulnerabilities. "
                 "Given recon data, identify potential security issues.",
    tools=[lookup_cve]  # vuln-specific tools
)

report_agent = Agent(
    name="Report Writer",
    instructions="You create professional security assessment reports. "
                 "Format findings with severity, impact, and remediation."
)

# Orchestrator that delegates to specialists
orchestrator = Agent(
    name="Security Assessment Lead",
    instructions="""You lead security assessments. For each target:
    1. Hand off to Recon Specialist for information gathering
    2. Hand off to Vulnerability Analyst for vuln analysis
    3. Hand off to Report Writer for final report
    Coordinate the team and ensure thorough coverage.""",
    handoffs=[recon_agent, vuln_agent, report_agent]
)

async def main():
    result = await Runner.run(
        orchestrator,
        "Conduct a security assessment of our new web application at app.example.com"
    )
    print(result.final_output)

asyncio.run(main())
```

### 2.5 Guardrails

```python
from agents import Agent, Runner, InputGuardrail, GuardrailFunctionOutput
from pydantic import BaseModel
import asyncio

class SafetyCheck(BaseModel):
    is_safe: bool
    reason: str

safety_agent = Agent(
    name="Safety Checker",
    instructions="Determine if the user request is safe and ethical. "
                 "Flag requests for attacking systems without authorization.",
    output_type=SafetyCheck
)

async def check_safety(ctx, agent, input_text):
    result = await Runner.run(safety_agent, input_text, context=ctx)
    output = result.final_output_as(SafetyCheck)
    return GuardrailFunctionOutput(
        output_info=output,
        tripwire_triggered=not output.is_safe
    )

secure_agent = Agent(
    name="Ethical Security Agent",
    instructions="You help with authorized security testing only.",
    input_guardrails=[
        InputGuardrail(guardrail_function=check_safety)
    ],
    tools=[scan_ports, check_ssl]
)
```

---

## 3. LangChain & LangGraph

The most popular open-source framework for building LLM applications and agents.

### 3.1 Installation

```bash
pip install langchain langchain-openai langchain-anthropic langgraph
```

### 3.2 LangChain Simple Agent

```python
from langchain_openai import ChatOpenAI
from langchain.agents import create_tool_calling_agent, AgentExecutor
from langchain.tools import tool
from langchain_core.prompts import ChatPromptTemplate

# Define tools
@tool
def search_web(query: str) -> str:
    """Search the web for information."""
    import httpx
    # Using a search API
    response = httpx.get(f"https://api.search.example/search?q={query}")
    return response.text

@tool  
def run_python(code: str) -> str:
    """Execute Python code and return the output."""
    import io, contextlib
    output = io.StringIO()
    with contextlib.redirect_stdout(output):
        exec(code)
    return output.getvalue()

# Create the agent
llm = ChatOpenAI(model="gpt-5.4", temperature=0)
prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful security research assistant."),
    ("human", "{input}"),
    ("placeholder", "{agent_scratchpad}")
])

agent = create_tool_calling_agent(llm, [search_web, run_python], prompt)
executor = AgentExecutor(agent=agent, tools=[search_web, run_python], verbose=True)

# Run
result = executor.invoke({"input": "Find the latest CVE for Log4j and write a Python detection script"})
print(result["output"])
```

### 3.3 LangGraph -- Stateful Agent Workflows

LangGraph is LangChain's framework for building complex, stateful agent workflows as graphs.

```python
from langgraph.graph import StateGraph, START, END
from langgraph.prebuilt import ToolNode
from langchain_openai import ChatOpenAI
from langchain_core.messages import HumanMessage, SystemMessage
from typing import TypedDict, Annotated
import operator

# Define state
class AgentState(TypedDict):
    messages: Annotated[list, operator.add]
    plan: str
    findings: list
    report: str

# Define nodes (each is a function)
def planner(state: AgentState):
    """Create an assessment plan."""
    llm = ChatOpenAI(model="gpt-5.4")
    messages = [
        SystemMessage(content="You are a security assessment planner. Create a step-by-step plan."),
        *state["messages"]
    ]
    response = llm.invoke(messages)
    return {"plan": response.content, "messages": [response]}

def scanner(state: AgentState):
    """Execute security scans based on the plan."""
    llm = ChatOpenAI(model="gpt-5.4")
    messages = [
        SystemMessage(content=f"Execute this security plan: {state['plan']}"),
        HumanMessage(content="Run the scans and report findings.")
    ]
    response = llm.invoke(messages)
    return {"findings": [response.content], "messages": [response]}

def reporter(state: AgentState):
    """Generate final report from findings."""
    llm = ChatOpenAI(model="gpt-5.4")
    findings_text = "\n".join(state.get("findings", []))
    messages = [
        SystemMessage(content="Generate a professional security report."),
        HumanMessage(content=f"Findings:\n{findings_text}")
    ]
    response = llm.invoke(messages)
    return {"report": response.content, "messages": [response]}

# Build the graph
workflow = StateGraph(AgentState)
workflow.add_node("planner", planner)
workflow.add_node("scanner", scanner)
workflow.add_node("reporter", reporter)

# Define edges (flow)
workflow.add_edge(START, "planner")
workflow.add_edge("planner", "scanner")
workflow.add_edge("scanner", "reporter")
workflow.add_edge("reporter", END)

# Compile and run
app = workflow.compile()
result = app.invoke({
    "messages": [HumanMessage(content="Assess the security of a REST API at api.example.com")],
    "findings": []
})
print(result["report"])
```

### 3.4 LangGraph with Conditional Routing

```python
def should_continue(state: AgentState):
    """Decide if more scanning is needed."""
    findings = state.get("findings", [])
    if len(findings) < 3:  # Need more scans
        return "scanner"
    return "reporter"

# Add conditional edge
workflow.add_conditional_edges("scanner", should_continue)
```

---

## 4. CrewAI -- Multi-Agent Teams

CrewAI makes it easy to create teams of AI agents that collaborate like a real team.

### 4.1 Installation

```bash
pip install crewai crewai-tools
```

### 4.2 Building a Security Assessment Crew

```python
from crewai import Agent, Task, Crew, Process
from crewai_tools import SerperDevTool, FileReadTool

# Define agents (team members)
recon_specialist = Agent(
    role="Reconnaissance Specialist",
    goal="Gather comprehensive information about the target",
    backstory="""You are an expert in OSINT and network reconnaissance.
    You use passive and active techniques to enumerate targets.""",
    tools=[SerperDevTool()],
    llm="gpt-5.4",
    verbose=True
)

vuln_researcher = Agent(
    role="Vulnerability Researcher",
    goal="Identify vulnerabilities from reconnaissance data",
    backstory="""You are a senior vulnerability researcher with deep 
    knowledge of CVEs, OWASP Top 10, and common misconfigurations.""",
    llm="claude-opus-4-6",
    verbose=True
)

report_writer = Agent(
    role="Security Report Writer",
    goal="Create professional, actionable security reports",
    backstory="""You write executive-quality security assessment reports.
    You categorize findings by severity and provide clear remediation.""",
    llm="gpt-5.4",
    verbose=True
)

# Define tasks
recon_task = Task(
    description="""Perform reconnaissance on {target}.
    Enumerate: subdomains, technologies, open ports, exposed services.
    Output a structured recon report.""",
    expected_output="Detailed reconnaissance report with all findings",
    agent=recon_specialist
)

vuln_task = Task(
    description="""Analyze the recon data and identify vulnerabilities.
    For each finding, determine: severity, exploitability, impact.
    Cross-reference with known CVEs.""",
    expected_output="List of vulnerabilities with severity ratings",
    agent=vuln_researcher,
    context=[recon_task]  # Gets output from recon
)

report_task = Task(
    description="""Create a professional security assessment report.
    Include: executive summary, findings table, detailed descriptions,
    remediation steps, and risk ratings.""",
    expected_output="Complete security assessment report in markdown",
    agent=report_writer,
    context=[recon_task, vuln_task]  # Gets all previous outputs
)

# Create and run the crew
crew = Crew(
    agents=[recon_specialist, vuln_researcher, report_writer],
    tasks=[recon_task, vuln_task, report_task],
    process=Process.sequential,  # Tasks run in order
    verbose=True
)

result = crew.kickoff(inputs={"target": "example.com"})
print(result)
```

### 4.3 CrewAI with Hierarchical Process

```python
# Manager agent coordinates the team
manager = Agent(
    role="Security Assessment Manager",
    goal="Coordinate the security assessment team for thorough coverage",
    backstory="Senior security manager with 20 years experience.",
    llm="gpt-5.4",
    allow_delegation=True
)

crew = Crew(
    agents=[recon_specialist, vuln_researcher, report_writer],
    tasks=[recon_task, vuln_task, report_task],
    process=Process.hierarchical,  # Manager decides task order
    manager_agent=manager,
    verbose=True
)
```

---

## 5. AutoGen (Microsoft)

AutoGen focuses on multi-agent conversations where agents chat with each other to solve problems.

### 5.1 Installation

```bash
pip install autogen-agentchat autogen-ext
```

### 5.2 Two-Agent Conversation

```python
from autogen_agentchat.agents import AssistantAgent
from autogen_agentchat.teams import RoundRobinGroupChat
from autogen_agentchat.conditions import TextMentionTermination
from autogen_ext.models.openai import OpenAIChatCompletionClient
import asyncio

# Create model client
model = OpenAIChatCompletionClient(model="gpt-5.4")

# Create agents
coder = AssistantAgent(
    name="Coder",
    model_client=model,
    system_message="""You are an expert Python developer. Write clean, 
    secure code. Always include error handling and type hints."""
)

reviewer = AssistantAgent(
    name="Reviewer",
    model_client=model,
    system_message="""You are a senior code reviewer specializing in security.
    Review code for vulnerabilities, suggest improvements.
    Say 'APPROVED' when the code is secure and production-ready."""
)

# Create team
termination = TextMentionTermination("APPROVED")
team = RoundRobinGroupChat(
    [coder, reviewer],
    termination_condition=termination,
    max_turns=6
)

async def main():
    result = await team.run(
        task="Write a secure password hashing utility in Python "
             "that supports argon2id, bcrypt, and scrypt."
    )
    print(result.messages[-1].content)

asyncio.run(main())
```

---

## 6. Framework Comparison

| Feature | OpenAI SDK | LangGraph | CrewAI | AutoGen |
|---------|-----------|-----------|--------|--------|
| Complexity | Low | Medium-High | Low | Medium |
| Multi-agent | Handoffs | Graph nodes | Crews | Conversations |
| Tool support | Function tools | LangChain tools | CrewAI tools | Function calling |
| State management | Basic | Excellent | Basic | Conversation |
| Model support | OpenAI only | Any (via LangChain) | Any | Any |
| Production-ready | Yes | Yes | Getting there | Getting there |
| Tracing/debugging | OpenAI dashboard | LangSmith | Built-in logging | Built-in |
| Learning curve | Easy | Steep | Easy | Medium |
| Best for | OpenAI users, simple agents | Complex workflows | Team simulation | Research, iteration |

### When to Use What

```
OpenAI Agents SDK:
  → You use OpenAI models
  → You want the simplest path to production
  → You need guardrails and tracing
  → Simple to moderately complex agents

LangGraph:
  → You need complex, stateful workflows
  → You want conditional routing and loops
  → You need to support multiple model providers
  → Enterprise applications with complex logic

CrewAI:
  → You want team-based agent collaboration
  → Your task has clear role separation
  → You want hierarchical management
  → Simpler setup than LangGraph for multi-agent

AutoGen:
  → You want agents that debate/iterate
  → Research and experimentation
  → Code generation with review cycles
  → Microsoft ecosystem
```

---

## 7. Agent Design Patterns

### 7.1 Tool-Augmented Generation

```python
# Agent decides when to use tools vs respond directly
agent = Agent(
    name="Smart Assistant",
    instructions="""Answer questions directly when you know the answer.
    Use tools only when you need real-time data or to perform actions.
    Always explain your reasoning.""",
    tools=[search_web, run_python, lookup_cve]
)
```

### 7.2 Supervisor Pattern

```python
# One agent manages a pool of specialist agents
supervisor = Agent(
    name="Supervisor",
    instructions="""Route tasks to the appropriate specialist:
    - Security questions → security_agent
    - Coding tasks → coding_agent  
    - Research → research_agent
    Always start by analyzing what type of task this is.""",
    handoffs=[security_agent, coding_agent, research_agent]
)
```

### 7.3 Pipeline Pattern

```python
# Agents process data in sequence
# Agent 1 output → Agent 2 input → Agent 3 input → Final output
pipeline = [
    ("Analyzer", "Analyze the input data for patterns"),
    ("Enricher", "Enrich findings with additional context"),
    ("Formatter", "Format into a professional report")
]
```

---

## 8. Practical Exercises

### Exercise 1: Simple Tool Agent
Build an agent with OpenAI Agents SDK that can:
- Search the web
- Execute Python code
- Read/write files
Test it with: "Find the current Bitcoin price and create a price chart"

### Exercise 2: Multi-Agent Security Team
Using CrewAI, build a 3-agent security team:
- Recon agent (gathers info)
- Analyst agent (finds vulnerabilities)
- Reporter agent (writes report)

### Exercise 3: Stateful Workflow
Using LangGraph, build a code review workflow:
- Node 1: Analyze code
- Node 2: Check for bugs (conditional: if bugs found → fix, else → approve)
- Node 3: Generate fixed code
- Node 4: Verify fix

### Exercise 4: Agent with Memory
Build an agent that remembers previous conversations using a vector store. It should get smarter over time by recalling relevant past interactions.

---

## 9. Key Takeaways

1. **OpenAI Agents SDK is the easiest path** to building production agents
2. **LangGraph is the most powerful** for complex stateful workflows
3. **CrewAI makes multi-agent easy** with its team metaphor
4. **Tools are what make agents useful** -- invest time in good tool design
5. **Guardrails are essential** for production deployment
6. **Start simple, add complexity** -- a single agent with good tools beats a complex multi-agent system for most tasks
7. **Test extensively** -- agents can behave unpredictably; test edge cases
8. **Monitor costs** -- agentic loops can consume many tokens

---

**Next Module:** [Module 9: MCP & The Tool Ecosystem](09-mcp-tools-ecosystem.md) -- Connect your agents to any database, API, or service.
