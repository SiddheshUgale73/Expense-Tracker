# 📊 Google Sheets Expense Tracker Setup

This guide defines the structure and setup for your AI-powered expense tracking system in Google Sheets.

## 1. Sheet Structure
Create a new Google Sheet and add the following headers to the first row (A1 to D1):

| Date | Amount | Category | Description |
| :--- | :--- | :--- | :--- |
| (YYYY-MM-DD) | (Numeric) | (e.g., Food, Travel) | (Details) |

### Sheet ID
The **Sheet ID** is the long string in the URL of your Google Sheet:
`https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID/edit`

Add this ID to your `.env` file as `EXPENSE_SHEET_ID`.

## 2. n8n Node Configuration (Google Sheets)
The Google Sheets node should use the **Append** operation to add new rows.

**Mapping Logic:**
- **Date**: `{{ $node["OpenAI Agent"].json.message.content.entities.date || $now.toFormat('yyyy-MM-dd') }}`
- **Amount**: `{{ $node["OpenAI Agent"].json.message.content.entities.amount }}`
- **Category**: `{{ $node["OpenAI Agent"].json.message.content.entities.subject }}`
- **Description**: `{{ $node["OpenAI Agent"].json.message.content.entities.body || 'No details provided' }}`

## 3. Example Entries

### Example 1: Simple Purchase
**Input:** "Spent $20 on a burger"
- **Date**: 2026-04-04
- **Amount**: 20.00
- **Category**: burger
- **Description**: No details provided

### Example 2: Detailed Expense
**Input:** "Paid 1500 for rent today via bank transfer"
- **Date**: 2026-04-04
- **Amount**: 1500
- **Category**: rent
- **Description**: via bank transfer

## 4. Automation Steps
1.  **Extract**: The OpenAI node parses the message.
2.  **Route**: The switch node directs the flow to the Google Sheets node.
3.  **Append**: The Google Sheets node adds a new row using the extracted data.
4.  **Notify**: Telegram sends a confirmation message to the user.
