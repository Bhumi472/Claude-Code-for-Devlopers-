# LangChain 1.x Bootcamp

Teaching material for the **Agentic AI Ultimate RAG Bootcamp** (Krish Naik), built around the LangChain **1.x** agent API. The numbered notebooks in `updatedlangchain/` form a sequential curriculum; see [CLAUDE.md](CLAUDE.md) for the full architecture overview.

## Setup

```bash
# Sync dependencies (uv is canonical; uv.lock is the source of truth)
uv sync
```

Secrets live in `.env` at the repo root. The notebooks and tests expect `OPENAI_API_KEY` (and, depending on the notebook, `GOOGLE_API_KEY`, `GROQ_API_KEY`, `PAGEINDEX_API_KEY`).

## Tests

There are two independent test files. Neither uses a shared test runner — run them directly as shown.

### `test_summarization_middleware.py`

A standalone verification script (not pytest) that proves `SummarizationMiddleware` actually compresses a conversation once its threshold is reached. Each test builds a fresh `create_agent` with deliberately low summarization thresholds so summarization fires within a handful of turns, then prints observable evidence (message counts, detected summaries) rather than asserting. Uses `gpt-4o-mini` for cost and **requires `OPENAI_API_KEY`** — it makes real model calls.

Run it:

```bash
uv run python test_summarization_middleware.py
```

| # | Test | What it verifies |
|---|------|------------------|
| 1 | `test_basic_summarization` | Message-count trigger — agent with `trigger=("messages", 6)`, `keep=("messages", 2)` over a 6-turn weather/calculator conversation. Flags when the message count drops below the un-summarized expectation. |
| 2 | `test_token_based_summarization` | Token trigger — `trigger=("tokens", 500)`, `keep=("messages", 4)` driven by long messages, reporting message count and an estimated token count per turn. |
| 3 | `test_inspect_summarization` | Inspects the full message list before/after summarization (`trigger=("messages", 4)`, `keep=("messages", 2)`), printing each message's type and a preview and marking anything that looks like a summary. |
| 4 | `test_custom_summary_prompt` | A custom `summary_prompt` plus `summary_prefix="📋 CONVERSATION SUMMARY: "`, then scans the state for the resulting summary message. |
| 5 | `test_streaming_with_summarization` | Confirms summarization still works under `agent.stream(..., stream_mode="updates")` across multiple turns. |

All five run from `__main__`; the suite prints a guide on what observations indicate summarization is working (message counts plateau/decrease, summary messages appear, recent messages preserved).

The shared fixtures are three `@tool` functions — `get_weather`, `calculator`, `get_time` — and every agent is paired with an `InMemorySaver` checkpointer and a per-test `thread_id`, which is what lets summarization persist across turns.

### `test_example.py`

A minimal, self-contained `unittest` example (no API calls, no dependencies) showing the standard `assert*` pattern. The code under test (`add`, `divide`) is inlined so the file runs on its own.

Run it:

```bash
uv run python -m unittest test_example -v
# or
uv run python test_example.py
```

| Test | What it verifies |
|------|------------------|
| `test_add_positive_numbers` | `add(2, 3) == 5` |
| `test_add_negative_numbers` | `add(-1, -4) == -5` |
| `test_divide_normal` | `divide(10, 2) == 5` |
| `test_divide_by_zero_raises` | `divide(1, 0)` raises `ValueError` |

## Agent view in Claude Code

