# From Prompts to Pipelines — Outline v4

**Status:** CANONICAL (supersedes outline-v3.md)
**Date:** 2026-06-24
**Hook:** "Everyone uses AI. Nobody uses systems. That's why it keeps failing you."

---

## The Thesis

Your reader already uses AI. They paste into ChatGPT, ask Claude for help, generate content with Gemini. It works sometimes. It fails unpredictably. They re-explain context every session. They can't trust output without checking it manually.

The gap isn't AI knowledge — it's systems thinking. They're using AI as a magic box when they should be wiring it into a system with memory, quality gates, and feedback loops.

This book doesn't teach AI. It teaches systems. And every system in this book makes money.

---

## The Three Throughlines

Each throughline is abstracted from a real production system. The architecture lessons are real. The domain details are generic enough to protect client identities while remaining specific enough to be credible.

| Book Name | Abstracted From | Complexity | Domain | Revenue Model |
|-----------|----------------|------------|--------|---------------|
| **CertScout** | Tone Security (PREA) | Entry | Compliance auditor lead gen | Retainer ($500-$2K/mo) or finder's fee |
| **Vetta** | AI-JobHunter | Mid | Job search + humanization | SaaS ($20-50/mo), placement fee, or white-label |
| **Prism** | Rome Major | Advanced | Multi-tenant creator/streamer management | Rev share (30-40% of growth) |

### CertScout — The Compliance Auditor's Lead Machine
A solo compliance auditor finds clients through word-of-mouth. Certifications expire on public record. The system scrapes expiring certifications from public data, scores leads by urgency and value, stores them in a database, and drafts personalized outreach. One data source, one user, one output channel. The simplest system that makes real money.

### Vetta — The Job Agent Done Right
Everyone's building job agents. Most produce obvious AI slop. Vetta is transparent about what it is — a job search system — and honest about what everyone gets wrong. The angle isn't "build a job agent" but "here's why yours doesn't work and mine does." The differentiator: a humanization pipeline that rejects its own output until it passes a quality rubric.

### Prism — One System, Many Clients
A content manager handles scheduling, voice matching, and multi-platform posting for streamers and creators. Each client has their own voice profile, brand rules, and approval workflow. Same codebase, isolated data. This is where a project becomes a business — one client is consulting, ten clients on the same system is a platform.

---

## Runtime

**Primary:** OpenClaw (open-source AI agent runtime, self-hosted)
**Why:** Orchestrator, not a coding tool. Skills system maps directly to the book's Skill component. 23+ messaging channels. Model-agnostic. 182K+ GitHub stars = audience actively searching for tutorials.

**Security posture:** Addressed honestly throughout. OpenClaw has real CVEs (including CVSS 9.9 privilege escalation). The book uses this as a teaching tool for the Control concept — not hidden, not apologized for, turned into a lesson.

**Runtime-agnostic in principle:** All patterns (skill files, plugin tools, scheduled workflows) transfer to Claude Code, Codex, or any agent runtime. OpenClaw is demonstrated, not required.

---

## The Framework (taught through building, not before it)

### 4 Universal Concepts

| Concept | What It Means | The Failure It Prevents |
|---------|--------------|------------------------|
| **Instruction** | What you tell the AI to do | "It didn't do what I wanted" |
| **Memory** | Anything that persists between sessions | "I have to re-explain everything" |
| **Control** | Checks and constraints that catch mistakes | "It gave me confident garbage" |
| **Flow** | Multi-step sequences where stages feed each other | "It tried to do everything at once" |

### 6 Components

| Concept | Component | What It Is | Where Introduced | Where Named |
|---------|-----------|-----------|------------------|-------------|
| Instruction | **Prompt** | Structured request / system prompt as contract | Ch 1 | Ch 3 sidebar |
| Instruction | **Skill** | Reusable knowledge doc loaded on demand | Ch 4 | Ch 4 sidebar |
| Memory | **State** | Persistent storage across sessions | Ch 1 (SQLite) | Ch 2 |
| Control | **Hook** | Automated check before/after actions | Ch 1 (dedup) | Ch 7 sidebar |
| Flow | **Connection** | External data — APIs, web search, MCP | Ch 8 | Ch 8 sidebar |
| Flow | **Pipeline** | Multi-stage workflow with quality gates | Ch 5 | Ch 5 sidebar |

### 3 Design Patterns

| Pattern | What It Does | Where Introduced | Where Named |
|---------|-------------|------------------|-------------|
| **Loop** | Process → Check → Improve → Repeat | Ch 1 (daily scrape) | Ch 5 sidebar |
| **Gate** | Stage → Quality Check → Pass or Rework | Ch 1 (dedup) | Ch 7 sidebar |
| **Router** | Decision Point → Path A or Path B | Ch 3 (scoring) | Ch 5 sidebar / Ch 6 deepened |

