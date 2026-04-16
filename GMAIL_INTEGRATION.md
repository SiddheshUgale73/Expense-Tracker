# 📧 Gmail API Integration with n8n

This guide covers setting up Gmail OAuth2 and implementing a "Draft then Send" workflow with user confirmation.

## 1. Google Cloud Console Setup (OAuth2)
1.  Go to the [Google Cloud Console](https://console.cloud.google.com/).
2.  Create a new project or select an existing one.
3.  **Enable APIs**: Go to `Enabled APIs & Services` -> `+ ENABLE APIS AND SERVICES` and search for **Gmail API**. Enable it.
4.  **OAuth Consent Screen**:
    - Choose `External` (or `Internal` if using Google Workspace).
    - Fill in app name, user support email, and developer contact info.
    - Add the scope: `https://www.googleapis.com/auth/gmail.send` and `https://www.googleapis.com/auth/gmail.compose`.
5.  **Credentials**:
    - Click `+ CREATE CREDENTIALS` -> `OAuth client ID`.
    - Application type: `Web application`.
    - **Authorized redirect URIs**: Copy the OAuth redirect URL from your n8n Gmail credential setup page (usually `https://your-n8n-domain/rest/oauth2-callback`).
6.  **Client ID & Secret**: Save the generated `Client ID` and `Client Secret` in your `.env` file.

## 2. n8n Credential Configuration
1.  In n8n, go to **Credentials** -> **Add Credential** -> **Gmail OAuth2 API**.
2.  Input your `Client ID` and `Client Secret`.
3.  Click **Sign in with Google** and authorize the app.

## 3. Workflow Logic: Draft & Confirm
To implement confirmation before sending, the workflow follows this logic:
1.  **AI Extraction**: Extract `recipient`, `subject`, and `body`.
2.  **Gmail Node (Create Draft)**: Create a draft instead of sending immediately.
3.  **Telegram Node (Ask Confirmation)**: Send a message to the user with "Confirm" and "Cancel" inline buttons.
4.  **Wait for Webhook**: n8n waits for the user to click a button.
5.  **Gmail Node (Send)**: If confirmed, send the drafted email (or send the email directly using the extracted data).

---

## 4. Gmail Node Configuration (JSON Snippets)

### Create Draft Node
```json
{
  "parameters": {
    "resource": "draft",
    "operation": "create",
    "subject": "={{ $node[\"OpenAI Agent\"].json.message.content.entities.subject }}",
    "message": "={{ $node[\"OpenAI Agent\"].json.message.content.entities.body }}",
    "toList": "={{ $node[\"OpenAI Agent\"].json.message.content.entities.email }}"
  }
}
```

### Send Email Node (After Confirmation)
```json
{
  "parameters": {
    "resource": "message",
    "operation": "send",
    "subject": "={{ $node[\"OpenAI Agent\"].json.message.content.entities.subject }}",
    "message": "={{ $node[\"OpenAI Agent\"].json.message.content.entities.body }}",
    "toList": "={{ $node[\"OpenAI Agent\"].json.message.content.entities.email }}"
  }
}
```
