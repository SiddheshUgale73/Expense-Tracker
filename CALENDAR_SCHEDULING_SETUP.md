# 📅 Google Calendar Scheduling Setup

This guide covers setting up Google Calendar OAuth2 and automating meeting scheduling using natural language.

## 1. Google Cloud Console Setup (OAuth2)
1.  Go to the [Google Cloud Console](https://console.cloud.google.com/).
2.  **Enable APIs**: Search for and enable **Google Calendar API**.
3.  **OAuth Consent Screen**: Ensure `https://www.googleapis.com/auth/calendar.events` is added as a scope.
4.  **Credentials**: 
    - Create an **OAuth client ID** (Web application).
    - Add your n8n redirect URI (e.g., `https://your-n8n-domain/rest/oauth2-callback`).
5.  **Client ID & Secret**: Save these to your `.env` file.

## 2. n8n Node Configuration (Calendar)
The Google Calendar node uses the **Create** operation for the `Event` resource.

**Dynamic Mapping Expressions:**
- **Start Time**: `{{ $node["OpenAI Agent"].json.message.content.entities.date }}T{{ $node["OpenAI Agent"].json.message.content.entities.time || '09:00' }}:00Z`
- **End Time**: `{{ $node["OpenAI Agent"].json.message.content.entities.date }}T{{ $node["OpenAI Agent"].json.message.content.entities.time ? ($node["OpenAI Agent"].json.message.content.entities.time.split(':')[0]*1 + 1).toString().padStart(2, '0') + ':00' : '10:00' }}:00Z`
- **Summary**: `{{ $node["OpenAI Agent"].json.message.content.entities.subject }}`
- **Description**: `{{ $node["OpenAI Agent"].json.message.content.entities.body || 'Scheduled via AI Assistant' }}`

## 3. Natural Language Examples
The AI Agent handles relative dates and times (e.g., "tomorrow", "5 PM") and converts them to ISO format.

### Example 1: Relative Date
**Input:** "Schedule a coffee chat tomorrow at 10am"
- **Date**: 2026-04-05
- **Time**: 10:00
- **Summary**: coffee chat

### Example 2: Specific Date
**Input:** "Meeting with Bob on May 10th at 2pm about the project"
- **Date**: 2026-05-10
- **Time**: 14:00
- **Summary**: Meeting with Bob: project

## 4. Automation Logic
1.  **Parse**: OpenAI extracts `date`, `time`, and `subject`.
2.  **Format**: n8n expressions combine date and time into RFC3339 format required by Google.
3.  **Execute**: Calendar node creates the event in your `primary` calendar.
4.  **Confirm**: Telegram sends a link to the created event.
