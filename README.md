# Mail Automation Workflow

![Workflow Diagram]()

This n8n workflow automates the process of sending personalized emails to a list of recipients using data from a Google Sheet, with support for multiple email templates and status tracking.

## Overview

The workflow reads recipient data from a Google Sheet, filters out already sent emails, selects a random email template, personalizes the message, sends it via Gmail, and updates the sheet with the send status.

## Features

- **Google Sheets Integration**: Reads recipient data and email templates from Google Sheets
- **Email Personalization**: Replaces placeholders (like `[name]`) with actual recipient data
- **Template Randomization**: Selects a random email template from a predefined list
- **Status Tracking**: Updates the Google Sheet with send status and timestamp
- **Batch Processing**: Processes emails in batches to manage rate limits
- **Gmail Integration**: Sends emails using Gmail OAuth2 authentication

## Prerequisites

- An n8n instance
- Google Sheets OAuth2 credentials
- Gmail OAuth2 credentials
- Two Google Sheets:
  - **Sheet 1**: Contains recipient data (columns: `Name`, `Email`, `Send Status`, `Time`)
  - **Sheet 2**: Contains email templates (columns: `Subject`, `Body`)

## Workflow Nodes

1. **Manual Trigger**: Initiates the workflow execution
2. **Get row(s) in sheet**: Reads recipient data from the first Google Sheet
3. **If**: Filters recipients where `Send Status` is not "SENT"
4. **Loop Over Items**: Processes each recipient in batches
5. **Get row(s) in sheet1**: Reads email templates from the second Google Sheet
6. **Code in JavaScript**: Selects a random template and converts body to HTML
7. **Merge**: Combines recipient data with the selected template
8. **Edit Fields**: Personalizes the email body (replaces `[name]` placeholder)
9. **Send a message**: Sends the personalized email via Gmail
10. **Merge1**: Combines data for status update
11. **Append or update row in sheet**: Updates the Google Sheet with send status and timestamp
12. **Wait**: Adds a delay between email sends to manage rate limits

## Setup Instructions

1. **Import the Workflow**:
   - In your n8n instance, go to the Workflows page
   - Click "Import" and paste the JSON workflow definition

2. **Configure Credentials**:
   - Add your Google Sheets OAuth2 credentials
   - Add your Gmail OAuth2 credentials
   - Link these credentials to the respective nodes

3. **Update Google Sheet IDs**:
   - Replace the `documentId` in the Google Sheets nodes with your actual Google Sheet IDs
   - Ensure the sheet names/gids match your sheets

4. **Prepare Google Sheets**:
   - **Sheet 1 (Recipients)**: Create columns `Name`, `Email`, `Send Status`, `Time`
   - **Sheet 2 (Templates)**: Create columns `Subject`, `Body`

5. **Test the Workflow**:
   - Execute the workflow manually to ensure all connections work correctly

## Usage

1. Add recipient data to your first Google Sheet
2. Add email templates to your second Google Sheet
3. Execute the workflow manually or set up a trigger
4. Monitor the `Send Status` column in the first sheet to track sent emails

## Important Notes

- The workflow uses OAuth2 for both Google Sheets and Gmail integration
- The "Wait" node helps manage Gmail's sending limits
- The workflow only processes recipients with a `Send Status` other than "SENT"
- Email body text is converted to HTML format (line breaks become `<br>` tags)
- The workflow updates the Google Sheet with the Gmail label ID as the send status

## Troubleshooting

- Ensure all required columns exist in your Google Sheets
- Verify that your OAuth2 credentials have the necessary permissions
- Check that the `[name]` placeholder exists in your email templates if personalization is desired
- Monitor Gmail's sending limits to avoid quota issues
