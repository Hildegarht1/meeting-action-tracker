# Make Scenario Design

This scenario turns meeting notes from the ActionFlow dashboard into structured follow-up work.

## Scenario Flow

```text
Custom Webhook
-> Parse transcript payload
-> Extract structured meeting data
-> Generate follow-up summary
-> Add meeting history row to Google Sheets
-> Optional task creation
-> Webhook response
```

## 1. Custom Webhook

Create a Make scenario and start with a Custom Webhook module.

Expected incoming fields:

```json
{
  "meeting_title": "Product launch meeting notes",
  "transcript": "Sarah will finalize the landing page copy by Friday...",
  "source": "actionflow-dashboard"
}
```

Copy the webhook URL into the ActionFlow dashboard under Automation endpoint.

## 2. Structured Extraction

Use either a Make AI step, a code step, or an LLM module.

The output should include:

```json
{
  "actions": [
    {
      "task": "Finalize landing page copy",
      "owner": "Sarah",
      "due": "Friday",
      "priority": "low",
      "status": "open"
    }
  ],
  "decisions": ["Beta launch stays scheduled for next week"],
  "risks": [
    {
      "text": "Legal approval may delay the public announcement",
      "severity": "high"
    }
  ],
  "blockers": [
    {
      "text": "Analytics dashboard is missing conversion events",
      "owner": "Unassigned",
      "severity": "high"
    }
  ],
  "followUpSummary": "1 action item captured. 1 decision recorded. 2 high-priority follow-ups need attention.",
  "summary": "Processed the meeting and returned structured follow-up work."
}
```

## 3. Google Sheets Log

Suggested sheet columns:

```text
created_at
meeting_title
source
action_count
decision_count
risk_count
blocker_count
actions_json
decisions_json
risks_json
blockers_json
follow_up_summary
raw_transcript
```

## 4. Webhook Response

End the scenario with a Webhook response module.

Return the same JSON shape shown in the Structured Extraction section. The hosted dashboard will render it automatically.

## Optional Extensions

- Create tasks in Trello, Asana, Notion, ClickUp, or Microsoft Planner.
- Send summaries to Slack, Teams, Gmail, or Telegram.
- Add a daily reminder route for open action items due soon.
- Add a manager digest sheet with weekly totals and unresolved blockers.
