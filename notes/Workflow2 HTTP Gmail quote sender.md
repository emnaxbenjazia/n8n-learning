# Quote Sender Workflow (HTTP + Gmail)

This workflow fetches a random inspirational quote from an API and sends it by email using Gmail.

## Workflow Breakdown

1. **When clicking 'Execute workflow' (Trigger)**
   - Manually starts the workflow when the "Execute Workflow" button is clicked.
   - This is just for testing/demonstration. In a real case, it could be scheduled or event-based.

2. **HTTP Request (zenquotes.io)**
   - Makes a GET request to [zenquotes.io/api/random](https://zenquotes.io/api/random).
   - The API responds with a random quote in JSON format.
   - Example output:
     ```json
     [
       {
         "q": "To plant a garden is to believe in tomorrow.",
         "a": "Audrey Hepburn",
         "h": "<blockquote>To plant a garden is to believe in tomorrow. &mdash; <footer>Audrey Hepburn</footer></blockquote>"
       }
     ]
     ```
   - Fields:
     - `q`: the quote text
     - `a`: the author
     - `h`: the HTML formatted version

3. **Gmail (Send Message)**
   - Takes the values from the HTTP Request output and sends them via email.
   - Subject: **Today's Quote**
   - Body:
     ```text
     Today's quote:
     {{ $json.h }}
     by {{ $json.a }}
     ```
   - The email is sent using the Gmail account previously connected in n8n.

## Key Points

- **No Edit Fields node** was needed because the HTTP API already gave all the required fields (`q`, `a`, `h`).
- Email is sent in **HTML mode**, so the formatted blockquote from the API displays nicely.
- This is a simple example of combining:
  - **Data retrieval (HTTP API)**
  - **Data delivery (Gmail)**
