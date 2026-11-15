# PayPal Invoice Reminder Automation

An automated invoice tracking and payment reminder workflow built using **Make.com, PayPal REST API, and Google Sheets**.

This automation retrieves invoices from PayPal based on specified invoice amount and date-range criteria, records the invoice information in Google Sheets, and sends payment reminders to the corresponding recipients through PayPal.

## Overview

Managing unpaid invoices manually can be time-consuming. This workflow automates invoice retrieval, record keeping, and payment reminder delivery using API integrations.

### Key Features

- **Automated Invoice Retrieval:** Fetches invoices using the PayPal Invoicing API.
- **Custom Invoice Filtering:** Searches invoices using configurable amount and invoice date ranges.
- **Google Sheets Integration:** Automatically records invoice information in a spreadsheet.
- **Automated Payment Reminders:** Sends payment reminders through PayPal.
- **Recipient Identification:** Extracts recipient names and email addresses from invoice data.
- **Invoice Tracking:** Stores invoice IDs and statuses for reference.

## Tech Stack

| Technology | Purpose |
|---|---|
| Make.com | Workflow automation |
| PayPal REST API | Invoice retrieval and reminders |
| Google Sheets | Invoice record management |
| HTTP Module | API communication |

## Workflow Architecture

The automation consists of three main modules:

### 1. PayPal – Search Invoices

Uses the PayPal Invoicing API to retrieve invoices.

**API Endpoint:**
`POST /v2/invoicing/search-invoices`

The search request supports:
- Invoice amount range
- Invoice date range
- PayPal sandbox environment

The current blueprint is configured to search for invoices with an amount of **$50.00**, within the configured date range.

### 2. Google Sheets – Record Invoice Details

The retrieved invoice information is automatically added to Google Sheets.

The following information is stored:

| Column | Data |
|---|---|
| A | Recipient Name |
| B | Invoice Status |
| C | Payment Term |
| D | Recipient Email |
| E | Invoice ID |

This creates a centralized record of the retrieved invoices.

### 3. PayPal – Send Invoice Reminder

The workflow sends a payment reminder using the PayPal Invoicing API.

**API Endpoint:**
`POST /v2/invoicing/invoices/{invoice_id}/remind`

The request includes:

- `send_to_invoicer`: true
- `additional_recipients`: Invoice recipient's email address

PayPal handles the reminder delivery.

## Setup Instructions

### Prerequisites

Before importing the workflow, you need:

- A Make.com account.
- A PayPal Developer account.
- PayPal API credentials.
- A Google account.
- A Google Spreadsheet with the required columns.

### Step 1: Configure PayPal API Access

1. Create an application in the [PayPal Developer Dashboard](https://developer.paypal.com/dashboard/).
2. Obtain your Client ID and Client Secret.
3. Generate an OAuth 2.0 access token.
4. Configure authentication in Make.com's HTTP modules.
5. Use the appropriate PayPal API environment.

### Step 2: Configure Google Sheets

1. Create a Google Spreadsheet.
2. Create a worksheet named `Sheet1`.
3. Add the following column headers:

   - Name
   - Status
   - Issue Date
   - Email
   - ID

4. Connect your Google account to Make.com.
5. Select the spreadsheet in the Google Sheets module.

### Step 3: Import the Make.com Blueprint

1. Download the blueprint JSON from this repository.
2. Open your Make.com account.
3. Create a new scenario.
4. Import the blueprint.
5. Reconfigure the PayPal and Google Sheets connections.
6. Verify the API endpoints, authentication headers, and invoice search parameters.

### Step 4: Configure Invoice Search Parameters

Customize the invoice search request according to your requirements.

You can modify:
- Minimum invoice amount.
- Maximum invoice amount.
- Invoice start date.
- Invoice end date.

### Step 5: Test the Automation

Run the scenario using PayPal sandbox credentials.

Verify that:
- PayPal returns the expected invoices.
- Google Sheets receives the correct invoice information.
- The reminder API request executes successfully.

## Important Notes

- The provided blueprint uses the **PayPal Sandbox environment**.
- The current invoice search request filters by amount and date range. It does not explicitly filter by unpaid status.
- The reminder module sends requests for the invoices returned by the search module. Ensure that only eligible invoices are selected before enabling reminders.
- Configure valid OAuth credentials and Google connections before running the scenario.
- Test the workflow thoroughly in sandbox mode before switching to production.

## Security

Never upload API access tokens, Client Secrets, passwords, or private connection credentials to a public GitHub repository.

Store credentials securely using Make.com's connection management and authentication features.

## Project Purpose

This project demonstrates how Make.com can integrate external REST APIs with Google Sheets to automate financial administration tasks, reduce repetitive manual work, and streamline invoice follow-ups.

---

**Built with:** Make.com | PayPal REST API | Google Sheets
