# Agentic Workflows

Patterns for AI agents using GitSocial autonomously — without a human in the loop for every action. Read this when building automated workflows, scheduled tasks, or agent-to-agent integrations.

---

## Safety Guardrails

### Safe to do autonomously (no confirmation needed)
- Search and read operations (all `search`, `show`, `list`, `status`)
- Add comments on issues and PRs
- Post social updates
- Fetch updates
- Create draft PRs
- Add labels to issues

### Requires confirmation before acting
- Merge PRs
- Close or retract issues/PRs/posts
- Create non-draft PRs
- Push changes
- Import from external platforms
- Delete lists or remove repos from lists

### Never do autonomously
- Retract other people's posts
- Force-merge without approvals
- Mass-close issues without human review

When in doubt, present what you intend to do and wait for confirmation.

---

## Core Patterns

### Search → Act loop
Most agentic workflows follow this pattern: search for items matching criteria, iterate over results, take action on each.

```
1. Search with filters: gitsocial search --type issue --state open --json
2. Parse results (read the JSON yourself)
3. For each item: decide action based on rules
4. Execute action: gitsocial pm issue comment <ref> "message"
```

Important: search returns items with `Hash` and `Extension` fields. Use these to construct refs for write commands: `#commit:<hash>`.

### Detect new items (polling)
GitSocial has no webhooks. To detect new items, poll with `--after` using the last check timestamp.

```
1. Store last check time (e.g., 2025-03-30)
2. Search: gitsocial search --type issue --after 2025-03-30 --json
3. Process new items
4. Update last check time to now
```

For agents running on a schedule, use yesterday's date:
```bash
gitsocial search --type pr --after 2025-03-30 --json
```

### Batch operations
To act on multiple items from a search result:

```
1. Search: gitsocial search --type pr --state open --reviewer me@email.com --json
2. For each result:
   a. Get detail: gitsocial show <hash> --json
   b. Decide: does this need a reminder?
   c. Act: gitsocial social comment <ref> "Friendly reminder: this PR is waiting for review"
```

Never run write operations in a tight loop without rate consideration. Space actions with brief pauses.

---

## Workflow Templates

### Coding agent: publish work
After implementing a feature or fix:

```
1. gitsocial review pr create "Subject" --base "#branch:main" --head "#branch:<current>" --draft
2. If linked issue exists: add --closes "<issue-ref>"
3. If reviewers known: add --reviewers "email1,email2"
4. When ready for review: gitsocial review pr ready <ref>
5. Optionally post an update: gitsocial social post "Created PR for <feature>"
```

### Review agent: automated code review
Periodically check for PRs needing review:

```
1. gitsocial search --type pr --state open --reviewer <agent-email> --json
2. For each PR:
   a. gitsocial show <hash> --json
   b. Analyze the PR (read diff, check for issues)
   c. gitsocial review feedback approve <ref> -m "LGTM"
      OR
      gitsocial review feedback request-changes <ref> -m "Found issues: ..."
   d. For inline comments:
      gitsocial review feedback comment "message" --pr <ref> --file <path> --commit <hash12> --new-line <N>
```

### Triage agent: auto-label and assign
Watch for new issues and apply rules:

```
1. gitsocial search --type issue --state open --after <last-check> --json
2. For each new issue:
   a. Read content, determine category
   b. If bug-related: gitsocial pm issue comment <ref> "Triaged as bug"
   c. If assignee can be determined from rules: note for human review
   d. Update last-check timestamp
```

Note: GitSocial doesn't have a direct "add label" or "assign" command post-creation. Labels and assignees are set at creation time or via edits. For triage, commenting with classification is the primary action.

### Standup agent: daily summary
Run daily to generate team standup data:

```
1. gitsocial search --type issue --state closed --after <yesterday> --group-by author --limit 200 --json
   → "What was completed"
2. gitsocial search --type issue --state open --assignee <team-member> --limit 200 --json
   → "What's in progress" (per person)
3. gitsocial search --type pr --state open --group-by author --limit 200 --json
   → "PRs in flight"
4. Combine into a summary, optionally post:
   gitsocial social post "Daily standup - March 31: ..."
```

### Release agent: automated release flow
Check readiness and create release:

```
1. Check blockers:
   gitsocial search --type issue --milestone "v2.0" --state open --count-only --limit 200 --json
   → If count > 0, report blockers and stop
2. Gather changelog:
   gitsocial search --type pr --state merged --after <last-release-date> --group-by label --limit 200 --json
3. Create release:
   gitsocial release create "v2.0 — Description" --tag v2.0 --version 2.0.0
4. Post announcement:
   gitsocial social post "Released v2.0! Highlights: ..."
```

### Notification agent: act on mentions
Poll for mentions and respond:

```
1. gitsocial notifications --type mention --json
2. For each unread mention:
   a. Read the item that mentions you
   b. Determine if action is needed
   c. Take action (comment, assign, etc.)
   d. Mark as read: gitsocial notifications read <id>
```

---

## Agent-to-Agent Communication

Agents can communicate through GitSocial's social layer:

- **Status updates**: Agent posts what it did (`gitsocial social post`)
- **Handoffs**: Agent A creates a draft PR, Agent B reviews it
- **Coordination**: Agents use issue comments to coordinate ("I'm working on this", "Done, ready for review")
- **Mentions**: Use @email in posts/comments to notify other agents or humans

---

## Scheduling Patterns

GitSocial has no built-in scheduler. Agentic workflows run via external triggers:

| Trigger | Mechanism |
|---------|-----------|
| Cron / scheduled task | External scheduler runs the agent on a cadence |
| Git hook (post-push) | Agent runs after code is pushed |
| CI/CD pipeline step | Agent runs as part of build/deploy |
| Manual invocation | Human triggers the agent ("run standup") |
| Polling loop | Agent runs continuously, polling with `--after` |

For Claude Code specifically, use the `/schedule` skill to set up recurring agent triggers.

---

## Error Handling in Autonomous Workflows

- If a command fails, log the error and continue to the next item (don't halt the entire workflow)
- If `gitsocial status` fails, the CLI isn't installed or we're not in a git repo — halt and report
- If an extension isn't initialized, run `gitsocial <ext> init` and retry once
- If search returns no results, that's not an error — it means nothing matches the criteria
- If a write operation fails (e.g., "already closed"), skip and continue

Never retry failed write operations in a loop — investigate the cause first.
