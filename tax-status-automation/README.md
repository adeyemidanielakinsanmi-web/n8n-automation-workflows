# Staff Tax Status Automation

A simple, rule-based automation in n8n that classifies staff into tax brackets from a submitted form and automatically notifies each person of their status by email — no manual sorting or individual emails required.

## What it does
1. Staff member submits their details through a form: First Name, Last Name, Email, and Salary bracket (0–800k or 800k+)
2. Their record is automatically appended (or updated, if they've submitted before) into a central Google Sheet, matched by email so re-submissions don't create duplicate rows
3. A Switch node evaluates their salary bracket and routes them down one of two paths:
   - **0–800k** → receives an email confirming they are not liable for tax
   - **800k+** → receives an email notifying them that tax payment is required
4. Each branch sends its own tailored email — no manual follow-up needed

## Stack
- **n8n** — workflow orchestration
- **Google Sheets** — staff records with computed tax status, matched/deduplicated by email
- **Gmail** — automated, branch-specific notification emails

## Design notes
Small in scope but built the right way: the Switch node's routing logic is driven directly by the form's own salary field rather than a separate manual classification step, and the `appendOrUpdate` operation (matched on email) means re-submitting the form updates a person's existing record instead of creating a duplicate — useful if a staff member's salary bracket changes and they resubmit.

## Status
Fully functional and tested — verified end-to-end from form submission through to correct email routing based on salary bracket.

*Demo uses fictional test names and salary data, not real staff information.*
