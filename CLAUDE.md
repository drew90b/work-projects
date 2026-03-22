# CLAUDE.md — Context for Claude Code

## What This Repo Is

This is a personal work management system for a maintenance supervisor. It tracks
projects, daily operations, incoming requests, and personnel actions using a
GTD-inspired folder structure. The repo is the single source of truth.

## System Setup

- **Bot:** ccbot running on a DigitalOcean droplet, accessible via Telegram
- **AI:** Claude Code runs on the same droplet, pushes commits to this repo
- **Repo:** GitHub (private), branch: main

## Folder Structure Rules

- **inbox/** — All new captures land here first. Process daily.
- **projects/** — One markdown file per active project. Mirrors the paper project list.
- **next-actions/** — Rolling list of next physical/visible actions, one per project.
  Also contains `waiting-for.md` for delegated/pending items.
- **daily-notes/** — One file per day in `YYYY/MM/YYYY-MM-DD.md` format. Includes the
  morning maintenance update sent to operators.
- **weekly-checks/** — Recurring checklists for work order verification.
- **boss-requests/** — Anything from the boss gets its own tracking. High priority.
- **maintenance-requests/** — Split into `active/` and `follow-up/` subfolders.
- **admin-requests/** — Administrative messages and tasks.
- **personnel/** — Discipline and coaching documentation. Sensitive — handle carefully.
- **meetings/** — Formal meeting notes distinct from daily notes (site visits, outage
  updates, stakeholder meetings). One file per meeting.
- **reference/** — Static reference material (SOPs, contacts, specs, someday/maybe list).
- **templates/** — Reusable templates for all document types.
- **archive/** — Completed items move here. Never delete, always archive.

## How to Process Incoming Messages

1. If it's from the boss → `boss-requests/`
2. If it's a maintenance issue → `maintenance-requests/active/`
3. If it's administrative → `admin-requests/`
4. If it's a personnel complaint or coaching need → `personnel/`
5. If it's a formal meeting record → `meetings/`
6. If unclear → `inbox/` for manual review

## Daily Note Format

File path: `daily-notes/YYYY/MM/YYYY-MM-DD.md`

Each daily note must include these sections:
- **Date header** (`# Daily Note — YYYY-MM-DD`)
- **Maintenance Update** — the message sent to operators each morning, with timestamp
- **Inbox Processed** — table of items routed during morning review (Item | Routed To | Action)
- **Projects Reviewed** — checkbox confirming project list and next actions were reviewed
- **Work Done** — tasks completed during the day
- **Boss Requests Status** — current state of any open boss items
- **Notes** — anything else noteworthy

## Commit Message Convention

Use prefixes:
- `daily:` — Daily note entries and maintenance updates
- `inbox:` — New captures from Telegram or other sources
- `project:` — Project file updates
- `boss:` — Boss request tracking
- `maint:` — Maintenance request updates
- `personnel:` — Coaching/discipline documentation
- `admin:` — Administrative request tracking
- `docs:` — Documentation changes (README, PROGRESS, ROADMAP, etc.)
- `fix:` — Bug fixes to the bot or system
- `feat:` — New features or automation

## What Claude Should Do

- When receiving a message via Telegram, save it to `inbox/` with a timestamp
- During morning processing, help categorize and route inbox items
- Help draft the daily maintenance update
- Remind about weekly checklist items on the appropriate days
- When asked about open items, check across `boss-requests/`, `maintenance-requests/active/`, and `next-actions/`
- Never delete files — move completed items to `archive/`
- Keep personnel documentation factual, dated, and professional

## What Claude Should NOT Do

- Do not share personnel file contents via Telegram
- Do not make up information about request status — check the files
- Do not modify archived files
- Do not push commits without meaningful changes
- Do not delete files — always move to `archive/` instead

## Notes on Legacy Files

- `FOLDER_STRUCTURE.md` — planning document from before the folder migration. The
  live folder structure in this repo is now the authoritative reference.
