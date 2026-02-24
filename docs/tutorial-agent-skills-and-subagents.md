# Agent Skills & Subagents: A Practical Tutorial

> A guide to extending AI coding agents with reusable skills and delegating complex work through subagents — covering Gemini CLI, Claude Code, and OpenAI Codex.

---

## Table of Contents

1. [What Are Skills and How They Work](#1-what-are-skills-and-how-they-work)
2. [What Are Subagents and How They Work](#2-what-are-subagents-and-how-they-work)
3. [How To: Gemini CLI](#3-how-to-gemini-cli)
4. [How To: Claude Code](#4-how-to-claude-code)
5. [How To: OpenAI Codex](#5-how-to-openai-codex)

---

## 1. What Are Skills and How They Work

**Skills** are self-contained packages of instructions, scripts, and resources that an AI agent can discover and load on demand. Rather than cramming every piece of domain knowledge into the agent's base context, skills let the agent pull in specialized expertise only when the task calls for it.

The [Agent Skills](https://agentskills.io) format was originally developed by Anthropic, released as an open standard, and is now supported by multiple agent tools including Gemini CLI and Claude Code.

### Core idea: Progressive Disclosure

At session start, the agent loads only each skill's **name** and **description** — a tiny amount of context. When the agent identifies a task that matches a skill, it loads the full instructions and any bundled assets. This means you can maintain a large library of skills without blowing up your context window on every conversation.

### The SKILL.md file

Every skill is built around a `SKILL.md` file with two parts:

```
skill-name/
├── SKILL.md        ← Required: YAML frontmatter + instructions
├── helper.py       ← Optional: scripts the skill can run
└── template.md     ← Optional: reference assets
```

**SKILL.md structure:**

```markdown
---
name: skill-name
description: What this skill does. When to use it. Example triggers.
---

# Skill Name

The instructions Claude follows when this skill is active.
Step-by-step procedures, guidelines, and any context the agent needs.
```

The `description` field is the most important piece — it's the only thing the agent reads when deciding whether to activate the skill. Write it to clearly state what the skill does, when it applies, and what kinds of requests should trigger it.

### How activation works (end to end)

1. **Discovery** — At startup, the agent scans skill directories and injects each skill's `name` and `description` into the system prompt (not the full content).
2. **Matching** — When you make a request, the agent checks whether any skill's description fits the task.
3. **Activation** — The agent calls an internal skill tool (e.g., `activate_skill` in Gemini CLI, `Skill` in Claude Code). Depending on the tool, you may be prompted to confirm.
4. **Injection** — The full `SKILL.md` body and the skill's directory path are loaded into the conversation. The agent can now read any bundled files.
5. **Execution** — The agent follows the skill's procedural guidance to complete your task.

You can also invoke a skill directly with a slash command: `/skill-name`.

### What skills are good for

- Encoding procedures that an agent should follow consistently (PR review process, deployment checklist, coding standards)
- Bundling domain expertise with the scripts and templates needed to apply it
- Sharing repeatable workflows across a team by committing skills to version control
- Building reusable capabilities that work across different agent tools (portability is a key benefit of the open standard)

---

## 2. What Are Subagents and How They Work

**Subagents** are independent, specialized AI agents that a main (orchestrator) agent can spawn and delegate tasks to. Each subagent runs in its own isolated context window with its own system prompt, its own set of allowed tools, and optionally a different model — entirely separate from the main conversation.

Think of a subagent as a contractor the main agent hires for a specific job. It receives a task, does the work independently, and reports back a summary. The main agent never sees all the intermediate steps — only the result.

### Why subagents matter

The main conversation context is a limited resource. When an agent does deep codebase exploration or processes a lot of intermediate output, all of that ends up in the main thread and crowds out space for subsequent work. Subagents solve this by isolating heavy or verbose work so the main conversation stays clean.

Beyond context management, subagents also enable:

- **Parallelism** — Multiple subagents can run simultaneously, turning a sequence of tasks into a parallel workflow.
- **Specialization** — Each subagent can have tailored instructions for a specific domain (security auditing, test generation, documentation).
- **Tool restriction** — A subagent can be locked down to a read-only toolset, reducing the risk of unintended side effects.
- **Model selection** — You can use a powerful reasoning model for complex analysis and a lighter model for fast exploration.

### How subagent delegation works

1. The main agent evaluates whether a task matches a subagent's `description`.
2. The main agent calls the subagent as a tool, passing a task prompt.
3. The subagent spins up with its own context, executes its tools, and works through the task independently.
4. When done, the subagent returns a result to the main agent.
5. The main agent incorporates the result and continues.

Because each subagent invocation is independent, a subagent starts fresh with no memory of previous invocations (unless you resume it explicitly).

### Skills vs. Subagents at a glance

| | **Skills** | **Subagents** |
|---|---|---|
| Runs in | Main conversation context | Isolated context window |
| Best for | Reusable procedures, conventions, domain knowledge | Complex or parallel tasks, tool isolation |
| Context cost | Loaded on demand; stays in main thread | Independent thread; doesn't pollute main context |
| Parallelism | No | Yes |
| Model override | No | Yes |
| Portability | High — open standard across tools | Tool-specific implementation |

> **Rule of thumb:** Use a skill when you need the agent to follow better *instructions*. Use a subagent when the task is complex enough to deserve its own *workspace*.

---

## 3. How To: Gemini CLI

### Setup

Install Gemini CLI and authenticate:

```bash
npm install -g @google/gemini-cli
gemini auth login
```

### Skills Setup

Gemini CLI discovers skills from three locations (highest to lowest precedence):

| Scope | Path | Use case |
|---|---|---|
| Workspace | `.agents/skills/` or `.gemini/skills/` | Team-shared, committed to git |
| User | `~/.agents/skills/` or `~/.gemini/skills/` | Personal, available everywhere |
| Extension | Bundled in installed extensions | Distributed with tools |

> Tip: Prefer `.agents/skills/` over `.gemini/skills/` — the generic `.agents/` alias works across different AI tools.

**Install a skill from GitHub:**

```bash
gemini skills install https://github.com/user/my-skills.git

# Install a specific skill from a monorepo
gemini skills install https://github.com/org/skills.git --path skills/frontend

# Install to workspace scope (shared with your team)
gemini skills install /path/to/skill --scope workspace
```

**Manage skills in an interactive session:**

```
/skills list              # Show all discovered skills and their status
/skills enable <n>        # Re-enable a disabled skill
/skills disable <n>       # Prevent a skill from being used
/skills reload            # Refresh skill discovery after adding new skills
/skills link <path>       # Symlink a local skills directory
```

**Create your first skill:**

```bash
mkdir -p .agents/skills/code-review
```

`.agents/skills/code-review/SKILL.md`:

```markdown
---
name: code-review
description: >
  Reviews code for quality, bugs, and best practices.
  Use when asked to review a PR, check a file, or audit recent changes.
---

# Code Review

When reviewing code:
1. Check for logic errors and edge cases
2. Flag security concerns (SQL injection, XSS, hardcoded secrets)
3. Suggest readability improvements
4. Note missing tests or documentation

Report findings clearly with file and line references.
```

Run `/skills reload` and test it: *"Review the changes in src/auth.js."*

### Enabling Subagents (Experimental)

> ⚠️ Subagents are currently experimental in Gemini CLI. Subagents run in "YOLO mode" — they execute tools without individual approval prompts. Use caution with powerful tools like `run_shell_command` or `write_file`.

Enable them in `~/.gemini/settings.json` (or `.gemini/settings.json` for a project):

```json
{
  "experimental": {
    "enableAgents": true
  }
}
```

**Built-in subagents (enabled by default):**

| Agent | Purpose |
|---|---|
| `codebase_investigator` | Deep code analysis, dependency mapping, reverse engineering |
| `cli_help` | Expert on Gemini CLI commands and configuration |
| `generalist_agent` | Routes tasks to the right specialist automatically |

**Create a custom subagent** by adding a `.md` file with YAML frontmatter to `.gemini/agents/` (project) or `~/.gemini/agents/` (user):

```bash
mkdir -p .gemini/agents
```

`.gemini/agents/security-auditor.md`:

```markdown
---
name: security-auditor
description: >
  Security vulnerability specialist. Use for all security audits,
  dependency vulnerability checks, and code security reviews.
  Examples: "audit this file for injection issues", "check for hardcoded secrets".
kind: local
tools:
  - read_file
  - grep_search
model: gemini-2.5-pro
temperature: 0.2
max_turns: 10
---

You are a ruthless Security Auditor. Analyze code for:
1. SQL Injection and command injection
2. XSS (Cross-Site Scripting)
3. Hardcoded credentials or API keys
4. Unsafe file and path operations

Report each vulnerability with: file, line, risk level, and a suggested fix.
Do not apply fixes — report only.
```

**Subagent configuration fields:**

| Field | Required | Description |
|---|---|---|
| `name` | ✅ | Unique slug; becomes the tool name. Lowercase, hyphens, underscores only. |
| `description` | ✅ | How the main agent decides when to use this subagent. Be specific and include examples. |
| `kind` | — | `local` (default) or `remote` |
| `tools` | — | Allowed tools list. Omit to use defaults. |
| `model` | — | Specific model, e.g. `gemini-2.5-pro`. Defaults to session model. |
| `temperature` | — | 0.0–2.0 |
| `max_turns` | — | Max conversation turns before the agent must return (default: 15) |
| `timeout_mins` | — | Max execution time in minutes (default: 5) |

**Example prompt to trigger parallel subagents:**

```
Review the current PR on these points. Spawn one agent per point,
wait for all results, then summarize:
1. Security issues
2. Code quality
3. Potential bugs
4. Test coverage
```

**Tuning tip:** If your subagent isn't being invoked, switch models with `/model` and ask: *"Why didn't you use the `security-auditor` subagent for this task given its description?"*

---

## 4. How To: Claude Code

### Setup

Install Claude Code and authenticate:

```bash
npm install -g @anthropic-ai/claude-code
claude auth login
```

### Skills Setup

Claude Code follows the Agent Skills open standard and extends it with additional features: invocation control, subagent execution via `context: fork`, and argument passing via `$ARGUMENTS`.

**Skill directory locations** (highest to lowest precedence):

| Scope | Path | Use case |
|---|---|---|
| Project | `.claude/skills/` | Team-shared, committed to git |
| User | `~/.claude/skills/` | Personal, available across all projects |

> Note: `.claude/commands/` (legacy custom slash commands) still works and is equivalent to `.claude/skills/`.

**Create your first skill:**

```bash
mkdir -p ~/.claude/skills/explain-code
```

`~/.claude/skills/explain-code/SKILL.md`:

```markdown
---
name: explain-code
description: >
  Explains code using visual diagrams and analogies.
  Use when explaining how code works, teaching about a codebase,
  or when the user asks "how does this work?"
---

# Explain Code

When explaining code:
1. Start with a high-level summary of what it does
2. Use an analogy to make the concept concrete
3. Walk through the key steps in plain language
4. If helpful, describe a simple diagram of the flow

Avoid jargon. Assume the reader is smart but unfamiliar with this codebase.
```

Invoke explicitly with `/explain-code` or let Claude load it automatically when you ask *"how does the authentication work?"*

**Passing arguments to a skill:**

```markdown
---
name: migrate-component
description: Migrates a UI component from one framework to another.
---

Migrate the component `$0` from `$1` to `$2`. Preserve all behavior and tests.
```

Usage: `/migrate-component SearchBar React Vue`

**Controlling who can invoke a skill:**

By default, both you and Claude can invoke any skill. Two frontmatter fields let you restrict this:

```markdown
---
name: deploy
description: Deploy the application to production.
disable-model-invocation: true   # Only YOU can invoke — Claude won't auto-trigger it
---
```

```markdown
---
name: legacy-context
description: Background knowledge about the legacy payment system.
user-invocable: false             # Only Claude can invoke — hidden from /command menu
---
```

Use `disable-model-invocation: true` for anything with side effects — deployments, commits, outbound messages. You don't want Claude deciding to deploy because the code looks ready.

**Manage skills interactively:**

```
/skills          # Interactive interface: create, list, enable, disable skills
/context         # Check if any skill descriptions are being excluded due to context budget
```

### Enabling Subagents

Subagents in Claude Code are stable and don't require experimental flags. Allow subagent use by enabling the `Task` tool:

```
/permissions add Task
```

Or use the interactive agent manager:

```
/agents          # Create, list, and manage subagents; includes MCP tool selection
```

**Built-in subagents:** Claude Code includes a general-purpose helper agent used automatically for research and exploration tasks when `Task` is in your allowed tools.

**Create a custom subagent:**

```bash
# Project-level (shared with team)
mkdir -p .claude/agents

# User-level (available everywhere)
mkdir -p ~/.claude/agents
```

`.claude/agents/code-reviewer.md`:

```markdown
---
name: code-reviewer
description: >
  Expert code review specialist. Use proactively to review code quality,
  identify bugs, and suggest improvements for readability and performance.
  Examples: "review this file", "check my recent changes", "audit this PR".
tools: Read, Grep
---

You are a thorough code reviewer. For every review:
1. Read the relevant files completely before commenting
2. Identify bugs, logic errors, and edge cases
3. Flag security concerns with file path and line number
4. Suggest readability improvements
5. Note missing tests

Prioritize high-impact issues over style nitpicks. Be concise and specific.
```

**Subagent with persistent memory** — the `memory` field gives the agent a persistent directory that survives across sessions, letting it accumulate codebase knowledge over time:

```markdown
---
name: codebase-expert
description: >
  Deep codebase knowledge agent. Use for architectural questions,
  dependency analysis, and understanding unfamiliar parts of the codebase.
tools: Read, Grep, Glob
memory: user
---

You are a codebase analyst. As you explore, update your memory file with
architectural patterns, key conventions, and recurring design decisions you
discover. Consult your memory before re-exploring familiar areas.
```

**Invoke a subagent explicitly:**

```
Use the code-reviewer subagent to check my recent changes
Let the codebase-expert subagent explain how the auth system works
```

Claude also invokes subagents automatically when the task matches their description. To make Claude invoke a subagent more eagerly, add *"Use proactively"* or *"Always use for..."* to the description.

**Example: parallel review workflow:**

```
Use the code-reviewer subagent and codebase-expert subagent simultaneously
to review the new payment module. Code-reviewer should flag implementation
issues; codebase-expert should check consistency with existing patterns.
```

> **Note:** Subagents cannot spawn further subagents. For nested delegation, chain subagents from the main conversation or use a skill with `context: fork` in its frontmatter.

---

## 5. How To: OpenAI Codex

### Setup

Install Codex CLI:

```bash
npm install -g @openai/codex
```

Authenticate with your OpenAI API key:

```bash
codex auth
# or: export OPENAI_API_KEY=your-key
```

### Skills Setup

Codex supports the Agent Skills open standard. Skills are stored in a `skills/` directory alongside the Codex config:

| Scope | Path |
|---|---|
| Project | `.codex/skills/` |
| User | `~/.codex/skills/` |

Create a skill using the standard `SKILL.md` format:

```bash
mkdir -p .codex/skills/api-conventions
```

`.codex/skills/api-conventions/SKILL.md`:

```markdown
---
name: api-conventions
description: >
  API design conventions for this codebase. Use when writing or reviewing
  API endpoints, route handlers, or HTTP response formatting.
---

# API Conventions

When writing API endpoints in this codebase:
- Use RESTful naming: `/resources` (plural), `/resources/:id`
- Return consistent error format: `{ error: string, code: string }`
- Validate all inputs at the route level before hitting service logic
- Use 422 for validation errors, 404 for not found, 500 for unexpected errors
- Include request IDs in all responses via the `X-Request-ID` header
```

### Enabling Multi-Agent (Experimental)

> ⚠️ Multi-agent workflows are currently experimental in Codex. Sub-agents inherit your sandbox policy but run with non-interactive approvals — any action requiring new approval will fail and surface the error to the parent agent.

**Enable from the Codex CLI:**

```
/experimental
```

Select **Multi-agents**, then restart Codex.

**Or add directly to `~/.codex/config.toml`:**

```toml
[features]
multi_agent = true
```

### Agent Roles

Codex organizes subagents around **roles**. Built-in roles are `default`, `worker`, and `explorer`. Define your own roles or override the built-ins in `~/.codex/config.toml` or a project-level `.codex/config.toml`. Each role can point to a separate config file that sets its model, reasoning effort, sandbox mode, and developer instructions.

**Define custom agent roles:**

`~/.codex/config.toml`:

```toml
[agents.default]
description = "General-purpose helper for everyday tasks."

[agents.reviewer]
description = "Finds security, correctness, and test risks in code."
config_file = "agents/reviewer.toml"

[agents.explorer]
description = "Fast codebase explorer for read-heavy analysis tasks."
config_file = "agents/explorer.toml"
```

`~/.codex/agents/reviewer.toml`:

```toml
model = "gpt-5.3-codex"
model_reasoning_effort = "high"
developer_instructions = """
Focus on high-priority issues only.
Write a failing test to reproduce any bug before flagging it.
For security issues, provide concrete reproduction steps.
"""
```

`~/.codex/agents/explorer.toml`:

```toml
model = "gpt-5.3-codex-spark"
model_reasoning_effort = "medium"
sandbox_mode = "read-only"
```

**Agent role configuration fields:**

| Field | Description |
|---|---|
| `agents.max_threads` | Max number of concurrently open agent threads |
| `agents.<n>.description` | Guidance for when Codex should pick this role |
| `agents.<n>.config_file` | Path to a TOML config applied when this role is spawned |

Any setting not defined in the role config is inherited from the parent session.

### Example Prompts

**Parallel PR review:**

```
Review the current PR (this branch vs main). Spawn one agent per point,
wait for all results, and summarize:
1. Security vulnerabilities
2. Code quality
3. Bugs
4. Race conditions
5. Test flakiness
6. Maintainability
```

**Parallel feature build:**

```
Implement the notification system. Spawn separate agents to:
- Write the backend service and API routes
- Write the frontend notification component
- Write unit tests for both
Summarize what was created and any remaining integration steps.
```

**Managing running agents:**

```
/agent        # Switch between active threads and inspect ongoing work
```

You can also ask Codex in natural language to steer, pause, or close any running sub-agent thread.

---

## Quick Reference

### SKILL.md Frontmatter Fields

| Field | Tools | Description |
|---|---|---|
| `name` | All | Slug; becomes the `/slash-command` |
| `description` | All | What the skill does and when to use it (primary activation signal) |
| `disable-model-invocation` | Claude Code | `true` = only you can invoke; prevents auto-activation |
| `user-invocable` | Claude Code | `false` = hidden from slash command menu; model-only |
| `allowed-tools` | Claude Code | Restrict which tools the skill can use |
| `context` | Claude Code | `fork` = run skill in an isolated subagent context |

### Subagent Frontmatter Fields

| Field | Tools | Description |
|---|---|---|
| `name` | Gemini, Claude Code | Unique agent identifier / tool name |
| `description` | Gemini, Claude Code | When to invoke this agent — be explicit and include examples |
| `tools` | Gemini, Claude Code | Allowed tools list (omit to inherit defaults) |
| `model` | Gemini, Claude Code | Specific model to use |
| `temperature` | Gemini | 0.0–2.0 |
| `max_turns` | Gemini | Max turns before agent must return (default: 15) |
| `memory` | Claude Code | Persistent memory directory scoped to `user` or `project` |
| `kind` | Gemini | `local` (default) or `remote` |

---

*Sources: [Agent Skills Open Standard](https://agentskills.io/home) · [Gemini CLI Skills](https://geminicli.com/docs/cli/skills/) · [Gemini CLI Subagents](https://geminicli.com/docs/core/subagents/) · [OpenAI Codex Multi-Agent](https://developers.openai.com/codex/multi-agent/) · [Claude Code Subagents](https://docs.anthropic.com/en/docs/claude-code/sub-agents) · [Claude Code Skills](https://docs.anthropic.com/en/docs/claude-code/slash-commands)*
