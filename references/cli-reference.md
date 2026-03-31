# GitSocial CLI Reference

Complete command reference with all flags. Load this file when you need exact flag names or syntax.

## Global Flags

```
--json              Output in JSON format
-C, --workdir       Working directory (default: current directory)
--cache-dir         Cache directory (default: ~/.cache/gitsocial)
```

---

## Core Commands

### status
```
gitsocial status
```
Show GitSocial status for current repository.

### fetch
```
gitsocial fetch [url]
  -l, --list        Fetch only repos from this list
  --since           Fetch since date (YYYY-MM-DD, default: 30 days ago)
  --before          Fetch before date (YYYY-MM-DD)
  -p, --parallel    Concurrent fetches (default: 4)
```

### push
```
gitsocial push
  --dry-run         Preview without pushing
```

### config
```
gitsocial config get <key>
gitsocial config set <key> <value>
gitsocial config list
```

### settings
```
gitsocial settings get <key>
gitsocial settings set <key> <value>
gitsocial settings list
```

### log
```
gitsocial log
  -n, --limit       Max entries (default: 20)
  -s, --scope       Scope: timeline, repository:my (default)
  -t, --type        Filter by types (comma-separated)
  --after           After date (YYYY-MM-DD)
  --before          Before date (YYYY-MM-DD)
  -a, --author      Filter by author email
```

### search
```
gitsocial search [query]
  -n, --limit       Max results (default: 20)
  -a, --author      Filter by author email
  -r, --repo        Filter by repository URL
  -t, --type        Filter by type (post|comment|repost|quote|pr|issue|milestone|sprint|release|feedback)
  --hash            Filter by commit hash prefix
  --after           After date (YYYY-MM-DD)
  --before          Before date (YYYY-MM-DD)
  -s, --scope       Scope: timeline (default), list:<name>, repository:<url>
  --sort            Sort by: score (default) or date
  --state           Filter by state (open, closed, merged, canceled)
  --labels          Filter by labels (comma-separated, any match)
  --assignee        Filter by assignee email (implies --type issue)
  --reviewer        Filter by reviewer email (implies --type pr)
  --milestone       Filter by milestone name (implies --type issue)
  --sprint          Filter by sprint name (implies --type issue)
  --draft           Filter draft PRs only (implies --type pr)
  --prerelease      Filter pre-releases only (implies --type release)
  --tag             Filter by release tag (implies --type release)
  --base            Filter by PR base branch (implies --type pr)
  --group-by        Group results by field (state, author, type, extension, repo, label, assignee, reviewer, milestone, base)
  --top             Max items per group (default: unlimited)
  --count-only      Show only group counts, no items
```

### notifications
```
gitsocial notifications
  -a, --all         Show all (not just unread)
  -n, --limit       Max notifications (default: 20)
  -t, --type        Filter by type (comma-separated)

gitsocial notifications count
gitsocial notifications read <id>
gitsocial notifications read-all
gitsocial notifications unread <id>
gitsocial notifications unread-all
```

### explore
```
gitsocial explore
  -l, --list        Filter by list name
  --limit           Max repositories (default: 100)
```

### related
```
gitsocial related <repository>
  -l, --limit       Limit results
```

### show
```
gitsocial show <ref>
```
Show full item details. Auto-detects extension (issue, PR, release, or post).

### history
```
gitsocial history <ref>
```
View edit history of a message.

### fork
```
gitsocial fork add <url>
gitsocial fork remove <url>
gitsocial fork list
```

### tui
```
gitsocial tui
```
Launch interactive TUI. Note: not suitable for agent use.

---

## Social Extension

### social status / init / config
```
gitsocial social status
gitsocial social init
  -b, --branch      Branch for social content (default: auto-detect)
gitsocial social config get|set|list
```

### social timeline
```
gitsocial social timeline
  -r, --repo        Filter by repo URL (use 'workspace' for current)
  -l, --list        Filter by list name
  -n, --limit       Max posts (default: 20)
```

### social post / edit / retract
```
gitsocial social post <text>
gitsocial social edit <post-id> <new-text>
gitsocial social retract <post-id>
```

### social comment / repost / quote
```
gitsocial social comment <post-id> <text>
gitsocial social repost <post-id>
gitsocial social quote <post-id> <text>
```

### social list
```
gitsocial social list show [name]
gitsocial social list ls
gitsocial social list create <id>
  -n, --name        Display name
gitsocial social list delete <id>
gitsocial social list add <list-id> <repo-url>
  -b, --branch      Branch to track (default: auto-detect)
  --all-branches    Follow all branches
gitsocial social list remove <list-id> <repo-url>
gitsocial social list repo <url>
```

### social fetch
```
gitsocial social fetch [url]
  -l, --list        Fetch only repos from this list
  --since           Fetch since date (YYYY-MM-DD, default: 30 days ago)
  -p, --parallel    Concurrent fetches (default: 4)
```

### social followers
```
gitsocial social followers
```

---

## PM Extension

### pm status / init / config
```
gitsocial pm status
gitsocial pm init
  -b, --branch      Branch (default: gitmsg/pm)
  -f, --framework   Framework: minimal, kanban (default), scrum
gitsocial pm config get|set|list
```

