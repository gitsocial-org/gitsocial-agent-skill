---
name: gitsocial
description: >
  Manage GitSocial — a git-native social coding platform. Use when the user
  wants to create or manage issues, pull requests, posts, releases, code reviews,
  milestones, sprints, or lists using GitSocial (gitmsg protocol). Handles
  project management, social interactions, release management, and code review
  — all stored as git commits. Also use for importing from GitHub.
license: MIT
compatibility: Requires gitsocial CLI in PATH. Works in any git repository.
metadata:
  author: gitsocial-org
  version: "0.1.0"
  repository: https://github.com/gitsocial-org/gitsocial-agent-skill
---

# GitSocial Agent Skill

Interact with GitSocial from natural language. Everything maps to `gitsocial` CLI commands run in the user's git repository.

## CRITICAL: No piping, no scripts, no processing

**STOP. Read this before doing anything.**

Every `gitsocial` command you run MUST be a standalone command with NO pipes. Specifically:

- **NEVER** use `| python3`, `| jq`, `| awk`, `| grep`, `| head`, or any pipe
- **NEVER** write inline scripts or one-liners to process output
- **NEVER** use `2>&1 | <anything>`
- **NEVER** use raw `git log`, `git show`, or other git commands to read GitSocial data. Raw git shows the committer, not the original author (imported items lose authorship). Always use `gitsocial search`, `gitsocial log`, or extension `show` commands.

Use `--group-by` for reports and summaries — it returns compact grouped output you can read directly. Use `--json` when you need structured data for specific items.

**Correct pattern:**
```bash
gitsocial search --type pr --after 2025-03-23 --group-by state --json
```

**Wrong pattern (NEVER do this):**
```bash
gitsocial search --type pr --json | python3 -c "import json, sys; ..."
```

## Prerequisites

1. **Check CLI exists**: Run `gitsocial status` first. If it fails, tell the user to install GitSocial.
2. **Check extension init**: Before using an extension (social, pm, review, release), check if it's initialized with `gitsocial <ext> status`. If not, offer to run `gitsocial <ext> init`.
3. **Working directory**: Always run commands from the user's git repository root. Use `--workdir` flag if needed.
4. **User identity**: For "my issues" / "my PRs" queries, get the user's email with `git config user.email` and use it with `--author` or `--assignee`.

## Answering Questions — Search First

When the user asks about project state, history, or content, use `gitsocial search` as your **first choice** — it queries across all extensions (issues, PRs, posts, releases) in one call.

**For reports and summaries, use `--group-by`:**
```bash
gitsocial search --type pr --after 2025-03-23 --group-by state --limit 200 --json
gitsocial search --type pr --after 2025-03-23 --group-by author --top 5 --limit 200 --json
gitsocial search --type issue --state open --group-by label --count-only --limit 200 --json
gitsocial search --type issue --state open --group-by assignee --limit 200 --json
gitsocial search --after 2025-03-23 --group-by type --count-only --limit 200 --json
```

`--group-by` returns compact grouped output with counts. Use `--top N` to limit items per group. Use `--count-only` for just counts. This keeps output small enough to read and present directly. Use `--limit 200` (or higher) with `--group-by` to capture all items — the default limit of 20 may miss results for reports.

**For multi-dimensional reports**, run 2-3 grouped searches:
```bash
# PR report: by state, then by author
gitsocial search --type pr --after 2025-03-23 --group-by state --top 5 --limit 200 --json
gitsocial search --type pr --after 2025-03-23 --group-by author --limit 200 --json
```

**For finding specific items:**
```bash
gitsocial search "authentication bug" --type issue --json
gitsocial search --type pr --state open --json
gitsocial search --type issue --state open --labels bug --assignee dev@example.com --json
gitsocial search --draft --json
```

**Filters:** `--type`, `--author`, `--repo`, `--after`/`--before`, `--scope`, `--sort`, `--state`, `--labels`, `--assignee`, `--reviewer`, `--milestone`, `--sprint`, `--draft`, `--prerelease`, `--tag`, `--base`. Extension-specific filters auto-imply their type.

