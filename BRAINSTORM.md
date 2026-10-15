# Cortex Drive - Brainstorming Document

*The drive that reads itself.* Written before any code, as the hackathon asks.

---

## 1. Reframing the problem

A traditional drive is a **noun**: a place where files sit. We wanted to make it a **verb**: something that works on your files while you do other things.

Three questions drove the whole session:

1. What does a person actually want from their files? **Answers, not documents.** Nobody opens Drive to look at a PDF; they open it to find out "what did we agree on?"
2. What work happens *after* a file arrives? Filing it, reading it, copying numbers out of it, comparing it with older files. All of it is mechanical, and none of it is the user's job.
3. Where do people get burned? **When information changes between files** and nobody notices: a budget that moved, a decision that was reversed, a deadline buried in minute 34 of a call.

**Core thesis:** the middleware between user and files is the product. Users never address folders; they address an agent, and the agent orchestrates the file system.

```
 User  ──►  Router  ──►  Librarian  (organise)      ──►  Active
 prompt     (intent)     Researcher (retrieve+cite)       documents
                         Writer     (synthesise)          + generated
                         Analyst    (workflows)           outputs
                         Watcher    (proactive insight)
```

---

## 2. Who are we building for?

| Persona | Pain today | "Aha" we want |
|---|---|---|
| **Project lead** (Sara) with weekly meeting transcripts | Decisions are scattered; numbers drift between meetings | "The budget was $120k in July, **$135k** after Aug 19." One question, with citations |
| **Product manager** (Tariq) after 3 discovery calls | Spends two days merging notes into a spec | Select 3 transcripts, get a cited requirements doc, **with the contradiction already flagged** |
| **Finance / ops** (Omar) | Invoices arrive by email and get typed into a sheet | Drop the PDF; the ledger updates itself |
| **New joiner** | Cannot tell which folder holds what | Folders that name themselves after the project |

---

## 3. Idea storm (everything we wrote down, no filtering)

Rapid-fire, 20 ideas in 15 minutes. Scored 1-5 on **Wow** (demo impact), **Feasible** (can we build it in a hackathon) and **Fit** (does it show "passive to active").

| # | Idea | Wow | Feas | Fit | Verdict |
|---|---|---|---|---|---|
| 1 | Auto-tag and auto-cluster uploads into named folders | 3 | 5 | 5 | **Build** (Librarian) |
| 2 | Ask-your-drive with citations to the exact source | 4 | 5 | 5 | **Build** (Researcher) |
| 3 | Select N files, "draft a unified doc" | 5 | 4 | 5 | **Build** (Writer) |
| 4 | Invoice lands, expense row appears | 4 | 5 | 5 | **Build** (Analyst) |
| 5 | Meeting transcript lands, action items extracted to a tracker | 4 | 5 | 5 | **Build** (Analyst) |
| 6 | **Change-of-mind detector**: flag when a later file reverses an earlier decision | 5 | 4 | 5 | **Build** (Watcher) |
| 7 | **Figure drift**: track a number (budget) across files over time | 5 | 4 | 5 | **Build** (Watcher) |
| 8 | Deadline radar pulled from every document | 4 | 5 | 4 | **Build** (Watcher) |
| 9 | Near-duplicate detector | 2 | 5 | 3 | Build (cheap) |
| 10 | Agent output is saved back into the drive and gets filed like any other file | 4 | 5 | 5 | **Build** (closes the loop) |
| 11 | Visible agent trace ("Router → Researcher → …") | 4 | 5 | 4 | **Build** (trust + demo) |
| 12 | Voice interface for the drive | 3 | 2 | 2 | Cut |
| 13 | Knowledge graph of people, projects, decisions | 5 | 2 | 4 | Roadmap |
| 14 | "Time travel": ask what the drive said about X *as of* a past date | 4 | 3 | 4 | Roadmap (our timeline data already supports it) |
| 15 | Auto-generated weekly digest emailed per project | 3 | 4 | 4 | Roadmap |
| 16 | Agents negotiate access: "Priya's agent asks for the Q3 deck" | 4 | 1 | 3 | Cut |
| 17 | Redaction agent for PII before sharing | 3 | 3 | 3 | Roadmap |
| 18 | Auto-translate and cross-lingual search | 3 | 3 | 2 | Cut |
| 19 | Folder "health score" (stale, duplicated, orphaned) | 2 | 3 | 3 | Cut |
| 20 | Agent-to-agent handoff via MCP so other tools can query the drive | 4 | 2 | 4 | Roadmap |

**What we kept:** everything scoring ≥ 4 on both Wow and Feasible, because the judges weight *innovation* and *visible agentic behaviour*, and the demo is 3 minutes.

---

## 4. The five agents and what each one does

| Agent | Trigger | What it does | Challenge-brief capability |
|---|---|---|---|
| **Librarian** | A file arrives, or "organise my drive" | Detects the file type, extracts keywords, clusters by content, names folders from shared titles. New files are filed incrementally so the folders the user already knows never reshuffle | 1. Proactive Organisation |
| **Researcher** | Any question | Hybrid retrieval over chunks, ranks sentences, answers with numbered citations and a source panel | 2. Semantic Retrieval |
| **Writer** | "Draft / write / generate …" | Picks the right files (or uses the selection), extracts requirements / decisions / constraints / risks / actions, dedupes, cites, appends conflicts, offers *Save to drive* | 3. Cross-Document Synthesis |
| **Analyst** | Event: invoice or transcript lands | Extracts fields, categorises, writes `expenses.csv` / `action_items.csv`. No human involved | 4. Automated Workflows |
| **Watcher** | Always on; also on request | Figure drift, reversed decisions, deadline radar, near-duplicates | *Beyond the brief* |