---

## Chapter-by-Chapter Outline

---

### ARC 1: CertScout — Your First System (Ch 1-5)

The reader builds a complete automated lead generation system for a compliance auditor. By Ch 5, it runs unattended, finds real opportunities, and the reader understands every component because they built each one.

---

#### Chapter 1: Twenty Minutes to a Working System

**Primary throughline:** CertScout
**Concepts used (unnamed):** Instruction, Memory, Control, Flow
**Components built (unnamed):** Prompt (skill file), State (SQLite via plugin)
**Runtime:** OpenClaw

**What happens:**

The reader installs OpenClaw, pastes a config, writes one skill file (the "daily scout" workflow), and runs it. Within 20 minutes they have a system that:
- Scrapes a public data source (CSV endpoint) for expiring certifications
- Scores each lead by urgency and estimated value
- Stores results in SQLite (deduplicating against prior runs)
- Sends a digest message via Telegram or terminal

The system works. The reader runs it again the next day — it remembers what it found yesterday and only shows new leads. This is the magic moment.

**What the reader does NOT know yet:** They don't know the words "Instruction," "Memory," "Prompt," or "State." They just used all four.

**Setup section:**
- Install OpenClaw (`npm i -g openclaw` or curl one-liner)
- Create `openclaw.json` with gateway bound to `127.0.0.1` (security — explained later)
- Set up OpenRouter API key in `.env`
- Model: DeepSeek V4 Flash (cheap, fast, good enough)
- Channel: Telegram or terminal output for first run

**Revenue sidebar:** This system finds $6K-$10K audit contracts. The person who built it for a compliance auditor charges a monthly retainer or takes a finder's fee on closed deals. You just built something worth money in 20 minutes.

**Quality gate:** Run the system twice. Second run shows only NEW leads, not duplicates. The system remembered.

---

#### Chapter 2: Why That Worked (And Why Most AI Doesn't)

**Primary throughline:** CertScout (reflection on Ch 1)
**Concepts named:** Instruction, Memory, Control, Flow

**What happens:**

The reader just built something that works across sessions. Now they learn WHY — and why their ChatGPT/Claude interactions don't.

This chapter names the four universal concepts by mapping each to something the reader already did:

- **Instruction:** The skill file. It told the system what to do, in what order, with what constraints. Not "find me leads" — a structured workflow.
- **Memory:** The SQLite database. It persists between sessions. The system doesn't re-explain itself because the state survives.
- **Control:** The deduplication check. The scoring rules. The `127.0.0.1` gateway binding. Constraints that prevent garbage output and unauthorized access.
- **Flow:** The multi-step workflow: scrape → dedupe → score → store → digest. Each stage feeds the next.

Then show what breaks when each concept is missing:
- Remove state (memory) → system re-reports old leads every run
- Remove scoring (control) → every lead looks equally important
- Remove stages (flow) → system tries to scrape, score, and draft emails in one confused prompt
- Remove the skill file (instruction) → vague "find me leads" produces hallucinated results

**"Before" picture:** The reader's own AI usage mapped to the four concepts. Which are present? Which are missing? This is the diagnostic tool they carry for the rest of the book.

**Quality gate:** Reader can name which of the four concepts is missing when they describe a frustrating AI interaction.

**Reuse:** ~40-50% from v3 Ch 1-2. The "coinflip problem" framing, the four concepts table, the "goldfish to colleague" spectrum, and the diagnostic exercise all transfer. Examples change to CertScout.

---

#### Chapter 3: Scoring Leads — Structured Data and Weighted Rules

**Primary throughline:** CertScout
**New concept through building:** Structured data extraction
**Components deepened:** Prompt (system prompt as contract), State (structured schema)

**What happens:**

The reader's scoring is basic. Now they build a real lead scoring system with weighted rules:
- Audit urgency: expired = highest (0-60 pts)
- Facility type: juvenile scores higher than adult — harder to fill (0-20 pts)
- Estimated contract value (0-20 pts)

The system prompt becomes a contract — not "find leads" but: "Extract these 8 fields. Score using these weights. Output as JSON matching this schema. If a field is missing, mark it null — do not invent data."

This is where "prompt engineering" gets reframed as "writing contracts, not wishes."

**Sidebar: "The Prompt as Contract"** — Names the Prompt component. Links back to Ch 2's Instruction concept. Not about "better prompts." About precision that removes ambiguity.

