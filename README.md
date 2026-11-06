# Production Agentic RAG Course

> Build a production-style AI research assistant one understandable step at a time.

This free, project-based course teaches you how to build a Retrieval-Augmented Generation (RAG) system that collects arXiv papers, searches them, answers questions with sources, monitors its work, caches repeated requests, and uses an agent to improve weak searches.

The course is designed for students who know basic Python but have never built a production AI application. Every module answers four questions: **What are we building? Why do we need it? How does it work? How do we know it works?**

## What you will build

By the end, you will have an AI research assistant with:

- an automated arXiv data-ingestion pipeline;
- PostgreSQL metadata storage and OpenSearch retrieval;
- BM25 keyword search and semantic vector search;
- hybrid retrieval and cited answers from a local LLM;
- a FastAPI backend and Gradio chat interface;
- Redis caching and Langfuse observability;
- a LangGraph agent that grades results and rewrites weak queries;
- an optional Telegram interface.

![Course architecture](static/mother_of_ai_project_rag_architecture.gif)

## Choose your learning route

### Beginner route (recommended)

Complete one week at a time. Read the week's README first, run its notebook, complete the checkpoint, and only then continue. Plan for 3–5 hours per week.

### Fast route

If you already know Docker, APIs, databases, and search, read [the course roadmap](docs/COURSE_ROADMAP.md), start the full stack, and use the notebooks as guided architecture tours.

### Low-resource route

