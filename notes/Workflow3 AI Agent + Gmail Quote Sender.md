# Workflow: AI Agent Sends Random Quote via Gmail

## Overview
This workflow uses **n8n** to generate and send a random inspirational quote via Gmail.  
It combines an AI agent (powered by DeepSeek) with external tools (Zen Quotes API + Gmail) to automate a random email.


## Workflow Steps

1. **Trigger**
   - The workflow starts when you manually click **Execute Workflow**.

2. **AI Agent (DeepSeek via OpenAI Node)**
   - Uses the **OpenAI Chat Model** node, but configured with:
     - **DeepSeek API Key**
     - **Base URL:** `https://api.deepseek.com/v1`
     - **Model:** `deepseek-chat`
   - Wrapped by n8n’s **AI Agent** node to manage:
     - Memory
     - Tools
     - Conversation flow

3. **Tools Integrated**
   - **HTTP Request**
     - Calls the [ZenQuotes API](https://zenquotes.io/) to fetch a random motivational quote.
   - **Gmail: Send Message**
     - Sends the retrieved quote to a personal Gmail account.
     - The email is formatted with **beautiful HTML** for readability.


## Example Use Case
- Automating daily inspirational quote emails.  
- Could be extended for:
  - Slack or Telegram notifications  
  - Journaling quotes in Google Sheets  
  - Adding AI-powered reflections on each quote

