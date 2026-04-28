#  Meeting Preparation Bot

This repository contains the configuration and documentation for the **Meeting Preparation Bot**, an automated n8n workflow designed to streamline your daily schedule by providing AI-generated briefings for your upcoming meetings.

---

##  Project Overview

The **Meeting Preparation Bot** automatically prepares and delivers a smart meeting briefing for your upcoming day. When triggered, it fetches all events scheduled for tomorrow from Google Calendar.

- **If meetings are found:** The workflow processes each event and uses AI to generate a concise, professional briefing including context, talking points, and expected outcomes. These are delivered as a styled **HTML** email digest.
- **If no meetings are found:** The workflow sends a *Free Day* notification, allowing you to plan for deep work or rest.

---

## Features

- **Automated Fetching:** Connects to Google Calendar to retrieve tomorrow's agenda.
- **AI-Powered Insights:** Uses Gemini AI to analyze meeting descriptions and attendees.
- **Smart Logic:** Branches between a full digest or a *Free Day* alert based on event availability.
- **Structured Delivery:** Sends a clean, visually structured **HTML** email via Gmail.

---

##  Setup Steps

### 1. Prepare Google Calendar

Ensure your Google Calendar is populated with upcoming events, including details like:
- Title & Time
- Description
- Attendees & Location

### 2. Connect Accounts in n8n

You will need to configure the following credentials within your n8n instance:
- **Google Calendar (OAuth2):** To read event data.
- **Gmail (OAuth2):** To send the final briefing email.
- **Gemini / Google AI **API**:** To generate the AI summaries.

### 3. Workflow Configuration

1. **Trigger:** Use a **Manual Trigger** for testing or a **Schedule (Cron) Trigger** for daily automation (e.g., 8:00 PM every evening).
2. **Fetch Events:** The Google Calendar node is configured to filter events for *Tomorrow.*
3. **Event Check:** An **IF Node** determines if the event count is greater than zero.
4. **Processing Loop:** A **Split In Batches** node ensures every meeting is processed individually by the AI.
5. **AI Generation:** An ****HTTP** Request** node calls the Gemini **API**:
    `**POST**: [https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent`](https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent`)
6. **Parsing & Formatting:** **Code Nodes** extract the AI text and wrap the content in **HTML**/**CSS** for the email body.

---

##  Workflow Nodes Breakdown

| Section | Description |
| :--- | :--- |
| **Fetch & Check** | Retrieves tomorrow’s events and calculates the total count. |
| **Decision Logic** | Evaluates if meetings exist; branches to *Digest* or *Free Day.* |
| **Free Day Notification** | Sends a simple email if the calendar is empty. |
| **Loop & Process** | Iterates through each specific meeting for personalized analysis. |
| **AI Summary & Parsing** | Generates professional briefings and structures the data for the email. |
| **Email Delivery** | Sends the final compiled HTML digest to the user. |

---

##  Technical Details

- **Platform:** [n8n](https://n8n.io/)
- **AI Model:** Gemini 1.5/2.5 Flash
- **Format:** **HTML**/**CSS** Email
- **Language:** JavaScript (for Code Nodes)

---

> **Note:** Ensure your Gemini **API** key has sufficient quota and that your Gmail settings allow for sending programmatic emails.
