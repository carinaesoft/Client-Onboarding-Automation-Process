# Automation Process — Customer Onboarding

## Introduction

This document outlines the analysis, design, and implementation of an automated customer onboarding process for a small business consulting firm. The primary goal of this project is to replace the current manual, time-consuming workflow with an automated system built primarily with **N8N**, **Airtable**, and **AI**. This will significantly reduce manual work.

### Background

A small business consulting firm currently handles customer onboarding manually. The process includes collecting client information, generating welcome packages, scheduling initial consultations, and sending invoices.

---

## Table of Contents

- [Current Process Analysis](#current-process-analysis)
- [Bottlenecks and Optimization Opportunities](#bottlenecks-and-optimization-opportunities)
- [Proposed Process Flowchart](#proposed-process-flowchart)
- [Database Design (Airtable)](#database-design-airtable)
- [Automation Scenarios](#automation-scenarios)
  - [Workflow: Client Onboarding (From Google Form)](#workflow-client-onboarding-from-google-form)
  - [Workflow: Invoicing and Consultation Scheduling](#workflow-invoicing-and-consultation-scheduling)
- [Error Handling and System Reliability](#error-handling-and-system-reliability)
- [AI Integration](#ai-integration)
- [Configuration Instructions](#configuration-instructions)

---

## Current Process Analysis

The existing onboarding process is entirely manual, relying on staff to perform a series of repetitive tasks. The key steps are:

1. **Data Collection:** A client fills out a Google Form with their basic information and business needs.
2. **Data Transfer:** Staff manually transcribes the information from the Google Form submission into internal spreadsheets.
3. **Welcome Package Generation:** A welcome package is manually created in Microsoft Word for each new client.
4. **Consultation Scheduling:** The initial consultation is scheduled through a series of back-and-forth emails with the client to find a suitable time.
5. **Invoicing:** Invoices are manually generated and sent to the client using a separate software or template.

### Current Process Flowchart

```
Client Fills Google Form
         │
         ▼
    ◇ Manual Work ◇
   ╱    │    │    ╲
  ▼     ▼    ▼     ▼
Copy   Create  Schedule  Manually
Data   Welcome  Consultation  Generate
to     Pack    via Email   Invoice
Spreadsheet  in Word
```

---

## Bottlenecks and Optimization Opportunities

The current process contains several significant bottlenecks that hinder scalability and efficiency.

### Bottleneck 1: Manual Data Entry

- **Problem:** Transcribing data from Google Forms to spreadsheets is time-consuming, creates a delay between client submission and action.
- **Optimization:** Automate the data capture from the form directly into a centralized database (Airtable).

### Bottleneck 2: Manual Document Creation

- **Problem:** Creating welcome packages manually is not scalable and consumes valuable staff time.
- **Optimization:** Implement dynamic PDF generation using a template populated with client data, ensuring consistency and personalization.

### Bottleneck 3: Email-Based Scheduling

- **Problem:** The "ping-pong" scheduling via email is inefficient, often delays the first consultation, and provides a poor user experience.
- **Optimization:** Utilize a dedicated scheduling tool (e.g., Calendly) to allow clients to book a slot instantly, with the event being automatically logged.

### Bottleneck 4: Manual Invoicing

- **Problem:** Manual invoice generation risks incorrect data, payment delays, and difficulty in tracking payment statuses.
- **Optimization:** Automate the generation and delivery of invoices triggered by specific events (e.g., a scheduled consultation), complete with tracking.

---

## Proposed Process Flowchart

The automated workflow is split into two stages:

### Stage 1: Data Capture & Initial Contact

```
Client Fills Google Form
         │
         ▼
   ┌─────────────┐
   │  N8N: Trigger │
   └──────┬──────┘
          ▼
    ◇ N8N: AI-Powered  ◇
    ◇ Needs Analysis    ◇
          │
          ▼
┌─────────────────────────────┐
│ N8N: Create/Update Client   │
│         in Airtable         │
└────────────┬────────────────┘
             ▼
┌─────────────────────────────────┐
│ N8N: Generate Personalized      │
│         Welcome Pack            │
└────────────┬────────────────────┘
             ▼
┌─────────────────────────────────────┐
│ N8N: Send Welcome Email with Pack   │
│         & Scheduling Link           │
└─────────────────────────────────────┘
```

### Stage 2: Scheduling & Invoicing

```
Client Books Slot in Calendly
             │
             ▼
   ┌───────────────────┐
   │ N8N: Webhook       │
   │     Listener       │
   └────────┬──────────┘
            ▼
┌───────────────────────────┐
│ N8N: Log Consultation     │
│       in Airtable         │
└───────────┬───────────────┘
            ▼
┌───────────────────────────┐
│ N8N: Update Client Status │
└───────────┬───────────────┘
            ▼
┌───────────────────────────┐
│ N8N: Generate & Send      │
│       Invoice             │
└───────────┬───────────────┘
            ▼
┌───────────────────────────┐
│ N8N: Log Invoice          │
│       in Airtable         │
└───────────────────────────┘
```

---

## Database Design (Airtable)

Airtable serves as the central database, or the "single source of truth," for all customer-related information. A well-structured base is critical for the reliability and efficiency of the entire automation workflow. The database consists of three interconnected tables: `Klienci`, `Konsultacje`, and `Faktury`.

### `Klienci` Table

This is the central table, storing all primary information about each client.

| Field Name | Field Type | Description |
|---|---|---|
| `ClientID` | Autonumber (PK) | A unique, auto-incrementing identifier for each client. |
| `FullName` | Single line text | The client's full name. |
| `Email` | Email | The client's primary email address, used for communication. |
| `PhoneNumber` | Phone number | The client's contact phone number. |
| `CompanyName` | Single line text | The name of the client's company. |
| `FormResponses` | Long text | Raw data submitted by the client via the Google Form. |
| `AINeedsAnalysis` | Long text | An AI-generated summary and analysis of the client's needs. |
| `SuggestedPackage` | Single select | The AI-suggested service package (e.g., Basic, Pro, Enterprise). |
| `Status` | Single select | The client's current stage in the process (e.g., New, Consultation Scheduled). |
| `Consultations` | Link to Consultations | A link to all consultation records for this client. |
| `Invoices` | Link to Invoices | A link to all invoice records for this client. |

### `Konsultacje` Table

This table tracks all scheduled meetings and consultations with clients.

| Field Name | Field Type | Description |
|---|---|---|
| `ConsultationID` | Autonumber (PK) | A unique identifier for each consultation. |
| `Client` | Link to Clients (FK) | A required link to the client for whom the consultation is scheduled. |
| `DateTime` | Date | The scheduled date and time of the consultation. |
| `Status` | Single select | The status of the consultation (e.g., Scheduled, Completed). |
| `AIGuide` | Long text | A personalized preparation guide for the client, generated by AI. |

### `Faktury` Table

This table stores information about all invoices generated for clients.

| Field Name | Field Type | Description |
|---|---|---|
| `InvoiceID` | Autonumber (PK) | A unique, internal identifier for each invoice. |
| `InvoiceNumber` | Formula | The official, formatted invoice number (e.g., `INV/2025/08/1`). |
| `Client` | Link to Clients (FK) | A required link to the client for whom the invoice was issued. |
| `IssueDate` | Date | The date the invoice was issued. |
| `NetAmount` | Currency | The net amount of the invoice. |
| `GrossAmount` | Currency | The gross amount of the invoice (including taxes). |
| `Status` | Single select | The payment status (e.g., Issued, Paid, Overdue). |
| `PDFLink` | URL | A clickable link to the PDF file of the invoice. |

### Entity Relationship Diagram

```
┌─────────────────────────────┐
│           Klienci            │
├──────────┬──────────────────┤
│ int      │ ClientID     (PK)│
│ string   │ Imie_i_Nazwisko  │
│ string   │ Email            │
│ string   │ Telefon          │
│ string   │ Nazwa_Firmy      │
│ string   │ Odpowiedzi_z_Form│
│ string   │ Analiza_Potrzeb  │
│ string   │ Sugerowany_Pakiet│
│ string   │ Status           │
└────┬─────────────┬──────────┘
     │ posiada     │ posiada
     ▼             ▼
┌──────────────┐  ┌──────────────────┐
│ Konsultacje  │  │     Faktury      │
├──────────────┤  ├──────────────────┤
│ConsultationID│  │ InvoiceID    (PK)│
│Klient_ID (FK)│  │ Numer_Faktury    │
│Data_i_Godzina│  │ Klient_ID   (FK) │
│Status        │  │ Data_Wystawienia │
│Przewodnik_AI │  │ Kwota_Netto      │
└──────────────┘  │ Kwota_Brutto     │
                  │ Status           │
                  │ Link_do_PDF      │
                  └──────────────────┘
```

---

## Automation Scenarios

This chapter details the two core automation workflows built in N8N that power the entire client onboarding and invoicing process. Each workflow is designed to be modular, robust, and handle potential errors gracefully.

### Workflow: Client Onboarding (From Google Form)

This workflow is the entry point for every new client. Its primary goal is to capture data from the initial contact form, enrich it using artificial intelligence, and send a personalized welcome email that encourages the client to schedule a consultation.

#### Logic and Step-by-Step Execution

1. **Google Sheets Trigger:** The process starts when a new row is added to the connected Google Sheet, which is populated by submissions from the Google Form.

2. **If:** A simple validation step checks if the `email` field in the submitted form is not empty. If it is, the workflow stops to prevent processing incomplete data.

3. **Airtable (Upsert):** The data from the form is used to create or update a client record in the `Klienci` table in Airtable. The operation is an **upsert** based on the client's email address. If a client with that email already exists, their record is updated with the new information; otherwise, a new record is created.

4. **Remove Duplicates:** This is a technical step to handle the specific way the Airtable node can sometimes return two items on an update. It ensures the rest of the workflow only runs once for each client.

5. **AI Agent (Key Node):** This is the core of the data enrichment process. The node sends the client's form responses to an AI model (OpenAI's GPT) with a precisely engineered prompt to generate a structured JSON object.
   - **Error Handling:** The node is configured with `Continue (using error output)`. This means if the AI call fails for any reason (API outage, invalid response), the workflow does not stop but instead routes the data to a separate **fallback path**.
   - **AI Prompt Configuration:** The agent's instructions are divided into a **System Message** (defining its role and rules) and a **User Prompt** (providing the specific data and task).

6. **Fallback Path (If AI Fails):**
   - **Generic output (Set node):** If the AI Agent fails, this node creates a predefined, generic JSON object with safe, neutral content for the welcome email.
   - **Update table with generic output:** This updates the client's record in Airtable with the generic data.
   - **Mail to admin after AI fail:** Crucially, it sends an immediate alert to the system administrator, informing them of the failure and which client was affected, allowing for manual follow-up.

7. **Update table with AI output:** On the success path, this node updates the client's Airtable record with the rich, personalized data generated by the AI.

8. **Gmail (Final Email):** The final step merges the data (either from the AI or the generic fallback) into a professional HTML email template and sends it to the client. This email contains the personalized welcome, the preparation guide, and a clear call-to-action to schedule a meeting.

---

### Workflow: Invoicing and Consultation Scheduling

This workflow triggers after a client has scheduled a consultation via Calendly. Its purpose is to formalize the meeting in the database, automatically generate a professional PDF invoice, and deliver it to the client.

#### Logic and Step-by-Step Execution

1. **Calendly Trigger:** The workflow starts the moment a client successfully creates an event in Calendly (`invitee.created`).

2. **Poszukanie usera (Airtable Search):** The system takes the client's email from the Calendly data and searches for their record in the `Klienci` table in Airtable.

3. **If:** This node checks if a client record was found.
   - **If false:** The client scheduled a meeting but isn't in the database. An email is sent to them explaining the situation and providing a link to the initial contact form. The workflow stops here for this path.
   - **If true:** The workflow proceeds down the main path.

4. **Dodanie konsultacji (Airtable Create):** A new record is created in the Consultations table, linking it to the found client record and populating it with the date and time from Calendly.

5. **Dodanie faktury do bazy (Airtable Create):** A new record is created in the `Faktury` table. It's linked to the client, and the net/gross amounts are set. The crucial `InvoiceNumber` field is not set here; it's generated automatically by a Formula field within Airtable, ensuring it is always unique and correctly formatted.

6. **Create HTML (Set node):** This node takes the data from the newly created invoice record (including the automatically generated `InvoiceNumber`) and the client's data and inserts it into a static HTML template for the invoice.

7. **Convert HTML to File & Convert File to PDF:** These two nodes prepare the HTML and send it to the ConvertAPI service, which returns a professionally rendered PDF file.

8. **Google Drive (Upload):** The generated PDF is uploaded to a designated "Invoices" folder in Google Drive for archival purposes.

9. **Update Linku (Airtable Update):** The shareable link to the newly uploaded PDF in Google Drive is saved back to the `Link do PDF` field in the corresponding invoice record in Airtable.

10. **Zmiana statusu klienta (Airtable Update):** The client's status in the Clients table is updated to "Zafakturowany" (Invoiced).

11. **Pobierz plik faktury (Google Drive Download):** The workflow downloads the binary data of the PDF file from Google Drive.

12. **Gmail (Send Invoice):** The final node sends an email to the client with a polite message and attaches the downloaded PDF invoice.

---

## Error Handling and System Reliability

A robust automation system is defined not just by how it works when everything is correct, but by how it behaves when things go wrong. This project implements a two-layered approach to error handling to ensure maximum reliability: a global "catch-all" handler for unexpected failures and a specific, logical fallback path for predictable issues with external services.

### Global Error Handling Workflow

The primary safety net for the entire system is a dedicated **Global Error Handler** workflow. Its sole purpose is to prevent any workflow from failing silently. Both the *Client Onboarding* and *Invoicing* workflows are configured in their settings to use this handler as their `Error Workflow`.

#### Logic and Step-by-Step Execution

1. **Error Trigger:** This is a special trigger that activates automatically whenever an unhandled error occurs in any connected workflow. It receives a detailed JSON object containing all context about the failure.

2. **Gmail (Send Alert):** The node immediately formats the received error data into a user-friendly HTML email and sends it to the system administrator. The email alert is designed for rapid diagnostics and includes:
   - The name of the workflow that failed.
   - The specific node that caused the error.
   - A clear error message.
   - A timestamp of the failure.
   - A direct link to the failed execution in the N8N interface for immediate debugging.
   - A collapsible section containing the full technical stack trace for in-depth analysis.

This global handler ensures that no critical failure goes unnoticed, allowing for swift intervention and resolution.

---

## AI Integration

The AI Integration elevates the process from simple data transfer to intelligent analysis and personalization. By leveraging a Large Language Model (OpenAI's GPT), the system can understand the client's needs, suggest appropriate actions, and craft personalized communication automatically.

### Prompt Engineering Strategy

The reliability of an AI-driven automation hinges entirely on the quality and structure of its prompts. The strategy chosen here is based on two core principles:

1. **Structured Output (JSON):** The most critical requirement is forcing the AI to respond in a predictable, machine-readable format. A raw text response would be unreliable for automation. Therefore, the AI is explicitly instructed to respond only with a valid JSON object. This output is then validated by N8N's `Structured Output Parser`, ensuring that the data can be reliably mapped to Airtable fields and email templates in subsequent steps.

2. **Defensive Design:** The prompt includes instructions for edge cases, such as what to do if a client's description of their needs is empty or unclear. By telling the AI to suggest "Basic" and note that clarification is needed, we prevent errors and ensure the workflow can always proceed gracefully.

### AI Agent Prompts

The following are the exact prompts used in the AI Agent node in the Client Onboarding workflow.

#### System Message (The AI's "Constitution")

This message sets the context and defines the strict rules for the AI's behavior and output format.

```
Jesteś ekspertem od analizy procesów biznesowych w firmie konsultingowej.
Twoim zadaniem jest analiza zapytań od nowych klientów.

Zawsze odpowiadaj TYLKO i WYŁĄCZNIE poprawnym obiektem JSON.
Nie dodawaj żadnych wstępów, wyjaśnień ani formatowania markdown (np. ```json).

Obiekt JSON musi zawierać cztery klucze:
"Analiza_Potrzeb_AI", "Sugerowany_Pakiet", "Przewodnik_AI" oraz "Personalizacja_Wiadomosci".

Pole "Przewodnik_AI" musi być wygenerowane jako gotowy do wklejenia fragment kodu HTML
w formacie listy uporządkowanej (np. <ol><li>Punkt 1</li><li>Punkt 2</li></ol>).

Pole "Personalizacja_Wiadomosci" ma być krótką (2-3 zdania) wiadomością powitalną
w języku polskim. Musi być napisana w imieniu firmy (używając formy "my", np.
"rozumiemy", "pomożemy") i zwracać się do klienta po imieniu.
Nigdy nie proponuj w tej wiadomości następnych kroków. Te podasz w przewodniku AI.

Do sugestii pakietu użyj kryteriów:
- "Basic" — Proste zadania, automatyzacja jednego procesu.
- "Pro" — Optymalizacja kilkuetapowego procesu, integracja systemów.
- "Enterprise" — Złożone, strategiczne projekty, wdrożenia AI.

Jeśli opis potrzeb klienta jest pusty lub niejasny, w polu "Analiza_Potrzeb_AI" wpisz
"Wymagany kontakt w celu doprecyzowania potrzeb.", a jako pakiet zasugeruj "Basic".
```

#### User Prompt (The AI's "Task")

This prompt provides the specific client data for each execution and asks the AI to perform the analysis.

```
Przeanalizuj poniższe zapytanie od klienta i wygeneruj odpowiedź JSON.

Dane klienta:
Imię: {{ $json.fields["Imię i Nazwisko"] }}
Opis potrzeb: {{ $json.fields["Odpowiedzi z Formularza"] }}

Na podstawie danych, przygotuj:
1. Podsumowanie potrzeb klienta w punktach do pola "Analiza_Potrzeb_AI".
2. Sugerowany pakiet ("Basic", "Pro" lub "Enterprise") do pola "Sugerowany_Pakiet".
3. 3-punktowy poradnik dla klienta, jak przygotować się do konsultacji,
   w formacie listy HTML (<ol>) do pola "Przewodnik_AI".
4. Spersonalizowaną wiadomość powitalną, napisaną w imieniu firmy,
   do pola "Personalizacja_Wiadomosci".

Pamiętaj, aby zwrócić tylko surowy obiekt JSON.
```

---

## Configuration Instructions

This chapter provides a step-by-step guide for deploying and configuring the entire automation solution. Following these instructions will allow you to connect your own accounts and services to the N8N workflows.

### Prerequisites

Before you begin, ensure you have active accounts for the following services:

- **N8N**
- **Airtable**
- **Google Account** (for Google Sheets and Gmail)
- **Calendly**
- **OpenAI** (with API access enabled)
- **ConvertAPI** (for PDF generation)

### N8N Credentials Setup

The first step is to securely connect your service accounts to N8N. In your N8N instance, navigate to the **Credentials** section from the left-hand menu and add the following credentials.

#### Google (OAuth2)

This single credential will be used for both the Google Sheets trigger and the Gmail nodes.

1. Click **Add credential**.
2. Search for **Google OAuth2 API** and select it.
3. Follow the on-screen instructions in the pop-up window to sign in with your Google account and grant N8N the necessary permissions.

#### Airtable (Personal Access Token)

This allows N8N to read and write data in your Airtable base.

1. Go to your Airtable developer hub: <https://airtable.com/create/tokens>
2. Click **Create new token**.
3. Give the token a name (e.g., "N8N Integration").
4. Grant the token required scopes.
5. Select the Airtable base this token should have access to.
6. Click **Create token**, copy the generated token string.
7. In N8N, add a new credential, search for **Airtable API**, and paste the token you just copied.

#### Calendly (OAuth2)

This allows N8N to trigger the invoicing workflow when a new event is scheduled.

1. In N8N, add a new credential and search for **Calendly OAuth2 API**.
2. Follow the on-screen instructions to connect your Calendly account.

#### OpenAI

This allows the AI Agent to perform the needs analysis.

1. Go to your OpenAI API keys page: <https://platform.openai.com/api-keys>
2. Click **Create new secret key**, give it a name, and copy the key.
3. In N8N, add a new credential, search for **OpenAI API**, and paste your secret key.

#### ConvertAPI (Bearer Token)

This is used by the HTTP Request node to generate PDFs.

1. After signing up for ConvertAPI, find your Secret in the account dashboard: <https://www.convertapi.com/a>
2. In N8N, add a new credential and search for **Header Auth**.
3. Set the **Name** to `Authorization`.
4. Set the **Value** to `Bearer YOUR_SECRET` (replace `YOUR_SECRET` with the secret you copied from ConvertAPI).

### Airtable Base Setup

To get started quickly, you can duplicate the pre-configured Airtable base used for this project:

> <https://airtable.com/appsrHARKLs8eeVIU/shrwyiW2IIqACG1Sk>

Alternatively, you can manually recreate the base structure (tables, fields, and relationships) by following the detailed documentation in the [Database Design](#database-design-airtable) section above.
