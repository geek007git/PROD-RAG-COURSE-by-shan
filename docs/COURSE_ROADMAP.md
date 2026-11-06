# Course Roadmap

This roadmap turns the repository into a seven-module learning program. Each module has an outcome, practical work, a checkpoint, and an optional stretch task.

## Module 0 — Understand the destination

**Outcome:** Explain RAG in plain English and prepare your machine.

Learn: retrieval, generation, grounding, hallucination, chunk, embedding, index.

Do:

1. Read the RAG mental model in the main README.
2. Complete the prerequisite checks in [SETUP.md](SETUP.md).
3. Draw the four-step pipeline on paper.

Checkpoint: Can you explain why an LLM alone cannot reliably answer questions about private or newly added documents?

Stretch: List three domains where this architecture could replace arXiv papers.

## Module 1 — Infrastructure foundation

**Outcome:** Run and inspect a multi-service AI application.

Learn: container, image, port, volume, health check, API, relational database.

Do:

1. Read `notebooks/week1/README.md`.
2. Complete `week1_setup.ipynb`.
3. Open the FastAPI docs and call the health endpoint.
4. Stop one service, observe the failure, then restore it.

Definition of done:

- `docker compose ps` shows the expected services.
- You can name which service stores metadata, searches text, schedules work, and runs the model.
- You can find the API logs without guessing.

Knowledge check: What is the difference between a Docker image, container, and volume?

## Module 2 — Reliable data ingestion

**Outcome:** Move a paper from an external API into your database.

Learn: rate limit, retry, timeout, idempotency, upsert, orchestration, parsing.

Do:

1. Complete `notebooks/week2/week2_arxiv_integration.ipynb`.
2. Fetch one paper, parse its PDF, and store its metadata.
3. Run the same operation twice and check that it does not create unwanted duplicates.
4. Inspect the Airflow DAG as a graph of tasks and dependencies.

Definition of done:

- At least one paper exists in PostgreSQL.
- A failed PDF does not crash the entire batch.
- You can explain why respectful API rate limiting matters.

Knowledge check: Why is an idempotent ingestion job safer to retry?

## Module 3 — Search before generation

**Outcome:** Build and evaluate a BM25 keyword search baseline.

Learn: inverted index, mapping, analyzer, token, BM25, field boost, relevance score.

Do:

1. Complete `notebooks/week3/week3_opensearch.ipynb`.
2. Compare an exact technical term with a broad natural-language query.
3. Inspect a query built by `src/services/opensearch/query_builder.py`.
4. Create five test questions and record whether a useful paper appears in the top five results.

Definition of done:

- Search returns ranked papers.
- Filters work without changing the query text.
- You have a five-question baseline evaluation set.

Knowledge check: Why can keyword search outperform embeddings for IDs, acronyms, and exact terminology?

## Module 4 — Chunking and hybrid retrieval

**Outcome:** Combine lexical precision with semantic similarity.

Learn: chunk size, overlap, embedding, vector similarity, semantic search, Reciprocal Rank Fusion (RRF).

Do:

1. Complete `notebooks/week4/week4_hybrid_search.ipynb`.
2. Inspect `src/services/indexing/text_chunker.py`.
3. Compare BM25, vector, and hybrid results on your five-question evaluation set.
4. Test one chunk-size change and note its effect.

Definition of done:

- Paper text is stored as retrievable chunks.
- Hybrid search returns results for both exact and conceptual queries.
- You can explain why “bigger chunks” are not always better.

Knowledge check: What information can overlap preserve, and what cost does it add?

## Module 5 — Complete grounded generation

**Outcome:** Generate a useful answer that cites retrieved evidence.

Learn: context window, prompt template, grounding, citation, streaming, dependency injection.

Do:

1. Complete `notebooks/week5/week5_complete_rag_system.ipynb`.
2. Ask one answerable and one unanswerable question.
3. Trace the request from API route to retrieval to model output.
4. Confirm sources shown in the UI actually support the answer.

Definition of done:

- Gradio returns an answer with sources.
- The system admits when the retrieved evidence is insufficient.
- You can identify where the final prompt is assembled.

Knowledge check: Why should the prompt tell the model what to do when context is insufficient?

## Module 6 — Observe, cache, and test

**Outcome:** Diagnose requests and avoid repeating expensive work.

Learn: trace, span, latency, cache key, TTL, cache hit, unit test, integration test.

Do:

1. Complete `notebooks/week6/week6_cache_testing.ipynb`.
2. Ask the same question twice and compare latency.
3. Find the request trace in Langfuse.
4. Run `uv run pytest` and inspect one test before reading its implementation.

Definition of done:

- A repeated query produces a cache hit.
- A trace shows major RAG steps.
- Tests pass, or you documented the exact environmental blocker.

Knowledge check: When can caching return a stale or incorrect answer?

## Module 7 — Agentic recovery

**Outcome:** Use controlled decisions to recover from poor retrieval.

Learn: state graph, node, edge, router, guardrail, document grading, query rewriting, termination condition.

Do:

1. Complete `notebooks/week7/week7_agentic_rag.ipynb`.
2. Read `src/services/agents/agentic_rag.py` as a graph, not as a linear script.
3. Test a strong query, weak query, and out-of-scope query.
4. Confirm the graph cannot retry forever.

Definition of done:

- Relevant questions reach answer generation.
- Weak retrieval can trigger rewriting and another retrieval attempt.
- Out-of-scope questions stop safely.
- The number of retries is bounded.

Knowledge check: Which decisions genuinely need an LLM, and which are safer as deterministic code?

## Final assessment

You are course-complete when you can:

- sketch the entire data and request flow from memory;
- demonstrate a cited answer and verify its evidence;
- diagnose whether a bad answer came from ingestion, retrieval, prompting, or generation;
- explain one engineering trade-off in every module;
- customize the system for a domain other than arXiv.

Choose a capstone from [PORTFOLIO_PROJECTS.md](PORTFOLIO_PROJECTS.md) to turn the course into a portfolio project.
