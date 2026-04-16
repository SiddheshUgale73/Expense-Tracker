# 📲 Telegram Bot Setup for n8n

Follow these steps to create your Telegram bot and connect it to your n8n AI Assistant.

## 1. Create the Bot via BotFather
1.  Open Telegram and search for **@BotFather**.
2.  Start a chat and send the command: `/newbot`
3.  **Name your bot**: Choose a display name (e.g., `My AI Assistant`).
4.  **Choose a username**: Must end in `bot` (e.g., `my_ai_assistant_bot`).
5.  **Save the API Token**: BotFather will provide an HTTP API token. Copy this; you'll need it for n8n.

## 2. API Setup in n8n
1.  Log in to your n8n instance (`http://localhost:5678`).
2.  Go to **Credentials** -> **Add Credential**.
3.  Search for **Telegram Bot API**.
4.  Paste the **Access Token** you got from BotFather.
5.  Save the credential.

## 3. Webhook Configuration
n8n uses the `Telegram Trigger` node to automatically handle webhooks. You don't need to manually set up the webhook URL on Telegram; n8n does this for you when you activate the workflow.

**Important Note on Webhooks**:
- For the webhook to work, your n8n instance must be accessible from the internet (e.g., via a tunnel like Ngrok or a public domain with SSL).
- Ensure `WEBHOOK_URL` in your `.env` file matches your public URL.

## 4. Troubleshooting Webhooks
If your bot isn't responding, check the following:
1.  **Public Accessibility**: Use a tool like `ngrok` or `localtunnel` if you're developing locally to expose port `5678`.
2.  **HTTPS**: Telegram requires webhooks to use HTTPS.
3.  **Webhook Activation**: The n8n workflow must be **Active** for the webhook to be registered with Telegram.
4.  **Credential Selection**: Ensure the correct Telegram Bot API credential is selected in both the `Telegram Trigger` and the `Telegram` nodes.