**Revenue sidebar:** The scoring model IS the product. Anyone can scrape public data. The scoring that surfaces the RIGHT leads first is what clients pay for.

**Quality gate:** Feed the system 10 leads with known attributes. Scoring ranks them in correct order. Change one weight — ranking changes predictably.

**Reuse:** ~30% from v3 Ch 4. The 4-part formula (Context, Input, Output, Constraints) transfers, applied to CertScout system prompt.

---

#### Chapter 4: Templates and Outreach — When AI Writes for Someone Else

**Primary throughline:** CertScout
**New components:** Skill (credentials file, outreach templates)

**What happens:**

The system finds leads. Now it drafts outreach. The reader builds:
- A **credentials skill file** — the client's bio, certifications, competitive edges. Loaded every time the system drafts anything. The system never forgets who it's working for.
- **Outreach templates** — cold email, follow-up, call script. Variable slots filled with lead data + credentials.
- A **generation step** that combines templates with scored lead data and credentials.

The skill file pattern: prompts change per task, skills are persistent domain knowledge.

**Sidebar: "Skills — Reusable Expertise"** — Names the Skill component. The reader already built one (credentials file). Maps to Instruction concept at a higher level: prompts are per-task, skills are per-domain.

**Skill design principles:**
- Show, don't describe (examples of the client's voice, not descriptions of it)
- Keep under 2,000 words (bloated skills waste tokens every turn)
- Version your skills (note what changed, roll back if worse)
- One skill per domain

**Revenue sidebar:** The outreach drafts are the deliverable. Client reviews and sends. Time saved: ~45 minutes per lead contacted. At 20 leads/week = 15 hours of work automated.

**Quality gate:** Generate 3 outreach emails. Each correctly references the client's specific credentials AND the lead's specific situation. No hallucinated credentials. No wrong facility names.

**Reuse:** ~40% from v3 Ch 6. Skill design principles, voice matching pedagogy, feedback loop between state and skill all transfer.

---

#### Chapter 5: Scheduled Agents and CertScout Wrap-Up

**Primary throughline:** CertScout (completion)
**New concepts:** Cron/scheduled execution, system maintenance
**Components completed:** Pipeline (scrape → score → store → draft → notify)

**What happens:**

The reader turns their manual system into an automated one. CertScout runs on a schedule:
- Cron via OpenClaw or system cron job — run at 6am and 6pm, not continuously
- Error handling: what happens when the data source is down
- State hygiene: archiving old leads, keeping active database lean
- Full pipeline diagram: every component visible, every stage named

**Sidebar: "The Pipeline — Stages That Feed Each Other"** — Names the Flow concept fully. The reader has been building a pipeline since Ch 1 without calling it that. Now the shape is visible: scrape (Connection) → dedupe (Control) → score (Control) → store (Memory) → draft (Instruction) → notify (Flow).

**Sidebar: "Three Design Patterns"** — Loop, Gate, Router. Named AFTER the reader has used all three:
- The daily scrape is a **Loop** (run daily, accumulate)
- Deduplication is a **Gate** (pass or reject)
- Scoring that routes different facility types to different value estimates is a **Router**

**Revenue sidebar:** The finished CertScout runs itself. Builder's ongoing value: tuning the scoring model and expanding to new data sources. Monthly retainer: $500-$2,000 for a system that surfaces $100K+ in contracts annually.

**CertScout complete system diagram:**
```
[Cron Trigger]
    ↓
[Scrape] → public CSV endpoint
    ↓
[Dedupe] → check against SQLite (GATE)
    ↓
[Score] → weighted rules (ROUTER by facility type)
    ↓
[Store] → SQLite (STATE)
    ↓
[Draft] → credentials skill + templates (INSTRUCTION)
    ↓
[Notify] → Telegram digest (FLOW)
    ↓
[Loop] → next scheduled run
```

**Quality gate:** System runs unattended for 3 days. Each day's digest shows only new leads. No duplicates, no missed leads, no errors.

**Reuse:** ~35% from v3 Ch 9. Pipeline entry/exit criteria, quality gates between stages, "Maintain It" sections from Ch 5 (state hygiene) and Ch 9 (pipeline bottlenecks).

---

### ARC 2: Vetta — The Job Agent Done Right (Ch 6-9)

The reader starts a new system from scratch. The entry is honest: everyone's building job agents. The difference between theirs and the 400 others shipping this month is that theirs won't produce obvious AI garbage.

---

#### Chapter 6: The Job Agent — Why Everyone's Building Them Wrong

**Primary throughline:** Vetta (begins)
**New concepts:** Multi-agent routing, when NOT to use agents

**What happens:**

