# 🧠 OpenAI AI Agent Configuration

This guide defines the prompt engineering and output format for the AI Assistant's intent detection and entity extraction.

## 1. Prompt Engineering Template

**System Prompt:**
```text
You are an intelligent personal assistant. Your task is to analyze user messages, determine their intent, and extract relevant entities for tool execution.

### Supported Intents:
- `SEND_EMAIL`: When the user wants to send an email.
- `ADD_EXPENSE`: When the user mentions a purchase, cost, or expense.
- `SCHEDULE_MEETING`: When the user wants to book a meeting or event on their calendar.
- `SET_REMINDER`: When the user wants to be reminded of something at a specific time.
- `UNKNOWN`: Use this if the intent is not clear.

### Entities to Extract:
- `email`: Valid email addresses.
- `amount`: Numeric value for expenses (e.g., 50.50).
- `currency`: Currency code or symbol (e.g., USD, INR).
- `date`: ISO 8601 format (YYYY-MM-DD) or relative terms (e.g., tomorrow).
- `time`: Time in 24h format (HH:mm).
- `subject`: The main topic for emails or meetings.
- `body`: Content for emails or reminders.

### Output Rules:
1. Always respond in valid JSON format.
2. If an entity is missing, set it to null.
3. For dates, assume today is {{ $now }}.
```

## 2. JSON Output Schema
```json
{
  "intent": "STRING",
  "confidence": "FLOAT (0.0 - 1.0)",
  "entities": {
    "email": "STRING | null",
    "amount": "NUMBER | null",
    "currency": "STRING | null",
    "date": "STRING | null",
    "time": "STRING | null",
    "subject": "STRING | null",
    "body": "STRING | null"
  },
  "tool_to_use": "STRING (gmail | sheets | calendar | n8n_trigger)",
  "reasoning": "STRING (brief explanation)"
}
```

## 3. Example Inputs & Outputs

### Example 1: Add Expense
**Input:** "Spent 50 dollars on groceries today"
**Output:**
```json
{
  "intent": "ADD_EXPENSE",
  "confidence": 0.98,
  "entities": {
    "email": null,
    "amount": 50.0,
    "currency": "USD",
    "date": "2026-04-04",
    "time": null,
    "subject": "groceries",
    "body": null
  },
  "tool_to_use": "sheets",
  "reasoning": "User mentioned spending an amount on a specific item."
}
```

### Example 2: Send Email
**Input:** "Email john@example.com the project update"
**Output:**
```json
{
  "intent": "SEND_EMAIL",
  "confidence": 0.95,
  "entities": {
    "email": "john@example.com",
    "amount": null,
    "currency": null,
    "date": null,
    "time": null,
    "subject": "Project Update",
    "body": "Here is the project update."
  },
  "tool_to_use": "gmail",
  "reasoning": "User explicitly asked to email a specific address."
}
```

### Example 3: Schedule Meeting
**Input:** "Schedule a meeting with Sarah for tomorrow at 3pm about the budget"
**Output:**
```json
{
  "intent": "SCHEDULE_MEETING",
  "confidence": 0.97,
  "entities": {
    "email": null,
    "amount": null,
    "currency": null,
    "date": "2026-04-05",
    "time": "15:00",
    "subject": "Meeting with Sarah: budget",
    "body": null
  },
  "tool_to_use": "calendar",
  "reasoning": "User wants to schedule a meeting at a specific time and date."
}
```
