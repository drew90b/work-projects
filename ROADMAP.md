# Roadmap

## Vision

A personal work management system where information flows from capture (Telegram, verbal,
paper) through processing and into organized, reviewable folders — powered by Claude and
accessible from anywhere.

---

## Phase 1: Foundation (Current)

**Goal:** Get the folder structure in place and start using it daily.

- Implement folder architecture in the repo
- Create templates for each document type
- Establish `meetings/` folder for formal meeting notes
- Establish morning routine: process inbox → review projects → write maintenance update
- Document the system in ARCHITECTURE.md and CLAUDE.md
- Fix Telegram bot timeout issues

## Phase 2: Automation

**Goal:** Reduce manual filing. Let the bot help route information.

- Bot auto-categorizes incoming messages (boss request vs. maintenance vs. admin)
- Bot creates daily-note file each morning from template
- Bot prompts for maintenance update and saves to daily-notes
- Bot routes formal meeting notes to `meetings/` automatically
- Claude assists with weekly work order checks
- Explore auto-tagging messages with priority levels

## Phase 3: Database & Querying

**Goal:** Add structured data layer for tracking and reporting.

- Add SQLite database for request tracking (status, priority, timestamps, assignee)
- Enable queries: "What's open from the boss?", "Overdue follow-ups?"
- Dual-write: markdown files for readability + database for querying
- Weekly summary generation from database

## Phase 4: Intelligence

**Goal:** Claude becomes a proactive assistant, not just a scribe.

- Morning briefing generated automatically (open items, due dates, priorities)
- Claude suggests next actions based on project state
- Pattern detection: recurring issues, common request types
- Personnel tracking with coaching history and follow-up reminders

## Phase 5: Team & Reporting

**Goal:** Share relevant information outward.

- Auto-generate maintenance updates for operators
- Boss-facing status reports on demand
- Work order completion tracking and reporting
- Dashboard view of system health (if desired)

---

## Non-Goals (For Now)

- Building a web UI — the repo and Telegram are the interfaces
- Multi-user access — this is a personal system
- Real-time notifications beyond Telegram