The chapter opens with the problem: "You're going to build a job search agent. So is everyone else. The difference is yours won't produce obvious AI garbage."

Introduces the 3-layer routing architecture:

- **Layer 1 — Deterministic routing:** No LLM call. Pattern matching on request type. `route("scrape")` → scraper workflow. `route("apply")` → resume workflow. No intelligence wasted on routing.
- **Layer 2 — Workflows:** Fixed-step prompt chains for predictable tasks. Scraping is always: search → extract → score → match. Resume generation is always: generate → audit → retry if score < threshold.
- **Layer 3 — Full agent:** Only for open-ended interaction. Chat messages that don't map to known commands.

The lesson: use the simplest implementation that works. Don't make everything an "agent" when a script or workflow handles it.

**Sidebar: "The Router at Three Levels"** — Deepens the Router pattern from Ch 5. Shows routing at the request level (Layer 1), workflow level (Layer 2), and agent level (Layer 3). The reader saw routing as a scoring decision in CertScout. Now it's an architectural principle.

**Revenue sidebar:** The job agent itself isn't the business (too many free alternatives). The business is customization: deploying it for career coaches ($5-15K/student), staffing agencies, or corporate HR teams.

**Quality gate:** Send 3 different request types through the system. All 3 reach the correct handler without an LLM routing call.

**Reuse:** ~20% from v3. Heavy reference to production code patterns from AI-JobHunter coordinator.

---

#### Chapter 7: The Humanization Problem — Quality Gates That Actually Work

**Primary throughline:** Vetta
**New concepts:** Quality gates, LLM-as-judge, humanization audit
**Components deepened:** Hook (automated checks), Control concept fully realized

**What happens:**

Every AI-generated cover letter sounds like AI. Recruiters can tell. The reader builds:

**1. The static rubric** (pure code, no AI call):
- Banned phrases detection
- Passive voice percentage
- Sentence length variance
- Filler word count
- Emdash frequency
- Scores 0-100. This is the Hook — automated, deterministic, costs zero tokens.

**2. The 3-pass humanization audit:**
- Pass 1: Generate cover letter using career profile (Skill) + job posting
- Pass 2: Run static rubric. Score < 60? Auto-reject and regenerate with specific feedback.
- Pass 3: LLM-as-judge reviews for "would a human write this?" with 6-dimension scoring rubric.

**3. The evaluator-optimizer loop:**
- Generate → audit → retry (max 2 rounds)
- If it can't pass after 2 retries, flag for human review — don't ship garbage

**Sidebar: "Hooks — Guard Rails That Cost Nothing"** — Names the Hook component. The static rubric IS a hook. It runs before output ships. It's code, not an AI call. Every check moved from "ask the AI to verify" to "run a script" is faster, cheaper, more reliable.

**Sidebar: "The Gate Pattern in Production"** — The evaluator-optimizer loop IS the Gate pattern. Pass or rework. Real stakes.

**Revenue sidebar:** The quality gate IS the product differentiation. Anyone can generate a cover letter. The system that rejects its own bad output and iterates until it passes — that's what clients pay for. That's the moat.

**Quality gate:** Generate 5 cover letters. All score >60 on static rubric. Feed in a deliberately AI-sounding letter — rubric catches and rejects it.

**Reuse:** ~30% from v3 Ch 7. Hook types, "most expensive mistake test," hook tuning content transfers.

---

#### Chapter 8: Multi-Source Ingestion — Feeding the System Real Data

**Primary throughline:** Vetta
**New concepts:** Connection (APIs, web search, scraping), TTL data management
**Components introduced:** Connection

**What happens:**

The job agent needs data from multiple sources:
- SearXNG meta-search (aggregates multiple job boards)
- Greenhouse/Lever APIs (company career pages)
- Direct job board scraping

The reader builds:
- Multi-source ingestion with normalization (different sources, same schema)
- TTL-based data expiry (jobs expire after 60 days — stale data is worse than no data)
- Company research connections for cover letter personalization (web search for recent news, values, tech stack)

**Sidebar: "Connections — When Local Files Aren't Enough"** — Names the Connection component. Decision framework: if information exists in your files and doesn't change, you don't need a connection. If you need current information, data from another tool, or data you don't have yet — you need a connection.

**Quality gate:** System ingests jobs from 2+ sources. A question about a specific company gets answered with recent data the system didn't have in its files.

**Reuse:** ~45% from v3 Ch 8. "When to connect vs. local files" decision framework, connection health, cost monitoring all transfer.

---

#### Chapter 9: The Full Job Agent Pipeline

**Primary throughline:** Vetta (completion)
**New concepts:** Design system enforcement, end-to-end pipeline with feedback loops