### pm issue
```
gitsocial pm issue list
  -s, --state       Filter: open, closed, all
  -l, --limit       Max results (default: 20)
  --labels          Comma-separated labels
  --filter          Advanced filter syntax
  --sort            Sort by: created, updated, priority
  -r, --repo        Repository URL
  -b, --branch      Branch name

gitsocial pm issue show <id>
gitsocial pm issue create <title>
  --body            Description
  --labels          Comma-separated labels
  --assignees       Comma-separated assignee emails

gitsocial pm issue close <id>
gitsocial pm issue reopen <id>
gitsocial pm issue comment <id> <text>
gitsocial pm issue comments <id>
```

### pm milestone
```
gitsocial pm milestone list
  --state           Filter: open, closed, all
  -l, --limit       Max results (default: 20)

gitsocial pm milestone show <id>
gitsocial pm milestone create <title>
  --due-date        Due date (YYYY-MM-DD)

gitsocial pm milestone close <id>
gitsocial pm milestone reopen <id>
gitsocial pm milestone cancel <id>
gitsocial pm milestone delete <id>
```

### pm sprint
```
gitsocial pm sprint list
gitsocial pm sprint show <id>
gitsocial pm sprint create <name>
gitsocial pm sprint start <id>
gitsocial pm sprint complete <id>
gitsocial pm sprint cancel <id>
gitsocial pm sprint delete <id>
```

### pm board
```
gitsocial pm board
```

---

## Review Extension

### review status / init / config
```
gitsocial review status
gitsocial review init
  -b, --branch      Branch (default: gitmsg/review)
gitsocial review config get|set|list
```

### review pr
```
gitsocial review pr create <subject>
  --base            Target branch ref (e.g., #branch:main)
  --head            Source branch ref (e.g., #branch:feature)
  --closes          PM issue refs to close on merge (comma-separated)
  --reviewers       Reviewer emails (comma-separated)
  --draft           Create as draft

gitsocial review pr list
  -s, --state       Filter: open (default), merged, closed
  -n, --limit       Max results (default: 50)
  -r, --repo        Repository URL
  -b, --branch      Branch name

gitsocial review pr show <ref>
  --versions        Show version history

gitsocial review pr update <ref>
gitsocial review pr merge <ref>
  --strategy        Merge strategy: ff (default), squash, rebase, merge

gitsocial review pr close <ref>
gitsocial review pr retract <ref>

gitsocial review pr diff <ref>
  --from            Source version number
  --to              Target version number

gitsocial review pr sync <ref>
  --strategy        Sync strategy: rebase (default), merge

gitsocial review pr ready <ref>
gitsocial review pr draft <ref>
```

### review feedback
```
gitsocial review feedback approve <pr-ref>
  -m, --message     Message (default: "LGTM!")

gitsocial review feedback request-changes <pr-ref>
  -m, --message     Message (required)

gitsocial review feedback comment <message>
  --pr              PR ref (required)
  --file            File path (required for inline)
  --commit          Commit hash, 12 chars (required for inline)
  --old-line        Line in old file, 1-indexed
  --new-line        Line in new file, 1-indexed
  --old-line-end    End line in old file
  --new-line-end    End line in new file
  --suggest         Mark as code suggestion
```

### review fork
```
gitsocial review fork add <url>
gitsocial review fork remove <url>
gitsocial review fork list
```

---

## Release Extension

### release status / init / config
```
gitsocial release status
gitsocial release init
  -b, --branch      Branch (default: gitmsg/release)
gitsocial release config get|set|list
```

### release create / edit / retract
```
gitsocial release create <subject>
  -t, --tag         Git tag (e.g., v1.0.0)
  -v, --version     Semver version (e.g., 1.0.0)
  --prerelease      Mark as pre-release
  --artifacts       Artifact filenames (comma-separated)
  --artifact-url    Base URL for hosted artifacts
  --checksums       Checksums filename (e.g., SHA256SUMS)
  --signed-by       Key fingerprint
  --sbom            SBOM filename (e.g., sbom.spdx.json)

gitsocial release edit <ref>
  --body            Updated body
  -t, --tag         Updated tag
  -v, --version     Updated version
  --sbom            Updated SBOM filename
  --artifacts       Artifact filenames (comma-separated)
  --artifact-url    Base URL for hosted artifacts
  --checksums       Checksums filename
  --signed-by       Key fingerprint

gitsocial release retract <ref>
```

### release list / show
```
gitsocial release list
  -n, --limit       Max releases (default: 20)
  -r, --repo        Repository URL
  -b, --branch      Branch name

gitsocial release show <ref>
```

### release artifacts
```
gitsocial release artifacts add <version> <file...>
gitsocial release artifacts list <version>
gitsocial release artifacts export <version> [filename...]
```

### release sbom
```
gitsocial release sbom <ref>
  --raw             Dump raw SBOM JSON
```

---

## Import

```
gitsocial import all <url>
gitsocial import pm <url>
gitsocial import release <url>
gitsocial import review <url>
gitsocial import social <url>
```

### Import Flags (all subcommands)
```
  -n, --limit       Max items per type (0 = unlimited)
  --since           Only items after date (YYYY-MM-DD)
  --dry-run         Preview without importing
  --update          Sync changes for already-imported items
  -y, --yes         Skip confirmation
  --map-file        Path to ID mapping file
  --labels          Label mapping: auto (default), raw, skip
  --skip-bots       Skip bot-authored items (default: true)
  --host            Force host: github, gitlab, gitea, bitbucket
  --api-url         Custom API base URL
  --token           API token
  -v, --verbose     Print each item
  --state           Filter: open, closed, merged, all (default)
  --email-map       Path to username=email mapping file
  --categories      Discussion category slugs (default: announcements,feature-requests,q-a)
```
