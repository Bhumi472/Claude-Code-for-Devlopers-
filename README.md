# 🚀 Claude Code for Developers

<div align="center">

![Python](https://img.shields.io/badge/Python-3.13+-blue?style=for-the-badge&logo=python)
![LangChain](https://img.shields.io/badge/LangChain-v1-green?style=for-the-badge)
![Claude Code](https://img.shields.io/badge/Claude-Code-orange?style=for-the-badge)
![MCP](https://img.shields.io/badge/MCP-Model_Context_Protocol-purple?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-success?style=for-the-badge)

**A Practical Learning Repository for Claude Code, LangChain v1, Agentic AI, Multi-Agent Systems, and Model Context Protocol (MCP).**

Learn by building real examples, notebooks, and AI workflows used in modern AI engineering.

⭐ If this repository helps you, consider giving it a star!

</div>

---

# 📚 Table of Contents

- Introduction
- Features
- Learning Roadmap
- Repository Structure
- Topics Covered
- Tech Stack
- Installation
- Environment Variables
- Running the Project
- Learning Path
- Resources
- Future Improvements
- Contributing
- License

---

# 📖 Introduction

This repository is a **hands-on learning guide** for developers interested in building modern AI applications using **Claude Code**, **LangChain v1**, **LLMs**, **MCP**, and **Agentic AI**.

Instead of only explaining concepts, every topic is demonstrated with **practical notebooks**, **real examples**, and **working implementations**.

The repository covers everything from creating your first AI agent to building multi-agent workflows and integrating external tools using the **Model Context Protocol (MCP)**.

---

# ✨ Features

- ✅ Claude Code Tutorials
- ✅ LangChain v1 Examples
- ✅ Agentic AI
- ✅ Multi-Agent Systems
- ✅ Model Context Protocol (MCP)
- ✅ Tool Calling
- ✅ Structured Output
- ✅ Human in the Loop
- ✅ Summarization Middleware
- ✅ Vectorless RAG
- ✅ PageIndex Integration
- ✅ Custom Claude Agents
- ✅ Claude Skills
- ✅ Claude Plugins
- ✅ Jupyter Notebook Examples

---

# 🎯 Learning Roadmap

```text
Python
      │
      ▼
Large Language Models
      │
      ▼
LangChain v1
      │
      ▼
Tools
      │
      ▼
Messages
      │
      ▼
Structured Output
      │
      ▼
Middleware
      │
      ▼
AI Agents
      │
      ▼
Claude Code
      │
      ▼
Multi-Agent Systems
      │
      ▼
Model Context Protocol (MCP)
      │
      ▼
Production AI Applications
```

---

# 📂 Repository Structure

```bash
.
│
├── updatedlangchain/
│   ├── 1-langchainintro.ipynb
│   ├── 3-tools.ipynb
│   ├── 4-messages.ipynb
│   ├── 5-structuredoutput.ipynb
│   ├── 7-vectorlessrag.ipynb
│   └── vibe_training_demo.ipynb
│
├── .claude/
│   ├── agents/
│   ├── skills/
│   ├── settings.json
│   ├── notify.ps1
│   └── posttool.sh
│
├── my-first-plugin/
│
├── blog_multi_agent_systems.md
├── CLAUDE.md
├── plan.md
├── requirements.txt
├── pyproject.toml
└── README.md
```

---

# 📚 Topics Covered

## 1️⃣ Claude Code

Learn the modern AI coding assistant developed by Anthropic.

Topics include:

- Claude CLI
- Claude Desktop
- Agent View
- Background Agents
- Sub Agents
- Agent Teams
- Claude Skills
- Claude Plugins
- CLAUDE.md
- Custom Instructions

---

## 2️⃣ LangChain v1

Learn the latest LangChain APIs.

Topics:

- create_agent()
- init_chat_model()
- Tool Calling
- Streaming
- Batch Processing
- Memory
- Middleware
- Structured Output

---

## 3️⃣ Agentic AI

Build autonomous AI agents capable of reasoning and acting.

Topics:

- Planning
- Memory
- Reflection
- Tool Usage
- Agent Loops
- Autonomous Execution

---

## 4️⃣ Multi-Agent Systems

Learn how multiple AI agents collaborate.

Includes:

- Supervisor Pattern
- Worker Pattern
- Swarm Architecture
- Hierarchical Agents
- Agent Teams
- Parallel Execution

---

## 5️⃣ Model Context Protocol (MCP)

Understand how AI models connect with external systems.

Topics:

- MCP Fundamentals
- MCP Client
- MCP Server
- Resources
- Tools
- Prompts
- External Integrations

---

## 6️⃣ Tool Calling

Learn how LLMs interact with external functions.

Examples include:

- Weather Tool
- Custom Python Tools
- Function Calling
- Tool Execution Loop

---

## 7️⃣ Structured Output

Generate reliable responses using:

- Pydantic
- TypedDict
- Dataclasses
- JSON Output

---

## 8️⃣ Middleware

Understand middleware in LangChain.

Topics include:

- Summarization Middleware
- Human-in-the-loop Middleware
- Conversation Compression
- Approval Workflow

---

## 9️⃣ Vectorless RAG

Build Retrieval-Augmented Generation without vector databases.

Topics:

- PageIndex
- Tree Retrieval
- Document Navigation
- Agent Integration

---

# 🛠 Tech Stack

| Category | Technologies |
|----------|--------------|
| Language | Python |
| Framework | LangChain v1 |
| LLM Providers | OpenAI, Google Gemini, Groq |
| AI Framework | LangGraph |
| Retrieval | PageIndex |
| Environment | Python-dotenv |
| Notebook | Jupyter Notebook |
| Package Manager | uv, pip |

---

# ⚙️ Installation

## Clone Repository

```bash
git clone https://github.com/Bhumi472/Claude-Code-for-Devlopers-.git
```

Move into the repository

```bash
cd Claude-Code-for-Devlopers-
```

---

## Install Dependencies

Using pip

```bash
pip install -r requirements.txt
```

Using uv

```bash
uv sync
```

---

# 🔑 Environment Variables

Create a `.env` file.

```env
OPENAI_API_KEY=

GOOGLE_API_KEY=

GROQ_API_KEY=

PAGEINDEX_API_KEY=
```

---

# ▶ Running the Project

Launch Jupyter Notebook

```bash
jupyter notebook
```

or

```bash
jupyter lab
```

---

# 📖 Learning Path

Follow the notebooks in the following order:

| Notebook | Topic |
|-----------|-------|
| 1 | LangChain Introduction |
| 2 | Model Integration |
| 3 | Tools |
| 4 | Messages |
| 5 | Structured Output |
| 6 | Middleware |
| 7 | Vectorless RAG |

---

# 🎓 What You'll Learn

By completing this repository, you'll be able to:

- Build AI Agents
- Create LangChain Applications
- Work with Claude Code
- Understand MCP
- Build Multi-Agent Systems
- Design AI Workflows
- Implement Tool Calling
- Create Structured Outputs
- Build RAG Systems
- Develop Production AI Applications

---

# 📚 Recommended Resources

- Claude Code Documentation
- Anthropic Documentation
- LangChain Documentation
- LangGraph Documentation
- OpenAI Documentation
- Google Gemini Documentation
- PageIndex Documentation

---

# 🚀 Future Improvements

- AI Memory
- Long-Term Memory
- MCP Projects
- AI Workflows
- CrewAI Examples
- AutoGen Examples
- OpenAI Agents SDK
- Production Deployment
- Docker Support
- Kubernetes Deployment

---

# 🤝 Contributing

Contributions are always welcome.

If you'd like to improve this repository:

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to your branch
5. Open a Pull Request

---

# 📄 License

This repository is intended for educational and learning purposes.

---

<div align="center">

## ⭐ Support

If you found this repository useful,

**Please leave a ⭐ on GitHub.**

It motivates me to create more open-source AI content.

Happy Learning 🚀

</div>