**Groupable fields:** state, author, type, extension, repo, label, assignee, reviewer, milestone, base.

**For full item detail, use `gitsocial show`:**
```bash
gitsocial show #commit:<hash>           # auto-detects extension
gitsocial show #commit:<hash> --json    # structured output
```
Takes a hash from any search result — no need to know the extension. Shows the complete item (issue with comments, PR with review summary, release with artifacts, post with thread).

**Prefer `search` and `show` for all read operations.** These two commands cover finding, filtering, grouping, and detail views across all extensions. This also means fewer commands for the user to approve. Only use extension-specific commands for writes (create, edit, close, merge) and specialized views (board, diff, artifacts).

## Global Flags

All commands accept:
- `--json` — machine-readable JSON output (use this when you need to parse results)
- `-C, --workdir <path>` — target a different git repository

## Extensions Overview

| Extension | Purpose | Key Commands |
|-----------|---------|--------------|
| `social` | Posts, timeline, lists | `social timeline`, `social post`, `social comment` |
| `pm` | Issues, milestones, sprints | `pm issue create/list/show/close`, `pm milestone`, `pm sprint` |
| `review` | Pull requests, code review | `review pr create/list/show/merge`, `review feedback` |
| `release` | Versions, artifacts, SBOM | `release create/list/show/edit` |

## Write Operations (search and show can't do these)

For exact flags on any command, run `gitsocial <command> --help`.

### Key write patterns

```bash
# Issues
gitsocial pm issue create "Fix login timeout" --body "Description" --labels "bug" --assignees "dev@example.com"
gitsocial pm issue comment <ref> "comment text"
gitsocial pm issue close <ref>

# Pull Requests
gitsocial review pr create "Add profiles" --base "#branch:main" --head "#branch:feature" --reviewers "email"
gitsocial review feedback approve <ref>
gitsocial review feedback request-changes <ref> -m "message"
gitsocial review pr merge <ref> --strategy squash

# Posts (markdown and LaTeX math supported)
gitsocial social post "text"
gitsocial social comment <ref> "text"

# Releases
gitsocial release create "v1.2.0 — Title" --tag v1.2.0 --version 1.2.0

# Lists
gitsocial social list create my-list --name "My Projects"
gitsocial social list add my-list https://github.com/user/repo
```

### Other operations

```bash
gitsocial fetch                                       # fetch updates
gitsocial push                                        # push local changes
gitsocial notifications                               # unread notifications
gitsocial notifications read-all                      # mark all read
gitsocial social timeline --list my-list              # chronological feed
gitsocial import all https://github.com/org/repo      # import from GitHub
```

## Ref Format

GitSocial uses refs to identify items: `[repo_url]#type:value`
- Full: `https://github.com/user/repo#commit:abc123def456`
- Workspace-relative: `#commit:abc123def456`
- Types: `commit`, `branch`, `tag`, `file`, `list`

When showing refs to the user, use the short format. When a command needs a ref, use what `--json` output provides.

## Error Handling

- **"extension not initialized"** → Run `gitsocial <ext> init` and retry
- **"not a git repository"** → The command must run inside a git repo
- **"repository not found"** → Check the URL, may need `gitsocial social list add` first
- **"command not found: gitsocial"** → User needs to install GitSocial CLI

## Tips

- Use `--group-by` for reports, `--json` for structured data on specific items
- The `log` command shows cross-extension activity: `gitsocial log --limit 10`
- For full CLI reference with all flags, read `references/cli-reference.md` or run `gitsocial <command> --help`
- For autonomous agent patterns (scheduled tasks, batch operations, agent-to-agent), read `references/agentic-workflows.md`
- Fork management: `gitsocial fork add <url>` to discover PRs from forks
- Settings: `gitsocial settings list` to see user preferences
