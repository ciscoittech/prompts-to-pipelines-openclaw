# From Prompts to Pipelines

**Everyone uses AI. Nobody uses systems. That's why it keeps failing you.**

You paste into ChatGPT. It works once. The next day, same prompt, different results. You re-explain your entire career to Claude every Monday. You generate a cover letter and it sounds like a robot cosplaying a human. You ask for a project plan and get something that could apply to literally any project on earth.

The AI isn't broken. Your process is. You're using the most powerful tool ever built like a magic 8-ball: shake it, hope for a good answer, start over.

This book fixes that. You build three real systems that solve real problems using the same principles that have always worked: find the bottleneck, build the process, measure the result. Solved problems make money. Systems make it repeatable.

---

## What You Build

Three systems. Each one more complex than the last. Each one abstracted from a production deployment that runs daily.

### Arc 1: CertScout (Ch 1-5) — Your First System

A compliance auditor finds clients by word of mouth. Meanwhile, a public registry tracks thousands of organizations whose certifications are expiring. She doesn't have time to check it manually.

You build the system that does it for her. In twenty minutes.

- **Ch 1** — Install OpenClaw, write one skill file, run it. The system scrapes the registry, scores leads, and stores results. Run it again and it only shows what's new. That's the magic moment.
- **Ch 2** — Name the four concepts that made it work. Diagnose why every other AI interaction you've had hasn't.
- **Ch 3** — Turn the system prompt from a wish into a contract. Weighted scoring with calibration.
- **Ch 4** — Skills: persistent knowledge the system loads automatically. It drafts personalized outreach using the right credentials for the right target.
- **Ch 5** — Schedule it. Error handling. State hygiene. The full pipeline runs unattended.

**Revenue:** $500-$2,000/month retainer. The system surfaces $100K+ in contracts annually.

### Arc 2: Vetta (Ch 6-9) — The Job Agent Done Right

Everyone's building job agents. 400 of them shipping this month. They all produce the same output: cover letters that sound like a robot wrote them.

The difference between yours and theirs: yours checks its own work.

- **Ch 6** — 3-layer routing. 80% of requests handled without an LLM call. When NOT to use agents.
- **Ch 7** — The humanization problem. A static rubric catches AI tells for zero tokens. An LLM judge reviews what passes. An evaluator-optimizer loop retries or admits defeat.
- **Ch 8** — Multi-source job ingestion. Normalize everything to one schema. TTL expiry so you never apply to dead postings.
- **Ch 9** — The full pipeline with feedback loops. Track which resume versions get callbacks. The system learns from outcomes.

**Revenue:** Deploy for career coaches at $2,000-$5,000/month. Replace 60-100 hours/week of manual work.

### Arc 3: Prism (Ch 10-13) — One System, Many Clients

CertScout serves one client. Vetta serves one user. Prism serves ten clients from one codebase, each with their own voice, their own brand rules, their own data. Nobody sees each other's work. This is where a project becomes a business.

- **Ch 10** — Multi-tenant MongoDB. Per-client skill loading. Same engine, different paint jobs.
- **Ch 11** — Voice profiles built from examples, not descriptions. Model routing: cheap model for classification, quality model for generation.
- **Ch 12** — Compliance gates that block publication, not just flag it. Subagent delegation: researcher, generator, reviewer, each with constraints on what they can and cannot do.
- **Ch 13** — Persistent memory with provenance labels. The system knows the difference between "the client said this" and "I inferred this." Conversations survive restarts.

**Revenue:** 30-40% of revenue growth above baseline. 10 clients = $12,000-$16,000/month from a system you maintain 10 hours/week.

### Arc 4: Synthesis (Ch 14-17) — Run It, Fix It, Sell It

- **Ch 14** — Token economics. CertScout costs $0.006/month. Prism costs $80-$160/month. Five strategies to cut costs without cutting quality.
- **Ch 15** — When systems break. A debugging protocol that maps symptoms to components in 60 seconds.
- **Ch 16** — Design your own system from scratch. Nine-step process. Start with a frustration, end with a revenue model.
- **Ch 17** — Packaging systems as businesses. Three pricing models. Why you never charge hourly.

---

## The Framework

You don't memorize this framework. You build with it. Each concept is named *after* you've already used it.

```
INSTRUCTION  ──  What you tell the system to do
                 Failure: "It didn't do what I wanted"

MEMORY       ──  Anything that persists between sessions
                 Failure: "I have to re-explain everything"

CONTROL      ──  Checks and constraints that catch mistakes
                 Failure: "It gave me confident garbage"

FLOW         ──  Stages where each feeds the next
                 Failure: "It tried to do everything at once"
```

Six components implement these concepts: **Prompt**, **Skill**, **State**, **Hook**, **Connection**, **Pipeline**. Three patterns shape every system: **Loop**, **Gate**, **Router**. One debugging protocol diagnoses any failure: **Symptom** → **Component** → **Isolate** → **Fix** → **Check**.

The tools change every six months. The framework doesn't.

---

## The Companion

