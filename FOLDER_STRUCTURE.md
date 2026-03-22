# Work-Projects — Proposed Folder Architecture

## Overview

This structure mirrors a GTD-inspired workflow adapted for a maintenance supervisor
who receives requests from multiple channels (Telegram, boss, operators, field staff)
and needs to track projects, daily operations, weekly checklists, and personnel actions.

```
work-projects/
│
├── README.md                          # Repo overview, setup, how to use
├── PROGRESS.md                        # Current sprint / weekly progress log
├── ROADMAP.md                         # Long-term vision and milestones
├── ARCHITECTURE.md                    # System architecture (bot, droplet, repo, flow)
├── CLAUDE.md                          # Instructions for Claude Code context
│
├── inbox/                             # ⬇️ CAPTURE — everything lands here first
│   ├── README.md                      # How inbox processing works
│   └── (timestamped .md files)        # Raw captures from Telegram, messages, etc.
│
├── projects/                          # 📋 GTD PROJECT LIST — mirrors your paper folder
│   ├── README.md                      # How to use projects, review cadence
│   ├── _project-template.md           # Template for new projects
│   └── (one .md file per project)     # e.g., boiler-replacement.md
│
├── next-actions/                      # ⚡ GTD NEXT ACTIONS — one clear next step per project
│   ├── README.md
│   └── next-actions.md                # Rolling list, reviewed each morning
│
├── daily-notes/                       # 📓 DAILY LOG — one file per day
│   ├── README.md
│   ├── _daily-template.md             # Template with sections for maintenance update, etc.
│   └── 2026/
│       └── 03/
│           ├── 2026-03-22.md
│           └── ...
│
├── weekly-checks/                     # ✅ RECURRING WEEKLY CHECKLISTS
│   ├── README.md
│   ├── _checklist-template.md
│   └── work-order-checks.md           # Items to verify on work orders throughout the week
│
├── boss-requests/                     # 🔴 REQUESTS FROM YOUR BOSS — tracked separately
│   ├── README.md
│   └── (one .md per request or rolling log)
│
├── maintenance-requests/              # 🔧 INCOMING MAINTENANCE & PRIORITIZATION
│   ├── README.md
│   ├── _request-template.md
│   ├── active/                        # Currently being worked
│   └── follow-up/                     # Needs follow-up / waiting on parts, people, etc.
│
├── admin-requests/                    # 📨 ADMINISTRATIVE MESSAGES & TASKS
│   ├── README.md
│   └── (ad-hoc admin tasks)
│
├── personnel/                         # 👥 DISCIPLINE, COACHING, DOCUMENTATION
│   ├── README.md                      # How to document, privacy note
│   ├── _coaching-template.md
│   └── (one .md per incident or per person)
│
├── reference/                         # 📚 STATIC REFERENCE — SOPs, contacts, specs
│   ├── README.md
│   └── (reference docs as needed)
│
├── templates/                         # 📝 ALL TEMPLATES IN ONE PLACE (optional mirror)
│   ├── daily-note.md
│   ├── project.md
│   ├── maintenance-request.md
│   ├── coaching-record.md
│   └── weekly-checklist.md
│
└── archive/                           # 🗄️ COMPLETED — moved here, not deleted
    ├── projects/
    ├── boss-requests/
    └── maintenance-requests/
```

## Daily Flow

```
Morning:
  1. Process inbox/ → move items to correct folders
  2. Review projects/ → update next-actions/
  3. Write maintenance update → saved in daily-notes/
  4. Check weekly-checks/ for today's items

Throughout the day:
  Messages → inbox/ (auto-captured via Telegram bot)
  Boss requests → boss-requests/
  Maintenance requests → maintenance-requests/active/
  Admin tasks → admin-requests/
  Personnel issues → personnel/

End of day:
  Update daily-notes/ with what happened
  Move completed items to archive/
```

## Database Consideration

A SQLite database (or JSON-based store) would add value for:
- Tracking request status, priority, and timestamps
- Querying open items across categories
- Generating reports (e.g., "all open boss requests", "overdue follow-ups")
- Logging Telegram message metadata

**Recommendation:** Start with flat markdown files. Add a `workbot.db` SQLite database
when you need to query across folders or automate status tracking. The bot can write to
both — markdown for human readability, database for querying.
