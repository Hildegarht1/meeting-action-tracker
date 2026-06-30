# ActionFlow Meeting Tracker

ActionFlow is a hosted automation tool that converts messy meeting notes into accountable follow-up work.

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
- meeting title field
- browser microphone transcription
- sample meeting notes
- extracted action board
- decisions and risks
- request history
- structured JSON
- CSV task export
- optional Make webhook integration
- Make AI summary route
- Google Sheets meeting log
- visible automation workflow

## Automation Architecture

```text
React/static dashboard
-> Make custom webhook
-> Make AI agent writes follow-up summary
-> Dashboard verifies actions, owners, deadlines, decisions, and risks
-> Google Sheets stores meeting history
-> webhook response updates the dashboard
```

## Current Status

The dashboard works as a self-contained app and can also send meeting notes to a Make custom webhook when a scenario URL is configured. The connected workflow can log processed meetings to Google Sheets before returning the result to the dashboard.

The Make scenario design is documented in:

```text
workflows/make-scenario-design.md
```

## Webhook Payload

When a Make webhook URL is saved in the dashboard, ActionFlow sends these fields as a browser-friendly form payload:

```json
{
  "meeting_title": "Product launch meeting notes",
  "transcript": "Sarah will finalize the landing page copy by Friday...",
  "source": "actionflow-dashboard"
}
```

The expected response is:

```json
{
  "actions": [],
  "decisions": [],
  "risks": [],
  "blockers": [],
  "followUpSummary": "Summary text",
  "summary": "Short extraction summary"
}
```
