# 🤖 AI-Powered Personal Assistant (n8n)

A complete project structure for building an AI Assistant that integrates Telegram, OpenAI, and Google Workspace (Gmail, Sheets, Calendar).

## 📁 Folder Structure
- `workflows/`: Exported n8n workflow JSON files for version control.
- `data/`: Persistent storage for n8n (SQLite/Postgres) and internal configurations.
- `config/`: External configuration or static assets.
- `logs/`: Operational logs for debugging.
- `scripts/`: Helper scripts for maintenance or automation.

## 🛠️ Prerequisites
- [Docker](https://www.docker.com/) and [Docker Compose](https://docs.docker.com/compose/)
- [Telegram Bot Token](https://t.me/BotFather) (UI)
- [OpenAI API Key](https://platform.openai.com/api-keys) (AI Brain)
- [Google Cloud Project](https://console.cloud.google.com/) (Gmail, Sheets, Calendar)
  - Enable Gmail API, Google Sheets API, and Google Calendar API.
  - Create OAuth 2.0 credentials for n8n.

## 🚀 Setup Instructions
1. **Clone/Copy** this repository.
2. **Copy `.env.example` to `.env`** and fill in your credentials.
   ```bash
   cp .env.example .env
   ```
3. **Launch the environment**:
   ```bash
   docker-compose up -d
   ```
4. **Access n8n**: Open `http://localhost:5678` in your browser.
5. **Configure Credentials** in n8n for:
   - Telegram Bot API
   - OpenAI API
   - Google OAuth2 (Gmail, Sheets, Calendar)
6. **Import Workflows**:
   - Go to `Workflows` -> `Import from File` and select files from the `workflows/` directory (once created).

## 🧩 Key Features & Nodes
- **Telegram Trigger**: Receives user messages.
- **AI Agent Node**: Uses OpenAI to understand intent and route tasks.
- **Google Sheets Node**: For expense tracking (logging transactions).
- **Google Calendar Node**: For scheduling meetings or setting reminders.
- **Gmail Node**: For sending automated email updates or summaries.
- **n8n Cron/Wait Triggers**: For the reminder system logic.

## 📦 Dependencies
- `n8n`: The core automation engine.
- `Postgres`: Robust database for workflow execution history and persistent data.
- `OpenAI`: Large Language Model for intelligent task processing.
