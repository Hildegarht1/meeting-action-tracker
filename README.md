# ActionFlow Meeting Tracker

ActionFlow is a hosted meeting automation tool that converts messy meeting notes or browser-transcribed audio into structured follow-up work. It extracts action items, owners, deadlines, decisions, risks, summaries, and exportable task data from a single dashboard.

Live app:

```text
https://hildegarht1.github.io/meeting-action-tracker/
```

## What It Does

- accepts typed meeting notes
- captures speech through the browser microphone when supported
- separates meeting title from transcript content
- extracts action items, owners, deadlines, decisions, risks, and blockers
- generates a meeting summary and key follow-up summary
- sends transcripts to a Make webhook when live mode is configured
- uses Make AI Agents for AI-assisted summary generation
- logs processed meetings to Google Sheets
- returns the result to the dashboard for review
- exports action items as CSV

## Tools Used

```text
GitHub Pages
HTML/CSS/JavaScript
Web Speech API
Make
Make AI Agents
Make Webhooks
Google Sheets
Browser localStorage
```

## How To Demo

1. Open the hosted dashboard.
2. Enter a meeting title.
3. Paste meeting notes or click **Start listening** to capture speech.
4. Click **Extract actions**.
5. Review the meeting summary, key follow-up, action board, decisions, risks, and JSON output.
6. Export action items as CSV if needed.
7. In live mode, confirm the Make run and Google Sheets log row.

Sample spoken input:

```text
Sarah will send the onboarding draft by Friday. We need to decide who gets priority support. The client is waiting, so this is high risk.
```

## Automation Architecture

```text
ActionFlow dashboard
-> Make custom webhook
-> Make AI Agent
-> Google Sheets meeting log
-> Webhook response
-> Dashboard result view
```

The dashboard also has a local extraction mode, so the interface remains usable even when the Make webhook is not configured.

## How It Works In The App

```text
Capture notes
-> Process with Make
-> Log the run
-> Review output
```

The interface shows the active route, input type, log route, and output status during each extraction.

## Google Sheets Log

Suggested sheet name:

```text
ActionFlow Meeting Logs
```

Suggested tab name:

```text
Meetings
```

Suggested columns:

```text
created_at
meeting_title
source
action_count
decision_count
risk_count
blocker_count
follow_up_summary
summary
actions_json
decisions_json
risks_json
blockers_json
raw_transcript
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
  "meetingSummary": "Human-readable meeting summary",
  "followUpSummary": "Key follow-up summary",
  "summary": "Short extraction summary"
}
```

## Make Scenario

Recommended scenario flow:

```text
Custom webhook
-> Set AI prompt variable
-> Make AI Agents: Run an agent
-> Google Sheets: Add a row
-> Webhooks: Webhook response
```

The Make scenario design is documented in:

```text
workflows/make-scenario-design.md
```

## Test Checklist

- Text transcript produces action items.
- Audio transcript is added to the transcript box in Chrome or Edge.
- Owners are detected from natural phrases such as "Sarah will..." and "Michelle needs to...".
- Decisions and risks appear in their tabs.
- Make webhook mode returns a dashboard result.
- Google Sheets receives a meeting log row.
- CSV export downloads the action board.

## Notes

Browser speech recognition support varies by browser. Chrome and Edge currently provide the most reliable support for the Web Speech API.

The dashboard includes transcript verification logic so action items, owners, deadlines, decisions, and risks can still be shown even if an external AI response returns an incomplete structured payload.
