# n8n Workflow Design

## Workflow: Meeting Action Extraction

```text
Webhook Trigger
→ Code Node: Extract Actions / Decisions / Risks
→ AI Agent: Write Follow-up Summary
→ Google Sheets: Append History
→ Task Tool: Create Tasks
→ Respond To Webhook
```

## Webhook Input

```json
{
  "meeting_title": "Product Launch Sync",
  "transcript": "Sarah will finalize the landing page copy by Friday...",
  "source": "dashboard"
}
```

## Structured Output

```json
{
  "actions": [
    {
      "task": "Finalize the landing page copy",
      "owner": "Sarah",
      "due": "Friday",
      "priority": "medium",
      "status": "open"
    }
  ],
  "decisions": [],
  "risks": [],
  "blockers": []
}
```

## Google Sheets Columns

```text
timestamp
meeting_title
source
transcript_preview
action_count
decision_count
risk_count
summary
structured_json
```

