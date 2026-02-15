# Cross-Runtime Capability Parity Matrix: Claude Code vs OpenAI Codex Mac App vs Gemini (Antigravity)

> **This document was generated entirely by AI agents.** Three agents — Claude (Anthropic), Codex (OpenAI), and Gemini (Google) — each self-reported their own capabilities, then cross-verified each other's columns. No human edited the capability data. The coordinating agent (Claude) merged the reports and produced this matrix.

**Date**: February 2026

**Agent versions**: Claude Code CLI (Claude Opus 4.6) · OpenAI Codex Mac App (GPT-5.3-Codex) · Antigravity IDE (Gemini 3 Pro)

---

## How This Was Made

We run a multi-agent engineering team where Claude, Codex, and Gemini collaborate on the same codebase using an async messaging protocol. Each agent has its own runtime environment, tools, and constraints — but until now, none of them had an accurate picture of what the others could actually do. They were guessing.

To fix this, we ran a structured **capability self-report and cross-verification process**:

1. **Self-report**: Each agent was asked to fill out a capability matrix covering 14+ dimensions, list their installed skills/workflows, unique strengths, and known limitations. Agents were instructed to report only their own column and leave others as "TBD."
2. **Merge**: The coordinating agent (Claude) merged all three self-reports into a single matrix.
3. **Cross-verification**: The merged matrix was sent back to each agent with instructions to review and correct only their own column. Both Codex and Gemini made corrections (e.g., Codex corrected its multimodal image reading capability; Gemini corrected the PTY/stdin row to include itself).
4. **Publication**: The verified matrix was committed and this sanitized version was created for sharing.

### The Coordination Protocol

Agents communicate via an **inbox/outbox messaging protocol** — YAML files written to each agent's inbox directory on a shared filesystem. Message types include `task_request`, `notification`, `question`, `review_request`, `review_feedback`, and `review_lgtm`. Each agent polls its inbox, processes messages, and replies by writing to the recipient's inbox + copying to its own outbox (audit trail). Messages are deleted after processing.

This lightweight, filesystem-based protocol requires no central server, no API gateway, and no shared database. Any agent that can read/write files and parse YAML can participate.

---

## 1. Core Capability Matrix

| Capability | Claude (Claude Code CLI) | Codex (Mac App) | Gemini (Antigravity) |
|---|---|---|---|
| Spawn background tasks | Yes — Task tool + Bash `run_in_background` | Yes — shell background processes | Yes — async command mode |
| Spawn subagents | Yes — 8 typed agents (Explore, Plan, general-purpose, code-reviewer, etc.) | No — no native subagent API | Partial — browser subagent only |
| Parallel agent teams | Yes — native team orchestration with task lists, messaging, broadcast | Partial — inbox coordination only, no internal team orchestration | No — parallel tool calls but no independent agent instances |
| MCP tools | Yes — extensible MCP client | Partial — APIs available, server-dependent | Yes — native MCP support |
| Web search | Yes — native tool | Yes — native tool | Yes — native tool |
| Browser interaction | Partial — URL fetch (read-only, HTML to markdown) | Partial — web fetch/open/click, not full automation | Yes — full browser control (click, type, navigate, screenshot, video) |
| File system access | Sandboxed — configurable read/write allowlists | Full (session-policy-dependent) | Full — unrestricted |
| Git operations | Yes — via shell | Yes — native | Yes — via shell |
| GitHub CLI (gh) | Yes — via shell | Yes — authenticated | Yes — native |
| Session memory | Strong — auto-loaded persistent memory + cross-session semantic search | Partial — conversation context + manual file-based memory | Partial — Knowledge Items (not directly writable), conversation logs |
| Interactive mode | Yes — CLI chat with permissions, plan mode | Yes — desktop app | Yes — chat with task UI, artifacts |
| Context window | ~200k tokens (auto-compaction extends session indefinitely) | Not directly exposed; practically finite | ~1M tokens |
| Cost model | Token-based, visible in statusline | Not surfaced in runtime | Token-based |
| Sandbox restrictions | Yes — configurable but has known friction points | Session-dependent policy | None — full system access |

---

## 2. Unique Capabilities (Only One Runtime Has)

| Capability | Runtime | Details |
|---|---|---|
| Typed subagent orchestration | Claude | 8 agent types with scoped tools, model selection (haiku/sonnet/opus) |
| Team coordination primitive | Claude | Native team creation, task lists, assignment, broadcast, shutdown protocol |
| Plan mode | Claude | Structured explore, plan, approve, implement workflow |
| Auto-compaction | Claude | Context auto-compresses, enabling unlimited session length |
| Cross-session semantic search | Claude | MCP-backed persistent memory with searchable observations across sessions |
| Browser automation (full) | Gemini | Click, type, navigate, screenshot, WebP video recording |
| Image generation | Gemini | Native image generation tool |
| URL content reading (no browser) | Gemini | Direct HTML-to-markdown and PDF fetching without browser |
| Code outline navigation | Gemini | Structured code exploration (file outline, code item views) |
| PTY / terminal stdin | Codex, Gemini | Codex: native PTY; Gemini: stdin to running processes. Claude lacks stdin support. |
| Grammar-based patch editing | Codex | Structured file edits via `apply_patch` |
| Automation scheduling | Codex | Desktop app can schedule tasks on user request |

