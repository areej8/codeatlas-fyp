# Contributing to CodeAtlas

All three of us agreed on these rules at the Day 1 kickoff. Follow them from the first commit.
CodeAtlas will later check its own PRs against these rules, so following them exactly matters.

## 1. Names

| Thing | Name |
|---|---|
| GitHub owner | `areej8` (target org: `codeatlas-fyp`, see note below) |
| Product repo | `codeatlas-fyp` → https://github.com/areej8/codeatlas-fyp |
| Demo target repo | `codeatlas-sandbox` |
| Jira project for our own team tasks | `CA` |
| Jira project for sandbox requirements used in demos | `SBX` |

> **Note:** The kickoff plan named the org `codeatlas-fyp` and the repo `codeatlas`. Right now the repo
> lives under a personal account. If we create the org later, use GitHub's
> *Settings → Transfer ownership*. GitHub redirects the old URL, so nothing breaks.

## 2. Branches

| Branch | Purpose | Who merges |
|---|---|---|
| `main` | Protected. Deploys. Release merges only. | Release PR from `develop` |
| `develop` | Integration branch. Every feature PR targets it. | Feature/fix PRs |
| `feature/CA-<n>-<short-name>` | New work for one Jira ticket | → `develop` |
| `fix/CA-<n>-<short-name>` | Bug fix for one Jira ticket | → `develop` |

Rules:
- `<short-name>` is lowercase kebab-case, e.g. `feature/CA-4-jira-changelog-fetcher`.
- Each branch covers one ticket. If work spans tickets, split it into separate branches.
- Branch from the latest `develop`: `git switch develop && git pull && git switch -c feature/CA-4-...`.

## 3. Commits

Format: `CA-<n> <imperative message>`

```
CA-4 add Jira changelog fetcher
CA-7 fix timezone handling in commit parser
```

- Start every commit with a Jira key. CI rejects commits that don't (see `scripts/check_commit_msgs.py`).
- Write the message in the imperative ("add", "fix", "remove"), not "added" or "fixes".
- Keep the first line at 72 characters or fewer. Put details in the body after a blank line.
- `SBX-<n>` keys are allowed **only in `codeatlas-sandbox`**, never in this repo.

## 4. Pull requests

- **feature/fix → `develop`** needs **1 approval + green CI**. Use squash merge or a merge commit, but
  keep the `CA-<n>` key in the final commit title.
- **`develop` → `main`** is for releases only. Open a PR titled `Release vX.Y.Z`.
- Start the PR title with the ticket key: `CA-4 Add Jira changelog fetcher`.
- Fill in the PR template. Don't approve your own PR.

## 5. Conventions

- **Python 3.12.** The code lives in `src/codeatlas` (src layout).
- **Timestamps are UTC ISO-8601** everywhere: storage, APIs, logs. Example: `2026-10-03T09:00:00Z`.
  Only use timezone-aware datetimes:
  ```python
  from datetime import datetime, timezone
  datetime.now(timezone.utc).isoformat()
  ```
  Never use naive `datetime.now()` or `datetime.utcnow()`.
- **Lint/format:** `ruff check .` and `ruff format .`. CI enforces both.
- **Tests:** `pytest`. Put them under `tests/`.

### Local setup

```bash
python3.12 -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -e ".[dev]"
pytest && ruff check . && ruff format --check .
```

## 6. Jira tickets

Create `CA-1` … `CA-12` for the Day 1–2 tasks before anyone starts coding, so every branch uses
a real key from the first hour. `CA-1` is the repository bootstrap (this file, CI, scaffold).

## 7. Branch protection (repo admin, one time)

The repo is **public**, so branch protection works on the free plan. Under
*Settings → Branches* (or *Rules → Rulesets*):

- **`main`**: require a PR, 1 approval, and the `ci` status check. Block force pushes and deletion.
  Only allow merges from `develop`.
- **`develop`**: require a PR, 1 approval, and the `ci` status check. Block force pushes.
- Set the default branch to **`develop`** so new PRs target it automatically.
