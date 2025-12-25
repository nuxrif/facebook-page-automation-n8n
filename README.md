# Facebook Page Auto Moderator & Messenger Auto-Reply (n8n)

This repository contains an **n8n workflow** that automates moderation and engagement for a **Facebook Page** using **Meta Webhooks + Graph API**.

It focuses on two things:
- **Post comments:** reply to **positive/normal** comments and **delete** **negative** comments
- **Messenger:** send short, friendly **auto-replies** to incoming messages (with a simple “human/admin handoff” rule)

## What it does

### ✅ Comment moderation (Feed)
- Watches for **new post comments** on your Facebook Page
- Classifies each comment:
  - **Positive/Normal** → posts an automatic reply
  - **Negative** → deletes the comment
- Skips your **own Page/admin comments** to avoid replying to yourself

### ✅ Messenger auto-reply
- Watches for **new Messenger messages** sent to your Page
- Generates a short reply (Bengali-friendly tone)
- If someone asks for a **human/admin/Sharif**, it sends a **handoff message** instead of continuing automation
- Keeps basic session context per user so responses stay consistent

## How it works (high level)

### 1) Webhook receive + verify
- A webhook endpoint receives events from Meta
- During verification, it returns the required challenge
- For POST events, it quickly returns `EVENT_RECEIVED` so Meta doesn’t retry

### 2) Route by event type
- **Messenger message** → Messenger flow
- **Feed comment** → Comment moderation flow

### 3) Comment moderation logic
- Extract comment text + IDs
- Classify comment
  - **Positive/Normal** → generate a reply → publish reply under the comment
  - **Negative** → delete the comment via Graph API

### 4) Messenger reply logic
- Extract sender ID and message text
- Generate a short response
- Apply “human request” handoff rule
- Send reply back via Graph API

## Features (summary)
- Auto-reply to **Positive/Normal** comments
- Auto-delete **Negative** comments
- Auto-reply to Messenger messages
- Human handoff trigger (admin request)
- Webhook verify + fast acknowledgment
- Built using **n8n + Meta Graph API**
- Works well for **Bangla/Bengali** audiences

## Requirements
- A running **n8n** instance (cloud or self-hosted)
- A **Meta App** with:
  - Webhooks configured for your Page
  - Required permissions for reading messages/comments and managing comments
- A **Facebook Page Access Token** (long-lived recommended)
- Optional: an AI/LLM API key if your workflow uses an AI node to generate replies

## Project structure (recommended)
- `workflows/`
  - `facebook-page-automation.json` (exported n8n workflow)
- `README.md`
- `.env.example` (optional, if you prefer env-based config)
- `LICENSE` (optional)

## Setup
1) Import the workflow into n8n  
   - n8n → **Workflows → Import from File** → select the exported JSON

2) Configure credentials / secrets in n8n  
   - Facebook Page Access Token  
   - Webhook Verify Token  
   - Optional: AI provider key

3) Configure Meta Webhooks  
   - Add your n8n webhook URL to Meta Webhooks  
   - Subscribe to the events you need (Messenger + Feed comments)  
   - Complete verification using your Verify Token

4) Activate the workflow  
   - Turn on the workflow in n8n  
   - Test with a message + a few comments

## Configuration notes
You can customize:
- Reply style (short/long, formal/casual)
- What counts as negative (strict vs soft moderation)
- Handoff keywords (e.g., “admin”, “human”, “Sharif”)

## Security
- Don’t commit tokens to GitHub
- Use n8n **Credentials** or environment variables
- Restrict access to your n8n dashboard and logs

## Troubleshooting
- Webhook verification fails:
  - Verify Token must match in Meta + n8n
  - Webhook must be publicly reachable over HTTPS

- Replies not posting / comments not deleting:
  - Check Page token permissions
  - Confirm the Page is subscribed to correct webhook fields
  - Check n8n execution logs for Graph API errors

## Credits
Created by **Sharif**

- 🐙 GitHub: [nuxrif](https://github.com/nuxrif/)
- 📘 Facebook: [nuxrif](https://www.facebook.com/nuxrif)
- ✉️ Email: [sa.mollick@gmail.com](mailto:sa.mollick@gmail.com)
- 📞 Phone: [+8801717361064](tel:+8801717361064)
- 💬 WhatsApp: [Chat on WhatsApp](https://wa.me/8801717361064)