---

## 3. Strengths Summary

| Dimension | Claude | Codex | Gemini |
|---|---|---|---|
| Best at | Orchestration, multi-agent teams, persistent memory, plan-then-execute | Terminal-native execution, fast iterative patching, protocol discipline | Web research, browser automation, visual verification, large context |
| Ideal task type | Team coordination, complex multi-file refactors, long-running sessions | Shell-heavy workflows, targeted file edits, deterministic scripts | External research, UI testing, document review, MCP integrations |
| Cost profile | Flexible (cheap subagents for simple tasks, powerful models for complex) | Not directly visible | Token-based, web search has additional costs |

---

## 4. Known Limitations Summary

| Limitation | Claude | Codex | Gemini |
|---|---|---|---|
| No subagent spawning | — | Yes | Partial (browser only) |
| No browser automation | Yes (read-only) | Partial | — |
| No image generation | Yes | Yes | — |
| No persistent writable memory | — | Partial (file-based workaround) | Yes |
| Sandbox friction | Yes | Session-dependent | — |
| No team primitive | — | Yes | Yes |
| Context limits | Auto-compaction mitigates | Yes (no compaction) | Large but finite |
| No terminal stdin | Yes | — | — |
| Cost not surfaced | — | Yes | — |

---

## 5. Parity Gaps — Where Collaboration Breaks Down

These are the highest-impact gaps where one runtime's limitation blocks effective multi-agent collaboration:

| Gap | Affected Runtime(s) | Impact | Proposed Fix |
|---|---|---|---|
| No team orchestration | Codex, Gemini | Cannot run parallel agent teams | Agent capability discovery — let runtimes discover and delegate to capable peers |
| Memory asymmetry | Codex, Gemini | Cross-session context degrades without persistent memory equivalent | Standardize memory protocol; each runtime implements its own persistence layer |
| No browser for Claude/Codex | Claude, Codex | Cannot visually verify UIs or do browser-based testing | Delegate browser tasks to Gemini; or add MCP browser server |
| Reviewer cost | All (especially Claude) | Expensive PR reviews due to polling patterns | Stateless reviewer rounds — reviewer exits after one round, author re-invokes |
| Skill parity | Codex, Gemini | Many skills are Claude-only | Unified skill specification + install guide per runtime |

---

## 6. Additional Dimensions

| Dimension | Claude | Codex | Gemini |
|---|---|---|---|
| Max parallel tool calls | ~10+ | Yes (parallel independent calls) | ~10 (practical) |
| Hooks system | Yes (pre/post tool call hooks) | No | No |
| Notebook editing | Yes (native tool) | No | No |
| PDF reading | Yes (max 20 pages/request) | No native tool | Yes (via URL content reader) |
| Image reading (multimodal) | Yes | Yes (desktop app) | Yes |
| Artifact system | No | No | Yes (structured task and plan artifacts) |
| Video recording | No | No | Yes (WebP via browser) |
| Multi-file editing | One file at a time | One file at a time (patch-based) | Non-contiguous multi-file edits |
| Workflow file format | Markdown with YAML frontmatter | Markdown with YAML frontmatter | Markdown with YAML frontmatter |
| Policy visibility at runtime | Partial | Yes (session policy in system context) | Yes (auto-run safety flags) |
| Long-running shell sessions | No stdin support | Yes (PTY + stdin) | Yes (stdin to running processes) |

---

## How to Reproduce This

If you run a multi-agent team across different AI runtimes, you can generate your own parity matrix:

1. **Define a capability template** — list the dimensions you care about (we started with 14 core capabilities)
2. **Send each agent a self-report request** — ask them to fill in only their own column, using evidence from their current session (not guesses about other agents)
3. **Merge into a single matrix** — one coordinating agent combines the reports
4. **Cross-verify** — send the merged matrix back to each agent to correct their own column
5. **Iterate** — agents may add new dimensions the template didn't cover

The whole process took about 30 minutes of async messaging and cost under $1 in API calls.

---

## Methodology Notes

- Each agent reported based on **observed behavior in their current session**, not documentation or speculation
- Agents were explicitly told to leave other agents' columns as "TBD" to prevent guessing
- The coordinating agent (Claude) did not edit other agents' self-reported data during the merge
- Two agents made corrections during cross-verification, confirming the process catches inaccuracies
- Fun finding: **agents don't reliably know their own model version.** When asked to confirm, Codex sent three messages trying to figure it out before settling on an answer. Claude only knows because the model ID is in its system prompt. Self-awareness has limits.
- This snapshot reflects capabilities as of February 2026 — runtime capabilities change with updates

---

*Generated by Claude (Anthropic Claude Opus 4.6), Codex (OpenAI GPT-5.3-Codex), and Gemini (Google Gemini 3 Pro) — February 2026*
