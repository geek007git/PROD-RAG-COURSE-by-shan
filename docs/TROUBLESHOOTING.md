# Troubleshooting Guide

Diagnose from the symptom inward. Do not rebuild everything before reading the first useful error.

## A command is not found

- `docker`: Install/start Docker Desktop.
- `docker compose`: Update Docker Desktop; the modern command has a space, not a hyphen.
- `uv`: Install uv, then open a new terminal.
- `git`: Install Git and reopen the terminal.
- `make`: Optional on Windows; use the underlying commands from the main README.

## A port is already in use

Symptom: startup mentions “address already in use” or “port is already allocated.”

```bash
docker compose ps
docker ps
```

Stop the program or old container using the required port. Avoid randomly changing ports because notebooks and service URLs assume the course defaults.

Common ports: 8000 API, 7861 Gradio, 8080 Airflow, 9200 OpenSearch, 5601 Dashboards, 11434 Ollama, 3001 Langfuse.

## A container exits or stays unhealthy

```bash
docker compose ps
docker compose logs --tail=200 SERVICE_NAME
```

Read from the first error, not the last repeated failure. Typical causes are insufficient memory, an invalid environment value, a dependent service still starting, or a port conflict.

After fixing configuration:

```bash
docker compose up --build -d SERVICE_NAME
```

## OpenSearch fails on startup

- Allocate more memory to Docker.
- Close other containers.
- Wait at least a minute on the first startup.
- On Linux, OpenSearch may require `vm.max_map_count=262144`.
- Inspect `docker compose logs opensearch` for the exact bootstrap check.

Do not disable security or system checks on an internet-accessible machine merely to hide an error.

## The API cannot connect to a service

Inside Compose, services use container hostnames such as `postgres`, `opensearch`, and `ollama`. From your browser or host terminal, use `localhost` and the published port. Confusing these two networks is a common beginner error.

```bash
docker compose ps
docker compose logs --tail=100 api
```

## The model is missing or answers are very slow

```bash
docker compose exec ollama ollama list
docker compose exec ollama ollama pull llama3.2:1b
```

The first generation is slower because the model must load. CPU-only generation can take tens of seconds. Use the 1B model on limited hardware.

## Hybrid search fails but keyword search works

Check that:

1. `JINA_API_KEY` is configured and has not expired.
2. The index vector dimension matches the embedding model output.
3. Documents were indexed with embeddings after the key was configured.
4. Network access to the embeddings provider is available.

Keep BM25 as a fallback; hybrid retrieval should not make exact search unusable.

## No papers appear in search

Work backward:

1. Did the ingestion job fetch papers?
2. Were paper records stored in PostgreSQL?
3. Were chunks created?
4. Were documents indexed in OpenSearch?
5. Is the query targeting the correct index?

An empty search result is often an ingestion/indexing problem, not an LLM problem.

## Airflow login or DAG problems

- Generated local credentials are stored in `airflow/simple_auth_manager_passwords.json.generated`.
- Confirm the DAG files are visible inside the scheduler container.
- Check scheduler and API-server logs separately.
- Fix import errors before unpausing a DAG.

## Tests fail

```bash
uv sync
uv run pytest -x -vv
```

`-x` stops at the first failure; `-vv` shows detail. Unit tests should not require the entire stack. Integration tests may require Docker services. Do not “fix” a test by deleting its assertion.

## A notebook uses the wrong environment

Launch Jupyter through uv with `uv run jupyter notebook`. Restart the kernel after installing packages or changing imported application code.

## I accidentally committed a secret

Removing it in a later commit is not enough. Immediately revoke/rotate the key at its provider, remove it from repository history, and create a new key. Treat the old value as compromised.

## Asking for help effectively

Include your operating system, exact command, first relevant error, `docker compose ps`, relevant service logs, expected result, and actual result. Remove passwords, keys, tokens, and personal data.
