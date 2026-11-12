# Lead Machine: Google Maps to Outreach

An n8n automation that finds local businesses on Google Maps, scores them, finds their contact details, writes a personalized opening line for each one with AI, and sends outreach emails only after a human approves them.

Built as a practice project to show an end-to-end lead generation system, from search to sent email.

## The problem

Finding local businesses, checking which ones are worth contacting, tracking down emails, and writing a personal first line for each takes hours of manual work. This workflow does the repetitive parts and leaves the decision to send with a person.

## What it does

1. **Takes a search** through an n8n form (business type and city).
2. **Scrapes Google Maps** using an Apify actor.
3. **Cleans the data**: removes closed businesses and duplicates, tidies URLs and emails.
4. **Scores each lead** (rating, review count, email, website, social presence) and tags it Hot, Warm or Cold.
5. **Filters out low scorers** so they never reach the sheet.
6. **Saves leads to Google Sheets**, updating existing rows by Place ID so re-running a search never creates duplicates.
7. **Routes leads** by what contact details they have:
   - Website and email: continue straight to AI writing.
   - Website only: the site is fetched and checked for an email address. Blocked or failed sites are handled without stopping the run.
   - No website: sent to a fallback path.
8. **Splits leads with and without an email.** Leads with no email go to a "Manual Follow-up" tab for phone or social outreach.
9. **Writes a personalized first line** for each emailable lead with an AI Agent (OpenAI chat model plus a structured output parser). The output includes the exact fact the line is based on, so it can be checked.
10. **Waits for human approval.** Every line is saved with the status `Draft`. Only rows changed to `Approved` are ever sent.

Lead Machine_ Google Maps to Outreach.json

## The three workflows

| Workflow | Purpose |
|---|---|
| **Lead Machine: Google Maps to Outreach** | Search, scrape, clean, score, route, enrich, and generate first lines |
| **Lead Machine: Send Approved Emails** | Sends emails for approved rows only, marks them `Sent` with a timestamp, and emails a short summary |
| **Lead Machine: Error Alert** | Emails an alert if the main workflow fails |

### Sending workflow

Manual trigger, then Get row(s) filtered on `Status = Approved`, then a Limit (to keep daily volume low), then Gmail, then Update Row (`Status = Sent`, `Sent At`), then a summary email to the owner. A lead can never be emailed twice because its status changes to `Sent`.

## Tech used

- n8n (Form trigger, Code, Filter, Switch, IF, HTTP Request, Google Sheets, Gmail, AI Agent)
- Apify (Google Maps scraper)
- OpenAI (chat model for first lines)
- Google Sheets (lead database and approval queue)
- Gmail (sending)

## Google Sheet structure

One spreadsheet with three tabs:

- **Google Map Leads**: Business Name, Services, Category, Website, Email, Phone, Address, Location, Lead Score, Lead Tier, Place ID, Status, Rating, Review count, Instagram
- **Personalized Details of leads**: Business name, Detail used, First line, Email, Place ID, Phone, Status (`Draft` / `Approved` / `Sent`), Sent At
- **Manual Follow-up**: Business Name, Phone, Website, Instagram, Place ID

`Place ID` is the matching key everywhere, which is how duplicates are avoided.

## Setup

1. Import the three JSON files into n8n.
2. Create credentials for **Apify**, **OpenAI**, **Google Sheets (OAuth)** and **Gmail (OAuth)**.
3. Create the Google Sheet with the tabs and headers above.
4. In each Google Sheets node, select your spreadsheet and tab (`YOUR_SHEET_ID` placeholders in the files).
5. In the main workflow settings, set **Lead Machine: Error Alert** as the error workflow.
6. Edit the email wording and sender name in the Gmail node of the sending workflow.

## What was tested

- A normal run of 10 leads through every branch.
- The same search run twice: no duplicate rows in any tab.
- A deliberately broken scraper: the run stops with a clear error and nothing is written.
- An empty search: the run stops without writing data.
- A website that blocks automated requests (Cloudflare): the lead goes to the manual follow-up list instead of stopping the run.
- Sending: only the `Approved` row was sent, and its status changed to `Sent`.


## Responsible use

- **Human approval first.** Nothing is sent until a person reads the line and sets the status to `Approved`.
- **Low volume.** The sending workflow is limited to a small number of emails per run to protect sender reputation.
- **Honest emails.** Each email uses a real name, an honest subject, and a clear way to opt out.
- **Follow the rules** that apply to where the recipients are (for example CAN-SPAM in the US, GDPR in the EU and UK, and NDPR in Nigeria).
- Business data is collected through a managed scraper, not by scraping Google directly.

## Possible improvements

- Follow-up emails after 3 and 7 days, stopping when someone replies.
- AI summaries of review themes to find a lead's pain points.
- A daily summary of new leads by tier.
- Sending from a dedicated domain with SPF, DKIM and DMARC set up.

## Author

Adeyemi Daniel Akinsanmi: [LinkedIn](https://www.linkedin.com/in/adeyemi-daniel-akinsanmi-a69a873a5/)
