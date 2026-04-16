# 🤝 Multi-Agent Architecture for AI Assistant

This document describes the multi-agent system design for the AI Personal Assistant.

## 1. System Design Overview
The system uses an **Orchestrator-Worker** pattern. A central "Orchestrator Agent" acts as the primary interface, while specialized "Worker Agents" handle domain-specific tasks.

### Agents & Roles:
- **Orchestrator Agent**:
  - Receives user messages via Telegram.
  - Analyzes the intent and selects the appropriate specialized agent.
  - Manages the final communication back to the user.
- **Email Agent**:
  - Specializes in drafting, confirming, and sending emails via Gmail.
- **Finance Agent**:
  - Specializes in parsing expense data and recording it in Google Sheets.
- **Scheduler Agent**:
  - Specializes in booking and managing calendar events via Google Calendar.
- **Reminder Agent**:
  - Specializes in setting up notifications and handling wait triggers.

## 2. Agent Communication Flow
1.  **Input**: User sends "Remind me to call Sarah at 3 PM".
2.  **Orchestrator**: Identifies the request as a reminder and calls the **Reminder Agent**.
3.  **Specialized Agent**: Processes the request (e.g., extracts 3 PM and "call Sarah").
4.  **Feedback**: The specialized agent returns a confirmation to the Orchestrator.
5.  **Output**: Orchestrator sends "✅ Reminder set for 3 PM" back to the user.

## 3. n8n Multi-Agent Workflow
In n8n, this is implemented using:
-   **Main Workflow**: The Telegram trigger and the Orchestrator (OpenAI node).
-   **Sub-Workflows**: Each specialized agent is a separate workflow, called via the `Execute Workflow` node.

## 4. Specialized Agent Details

### Email Agent (Sub-Workflow)
- **Role**: Drafts and sends emails via Gmail.
- **Workflow Steps**: 
  - Receives payload from Orchestrator.
  - Generates draft.
  - Asks user for confirmation via Telegram buttons.
  - Sends email upon confirmation.

### Finance Agent (Sub-Workflow)
- **Role**: Records expenses in Google Sheets.
- **Workflow Steps**:
  - Receives amount, category, and date from Orchestrator.
  - Appends a new row to the predefined sheet.
  - Returns a summary of the record.

### Scheduler Agent (Sub-Workflow)
- **Role**: Manages calendar events.
- **Workflow Steps**:
  - Receives event title, date, and time.
  - Creates the event in Google Calendar.
  - Returns the event link.

### Reminder Agent (Sub-Workflow)
- **Role**: Sets notifications.
- **Workflow Steps**:
  - Receives reminder message and target time.
  - Pauses the workflow using a `Wait` node.
  - Sends the notification when the timer expires.

## 5. Advantages
- **Scalability**: Add new agents (e.g., Weather Agent, Research Agent) without modifying existing logic.
- **Accuracy**: Specialized agents have domain-specific prompts and tools.
- **Modularity**: Easier to debug and update individual agent behaviors.