The book comes with a Build Coach. Paste a chapter into any AI tool alongside the Build Coach prompt. Your AI becomes a guided tutor for that chapter's build, checking your work against the quality gate and pointing to specific sections when you're stuck.

**Free:** Chapter 2's Build Coach prompt (included in the book, works with any AI tool, no setup).

**With purchase:** Build Coach prompts for all 17 chapters, plus API access to the companion system with the features that turn a working system into a production system.

---

## Beyond the Book

The book teaches you to build. The companion teaches you to operate. These are the skills that separate a side project from a business.

**RAG-Powered Knowledge Base** — The entire book is embedded and searchable. Ask a question, get an answer with chapter citations and working code references. Your AI tool becomes book-aware through the companion API or MCP server integration.

**Golden Evals** — Pre-built test suites for every system. Feed your CertScout output and find out if the scoring weights are calibrated. Run your Vetta cover letters through the humanization rubric. Verify Prism's tenant isolation. These are the same quality gates from the book, automated so you can run them on demand.

**Observability** — Every AI call traced with model, tokens, cost, and latency. Know exactly what your system costs to run before your client asks. Langfuse integration means you can see which pipeline stage is the bottleneck, which model is underperforming, and where you're burning tokens on work that could be handled by code.

**Model Distillation Strategy** — The book teaches you to prototype on frontier models and distill the reasoning into cheaper ones. Use Claude or GPT for the hard work (voice matching, complex analysis), then move the patterns you've validated into DeepSeek, Qwen, or open-weight models at 1/50th the cost. The model ladder in Chapter 14 shows you exactly when to use a $0.04/M-token model versus a $15/M-token model. The goal is making the latest model release irrelevant to your bottom line.

**Self-Correcting Systems** — The evaluator-optimizer pattern from Chapter 7 is the foundation: generate, check, retry, or admit defeat. Golden evals extend this across your entire system. When a quality gate fails, the system doesn't ship garbage. It flags what broke, maps it to the responsible component, and gives you the fix. Your systems get better because they measure themselves.

**Cost Tracking** — Token-level cost attribution per client, per pipeline stage, per model. CertScout runs for $0.006/month. Prism runs for $80-$160/month across 10 clients. The difference is deliberate, not accidental. You learn to engineer your costs the same way you engineer your pipelines.

---

## Read the Free Chapter

**[Chapter 2: Why That Worked (And Why Most AI Doesn't)](book/chapters/ch02-v4-draft.md)** — The diagnostic framework that lets you identify what's broken in any AI interaction in 60 seconds. No setup required. Includes the free Build Coach prompt.

---

## What's Inside

17 chapters across 4 arcs, ~61,000 words. Each chapter builds a working system.

| Arc | Chapters | System | Complexity |
|-----|----------|--------|------------|
| Arc 1 | Ch 1-5 | CertScout (compliance lead gen) | Entry |
| Arc 2 | Ch 6-9 | Vetta (job agent + humanization) | Mid |
| Arc 3 | Ch 10-13 | Prism (multi-client management) | Advanced |
| Arc 4 | Ch 14-17 | Synthesis (cost, debugging, design, revenue) | Applied |

---

## Who This Is For

You already use AI. It kind of works. You want it to actually work: reliably, across sessions, without babysitting.

You might be:
- A freelancer who wants to build systems for clients (and charge monthly for them)
- A developer who's tired of one-shot prompts that don't compose into anything durable
- A consultant who sees AI transforming their industry and wants to be the one building the tools
- Someone who's built one AI thing that works and wants to know how to build the next ten

You don't need to be a programmer. You do need to be willing to open a terminal and type commands. The book walks you through everything, but it doesn't pretend the terminal doesn't exist.

---

## Runtime

All examples use [OpenClaw](https://openclaw.ai), an open-source AI agent runtime with 182K+ GitHub stars. OpenClaw is an orchestrator, not a coding tool. It connects to messaging apps, runs on a schedule, and executes tasks using whatever AI model you point it at.

The book is honest about OpenClaw's security track record (multiple CVEs in 2026, including a CVSS 9.9 privilege escalation). These aren't hidden. They're used as teaching tools for the Control concept. Every security concern becomes a lesson in why automated checks matter.

All patterns (skill files, plugins, scheduled workflows, quality gates) transfer to Claude Code, Codex, or whatever ships next. OpenClaw is demonstrated. The framework is portable.

---

## About

**Terrence Battlehunt.** Network engineer turned AI systems builder. Founder of [Frontier Engineering](https://frontierengineering.dev). Building products and raising his daughter from abroad.

The three systems in this book are abstracted from production deployments across multiple businesses. The framework comes from building AI systems that have to work every day, for real clients, with real money on the line. The failures are real. The fixes are real. The revenue is real.

The hardest part of AI isn't building it. It's finding the person with the boring problem who'll pay monthly for it. The answer isn't a marketing funnel. It's the people you already know. Every system in this book started with a relationship, not a landing page.

[@bmoreopensource](https://twitter.com/bmoreopensource) · [Engineering Abroad](https://engineeringabroad.substack.com)
