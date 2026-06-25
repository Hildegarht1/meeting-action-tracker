# ActionFlow Meeting Tracker

ActionFlow is a hosted automation demo that converts messy meeting notes into accountable follow-up work.

It extracts:

- action items
- owners
- deadlines
- decisions
- risks and blockers
- structured JSON for automation tools
- manager-ready follow-up summaries
- CSV exports for tasks

## Why It Is Useful

Teams often leave meetings with vague notes and no clear ownership. ActionFlow turns free-form transcripts into an operational follow-up board that can feed task tools, reminders, and reporting.

## What Recruiters Can See

The hosted page includes:

- transcript input
- sample meeting notes
- extracted action board
- decisions and risks
- request history
- structured JSON
- CSV task export
- visible n8n-style automation workflow

## Automation Architecture

```text
React/static dashboard
-> n8n webhook
-> Code node extracts structured meeting data
-> AI node writes follow-up summary
-> Google Sheets stores meeting history
-> task tool creates follow-up tasks
-> reminder loop tracks deadlines
```

## Current Status

The hosted dashboard works as a self-contained demo so recruiters can click and use it immediately.

The n8n workflow design is documented in:

```text
workflows/n8n-workflow-design.md
```

## Portfolio CV Bullet

Built ActionFlow, a hosted meeting automation dashboard that extracts action items, owners, deadlines, decisions, and risks from free-form meeting notes. Designed an n8n-ready workflow architecture for webhook intake, structured JSON extraction, AI follow-up summaries, Google Sheets history logging, task creation, and reminder automation.