**What happens:**

Assemble the full Vetta pipeline:

```
[Ingest] → pull jobs from connections (scheduled)
    ↓
[Match] → score against user profile
    ↓
[Generate] → tailored resume + cover letter (Skill: career profile)
    ↓
[Audit] → static rubric + LLM-as-judge (Hook: humanization gate)
    ↓
[Review] → present to user with scorecard
    ↓
[Track] → update state with application details, outcome
    ↓
[Learn] → feedback loop — which approaches get callbacks? Adjust strategy.
```

**Copy style guide as Skill:** The output (resume, cover letter, emails) must conform to a copy style guide. Reader builds it as a Skill and enforces it through Hooks.

**Sidebar: "The Loop at System Scale"** — The feedback loop from application outcomes to generation strategy IS the Loop pattern operating across weeks, not within a single session.

**Revenue sidebar:** Complete Vetta replaces 3-5 hours of job search work per day. Deployed for a career coach with 20 clients = 60-100 hours/week automated. Monthly value: $2,000-$5,000 per deployment.

**Quality gate:** Run full pipeline on 3 real job postings. Each produces a tailored application that passes humanization gate. State file tracks all 3 with correct statuses.

**Reuse:** ~35% from v3 Ch 9. Entry/exit criteria, stage-level monitoring, when to add/remove stages.

---

### ARC 3: Prism — One System, Many Clients (Ch 10-13)

The most complex system. CertScout serves one client. Vetta serves one user. Prism serves many clients, each with their own voice, rules, and data. This is where systems become businesses.

---

#### Chapter 10: Multi-Tenant Architecture

**Primary throughline:** Prism (begins)
**New concepts:** Multi-tenant design, per-client isolation, namespace separation

**What happens:**

The framing: "CertScout serves one client. Vetta serves one user. Prism serves many clients, each with their own voice, rules, and data. This is where systems become businesses."