When you work on this repo with **Claude Code** (Anthropic's CLI coding agent), you are not limited to a single linear chat. Claude Code can spin up **multiple agents** — the main agent you talk to plus any number of **subagents** it delegates work to — and the **Agent view** is the interface that lets you see, steer, and manage all of them at once. Think of it as a control panel for a small fleet of AI workers instead of one chat window.

This matters for a bootcamp repo like this one: you can have one agent refactoring a notebook while another runs `test_summarization_middleware.py` and a third drafts documentation — all in parallel, without losing track of who is doing what.

### What "agents" means here

Claude Code has three related concepts that the Agent view brings together:

| Concept | What it is | How it starts |
|---------|-----------|---------------|
| **Main agent** | The primary session you type into. It plans the work and decides what to delegate. | Launching `claude` in a terminal (or the desktop / web / IDE app). |
| **Subagent** | A separate agent the main agent launches to handle a focused, multi-step task. It has its **own context window**, so noisy work (broad searches, log trawling) doesn't pollute the main conversation. | The main agent calls the `Task`/`Agent` tool, or you ask it to "use a subagent for X". |
| **Background agent / job** | A long-running agent that keeps working across turns and notifies you when it finishes, instead of blocking the chat. | Running a task in the background, or a `/loop` / scheduled run. |

A subagent does the work and reports **only its conclusion** back to the agent that spawned it — the parent keeps the summary, not the full transcript. That isolation is the whole point: it keeps each context window focused.

### Built-in vs. custom subagent types

Claude Code ships with several **specialized subagent types**, and you can define your own. Each type has a name, a description that tells the main agent *when* to use it, an optional model override, and a restricted tool set.

Common built-in types you'll see:

- **`general-purpose`** — the catch-all for open-ended research and multi-step tasks.
- **`Explore`** — a fast, **read-only** search agent for sweeping many files when you only need the answer, not the file contents.
- **`Plan`** — a software-architect agent that designs an implementation strategy and returns a step-by-step plan.
- **Task-specific helpers** (e.g. a code-improvement reviewer, a status-line setup helper) that exist when configured for the project.

You create your **own** subagents with the `/agents` command. Custom agents are stored as Markdown files with YAML frontmatter:

- **Project-scoped:** `.claude/agents/<name>.md` (checked into this repo, shared with the bootcamp).
- **User-scoped:** `~/.claude/agents/<name>.md` (available across all your projects).

A minimal custom agent definition looks like this:

```markdown
---
name: notebook-reviewer
description: Reviews LangChain notebook cells for v1 API correctness. Use after editing any notebook in updatedlangchain/.
tools: Read, Grep, Glob
model: sonnet
---

You review LangChain 1.x notebook cells. Check that examples use
`create_agent` (never the deprecated `AgentExecutor`/`initialize_agent`),
that stateful agents pair a checkpointer with a `thread_id`, and that
model names match the ones already used in the curriculum. Report issues
as a short bulleted list — do not rewrite the cells yourself.
```

Restricting `tools` (here: read-only) keeps a reviewer agent from accidentally editing files, and the `description` is what the main agent reads to decide when to delegate to it automatically.

### What the Agent view shows you

The Agent view is the dashboard for everything above. From it you can:

- **See every active agent** — the main session plus all running subagents and background jobs — and their live status (running, waiting on input, completed, failed).
- **Read what each one is doing** — the narrated step-by-step progress, so you can tell at a glance which agent is editing, which is testing, and which is blocked.
- **Run agents in parallel** — fan out independent work across several agents and let them proceed concurrently instead of serially.
- **Steer or interrupt** — send a follow-up message to a specific agent, answer a question it's blocked on, or stop one that's gone off track.
- **Collect results** — when a subagent finishes, its final summary flows back to the agent (or to you) that needs it.

### Managing agents

| Action | How |
|--------|-----|
| List, create, edit, or delete subagent types | Run `/agents` in Claude Code |
| Delegate to a subagent | Ask the main agent to "use a subagent" / "explore in parallel", or let it auto-delegate based on each type's `description` |
| Run something long without blocking | Ask for it to run **in the background**; you'll be notified when it completes |
| Share a project agent with the bootcamp | Commit `.claude/agents/<name>.md` to the repo |

### When to reach for it

- **Parallel, independent work** — e.g. update a notebook *and* run the summarization tests at the same time.
- **Noisy investigation** — broad `grep`/log sweeps belong in an `Explore` subagent so the findings come back clean and the main chat stays readable.
- **Planning before building** — use a `Plan` agent to design the change to a notebook before any edits are made.
- **Long-running tasks** — let a background agent run a slow test suite or a scheduled job while you keep working.

> Tip: keep each agent's job narrow and well-described. A focused subagent with the right `description` and a minimal tool set is easier for the main agent to use correctly — and easier for *you* to follow in the Agent view.

## Agent Teams

Subagents are one-shot: the main agent spawns one, it does its job, returns a single summary, and disappears. **Agent Teams** turn that fleet into something more like a crew. A team is a set of **persistent, named teammates** that stay alive across turns, hold their own context, talk to each other, and coordinate around a **shared task list** — instead of each working blind and reporting back only at the end.

It's an experimental Claude Code feature. Enable it by setting an env var in `.claude/settings.json`:

```json
{
  "env": { "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1" }
}
```

### Subagents vs. teammates

| | Plain subagent | Teammate (in a team) |
|--|----------------|----------------------|
| **Lifetime** | Runs once, returns one result, exits | Stays alive across turns until the team is torn down |
| **Identity** | Anonymous, addressed only by its parent | **Named** — others address it by name |
| **Communication** | Reports its conclusion back to the parent only | Messages the lead **and** other teammates; messages are delivered automatically |
| **Coordination** | None — each is independent | A **shared task list** all teammates see and update |
| **Parallelism** | Parent fans out, then joins | Teammates work concurrently *and* hand work off to each other |

### The tool workflow

| Step | Tool | What it does |
|------|------|--------------|
| 1. Stand up the team | `TeamCreate` | Creates the team and its shared task list |
| 2. File the work | `TaskCreate` | Adds work items (with descriptions, owners, and `blockedBy` dependencies) to the list |
| 3. Spawn teammates | `Agent` (with `team_name` + `name`) | Launches **named** teammates into the team |
| 4. Claim / assign | `TaskUpdate` | A teammate sets a task's `owner` to itself and moves it `pending → in_progress → completed` |
| 5. Coordinate | `SendMessage` | Teammates message the lead or each other; delivery is automatic — no inbox polling |
| 6. Tear down | `TeamDelete` | Cleans up the team **after** its teammates have shut down |

### A bootcamp example

Say you want to add a middleware example to `6-middleware.ipynb` without breaking the verification script. You stand up a two-person team:

- **`author`** edits the notebook, adding the new `SummarizationMiddleware` cell.
- **`tester`** keeps `test_summarization_middleware.py` green, re-running it after each change.

They share one task list. `author` claims "add cell" and, when done, marks it `completed` — which unblocks `tester`'s "verify tests pass" task. If the new cell changes the summarization trigger, `tester` `SendMessage`s `author` with the failing output instead of silently finishing, and `author` fixes the cell in the same session. The lead watches both progress in the shared list rather than stitching together two separate one-shot reports.

### Team vs. plain subagent

Reach for a **team** when agents must **coordinate or share evolving state** — work that hands off between roles, has cross-cutting dependencies, or needs a back-and-forth (the author/tester loop above). Reach for a **plain subagent** when the work is **independent and one-shot** — a single broad search, a self-contained refactor, a planning pass. A team carries real overhead (multiple live contexts, message passing, a task list to maintain); for work that doesn't need the conversation, a subagent is simpler and cheaper.

> Tip: give every teammate a clear name and a single task to own, and let the **task list** — not chat — be the source of truth for who's doing what. A teammate that updates its task status promptly keeps the whole crew (and you) unblocked.
