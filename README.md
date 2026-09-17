## Overview
This n8n workflow automates executive business operations by processing incoming emails, classifying them with AI, routing to appropriate actions (task creation, meeting scheduling, alerts), logging activity to Google Sheets, and delivering daily/weekly briefings via email.

## How It Works
1. 'Gmail Trigger' watches for new unread emails.
2. 'Extract Email Fields' pulls sender, subject, body, date, IDs.
3. 'AI Email Classifier' (LangChain agent) categorizes each email and extracts priority, summary, action needed, and meeting flags.
4. 'Route by Category' splits flow into urgent, action‑required, meeting‑request, and FYI branches.
5. Each branch performs the relevant action: Asana task creation, Google Calendar event creation, or simple logging.
6. All branches merge and the data is appended to a Google Sheet for tracking.
7. Separate scheduled triggers generate a morning brief, an end‑of‑day summary, and a weekly report, pulling data from the Sheet and sending formatted emails.
8. A status‑check sub‑workflow scans pending rows every 30 minutes, searches for replies, uses an LLM to infer status changes, updates the Sheet, and notifies the user.

## Nodes & Tools Used
- 'Gmail Trigger' & 'Gmail' nodes (email handling)
- 'Set' nodes (field extraction, action tracking)
- LangChain Agent & Output Parser (AI classification)
- 'Code' nodes (custom JS for parsing and brief composition)
- 'Switch' (routing by category)
- Asana (task creation)
- Google Calendar (event creation & fetching)
- Google Sheets (logging, reading, updating)
- Schedule Trigger (timed briefings and status checks)
- 'If' nodes (conditional logic)
- Merge (combining branches)
- NoOp (placeholder)

## Prerequisites
- An n8n instance (self‑hosted or n8n.cloud)
- OAuth2 credentials for Gmail, Asana, Google Calendar, Google Sheets
- Access to a LangChain‑compatible LLM (Mistral Cloud or OpenRouter) for classification
- A Google Sheet with columns: Timestamp, Email_sender, Email_subject, Priority_score, Category, Summary, Action_taken, Task_id, Calendar_event_id, Status
- An Asana workspace and project for task creation

## Setup & Usage
1. Import the workflow JSON into your n8n instance.
2. Replace all placeholder credential IDs (REPLACE_WITH_YOUR_CREDENTIAL_ID) with your own OAuth2 credentials.
3. Set the Google Sheet ID in the documentId fields of every Google Sheets node.
4. Adjust the Asana workspace ID and default calendar ID if needed.
5. Activate the workflow.
6. Test by sending an email to the monitored inbox; observe task creation, calendar events, and sheet logging.
7. The scheduled triggers will automatically send morning briefs at 7 AM, EOD summaries at 6 PM, and weekly reports on Fridays at 5 PM.

## Use Cases
- Executives who want AI‑driven email triage and automatic follow‑up.
- Teams that need a lightweight CRM‑like log of communications without leaving n8n.
- Managers who desire daily briefings and weekly performance reports generated from processed email activity.