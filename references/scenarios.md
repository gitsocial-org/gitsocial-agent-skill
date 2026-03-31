# What You Can Ask GitSocial

Examples of what you can ask an AI agent with the GitSocial skill installed. Just describe what you need in natural language.

---

## Daily Developer Workflow

### Morning catchup
"What happened overnight?" / "Catch me up"
- Unread notifications grouped by type (PRs needing review, mentions, issue assignments)
- New posts on timeline since last check
- PRs that changed state (merged, closed, new feedback)

### My work
"What am I working on?" / "What's on my plate?"
- Open issues assigned to me, grouped by priority/milestone
- My open PRs and their review status (approved, changes requested, waiting)
- My draft PRs

### What's blocking me
"Am I blocked on anything?"
- My issues that are blocked by other issues
- My PRs waiting for review (no feedback yet)
- My PRs with changes requested

### Quick status check
"Status of issue X" / "What's happening with PR Y"
- Full detail of a specific item with comments thread

---

## PR Review Cycle

### PRs needing my review
"What PRs need my review?"
- Open PRs where I'm listed as reviewer, excluding ones I already approved
- Sorted by age (oldest first)

### PR report
"PR report for last week" / "PR activity this sprint"
- Breakdown by state: merged, closed, open
- By author: who merged how many
- By topic: group by labels or subject keywords
- Include counts and one-line summaries

### PR health
"Any stale PRs?" / "PRs that need attention"
- Open PRs older than N days
- PRs with no reviews
- PRs with unresolved change requests
- Draft PRs that have been open too long

### Review turnaround
"How fast are we reviewing PRs?"
- PRs merged last week: time from open to merge
- PRs still waiting for first review

---

## Issue Management

### Issue dashboard
"Show me open issues" / "Issue breakdown"
- Open issues grouped by: labels, milestone, assignee
- Counts per group
- Unassigned issues highlighted

### Issue triage
"What needs triage?" / "Unassigned issues"
- Issues with no assignee
- Issues with no labels
- Issues with no milestone
- Recently created issues

### Blockers
"What's blocked?" / "Show me dependency chains"
- Issues with blocks/blocked-by relationships
- Highlight circular or deep chains

### Sprint backlog
"What's in the current sprint?" / "Sprint status"
- Issues in active sprint, grouped by state (open/closed)
- Progress percentage
- Who's overloaded (assignee distribution)

---

## Sprint & Milestone Cadences

### Sprint planning
"Prepare for sprint planning" / "What should go in next sprint?"
- Backlog issues not in any sprint, sorted by priority
- Carryover from last sprint (open issues from completed sprint)
- Capacity hints: issues per assignee in last sprint

### Standup prep
"Daily standup summary"
- Per team member: what they closed yesterday, what's in progress, any blockers
- Derived from issue state changes and PR activity in last 24h

### Sprint retrospective
"Sprint retro data"
- Completed sprint metrics: issues planned vs completed
- PR throughput during sprint
- Items carried over
- Velocity trend (compare to previous sprints)

### Milestone check
"How's milestone v2.0 going?"
- Issues by state (open/closed)
- Progress percentage
- Due date and days remaining
- Risk items (overdue, blocked)

---

## Release Management

### Release readiness
"Are we ready to release?" / "Release checklist"
- Open issues in release milestone
- Open PRs targeting release branch
- PRs merged since last release (changelog material)
- Any blocking issues

### Changelog generation
"What went into this release?" / "Generate changelog"
- PRs merged between two tags/dates
- Grouped by label/topic (features, fixes, chores)
- Author attribution

### Release history
"Show me recent releases" / "Compare releases"
- List releases with version, date, artifact count
- Diff between two releases (what changed)

### SBOM / compliance
"What's in the SBOM for v2.0?" / "License audit"
- Packages and their licenses
- Flagged licenses (if any non-OSS)

---

## Cross-Team Visibility

### Team activity
"What did the platform team ship this week?"
- Activity scoped to a list of repos (team's repos)
- PRs merged, issues closed, releases published
- By author within that scope

### Cross-repo search
"Find all issues mentioning authentication" / "Who's working on payments?"
- Search across all followed repos
- Group results by repo and type

### Contributor report
"Who are the top contributors this month?"
- Activity by author across all extensions
- PRs merged, issues closed, posts, reviews given

### Dependency awareness
"What repos are we following?" / "Show me our lists"
- Lists with repo counts
- Last fetch time per repo
- New followers detected

---

## Social & Communication

### Post update
"Post about what we shipped" / "Share this release"
- Create a post or quote a release
- Draft the content for user approval

### Discussion summary
"Summarize the discussion on post X"
- Post with full comment thread
- Key points extracted

### Who follows us
"Who's watching our repo?"
- Followers list with their repos
- New followers since last check

---

## Onboarding & Setup

### First-time setup
"Set up GitSocial for this repo"
- Check what's initialized, init what's missing
- Suggest lists to follow based on repo topics

### Import from GitHub
"Import everything from our GitHub repo"
- Dry-run preview (item counts)
- Execute import with progress
- Post-import fetch to populate cache

### Add repos to track
"Follow these repos" / "Set up a team list"
- Create list, add repos, fetch initial data

---

## Notifications & Triage

### Notification inbox
"What's new?" / "Show notifications"
- Unread notifications grouped by type and repo
- Mark batches as read

### Mention tracking
"Where was I mentioned?"
- Posts/issues/PRs mentioning my email
- Across all followed repos

---

## Recurring Reports

### Daily digest
"Morning summary"
- Unread notification count
- PRs merged/closed yesterday
- New issues assigned to me
- Upcoming due dates (next 3 days)

### Weekly report
"End-of-week summary"
- PR throughput (opened, merged, closed)
- Issue velocity (opened vs closed)
- Release activity
- Top contributors
- By author and by repo

### Sprint report
"Sprint wrap-up"
- Planned vs completed issues
- Velocity (story points or issue count)
- Carryover items
- PR review metrics

### Monthly overview
"Monthly summary"
- Milestone progress across all active milestones
- Release cadence (how many releases, regularity)
- Community growth (new followers, new forks)
- Cross-repo trends
