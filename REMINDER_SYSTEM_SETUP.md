# 🔔 n8n Reminder System Setup

This guide explains how to build a flexible reminder system using n8n and the `Wait` node.

## 1. System Logic
The reminder system follows a 4-step process:
1.  **Receive**: User sends a reminder request via Telegram (e.g., "Remind me to call Mom in 2 hours").
2.  **Extract**: The OpenAI node extracts the **time** (relative or absolute) and the **message**.
3.  **Wait**: The `Wait` node pauses the workflow until the specified time.
4.  **Notify**: Once the time is reached, the workflow resumes and sends a Telegram notification.

## 2. n8n Node Configuration (Wait Node)
The `Wait` node (v1+) is highly effective for this task.

**Configuration:**
- **Resume**: `At a specific time`
- **Wait Until**: `{{ $node["OpenAI Agent"].json.message.content.entities.date }}T{{ $node["OpenAI Agent"].json.message.content.entities.time || '09:00' }}:00Z`

## 3. Example Execution

### Example 1: Relative Time
**Input:** "Remind me to drink water in 30 minutes"
- **Extracted Date**: 2026-04-04 (Today)
- **Extracted Time**: 00:43 (Calculated by AI based on current time)
- **Message**: drink water
- **Outcome**: Telegram message sent at 00:43 AM.

### Example 2: Specific Time
**Input:** "Set a reminder for my flight tomorrow at 6 PM"
- **Extracted Date**: 2026-04-05
- **Extracted Time**: 18:00
- **Message**: my flight
- **Outcome**: Telegram message sent tomorrow at 6:00 PM.

## 4. Key Considerations
- **Workflow Persistence**: n8n handles long waits by storing the execution state in the database. Even if n8n restarts, the reminder will still trigger.
- **Timezone**: Ensure the `GENERIC_TIMEZONE` in your `.env` matches your local timezone for accurate scheduling.
- **User Feedback**: The system sends an immediate confirmation (e.g., "Reminder set for tomorrow at 6 PM") so the user knows it's active.
