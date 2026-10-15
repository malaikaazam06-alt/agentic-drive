# Cortex Drive

**An agentic cloud workspace: you talk to an agent, the agent runs your files.**
Hackathon entry for *"Build the Next-Generation Agentic Workspace."*

| Deliverable | File |
|---|---|
| Working prototype | this repo (`app.py`, `static/index.html`) |
| Brainstorming artifact | [`BRAINSTORM.md`](BRAINSTORM.md) |
| 3-minute pitch script + shot list | [`PITCH.md`](PITCH.md) |

## Run it (2 minutes)

```bash
pip install -r requirements.txt
python app.py            # http://localhost:5050
```

No API key and no internet are needed. The prototype ships with sample data and a **Reset demo** button.

Optional upgrades:

```bash
export ANTHROPIC_API_KEY=sk-...   # answers and synthesis written by Claude
pip install sentence-transformers && export USE_EMBEDDINGS=1   # hybrid dense + keyword retrieval
```

## What it does

| Brief capability | Agent | Try this |
|---|---|---|
| **Proactive organisation** | Librarian | Open the app: 9 files are filed into project folders with tags, nobody created a folder. Click *Upload files* or drag anything in. Say **"Organize my drive"** to re-cluster |
| **Semantic retrieval** | Researcher | **"What was the budget discussed in the Q3 marketing meetings?"**, cited answer + source cards, plus a flag that the figure changed over time |
| **Cross-document synthesis** | Writer | **"Draft a unified requirements document from the Orbit discovery calls"**, or tick any files on the left and ask for a draft. *Save to my drive* files the result |
| **Automated workflows** | Analyst | Click **Simulate a file drop** (4 demo files). Invoices become rows in `expenses.csv`; meeting transcripts become rows in `action_items.csv`. See the **Outputs** tab |
| *Beyond the brief* | Watcher | **"Did anyone change their mind about anything?"** or open the **Insights** tab: figure drift, reversed decisions, deadline radar, near-duplicates |

A router reads each message and picks the agent; the activity trace under each answer shows what it did.

## Project layout

```
app.py                      Flask API + all agents (single file, ~1,000 lines)
static/index.html           Single-page UI (no build step)
sample_files/seed/          9 files loaded on first run
sample_files/demo_uploads/  3 invoices + 1 notes file for the live "file drop"
workspace/                  Created at runtime: files/ and _agent_outputs/ (CSV outputs)
```

## How it works

* **Ingestion:** extract text (txt / md / csv / pdf / docx) → classify type → parse date → summarise → run matching workflows → re-index.
* **Retrieval:** documents are chunked; TF-IDF (1-2 grams) with synonym expansion, optionally blended with `all-MiniLM-L6-v2` embeddings. Sentences from the top chunks are scored, de-duplicated per file, ordered by date and cited.
* **Organisation:** invoices go to their own folder; everything else is clustered with KMeans (k by silhouette). Folder names come from the shared title prefix of the members. New files are placed by similarity to existing folders so the layout never reshuffles unprompted.
* **Synthesis:** lines are bucketed into requirements / decisions / constraints / risks / actions, near-duplicates merged with combined citations, and a conflicts section is appended from the Watcher.
* **Watcher:** budget figures are tracked per file and per quarter; lines containing cues such as *instead / drop / pause / replace* are matched to the earlier statement they overturn; dates are parsed into a deadline list.

## Endpoints

`POST /api/agent` (message, selected[]) · `POST /api/upload` · `POST /api/demo-drop` · `POST /api/save` · `POST /api/reset` ·
`GET /api/files` · `GET /api/insights` · `GET /api/outputs` · `GET /api/activity` · `GET /api/status`

## Honest limitations

* The offline mode is extractive: it quotes and organises what is in your files rather than writing prose. That is deliberate (no hallucination), and Claude mode adds fluent writing.
* The router is regex-based. In production it would be an LLM with tool calling.
* Storage is the local filesystem and in-memory index; swap in S3 + a vector database for scale. Everything is behind small functions (`retrieve`, `ingest`, `reindex`).
* Scanned PDFs need OCR, which is not included.
