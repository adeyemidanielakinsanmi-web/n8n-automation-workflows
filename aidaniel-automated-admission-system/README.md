# AIDANIEL University — Automated Admission Processing & Notification System

## Overview

This project is a workflow automation solution built with **n8n** to streamline the processing of student applications and the delivery of admission status notifications.

The workflow receives applicant information, generates an applicant reference ID, evaluates UTME and Pre-Degree scores against predefined rules, routes applicants into the appropriate category, and sends personalized email notifications through Gmail.

This project demonstrates how workflow automation can reduce repetitive administrative tasks and make business processes more consistent.

## Business Problem

Manual application processing can require staff to review applicant details, evaluate eligibility, assign reference numbers, classify applications, and send individual emails.

These repetitive activities can consume time and introduce avoidable inconsistencies.

## Solution

The workflow automates the main processing steps:

1. **Application intake:** Receives applicant data through a webhook.
2. **Applicant identification:** Generates an applicant reference ID.
3. **Score evaluation:** Applies defined UTME and Pre-Degree score thresholds.
4. **Admission classification:** Assigns one of three statuses.
5. **Workflow routing:** Directs each applicant to the appropriate branch using an n8n Switch node.
6. **Email notification:** Sends a personalized message through Gmail based on the assigned status.

## Admission Rules

The prototype uses the following configurable rules:

| UTME Score  | Pre-Degree Score | Status                    |
| ----------- | ---------------- | ------------------------- |
| 50 or above | 50 or above      | First Degree              |
| Below 50    | Below 50         | Not Admitted              |
| 50 or above | Below 50         | University School Diploma |
| Below 50    | 50 or above      | University School Diploma |

A score of exactly 50 counts as meeting the threshold.

## Technology Stack

* **n8n:** Workflow orchestration and conditional routing
* **Webhook:** Application data intake
* **Edit Fields:** Data preparation and applicant ID generation
* **Switch:** Admission classification and routing
* **Gmail:** Automated applicant notifications

## Expected Benefits

* Reduces repetitive application-processing tasks.
* Standardizes score-based classification.
* Automates applicant reference generation.
* Sends consistent, status-specific email notifications.
* Provides a foundation for extending the workflow with data storage, duplicate detection, error handling, and reporting.

## Workflow Architecture

Webhook → Edit Fields → Admission Logic / Switch → Status-Specific Gmail Notification

## Testing

The workflow was tested with sample applications covering all three outcomes. See [test-cases.md](test-cases.md) for inputs and results.

* Both scores at or above 50.
* Both scores below 50.
* One score at or above 50 and the other below 50.

Verify the assigned status, routing destination, applicant ID, and generated email for each test case.

**Testing status:** Update this section with the results of your actual execution tests before publishing.

## Security and Scope

This repository is a portfolio demonstration. Use fictional applicant data when testing or sharing screenshots.

Do not commit credentials, API keys, private applicant information, or unredacted production data. Exported n8n workflows should be reviewed and credentials removed before publication.

The admission decisions in this prototype are based on predefined score rules. They are not a substitute for an institution's approved admission policy or authorized human review.

## Future Improvements

* Persistent applicant records using Google Sheets or a database.
* Duplicate application detection.
* Error logging and failure notifications.
* Application status reporting and an administrative dashboard.
* Additional validation for missing or invalid scores.

## Author

Built as part of my workflow automation portfolio to demonstrate practical business process automation using n8n.

Interested in automating repetitive business operations, application processing, customer communication, and administrative workflows? Let's connect.