If your computer has less than 16 GB RAM, start only the services needed by the current lesson. See [the setup guide](docs/SETUP.md#low-resource-setup) for commands and expectations.

## Curriculum

| Module | Build | Core idea | Proof of completion |
|---|---|---|---|
| 0 | Mental model and setup | What RAG is and is not | Prerequisite check passes |
| 1 | Service foundation | Containers, APIs, databases | Health endpoints respond |
| 2 | Paper ingestion | Reliable data pipelines | A paper is stored in PostgreSQL |
| 3 | Keyword search | BM25, mappings, relevance | Search returns ranked papers |
| 4 | Hybrid retrieval | Chunking, embeddings, RRF | Exact and semantic queries work |
| 5 | Complete RAG app | Grounded generation and citations | UI answers with sources |
| 6 | Production operations | Tracing, caching, testing | Cache hit and trace are visible |
| 7 | Agentic RAG | Grade, route, rewrite, retry | Weak query follows recovery path |

Open the [complete roadmap](docs/COURSE_ROADMAP.md) for objectives, deliverables, exercises, and knowledge checks.

## Start here

### 1. Prerequisites

- Git
- Docker Desktop with Docker Compose
- Python 3.12
- 12 GB RAM recommended (8 GB works with the low-resource route)
- 20 GB free disk space
- [`uv`](https://docs.astral.sh/uv/) for Python dependencies

You do **not** need prior experience with RAG, OpenSearch, Airflow, LangGraph, or vector databases.

### 2. Clone and configure

```bash
git clone https://github.com/geek007git/PROD-RAG-COURSE-by-shan.git
cd PROD-RAG-COURSE-by-shan
cp .env.example .env
uv sync
```

Windows PowerShell equivalent:

```powershell
Copy-Item .env.example .env
uv sync
```

Keep `.env` private. Never commit API keys, bot tokens, or passwords.

### 3. Start the system

```bash
docker compose up --build -d
docker compose ps
```

The first build can take 10–30 minutes. A container marked `starting` is not necessarily broken; give it time, then inspect its logs.

### 4. Verify the API

Open <http://localhost:8000/docs> and run the health endpoint, or use:

```bash
curl http://localhost:8000/api/v1/health
```

### 5. Begin Module 1

```bash
uv run jupyter notebook notebooks/week1/week1_setup.ipynb
```

Read [the full setup guide](docs/SETUP.md) if any command is unfamiliar.

## The simple RAG mental model

```text
Question
  -> retrieve useful passages
  -> place passages in the model prompt
  -> generate an answer constrained by those passages
  -> return the answer with sources
```

“Agentic” RAG adds decisions around this pipeline:

```text
Question -> guardrail -> retrieve -> grade evidence
                                  | good -> answer
                                  | weak -> rewrite -> retrieve again
```

The model is not the knowledge base. Your indexed documents are the knowledge base; the model explains evidence retrieved from it.

## Services you will use

| Service | Local address | Why it exists |
|---|---|---|
| FastAPI docs | <http://localhost:8000/docs> | Test backend endpoints |
| Gradio | <http://localhost:7861> | Chat UI; start separately with `uv run python gradio_launcher.py` |
| OpenSearch | <http://localhost:9200> | Store and retrieve searchable text |
| OpenSearch Dashboards | <http://localhost:5601> | Inspect indexes and queries |
| Airflow | <http://localhost:8080> | Schedule ingestion pipelines |
| Langfuse | <http://localhost:3001> | Inspect traces and latency |
| Ollama | <http://localhost:11434> | Run a local language model |

Some services take longer to become healthy. Airflow credentials are generated locally; see `airflow/simple_auth_manager_passwords.json.generated` after startup.

## How to study each module

1. Read the week's `README.md` and write down the learning objective.
2. Predict what each new component does before opening its implementation.
3. Run notebook cells in order; do not skip failed cells.
4. Change one input and observe the output.
5. Complete the “definition of done” in the [roadmap](docs/COURSE_ROADMAP.md).
6. Explain the data flow aloud without looking at the diagram.
7. Commit your work with a message describing what you learned.

## Student resources

- [Course roadmap](docs/COURSE_ROADMAP.md) — weekly objectives, exercises, and completion checks
- [Setup guide](docs/SETUP.md) — Windows, macOS, Linux, and low-resource instructions
- [Troubleshooting](docs/TROUBLESHOOTING.md) — symptom-first fixes
- [Glossary](docs/GLOSSARY.md) — plain-English definitions
- [Portfolio projects](docs/PORTFOLIO_PROJECTS.md) — extensions that make the project yours
- [Contributing guide](CONTRIBUTING.md) — how to propose improvements

## Repository map

```text
.
├── src/                 # Application code
│   ├── routers/         # HTTP endpoints
│   ├── services/        # Search, RAG, agents, cache, bots
│   ├── repositories/    # Database access
│   └── schemas/         # Validated request/response models
├── notebooks/week1..7/  # Guided hands-on lessons
├── airflow/dags/         # Scheduled ingestion workflow
├── tests/                # Unit, API, and integration tests
├── static/               # Architecture diagrams
├── docs/                 # Student-first learning guides
├── compose.yml           # Local multi-service environment
└── .env.example          # Safe configuration template
```

## Useful commands

```bash
docker compose up --build -d       # start
docker compose ps                  # status
docker compose logs -f api         # follow API logs
docker compose down                # stop, keep data
uv run pytest                      # run tests
uv run ruff check src tests        # lint
uv run jupyter notebook            # open lessons
uv run python gradio_launcher.py   # start the chat UI
```

Avoid `docker compose down -v` unless you intentionally want to delete local course data.

## Responsible use and security

- Generated answers can be wrong; verify claims against cited papers.
- Treat external documents as untrusted input.
- Do not expose local development services directly to the internet.
- Replace example secrets before deployment.
- Restrict Telegram access if you enable the bot.
- Do not upload private or copyrighted documents without permission.

This repository is a learning environment, not a hardened production deployment.

## Credits and license

This course edition is maintained by **Shanmugapriyan** and expands the original open-source **Mother of AI / Production Agentic RAG Course** by **Jam With AI**. The structure, explanations, exercises, and student guides in this edition are customized for a clearer beginner experience.

### Code contributors and acknowledgements

- **Jam With AI** — creators of the original open-source course and implementation.
- **[Andrej Karpathy](https://github.com/karpathy)** — code contributor credit for the base code used to build the Agentic RAG implementation in `src/services/agents/`.

The project is available under the MIT License. Because this is a derivative work, both the original 2025 Jam With AI copyright notice and the 2026 Shanmugapriyan notice are preserved in [LICENSE](LICENSE).

---

If you can explain the pipeline, debug a failed retrieval, and prove your answer came from evidence, you have learned more than how to copy a RAG demo—you have learned how to engineer one.
