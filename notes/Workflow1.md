# Workflow 1: Gmail → Google Sheets Integration

This workflow demonstrates how to send an email via Gmail in **n8n** and automatically log the email message and timestamp into a **Google Sheet**.

---

## Workflow Overview
The workflow has 4 main nodes:

1. **When clicking 'Execute workflow' (Trigger)**  
   - Manually starts the workflow when executed in the n8n editor.  
   - Useful for testing before setting up automatic triggers.  

2. **Edit Fields (Set node)**  
   - Defines custom variables:  
     - `to`: recipient email  
     - `subject`: subject line of the email  
     - `message`: message body text  
     - `dateTime`: timestamp generated dynamically with  
       ```js
       {{$now.format("dd-MM-yyyy HH:mm:ss")}}
       ```  
   - These fields are then available to other nodes in the workflow.

3. **Email (Gmail node)**  
   - Sends the actual email using the Gmail API.  
   - Uses the variables from the Edit Fields node for `to`, `subject`, and `message`.

4. **Append Row in Sheet (Google Sheets node)**  
   - Logs the values (`dateTime` and `message`) into a Google Sheet.  
   - Each email sent is recorded as a new row with the timestamp and message content.

---

## Google Cloud Setup

To use Gmail & Google Sheets in n8n, I had to configure APIs and credentials in **Google Cloud Console**.

### Steps:
1. **Create a Google Cloud Project**  
   - Named it `n8n-gmail`.  
   - A project is like a container for APIs, credentials, and users.

2. **Enable APIs**  
   - Enabled the following APIs:  
     - Gmail API  
     - Google Sheets API  
     - Google Drive API (needed for Sheets access)  

3. **Create OAuth Credentials**  
   - Generated an **OAuth 2.0 Client ID & Secret**.  
   - Configured the **Redirect URI** to:  
     ```
     http://localhost:5678/rest/oauth2-credential/callback
     ```  
     (this is how n8n receives the access token after login).

4. **Configure OAuth Consent Screen**  
   - Set myself as a test user (using my Gmail).  
   - Without adding myself, I got the error:  
     *"Access blocked: app has not completed the Google verification process"* (Error 403).  
   - Adding myself as a test user solved the problem.

---

## Problems Encountered
- **Sheets node showing "No columns found":**  
  This happened when I hadn’t enabled Google Drive API.

---

## Integration with Google Sheets
1. Created a new spreadsheet in Google Drive called `n8n-test`.  
   - Columns: `Date & Time`, `Message`.

2. In the Google Sheets node (n8n):  
   - Chose **Append Row** as the operation.  
   - Selected the sheet by ID.  
   - Mapped the fields manually:  
     - `Date & Time` → `{{$node["Edit Fields"].json.dateTime}}`  
     - `Message` → `{{$node["Edit Fields"].json.message}}`

3. Final result:  
   - Each time I click *Execute workflow*, the email is sent **and** a new row is added to the sheet with the timestamp and message.

---

## Key Learnings
- n8n requires **OAuth 2.0 credentials** for Google integrations.  
- Always add yourself as a test user in the **OAuth consent screen**.  
- The **Set node (Edit Fields)** is useful when the previous node does not expose the exact output you want.  
- By combining Gmail + Google Sheets, I automated both **sending messages** and **recording them for tracking**.