The reader builds:
- Multi-tenant MongoDB namespaces (each client gets their own data)
- Per-client skill loading (each client's voice profile, brand rules, content rules)
- Same codebase, different context per tenant
- Telegram bot + Mini App as the client interface

The scaffold: one codebase, two test clients, each seeing only their own data and voice profile.

**Sidebar: "State at Scale — From Files to Databases"** — The progression: markdown state files (early CertScout concept) → SQLite (CertScout) → MongoDB with TTL (Vetta) → MongoDB with namespaces (Prism). Same concept (Memory), different implementation as scale demands.

**Revenue sidebar:** Multi-tenant is the difference between a project and a business. One client = consulting. Ten clients on the same system = a platform. Management takes 30-40% of revenue generated above baseline. At 5 clients averaging $5K/month in new revenue = $7,500-$10,000/month in management fees.

**Quality gate:** Two test clients in the same system. Client A's content uses Client A's voice. Client B's uses Client B's. Neither sees the other's data.

---

#### Chapter 11: Voice Matching and Content Pipelines

**Primary throughline:** Prism
**New concepts:** Voice matching, content pipeline, model routing
**Components deepened:** Skill (voice profile as production asset), Pipeline (content-specific)

**What happens:**

The content pipeline:

```
[Plan] → batch content calendar (categorized by platform + type)
    ↓
[Generate] → draft using client's voice profile Skill + platform rules
    ↓
[Check] → quality gate with voice score (≥75 auto-approve, <75 revise)
    ↓
[Approve] → client review via Telegram (approve/edit/reject)
    ↓
[Publish] → schedule through posting tool or direct API
```

**Model routing:** Different models for different tasks. Fast/cheap for drafting, standard for analysis, specialized for domain-specific content. The system routes based on content type, not user choice.

**Voice matching:** The voice profile Skill is built from real data — actual posts, actual engagement metrics, actual phrases the client uses. Not a description of their voice but examples OF their voice.

**Sidebar: "Model Routing — The Right Brain for the Job"** — Match the model to the task, not the price to the prestige. A $0.04/M-token model handles classification. A $15/M-token model handles voice-matched generation. Use both.

**Revenue sidebar:** The voice profile is the moat. Anyone can post content. Content that sounds exactly like the client — provably, measurably — justifies management fees.

**Quality gate:** Generate 5 pieces of content for one client. Show alongside 5 pieces the client actually wrote. A reader can't reliably distinguish which is which.

**Reuse:** ~30% from v3 Ch 6 (voice matching pedagogy) + v3 Ch 11 (model routing/cost).

---

#### Chapter 12: Compliance Gates and Subagent Delegation

**Primary throughline:** Prism
**New concepts:** Compliance gates, subagent delegation, TDD agent teams
**Components deepened:** Hook (compliance), Pipeline (multi-agent)

**What happens:**

**Compliance gates:**
- Content compliance checking (platform-specific restrictions, brand safety rules)
- The compliance gate as a Hook that BLOCKS publication, not just flags
- Different compliance rules per client, loaded from per-client Skill files

**Subagent delegation — the TDD agent team pattern:**
- **Researcher:** Gathers information. Cannot write code.
- **Architect:** Writes tests first. Cannot implement.
- **Engineer:** Implements to pass tests. Cannot skip tests.
- **Reviewer:** Read-only validation against spec. Cannot modify.

Each agent has explicit constraints on what it CAN and CANNOT do. The orchestrator delegates and enforces handoff rules.

**Sidebar: "Control at the Guardrail Level"** — Compliance gates are Hooks with legal and financial consequences. The cost of a false negative (bad content published) isn't "client is annoyed" — it's legal liability. This is why Hooks exist.

**OpenClaw security case study:** CVE-2026-32922 (CVSS 9.9) happened because OpenClaw accepted a URL from a query string without validating the source. A gate that checked the origin would have prevented it. Real-world example of Control failure at the infrastructure level.

**Revenue sidebar:** Compliance automation is insurance. The alternative: a human reviewing every piece for legal issues. At 20+ pieces per client per week across 10 clients = a full-time job. The system does it in seconds.

**Quality gate:** Feed the system 3 pieces with deliberate compliance violations. All 3 blocked before publication.

**Reuse:** ~25% from v3 Ch 7. "Most expensive mistake test," false positive/negative tuning, calibration loop.

---

#### Chapter 13: Persistent Memory and Prism Wrap-Up

**Primary throughline:** Prism (completion)
**New concepts:** Persistent conversational memory, checkpointing
**Components completed:** Full Prism system

**What happens:**

Persistent conversational memory — the system remembers conversations across restarts, maintains multi-turn context per client, uses history to improve future responses.

OpenClaw's memory system with provenance labels maps directly:
- **Observed from source** = data scraped or received
- **Confirmed by user** = client-approved content/decisions
- **Inferred by model** = AI conclusions (lowest trust, flagged)
- **Imported from transcript** = conversation history

**Sidebar: "Memory Beyond State Files"** — The progression across the book:
1. SQLite (CertScout) — simple key-value, single user
2. MongoDB with TTL (Vetta) — structured docs, expiring data
3. MongoDB with namespaces (Prism) — per-client isolation
4. MongoDB with checkpointing + provenance (Prism complete) — conversations survive restarts, memory carries trust metadata

Same concept, four implementations. Each matches the system's needs.

**Full Prism system diagram:**
```
[Client A Telegram] ──┐
                      ├──→ [Router] → per-client context loading
[Client B Telegram] ──┘        ↓
                          [Voice Skill A or B]
                               ↓
                    [Content Pipeline]
                    Plan → Generate → Check → Approve → Publish
                               ↓
                    [Compliance Gate] (BLOCKS if violation)
                               ↓
                    [Memory] (checkpointed, provenance-tagged)
                               ↓
                    [Model Router] (right model per task)
                               ↓
                    [Subagent Teams] (researcher → architect → engineer → reviewer)
```

**Revenue sidebar:** Complete Prism manages a roster of clients with minimal human intervention. At 10 clients, the system runs operations that would require 2-3 full-time employees. The builder becomes a one-person agency.

**Quality gate:** Simulated week across 2 test clients. Content generated, checked, approved, scheduled. Conversations persist across restarts. Each client's data and voice remain isolated.

**Reuse:** ~20% from v3 Ch 13. Cross-system patterns, composition pedagogy.

---

### ARC 4: Synthesis — Running, Fixing, and Selling Systems (Ch 14-17)

The reader has three production-grade system blueprints. Now they learn to run them, fix them, design new ones, and package them as businesses.

---

#### Chapter 14: The Cost of Running Systems

**All three throughlines.**

The reader has three working systems. What does it cost to run them?

- Token economics with real numbers from each system
- The model ladder: fast ($0.04-1/M), standard ($3-15/M), reasoning ($15-75/M)
- 5 cost reduction strategies:
  1. Lean state (don't load everything every turn)
  2. Hooks over model judgment (code checks are free, LLM checks cost tokens)
  3. Selective skill loading (only load what's needed for this task)
  4. Cache connections (don't re-fetch what hasn't changed)
  5. Batch pipelines (process 10 leads at once, not one at a time)
- Monitoring: built-in tracking, state file cost logging, Langfuse for production
- Model routing as cost strategy (CertScout scoring on a $0.04 model, Prism voice matching on a $15 model)

**Quality gate:** Estimate session cost within 50% before running it. Apply 2+ cost strategies and measure the difference.

**Reuse:** ~70% from v3 Ch 11. Nearly all cost content transfers with new system examples.

---

#### Chapter 15: When Systems Break

**All three throughlines.**

Failure taxonomy: Symptom → Component → Isolate → Fix → Add check.

Specific scenarios per system:
- **CertScout:** Data source format changes, scoring produces wrong rankings, stale leads not archived
- **Vetta:** Generic voice in cover letters, wrong company details, humanization loop stuck in retry, tracker bloat
- **Prism:** Voice drift between clients, compliance gate false positives, memory sludge (inferred facts treated as confirmed), tenant data leak

Debugging protocol:
1. Identify the symptom
2. Map to the responsible component (Instruction? Memory? Control? Flow?)
3. Isolate the component
4. Fix
5. Add a check so it doesn't happen again

**Quality gate:** Diagnose a real failure in one system, identify the component, fix it, add a check.

**Reuse:** ~75% from v3 Ch 12. Failure taxonomy, debugging protocol, debugging mindset. System-specific scenarios are new.

---

#### Chapter 16: Building Your Own — From Problem to System

**Reader's choice of system.**

The reader designs and builds a new system for a problem not in the book. The design process with one addition from v3: the revenue question.

Design process:
1. **Frustration:** What repetitive task wastes your time (or your client's time)?
2. **Map:** Which of the four concepts does this problem need?
3. **Revenue:** Who pays for this? How much? Why?
4. **Constraint:** What CAN'T the system do? (This is the spec, not a limitation.)
5. **Pattern:** Which of the three shapes (Loop, Gate, Router) fits?
6. **Minimal:** What's the smallest version that proves it works?
7. **Iterate:** Build one component at a time.
8. **Test:** Break it on purpose.
9. **Document:** Write the system diagram.

Anti-patterns:
- "Just add more prompt" (add a component instead)
- "Build it all at once" (one component per iteration)
- "Automate everything" (automate the boring parts, keep judgment human)
- "Set and forget" (systems need maintenance)

**Quality gate:** Working system for a new problem. Reader can articulate what each component does and what the revenue model is.

**Reuse:** ~65% from v3 Ch 14. 8-step design process, anti-patterns. Revenue question is new.

---

#### Chapter 17: Systems That Print Money

**All three throughlines, forward-looking.**

Not a recap. The payoff chapter.

- **Packaging systems as products vs. services:**
  - Product (SaaS): Vetta model — subscription, many users, lower touch
  - Service (consulting): CertScout model — retainer, few clients, higher touch
  - Platform (rev share): Prism model — percentage of growth, aligned incentives

- **The progression:** "I built this for one client" → "I deploy this for many clients"

- **Pricing frameworks:**
  - Retainer: fixed monthly fee (CertScout — $500-$2K/mo)
  - Per-outcome: finder's fee, placement fee (Vetta — 5% of salary)
  - Rev share: percentage of revenue above baseline (Prism — 30-40%)
  - Never hourly. Price on outcome, not effort.

- **Repeating the pattern:** Every system the reader built follows the same shape:
  1. Find someone trading time for money
  2. Build the system that gives them time back
  3. Price on outcome
  4. Keep the IP, repeat for the next client

- **The framework they carry forward:** 4 concepts, 6 components, 3 patterns, 1 debugging protocol. Evaluate any new AI tool in 30 minutes by mapping it to the four concepts.

**Quality gate:** Reader can evaluate a new AI tool within 30 minutes by mapping it to the four concepts. Reader has a revenue model for at least one system they've built.

**Reuse:** ~40% from v3 Ch 15. "Staying current," "teaching others" sections. Revenue/packaging content is new.

---

## Component Introduction Schedule

| Ch | Components Used | Concepts Active | Named After Building |
|----|----------------|-----------------|---------------------|
| 1 | Prompt (skill file), State (SQLite) | Instruction, Memory | No — just build |
| 2 | — (reflection) | All 4 named | Yes — naming Ch 1 |
| 3 | Prompt deepened (system prompt as contract) | Instruction deepened | "Prompt as Contract" sidebar |
| 4 | Skill introduced | Instruction (advanced) | "Skills" sidebar |
| 5 | Pipeline completed, 3 patterns named | Flow, all patterns | "Pipeline" + "3 Patterns" sidebars |
| 6 | Router at architecture level | Flow (routing) | "Router at Three Levels" |
| 7 | Hook introduced | Control | "Hooks" + "Gate Pattern" sidebars |
| 8 | Connection introduced | Flow (external) | "Connections" sidebar |
| 9 | Pipeline (full Vetta), Loop at system scale | Flow (feedback) | "Loop at System Scale" |
| 10 | All components at multi-tenant scale | All | "State at Scale" sidebar |
| 11 | Skill (voice profile), Pipeline (content), Router (model) | All | "Model Routing" sidebar |
| 12 | Hook (compliance), Pipeline (multi-agent) | Control | "Control at Guardrail Level" |
| 13 | State (persistent memory) | Memory (advanced) | "Memory Beyond State Files" |
| 14-17 | All | All | Synthesis |

---

## Revenue by Chapter

| Ch | System | Revenue Angle |
|----|--------|--------------|
| 1 | CertScout | $6-10K contracts found per lead |
| 3 | CertScout | Scoring model = the product |
| 4 | CertScout | 15 hrs/week saved in outreach |
| 5 | CertScout | $500-$2K/mo retainer |
| 6 | Vetta | Customization for coaches ($5-15K/student) |
| 7 | Vetta | Quality gate = the differentiator |
| 9 | Vetta | $2-5K/mo per deployment |
| 10 | Prism | Rev share at 30-40% of growth |
| 11 | Prism | Voice accuracy justifies fees |
| 12 | Prism | Compliance replaces full-time reviewer |
| 13 | Prism | One-person agency managing 10 clients |
| 17 | All | Packaging: retainer vs. per-outcome vs. rev share |

---

## Content Reuse Estimate

| v3 Chapter | Words | Reuse % | What Transfers |
|-----------|-------|---------|---------------|
| Ch 1-2 (Concepts) | ~7,300 | 40-50% | Coinflip problem, 4 concepts, diagnostic exercise |
| Ch 4 (Prompts) | ~2,600 | 30% | 4-part formula |
| Ch 6 (Skills) | ~3,600 | 40% | Skill design principles, voice matching |
| Ch 7 (Hooks) | ~4,000 | 30% | Hook types, "most expensive mistake" |
| Ch 8 (Connections) | ~4,100 | 45% | Connection types, decision framework |
| Ch 9 (Pipelines) | ~4,700 | 35% | Stage design, entry/exit criteria |
| Ch 11 (Cost) | ~2,900 | 70% | Nearly all cost content |
| Ch 12 (Debugging) | ~2,900 | 75% | Failure taxonomy, protocol |
| Ch 13 (Composing) | ~3,200 | 20% | Cross-system patterns |
| Ch 14 (Design New) | ~3,000 | 65% | Design process, anti-patterns |
| Ch 15 (What's Next) | ~2,900 | 40% | Staying current, teaching others |

**Total reusable:** ~21,000 words (~40% of v3's 53K)
**New content needed:** ~35,000-39,000 words
**Estimated book total:** ~60,000 words (17 chapters × ~3,500 avg)

---

## Appendices

| Appendix | Content |
|----------|---------|
| **A** | System Component Reference — every component, when to add it, templates, common mistakes |
| **B** | Complete System Files — CertScout, Vetta, Prism fully built, ready to copy |
| **C** | OpenClaw Setup & Hardening — install, config, security checklist |
| **D** | Troubleshooting — common problems by component with symptoms and fixes |
| **E** | Glossary — one-sentence plain-language definitions |
| **F** | Revenue Models — retainer, per-outcome, rev share templates with sample contracts |

---

## Drafting Sequence

| Phase | Chapters | Why This Order |
|-------|----------|---------------|
| **Phase 1** | Ch 1-5 (CertScout) | Self-contained arc. Tone Security code exists. All concepts introduced. |
| **Phase 2** | Ch 6-9 (Vetta) | Builds on Phase 1 concepts. AI-JobHunter is most mature codebase. |
| **Phase 3** | Ch 10-13 (Prism) | Requires both prior arcs. Rome Major codebase active. |
| **Phase 4** | Ch 14-17 (Synthesis) | ~65-75% adapted from v3. Draft last. |
| **Phase 5** | Appendices | Reference material, compiled after all chapters. |

---

## Scoring Rubric (unchanged from v3)

6 dimensions, 1-5 each. Minimum 20/30 to publish. Clarity < 3 auto-flags rewrite.

| Dimension | What It Measures |
|-----------|-----------------|
| Clarity | Can a non-technical reader follow every sentence? |
| System Thinking | Does the chapter teach a systems concept, not just a task? |
| Voice | Conversational, not textbook? Free of AI tells? |
| Build Quality | Does the project produce a working system the reader can verify? |
| Progression | Adds exactly one new concept? Builds on prior chapters? |
| Quality Gate | Is "you got it right when..." concrete and verifiable? |
