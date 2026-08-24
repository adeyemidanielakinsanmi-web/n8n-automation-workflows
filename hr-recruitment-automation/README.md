# HR Recruitment Automation

An automated recruitment CRM built in n8n that replaces manual CV collection (WhatsApp/email) with a structured application-to-hire pipeline — auto-classifying applicants, notifying HR, and sending shortlist/offer communications without manual data entry.

## The problem
The company was collecting job applications manually via WhatsApp and email, reviewing every CV by hand, and manually sending shortlist and offer emails one at a time. No central applicant database, high chance of losing applicant details, and no consistency in communication.

## What it does
- Applicants apply through a structured form (name, phone, email, position, qualifications, work experience, years of experience, location, CV upload) instead of messaging CVs directly
- Every applicant is automatically logged into a central applicant database (Google Sheets)
- A Code node evaluates each applicant against qualification criteria and classifies them into one of three categories:
  - **Automatically Qualified** — B.Sc. and 5+ years of experience
  - **Manual Review** — has either a B.Sc. or 5+ years, but not both
  - **Unqualified** — neither requirement met
- A Switch node routes each category down its own path:
  - **Automatically Qualified** → appended to a dedicated database, receives an auto-generated offer letter (Google Docs template → PDF), and HR is notified by email
  - **Manual Review** → appended to a review database, applicant receives a "your application is under review" email, HR is notified a review is needed
  - **Unqualified** → receives a courteous rejection/encouragement email; not added to a separate database since they're already logged in the main applicant sheet

## Stack
- **n8n** — workflow orchestration
- **Google Forms** — application intake (trigger)
- **Google Sheets** — applicant database (all applicants + category-specific sheets)
- **Google Docs** — offer letter template, dynamically filled and exported to PDF
- **Gmail** — automated applicant and HR notifications

## Technical notes
- Classification logic (qualified / manual review / unqualified) built with a Code node evaluating qualification + years of experience together, not just a single condition
- Switch node branches the workflow into three independent notification + database paths based on that classification
- Offer letter generation uses a live Google Docs template — the workflow copies it, fills applicant-specific fields, and exports to PDF before attaching it to the offer email
- Each branch has its own HR notification, so the reviewer always knows *why* an applicant landed where they did, not just that a new applicant exists

## Status
Fully functional end-to-end — tested with sample applicant data through all three classification branches (auto-qualified, manual review, unqualified), including offer letter generation and email delivery.

*Sample data included in this repo is fictional test data, not real applicant information.*
