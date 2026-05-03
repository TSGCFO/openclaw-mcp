# Openclaw-Mcp_Docs - Automation

**Pages:** 8

---

## Background tasks

**URL:** https://docs.openclaw.ai/automation/tasks

**Contents:**
- Background tasks
- Documentation Index
- ​TL;DR
- ​Quick start
- ​What creates a task
- ​Task lifecycle
- ​Delivery and notifications
  - ​Notification policies
- ​CLI reference
- ​Chat task board (/tasks)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Notify defaults for cron and media

Concurrent video_generate guardrail

What does not create tasks

tasks flow list | show | cancel

**Examples:**

Example 1 (markdown):
```markdown
# List all tasks (newest first)
openclaw tasks list

# Filter by runtime or status
openclaw tasks list --runtime acp
openclaw tasks list --status running
```

Example 2 (markdown):
```markdown
# Show details for a specific task (by ID, run ID, or session key)
openclaw tasks show <lookup>
```

Example 3 (markdown):
```markdown
# Cancel a running task (kills the child session)
openclaw tasks cancel <lookup>

# Change notification policy for a task
openclaw tasks notify <lookup> state_changes
```

Example 4 (markdown):
```markdown
# Run a health audit
openclaw tasks audit

# Preview or apply maintenance
openclaw tasks maintenance
openclaw tasks maintenance --apply
```

---

## Webhooks

**URL:** https://docs.openclaw.ai/cli/webhooks

**Contents:**
- Webhooks
- Documentation Index
- ​openclaw webhooks
- ​Gmail
  - ​webhooks gmail setup
  - ​webhooks gmail run
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (elixir):
```elixir
openclaw webhooks gmail setup --account you@example.com
openclaw webhooks gmail run
```

Example 2 (elixir):
```elixir
openclaw webhooks gmail setup --account you@example.com
openclaw webhooks gmail setup --account you@example.com --project my-gcp-project --json
openclaw webhooks gmail setup --account you@example.com --hook-url https://gateway.example.com/hooks/gmail
```

Example 3 (elixir):
```elixir
openclaw webhooks gmail run --account you@example.com
```

---

## Hooks

**URL:** https://docs.openclaw.ai/cli/hooks

**Contents:**
- Hooks
- Documentation Index
- ​openclaw hooks
- ​List all hooks
- ​Get hook information
- ​Check hooks eligibility
- ​Enable a Hook
- ​Disable a Hook
- ​Notes
- ​Install hook packs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw hooks list
```

Example 2 (yaml):
```yaml
Hooks (4/4 ready)

Ready:
  🚀 boot-md ✓ - Run BOOT.md on gateway startup
  📎 bootstrap-extra-files ✓ - Inject extra workspace bootstrap files during agent bootstrap
  📝 command-logger ✓ - Log all command events to a centralized audit file
  💾 session-memory ✓ - Save session context to memory when /new or /reset command is issued
```

Example 3 (unknown):
```unknown
openclaw hooks list --verbose
```

Example 4 (unknown):
```unknown
openclaw hooks list --json
```

---

## Hooks

**URL:** https://docs.openclaw.ai/automation/hooks

**Contents:**
- Hooks
- Documentation Index
- ​Quick start
- ​Event types
- ​Writing hooks
  - ​Hook structure
  - ​HOOK.md format
  - ​Handler implementation
  - ​Event context highlights
- ​Hook discovery

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (markdown):
```markdown
# List available hooks
openclaw hooks list

# Enable a hook
openclaw hooks enable session-memory

# Check hook status
openclaw hooks check

# Get detailed information
openclaw hooks info session-memory
```

Example 2 (unknown):
```unknown
my-hook/
├── HOOK.md          # Metadata + documentation
└── handler.ts       # Handler implementation
```

Example 3 (json):
```json
---
name: my-hook
description: "Short description of what this hook does"
metadata:
  { "openclaw": { "emoji": "🔗", "events": ["command:new"], "requires": { "bins": ["node"] } } }
---

# My Hook

Detailed documentation goes here.
```

Example 4 (javascript):
```javascript
const handler = async (event) => {
  if (event.type !== "command" || event.action !== "new") {
    return;
  }

  console.log(`[my-hook] New command triggered`);
  // Your logic here

  // Optionally send message to user
  event.messages.push("Hook executed!");
};

