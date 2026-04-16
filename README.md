# GitSocial Agent Skill

*[Agent skill](https://agentskills.io) for [GitSocial](https://gitsocial.org).*

Manage issues, PRs, posts, releases, and more through natural language. Works with Claude Code, Cursor, Copilot, and other agents.

## What You Can Ask

Just describe what you need in natural language. The skill translates your intent into the right `gitsocial` CLI commands.

| Category | Example prompts |
|----------|----------------|
| Reports & insights | Morning, weekly, or monthly summary. Main themes for the quarter or the year. Show notifications. |
| Search & filter | Find all issues mentioning authentication. Which areas have the most bugs? |
| Issues & sprints | What needs triage? What's blocked? Sprint status. How's milestone v2.0 going? |
| PRs & reviews | What PRs need my review? Any stale PRs? How fast are we reviewing PRs? |
| Releases | Are we ready to release? Generate changelog. What went into this release? |
| Social | Post about what we shipped. Summarize the discussion on the authentication feature. |
| Setup & import | Set up GitSocial for this repo. Import everything from our GitHub repo. |

## Install

Requires the `gitsocial` CLI in your PATH.

### User-level (all projects)

```bash
git clone https://github.com/gitsocial-org/gitsocial-agent-skill ~/.claude/skills/gitsocial
```

Or for cross-client compatibility:

```bash
git clone https://github.com/gitsocial-org/gitsocial-agent-skill ~/.agents/skills/gitsocial
```

### Project-level

```bash
git clone https://github.com/gitsocial-org/gitsocial-agent-skill .claude/skills/gitsocial
```

## Documentation

- [SKILL.md](SKILL.md) — full skill documentation (loaded by your agent automatically)
- [references/agentic-workflows.md](references/agentic-workflows.md) — autonomous agent patterns (scheduled tasks, batch operations)
- [references/cli-reference.md](references/cli-reference.md) — complete CLI flag reference
