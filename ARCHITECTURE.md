# Architecture

## System Overview

```
┌─────────────┐     ┌──────────────────┐     ┌────────────────┐
│  Telegram    │────▶│  ccbot           │────▶│  GitHub Repo   │
│  (Mobile)    │◀────│  (DigitalOcean   │     │  (work-projects│
│              │     │   Droplet)       │     │   /main)       │
└─────────────┘     └──────┬───────────┘     └────────────────┘
                           │                         ▲
                           │                         │
                    ┌──────▼───────────┐             │
                    │  Claude Code     │─────────────┘
                    │  (on Droplet)    │  pushes commits
                    └──────────────────┘
```

## Components

### Telegram Bot (ccbot)

- **Role:** Primary input interface. Receives messages, commands, and requests.
- **Host:** DigitalOcean Droplet
- **Known issue:** Occasional timeouts under load or long-running operations
- **Future:** Auto-categorize messages and route to correct repo folders

### DigitalOcean Droplet

- **Role:** Server hosting the bot and Claude Code
- **Services running:** ccbot process, Claude Code agent
- **Repo sync:** Claude Code pushes work comments and updates to GitHub

### Claude Code

- **Role:** AI assistant running inside the architecture
- **Current function:** Updates the GitHub repo with work comments
- **Future function:** Process inbox, generate daily notes, assist with weekly checks,
  suggest next actions

### GitHub Repository (work-projects)

- **Role:** Single source of truth for all work information
- **Branch:** main
- **Structure:** See CLAUDE.md or the repo directory layout
- **Access:** Private repo, updated by Claude Code and manual edits

## Information Flow

```
INPUT SOURCES                    PROCESSING                    STORAGE
─────────────                    ──────────                    ───────
Telegram messages ──┐
                    │
Boss requests ──────┤
                    ├──▶ inbox/ ──▶ Morning Review ──▶ Correct folder
Operator updates ───┤         (process & route)      ├─ projects/
                    │                                 ├─ boss-requests/
Maintenance calls ──┤                                 ├─ maintenance-requests/
                    │                                 ├─ admin-requests/
Personnel issues ───┤                                 ├─ personnel/
                    │                                 ├─ meetings/
Formal meetings ────┘                                 └─ daily-notes/
```

## Request Types & Routing

| Request Type         | Source              | Routed To               | Priority  |
|---------------------|---------------------|-------------------------|-----------|
| Boss directive       | Boss (any channel)  | boss-requests/          | High      |
| Maintenance request  | Operators, field    | maintenance-requests/   | Varies    |
| Admin/message update | Various             | admin-requests/         | Normal    |
| Personnel/coaching   | Complaints, observe | personnel/              | Sensitive |
| Project work         | Self-initiated      | projects/               | Planned   |
| Daily maintenance    | Morning routine     | daily-notes/            | Daily     |
| Formal meeting notes | On-site, calls      | meetings/               | Reference |

## Database (Future)

When added, a SQLite database (`workbot.db`) will sit alongside the markdown files:

```
workbot.db
├── requests      (id, type, source, priority, status, created, updated, folder_path)
├── daily_logs    (id, date, maintenance_update, notes)
├── projects      (id, name, status, next_action, last_reviewed)
└── personnel     (id, person, type, date, summary, follow_up_date)
```

The bot writes to both markdown (human-readable) and the database (queryable).
This dual-write approach means you never lose the readability of plain files
but gain the ability to ask "show me everything open from the boss" programmatically.

## Security & Privacy Notes

- Personnel files contain sensitive information — keep repo private
- Coaching/discipline records should follow your organization's documentation standards
- Consider whether personnel records belong in this repo or a separate, more restricted
  location
