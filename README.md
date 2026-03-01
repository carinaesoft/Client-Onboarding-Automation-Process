# Customer Onboarding Automation

Automated customer onboarding and invoicing pipeline for a small business consulting firm. Replaces manual data entry, document generation, scheduling, and invoicing with an end-to-end workflow powered by **N8N**, **Airtable**, and **OpenAI**.

## What It Does

A new client fills out a Google Form → the system automatically:

1. Creates/updates their record in Airtable
2. Runs AI-powered needs analysis (package suggestion, personalized welcome)
3. Generates and sends a welcome email with a Calendly scheduling link
4. On booking — logs the consultation, generates a PDF invoice, uploads it to Google Drive, and emails it to the client

Zero manual steps. Full error handling with admin alerts on failure.

## Workflows

### Client Onboarding (Google Form → Welcome Email)

<img width="1046" height="417" alt="Client Onboarding workflow" src="https://github.com/user-attachments/assets/10015991-09da-4eb9-83a7-c46ec79fd0b9" />

**Trigger:** New Google Form submission  
**Output:** AI-enriched client record in Airtable + personalized welcome email with scheduling link  
**Fallback:** If AI fails → generic welcome + admin alert

### Invoicing & Scheduling (Calendly → PDF Invoice)

<img width="1164" height="451" alt="Invoicing workflow" src="https://github.com/user-attachments/assets/7018188a-9a78-4da8-b1c3-87b5e9d7e6bd" />

**Trigger:** Client books a Calendly slot  
**Output:** Consultation logged, PDF invoice generated (ConvertAPI), archived in Google Drive, emailed to client  
**Edge case:** Unknown client → redirect email with form link

## Tech Stack

| Layer | Tools |
|---|---|
| Orchestration | N8N (self-hosted) |
| Database | Airtable (3 tables: Klienci, Konsultacje, Faktury) |
| AI | OpenAI GPT — structured JSON output with fallback path |
| Scheduling | Calendly (OAuth2 webhook) |
| PDF Generation | ConvertAPI (HTML → PDF) |
| Storage | Google Drive |
| Email | Gmail (OAuth2) |

## Quick Start

1. Import the N8N workflow JSON files into your instance
2. Set up credentials (Google OAuth2, Airtable, Calendly, OpenAI, ConvertAPI) — see [docs/setup.md](docs/setup.md)
3. Duplicate the [Airtable base template](https://airtable.com/appsrHARKLs8eeVIU/shrwyiW2IIqACG1Sk) or create tables manually per the schema in [docs/documentation.md](docs/documentation.md)
4. Connect your Google Form to the trigger sheet
5. Done — new submissions flow through automatically

## Documentation

Full technical documentation (process analysis, database schema, workflow logic, AI prompts, configuration guide) lives in:

→ **[docs/documentation.md](docs/documentation.md)**

## Project Structure

```
├── README.md
├── docs/
│   ├── documentation.md    # Full technical documentation
└── workflows/
    ├── client-onboarding.json
    ├── invoicing.json
    └── error-handler.json
```

## Author

**Bartosz Błażewicz** — [blazewicz.dev](https://blazewicz.dev)