---

## 5. Agentic user flows (storyboards)

### Flow A: "Drop it and forget it"
```
User drops invoice_nimbus.pdf  (no folder chosen)
   └─► Librarian: reads it, detects "invoice", files it under Finance - Invoices, tags: Infrastructure
   └─► Analyst:   extracts vendor / number / date / total, categorises, appends to expenses.csv
   └─► Activity feed shows both steps; toast tells the user in one line
   User later asks: "How much have we spent on invoices?"  →  Analyst answers from the ledger, with a chart
```

### Flow B: "What did we decide?" (the aha)
```
User: "What was the budget discussed in the Q3 marketing meetings?"
   └─► Router → Researcher: retrieves passages across 3 transcripts + a spreadsheet
   └─► Answer, ordered by date, each line cited [1][2][3]
   └─► Watcher adds: "This figure moved: $120,000 (Jul 8) → $135,000 (Aug 19)"
```
The user asked for *a* number. The drive told them the number *changed*, and when.

### Flow C: "Three calls, one spec"
```
User: "Draft a unified requirements document from the Orbit discovery calls"
   └─► Writer auto-selects calls 1 and 2 by relevance, then notices call 3 sits in the same project folder and pulls it in
   └─► Extracts 15 requirements, 2 decisions, 4 actions, 2 open questions, de-duplicated, every line cited
   └─► Conflict section: "Call 1 asked for SMS notifications; call 3 dropped SMS for push. Resolve before building."
   └─► [Save to my drive]  →  the new doc is filed next to its sources
```

### Flow D: "Anything I should know?"
```
User: "Did anyone change their mind about anything?"
   └─► Watcher scans every pair of documents in the same project
   └─► Reversals + figure drift + upcoming deadlines (with days remaining)
```

---

## 6. Design principles (what the UX is optimising for)

1. **Complexity hidden behind a single input.** One box. The router decides which agent to call; the user never picks a tool.
2. **Show the work.** A short trace under each answer ("Retrieved 7 passages from 4 files…") builds trust and makes the agentic behaviour visible in a 3-minute video.
3. **Citations are first-class.** Every claim links to a file and a snippet. An answer without a source is a hallucination risk.
4. **Never move things the user already learned.** New files are filed incrementally. Re-clustering only happens when the user asks to organise.
5. **Agent output is just another file.** Generated documents are saved back and get filed, indexed and cited like everything else, so the drive compounds.
6. **Works offline, better with Claude.** No API key needed for the demo; with a key, answers and synthesis are written by Claude.

---

## 7. Architecture

```
 Browser (single page)                     Flask API
 ┌───────────────────────┐   /api/agent   ┌────────────────────────────────────┐
 │ Folders (live)        │ ─────────────► │ Router (intent)                     │
 │ Chat + agent trace    │                │  ├─ Researcher → retrieve()         │
 │ Activity / Insights / │ ◄───────────── │  ├─ Writer     → synth()            │
 │ Outputs               │                │  ├─ Analyst    → expenses / actions │
 └───────────────────────┘                │  ├─ Librarian  → reindex()          │
        ▲ upload / drop                   │  └─ Watcher    → insights()         │
        └──────────────── /api/upload ──► │ Ingestion → type, date, summary,    │
                                          │   triggers workflows → reindex      │
                                          └────────────┬───────────────────────┘
                                                       ▼
                       workspace/files   +   TF-IDF chunk index (optional dense embeddings)
                       workspace/_agent_outputs/expenses.csv, action_items.csv
```

* **Retrieval:** chunked TF-IDF with synonym expansion; optional hybrid with `all-MiniLM-L6-v2` embeddings (`USE_EMBEDDINGS=1`). Interface is one function, so swapping in Pinecone / pgvector is a drop-in.
* **Generation:** extractive and rule-based offline; Claude when `ANTHROPIC_API_KEY` is set. Same prompts and citation contract either way.
* **Clustering:** KMeans on document TF-IDF, with k chosen by silhouette score; folder names come from the shared title prefix of the members.

---

## 8. Risks we thought about

| Risk | Mitigation |
|---|---|
| Hallucinated answers | Answers are built from retrieved sentences; every line carries a citation; an empty result says so plainly |
| Agent reorganises your life without asking | Incremental filing only; full re-cluster on explicit request; nothing is deleted or renamed on disk |
| Wrong extraction in a workflow (e.g. total) | Outputs are plain CSVs the user can inspect; the activity feed states exactly what was extracted |
| Privacy of documents sent to an LLM | Offline mode is the default; LLM mode sends only the retrieved passages, not the whole drive |
| Demo fragility | Deterministic seed data, one-click *Reset demo*, no network dependency |

---

## 9. What we cut, and what comes next

**Cut for the hackathon:** voice input, access-negotiating agents, multilingual search.

**Next:** (1) a people / project / decision knowledge graph; (2) "time travel" queries, since figure drift already stores values by date; (3) a weekly per-project digest; (4) MCP server so Slack, email and IDE agents can query the drive; (5) PII redaction before sharing.
