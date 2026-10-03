# CodeAtlas

Traceability between Jira requirements and GitHub code changes. This is our final year project.

- Contributing rules (branches, commits, PRs, conventions): [CONTRIBUTING.md](CONTRIBUTING.md)
- Demo target repo: `codeatlas-sandbox` (Jira project `SBX`)

## Quick start

```bash
python3.12 -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -e ".[dev]"
pytest
```
