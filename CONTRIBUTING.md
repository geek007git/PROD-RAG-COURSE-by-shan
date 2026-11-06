# Contributing

Student-friendly improvements are welcome: clearer explanations, reproducible bug fixes, tests, accessibility improvements, and small examples.

## Before opening a change

1. Search existing issues and pull requests.
2. Keep one change focused on one problem.
3. Never include `.env`, API keys, tokens, generated credentials, or private documents.
4. Preserve the license and attribution notices.

## Local checks

```bash
uv sync
uv run ruff check src tests
uv run pytest
```

If a check requires infrastructure you cannot run, state exactly what you did and did not verify.

## Documentation style

- Define a term before using its abbreviation.
- Explain why before implementation detail.
- Use copyable commands and state the expected outcome.
- Include Windows alternatives when shell syntax differs.
- Do not describe features the repository does not implement.
- Prefer honest limitations over marketing claims.
