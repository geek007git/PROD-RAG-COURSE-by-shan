# Setup Guide

Use this guide when setting up the course for the first time. Run one command at a time and read its output before continuing.

## 1. Check the prerequisites

```bash
git --version
docker --version
docker compose version
python --version
uv --version
```

Expected Python version: 3.12.x. If a command is not found, install that tool before continuing.

On Windows, enable the WSL 2 backend in Docker Desktop. On macOS and Windows, wait until Docker Desktop reports that its engine is running.

## 2. Clone the course

```bash
git clone https://github.com/geek007git/PROD-RAG-COURSE-by-shan.git
cd PROD-RAG-COURSE-by-shan
```

If the repository already exists, do not clone it again. Enter its folder and run `git pull`.

## 3. Create local configuration

macOS/Linux:

```bash
cp .env.example .env
```

Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

The included defaults are for local learning. Some later modules require external keys: Jina for hosted embeddings, Langfuse for tracing, and Telegram only if you enable the optional bot.

Leave optional integrations disabled until their module. Never commit `.env`.

## 4. Install Python dependencies

```bash
uv sync
```

This creates an isolated environment and installs versions locked in `uv.lock`.

## 5. Start infrastructure

```bash
docker compose up --build -d
docker compose ps
```

The first run downloads several large images. Startup is complete only after required services are healthy, not merely after containers exist.

Useful inspection commands:

```bash
docker compose logs api
docker compose logs opensearch
docker compose logs airflow
```

## 6. Verify in increasing order

1. Docker: `docker compose ps`
2. OpenSearch: open <http://localhost:9200>
3. FastAPI: open <http://localhost:8000/docs>
4. Airflow: open <http://localhost:8080>
5. Langfuse: open <http://localhost:3001>
6. Gradio: run `uv run python gradio_launcher.py`, then open <http://localhost:7861>

When verification fails, fix the first failing dependency before testing later ones.

## 7. Download a local model

The configured default is `llama3.2:1b`, chosen for modest hardware.

```bash
docker compose exec ollama ollama pull llama3.2:1b
docker compose exec ollama ollama list
```

## Low-resource setup

The full stack is intentionally realistic and can be heavy. For early modules, start only what you need.

```bash
docker compose up -d postgres opensearch redis api
docker compose up -d ollama
```

Starting `api` also starts its declared dependencies. Confirm exact service names with `docker compose config --services`. Close memory-heavy applications, give Docker at least 6–8 GB RAM, use the 1B model, and start Airflow only when working on orchestration.

## Safe stopping and resetting

```bash
docker compose down       # stop while preserving data
docker compose down -v    # destructive: also delete local course data
```

Use the second command only when you intentionally want a clean environment.

## Next step

Open `notebooks/week1/README.md`, then launch:

```bash
uv run jupyter notebook notebooks/week1/week1_setup.ipynb
```

If anything fails, use [TROUBLESHOOTING.md](TROUBLESHOOTING.md).
