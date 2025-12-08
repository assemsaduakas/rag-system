# rag-system
RAG-based AI system 

# Local City Guide + Event Assistant — Project Description

**Project title:** CityBuddy — Local City Guide & Event Assistant

**One-line pitch:** A RAG-powered assistant that answers questions about local attractions, events, venues, and practical logistics by combining official tourism pages, event listings, venue descriptions, and user-contributed notes.

---

## 1. Main idea & user stories

**Goal:** Let residents and visitors ask natural-language questions about the city and get answers grounded in trusted local sources (events, museums, restaurants, transport info), with citations and times/locations.

**Primary users:** tourists, new residents, event-goers, and city service planners.

**Example user stories:**

* "What family-friendly museums are open on Sunday near the canals?"
* "Which concerts are happening this weekend in Amsterdam under €30?"
* "What's the nearest accessible tram stop to the Rijksmuseum?"

---

## 2. Concepts & design details

**Architecture overview (logical):**

UI (Streamlit/React) → Query embedding → Vector DB search (Qdrant) → Retrieve top-k documents → Rerank/summarize (optional) → LLM prompt (with retrieved docs) → Answer + cited sources to UI.

**Key components:**

* Preprocessor: scrapes and normalizes raw sources (HTML, PDFs, ICS, JSON feeds).
* Chunker: splits long pages into overlapping chunks (200–400 words) with metadata.
* Embedding client: creates vector embeddings for chunks (sentence-transformers or OpenAI).
* Vector DB: Qdrant (Docker) for vector storage & metadata payloads.
* LLM client: OpenAI or local Llama2 for generation; prompt templates enforce source citation and non-hallucination.
* UI: Streamlit for rapid prototype and demo.

---

## 3. Dataset concept

**Sources (initial):**

* City tourism pages (official sites) saved as HTML/TXT.
* Event aggregator feeds (Eventbrite public pages, Meetup public event pages) saved as JSON/HTML.
* Local museums & venue pages (schedules, admission info) as pages or small PDFs.
* Public transport info (static schedules in text or GTFS extracts) for accessibility queries.
* A few user-contributed notes (e.g., local tips) as plain text.

**Metadata fields to store per chunk:**

* `id` (unique)
* `title`
* `source_url`
* `source_type` (event, venue, transport, blog)
* `date` (for events)
* `start_time`, `end_time` (for events)
* `price` (if available)
* `tags` (family_friendly, outdoor, music, museum)
* `geo` (latitude, longitude) — optional but recommended

**Annotation plan:**

* Manually annotate 30–100 representative entries with tags and geo coordinates.
* For events include `date`, `price`, and `venue` fields where possible.

---

## 4. System technical details

**Chosen stack (recommended for demo):**

* Vector DB: **Qdrant** (Docker image `qdrant/qdrant`).
* Embeddings: **sentence-transformers** (`all-mpnet-base-v2`) — local, no API key.
* LLM: **OpenAI GPT-4o-mini** or **gpt-4.1** (if available) — or local Llama2 if offline constraints.
* UI: **Streamlit**.
* Language: Python 3.10+

**Key libraries:**

* `sentence-transformers`
* `qdrant-client`
* `requests`, `beautifulsoup4`, `pdfplumber`
* `streamlit`
* `openai` (optional)

---

## 5. Requirements & hardware

* CPU: modern CPU, 8+ GB RAM recommended for embeddings locally.
* Disk: ~2–5 GB for demo dataset & DB data.
* Docker installed (for Qdrant). WSL2 on Windows if needed.
* Python 3.10+ and pip.

**Python requirements (requirements.txt):**

```
sentence-transformers
qdrant-client
streamlit
beautifulsoup4
pdfplumber
requests
openai
python-dotenv
```

---

## 6. Limitations & safety

* **Data freshness:** Event schedules change rapidly — include `date` and `last_updated` metadata and warn users about possible changes.
* **Hallucinations:** Always include retrieved sources in the prompt and instruct the LLM to refuse to answer outside documented info.
* **Coverage bias:** The assistant is only as good as sources included; it may miss small or private events.
* **Legal/medical/critical instructions:** Provide disclaimers for advice not to be used as official guidance.

