# ActionFlow Meeting Tracker - Project Documentation

## Project Overview

ActionFlow Meeting Tracker is a hosted automation dashboard for turning meeting notes into structured follow-up work. Users can type notes, paste transcripts, or use browser microphone transcription to capture meeting discussion. The app then extracts action items, owners, deadlines, decisions, risks, blockers, meeting summaries, and exportable task data.

The project combines a GitHub Pages dashboard with a Make automation scenario. The Make workflow receives transcript data through a webhook, runs an AI agent, logs the meeting to Google Sheets, and returns the result to the dashboard.

## Problem It Solves

Meetings often end with unclear ownership, scattered notes, and no reliable follow-up record. ActionFlow solves this by converting messy meeting discussion into a usable operational view:

- who owns each task
- what needs to happen next
- which decisions were made
- which risks need attention
- what summary should be shared after the meeting
- where the meeting history is logged

## Core Features

- Hosted dashboard on GitHub Pages
- Meeting title field
- Typed transcript input
- Browser microphone transcription
- English and German audio language selection
- Local transcript verification
- Make webhook integration
- Make AI Agent summary route
- Google Sheets logging
- Meeting summary
- Key follow-up summary
- Action items with owners, priorities, due dates, and status
- Decision and risk tabs
- JSON output view
- CSV export
- Local mode when Make is not connected

## Tools And Technologies

```text
GitHub Pages
HTML
CSS
JavaScript
Web Speech API
Make
Make AI Agents
Make Webhooks
Google Sheets
Browser localStorage
```

## Workflow Architecture

```text
ActionFlow dashboard
-> Make custom webhook
-> Prompt builder / Set variable
-> Make AI Agent
-> Google Sheets: Add a row
-> Webhooks: Webhook response
-> Dashboard result view
```

The dashboard also includes local extraction logic. This keeps the app usable when Make is not configured, sleeping, rate-limited, or returning incomplete structured data.

## How The App Works

1. The user enters a meeting title.
2. The user pastes notes or starts browser audio transcription.
3. The transcript is added to the dashboard.
4. The user clicks **Extract actions**.
5. If a Make webhook URL is configured, the dashboard sends the meeting title, transcript, and source metadata to Make.
6. Make runs the AI agent and logs the run to Google Sheets.
7. Make returns a webhook response to the dashboard.
8. The dashboard renders summary, actions, decisions, risks, JSON, and CSV export.
9. If Make returns incomplete arrays, the dashboard verifies tasks directly from the transcript.

## Make Scenario

The Make scenario is designed as:

```text
Custom webhook
-> Set AI Extraction Prompt
-> Make AI Agents: Run an agent
-> Google Sheets: Add a row
-> Webhook response
```

The webhook receives:

```json
{
  "meeting_title": "Weekly product operations meeting",
  "transcript": "Sarah will send the onboarding draft by Friday...",
  "source": "actionflow-dashboard"
}
```

The response returns:

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

## Google Sheets Logging

The Google Sheets log captures persistent meeting history.

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

## Problems Faced And Fixes

### 1. Make returned `Accepted` instead of JSON

Problem:
The dashboard sometimes received `Accepted` instead of the final webhook response.

Fix:
We tested the difference between manual `Run once` mode and live scenario mode. We also clarified that the final module should be the webhook response module, and that Make scenario settings can affect whether the final response is returned.

### 2. Make AI returned summaries but empty arrays

Problem:
Make AI sometimes returned a useful summary but empty `actions`, `decisions`, `risks`, and `blockers` arrays.

Fix:
The dashboard now verifies actions, owners, deadlines, decisions, and risks directly from the transcript if the AI response is incomplete.

### 3. Browser audio stopped without saving text

Problem:
The browser sometimes kept speech as interim text and did not mark it as final before the user clicked Stop.

Fix:
The app now saves the latest interim speech if no final transcript is available.

### 4. Spoken transcripts were messy

Problem:
Speech transcription produced phrases like:

```text
So Sarah will...
And Michelle needs...
Marta, kindly look into...
daniel also has to...
```

The first parser missed owners in some of these cases.

Fix:
Owner detection was expanded to handle spoken fillers, lowercase names, comma-separated instructions, and phrases such as `kindly look into`, `also has to`, and `should follow up`.

### 5. Meeting title became the transcript

Problem:
For audio notes, the first transcript sentence was sometimes used as the meeting title.

Fix:
A dedicated meeting title field was added. If no title is provided, the app generates a clean fallback title.

### 6. `next steps` was detected as a date

Problem:
The parser treated `next steps` as a due date because it matched a broad `next ...` pattern.

Fix:
Due-date detection was tightened to specific date phrases such as `next week`, `next Monday`, `tomorrow`, or `end of the week`.

### 7. Google Sheets mapping shifted columns

Problem:
Some Google Sheets fields were mapped into the wrong columns.

Fix:
The mapping was reviewed column by column. The recommended mapping now prioritizes meeting title, source, transcript, summary, and log evidence.

## Current Limitations

- Browser speech recognition support depends on the browser. Chrome and Edge are recommended.
- Make free/manual testing may require `Run once` depending on scenario configuration.
- Make AI may return incomplete structured arrays, so the dashboard includes local verification logic.
- Google Sheets logging depends on correct column mapping in Make.
- Audio transcription quality depends on microphone quality, browser support, background noise, and language selection.

## Future Add-Ons

- Screenshot gallery in the README
- Slack or Microsoft Teams follow-up messages
- Email summary to meeting participants
- Task creation in Trello, Asana, Notion, ClickUp, or Microsoft Planner
- Reminder loop for upcoming due dates
- Multi-language extraction prompts for German transcripts
- File upload for audio recordings
- Speaker labels
- Authentication for private team use
- Admin dashboard for past meetings and unresolved tasks
- Better analytics across meeting logs

## Final Demo Checklist

- Text transcript produces actions, owners, decisions, and risks.
- English audio transcript is captured.
- German audio transcript is captured.
- Make webhook mode returns a result.
- Google Sheets receives a row.
- CSV export works.
- README links open correctly.
- Screenshots are added after the final UI is approved.

## Project Links

```text
Live app: https://hildegarht1.github.io/meeting-action-tracker/
Source code: https://github.com/Hildegarht1/meeting-action-tracker
```
