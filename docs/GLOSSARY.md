# Plain-English Glossary

**Agent:** A program that uses a model to choose among a limited set of actions. It should have explicit tools, state, boundaries, and stopping rules.

**Airflow DAG:** A directed graph of scheduled tasks. It describes what must run and which tasks depend on others.

**API:** A defined way for software systems to communicate. FastAPI exposes this project's HTTP API.

**BM25:** A keyword-ranking algorithm. It rewards useful term matches while accounting for term rarity and document length.

**Cache:** Stored output reused for repeated work. It improves speed and cost but needs expiration rules to avoid stale results.

**Chunk:** A smaller passage cut from a document for indexing and retrieval.

**Context:** Text supplied to a model for the current request. In RAG, it normally contains retrieved passages.

**Embedding:** A list of numbers representing aspects of a text's meaning. Similar meanings tend to have nearby vectors.

**Grounding:** Constraining an answer to supplied evidence instead of relying only on model memory.

**Guardrail:** A check that restricts unsafe, invalid, or out-of-scope behavior.

**Hallucination:** A fluent but unsupported or false model output.

**Hybrid search:** Combining keyword and vector retrieval to benefit from exact matching and semantic similarity.

**Idempotent:** Safe to repeat without creating unintended additional effects.

**Index:** A data structure optimized for retrieval. It is not simply a folder of original documents.

**LangGraph:** A library for describing stateful model workflows as nodes and transitions.

**LLM:** Large Language Model. In this course, Ollama runs a model locally.

**Mapping:** OpenSearch's definition of fields and their data types and indexing behavior.

**Metadata:** Structured facts about a document, such as title, authors, date, category, and URL.

**Observability:** Understanding a running system through traces, logs, metrics, and related evidence.

**Prompt:** Instructions and context sent to a language model.

**RAG:** Retrieval-Augmented Generation. Retrieve external evidence, supply it to a model, and generate a grounded response.

**Reciprocal Rank Fusion (RRF):** A method that combines ranked result lists without requiring raw scores to use the same scale.

**Retriever:** The component that returns documents or passages relevant to a query.

**Semantic search:** Retrieval based on similarity of meaning, commonly using embeddings.

**Token:** A unit of text processed by a model or analyzer; it is not always a whole word.

**Trace:** A record of a request's steps and timing across a system.

**TTL:** Time to live—the duration before cached data expires.

**Upsert:** Insert a record if absent, otherwise update the existing record.

**Vector search:** Retrieving items whose embedding vectors are close to a query vector.

**Volume:** Docker-managed persistent storage that can outlive an individual container.
