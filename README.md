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

## Features

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

The dashboard works as a self-contained demo. The workflow design documents how the same logic can connect to n8n, Google Sheets, task tools, and reminder automation.

The n8n workflow design is documented in:

```text
workflows/n8n-workflow-design.md
```
