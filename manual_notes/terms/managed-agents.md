---
term: Claude Managed Agents (Anthropic)
sphere: ai-models
source: Anthropic — launched public beta April 8, 2026
tldr: Anthropic's fully managed cloud platform for running Claude as a long-running, stateful, autonomous agent — removes all infrastructure scaffolding so teams ship production agents in days, not months.
---

## Definition

Claude Managed Agents is Anthropic's hosted agent execution platform, launched April 8, 2026. Official tagline: "get to production 10x faster." It provides the complete infrastructure required to run Claude as a persistent, multi-step autonomous agent — without the developer having to build any of the execution environment, sandboxing, session management, or tool integration layer.

## The problem it solves

Before Managed Agents, every team building a production Claude agent had to construct the same scaffolding from scratch. Anthropic's own characterisation:

> "Shipping a production agent requires sandboxed code execution, checkpointing, credential management, scoped permissions, and end-to-end tracing. That's months of infrastructure work before you ship anything users see."

The default pattern — bundling the LLM controller, tools, execution environment, and session state into a single long-running container — was hard to scale, a single point of failure, and a security liability. Teams were reinventing the same infrastructure repeatedly.

## How it works

The architecture decouples three mutually distrustful components:

**Agent** — model + system prompt + tools + MCP server definitions. Created once, referenced by ID across many sessions.

**Environment** — a configured cloud container template (pre-installed packages, network access rules, mounted files). Provisioned on-demand only when Claude actually needs to execute code — not at session start.

**Session** — a running agent instance. The session is an append-only, durable event log stored externally — it survives harness crashes, network drops, and container failures. The "brain" (Claude + routing harness) is decoupled from the "hands" (Linux sandbox containers).

**Result:** p50 time-to-first-token dropped ~60%; p95 dropped over 90%.

## API surface

```
POST /v1/agents          — define model, system prompt, tools
POST /v1/environments    — configure container
POST /v1/sessions        — launch session (agent + environment)
POST/GET /v1/sessions/{id}/events  — send messages, receive streamed responses (SSE)
```
All requests: `anthropic-beta: managed-agents-2026-04-01` header.

**Built-in tools:** Bash, file operations, web search/fetch, MCP server connections.

## Pricing
- **Tokens:** standard model rates (Claude Opus: $5/M input, $25/M output)
- **Session runtime:** $0.08/session-hour, billed to the millisecond; idle time free
- **Web search:** $10 per 1,000 calls

## vs. Messages API

| | Messages API | Managed Agents |
|--|---|---|
| What you build | Agent loop + tools + sandbox | Nothing |
| Best for | Fine-grained control | Long-running, async, production |
| State | Developer manages | Durable server-side event log |
| Infrastructure | Self-provisioned | Fully managed |
| Time to production | Months | Days |

## Early enterprise customers
- **Notion** — task delegation within workspace
- **Rakuten** — sales/marketing/finance agents integrating with Slack and Teams (live within a week)
- **Asana** — project management automation

## Why it matters for GPU compute infrastructure

1. **Workload shift:** Sessions run for minutes or hours — changing GPU demand from short-burst requests to long-horizon sustained workloads. This changes provisioning models for GPU infrastructure providers.
2. **New cost axes:** Agent workloads generate costs across tokens, runtime hours, and tool usage simultaneously — traditional GPU/token-cost frameworks are insufficient for enterprise budgeting.
3. **Storage pressure:** Each session generates a durable event log (conversation history, tool call outputs, file artefacts). At enterprise scale, this is significant persistent storage demand — a natural use case for IC S3.
4. **Networking:** Multi-agent coordination and persistent sessions place new demands on cluster networking and storage I/O that bare-metal providers must account for.
5. **Competitive signal:** Anthropic managing the infrastructure layer means GPU providers must provide the raw compute environment (containers, networking, storage) at a higher quality floor — the "last mile" of agent reliability falls on infrastructure.

## Why it matters for ICN positioning

The proliferation of managed agent platforms (Anthropic's, plus equivalents emerging from OpenAI, Google, and AWS) accelerates the demand for:
- **Persistent, low-cost storage** for session event logs, agent memory, and artefacts → IC S3
- **Reliable, low-latency compute** for session runtime → ICN GPU infrastructure
- **Context portability** — as enterprises accumulate months of agent session history on one provider, switching cost grows. This is the "context is gold" dynamic: the data gravity moat compounds over time.

## Key sources

- [Claude Managed Agents overview — Anthropic Docs](https://platform.claude.com/docs/en/managed-agents/overview)
- [Launch blog — Anthropic](https://claude.com/blog/claude-managed-agents)
- [Engineering post — Anthropic](https://www.anthropic.com/engineering/managed-agents)
- [SiliconANGLE coverage](https://siliconangle.com/2026/04/08/anthropic-launches-claude-managed-agents-speed-ai-agent-development/)