export default handler;
```

---

## Cron

**URL:** https://docs.openclaw.ai/cli/cron

**Contents:**
- Cron
- Documentation Index
- ​openclaw cron
- ​Sessions
- ​Delivery
  - ​Delivery ownership
  - ​Failure delivery
- ​Scheduling
  - ​One-shot jobs
  - ​Recurring jobs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Isolated session semantics

**Examples:**

Example 1 (unknown):
```unknown
openclaw cron edit <job-id> --announce --channel telegram --to "123456789"
```

Example 2 (unknown):
```unknown
openclaw cron edit <job-id> --no-deliver
```

Example 3 (unknown):
```unknown
openclaw cron edit <job-id> --light-context
```

Example 4 (unknown):
```unknown
openclaw cron edit <job-id> --announce --channel slack --to "channel:C1234567890"
```

---

## Task flow

**URL:** https://docs.openclaw.ai/automation/taskflow

**Contents:**
- Task flow
- Documentation Index
- ​When to use Task Flow
- ​Reliable scheduled workflow pattern
- ​Sync modes
  - ​Managed mode
  - ​Mirrored mode
- ​Durable state and revision tracking
- ​Cancel behavior
- ​CLI commands

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw cron add \
  --name "Market intelligence brief" \
  --cron "0 7 * * 1-5" \
  --tz "America/New_York" \
  --session session:market-intel \
  --message "Run the market-intel Lobster workflow. Verify source freshness before summarizing." \
  --announce \
  --channel slack \
  --to "channel:C1234567890"
```

Example 2 (yaml):
```yaml
name: market-intel-brief
steps:
  - id: preflight
    command: market-intel check --json
  - id: collect
    command: market-intel collect --json
    stdin: $preflight.json
  - id: summarize
    command: market-intel summarize --json
    stdin: $collect.json
  - id: approve
    command: market-intel deliver --preview
    stdin: $summarize.json
    approval: required
  - id: deliver
    command: market-intel deliver --execute
    stdin: $summarize.json
    condition: $approve.approved
```

Example 3 (json):
```json
{
  "sourceUrl": "https://example.com/report",
  "retrievedAt": "2026-04-24T12:00:00Z",
  "asOf": "2026-04-24",
  "title": "Example report",
  "content": "..."
}
```

Example 4 (yaml):
```yaml
Flow: weekly-report
  Step 1: gather-data     → task created → succeeded
  Step 2: generate-report → task created → succeeded
  Step 3: deliver         → task created → running
```

---

## Automation & tasks

**URL:** https://docs.openclaw.ai/automation

**Contents:**
- Automation & tasks
- Documentation Index
- ​Quick decision guide
  - ​Scheduled Tasks (Cron) vs Heartbeat
- ​Core concepts
  - ​Scheduled tasks (cron)
  - ​Tasks
  - ​Inferred commitments
  - ​Task Flow
  - ​Standing orders

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Standing orders

**URL:** https://docs.openclaw.ai/automation/standing-orders

**Contents:**
- Standing orders
- Documentation Index
- ​Why standing orders
- ​How they work
- ​Anatomy of a standing order
- ​Standing orders plus cron jobs
- ​Examples
  - ​Example 1: content and social media (weekly cycle)
  - ​Example 2: finance operations (event-triggered)
  - ​Example 3: monitoring and alerts (continuous)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (markdown):
```markdown
## Program: Weekly Status Report

**Authority:** Compile data, generate report, deliver to stakeholders
**Trigger:** Every Friday at 4 PM (enforced via cron job)
**Approval gate:** None for standard reports. Flag anomalies for human review.
**Escalation:** If data source is unavailable or metrics look unusual (>2σ from norm)

### Execution steps

1. Pull metrics from configured sources
2. Compare to prior week and targets
3. Generate report in Reports/weekly/YYYY-MM-DD.md
4. Deliver summary via configured channel
5. Log completion to Agent/Logs/

### What NOT to do

- Do not send reports to external parties
- Do not modify source data
- Do not skip delivery if metrics look bad — report accurately
```

Example 2 (yaml):
```yaml
Standing Order: "You own the daily inbox triage"
    ↓
Cron Job (8 AM daily): "Execute inbox triage per standing orders"
    ↓
Agent: Reads standing orders → executes steps → reports results
```

Example 3 (sass):
```sass
openclaw cron add \
  --name daily-inbox-triage \
  --cron "0 8 * * 1-5" \
  --tz America/New_York \
  --timeout-seconds 300 \
  --announce \
  --channel bluebubbles \
  --to "+1XXXXXXXXXX" \
  --message "Execute daily inbox triage per standing orders. Check mail for new alerts. Parse, categorize, and persist each item. Report summary to owner. Escalate unknowns."
```

Example 4 (markdown):
```markdown
## Program: Content & Social Media

**Authority:** Draft content, schedule posts, compile engagement reports
**Approval gate:** All posts require owner review for first 30 days, then standing approval
**Trigger:** Weekly cycle (Monday review → mid-week drafts → Friday brief)

### Weekly cycle

- **Monday:** Review platform metrics and audience engagement
- **Tuesday–Thursday:** Draft social posts, create blog content
- **Friday:** Compile weekly marketing brief → deliver to owner

### Content rules

- Voice must match the brand (see SOUL.md or brand voice guide)
- Never identify as AI in public-facing content
- Include metrics when available
- Focus on value to audience, not self-promotion
```

---
