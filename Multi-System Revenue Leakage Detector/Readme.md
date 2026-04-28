# Multi-System Revenue Leakage Detector

This n8n workflow automatically detects revenue leakage by cross-referencing expected revenue from **Salesforce** with actual billing data stored in **Google Sheets**. It leverages **Google Gemini AI** to analyze discrepancies and provide actionable insights.

##  Overview

The **Multi-System Revenue Leakage Detector** ensures finance and operations teams can identify missing revenue without manual audits. It fetches *Closed Won* deals, compares them against billing records, and uses AI to classify severity and root causes before alerting the team via Slack.

### Key Features

- **Automated Data Reconciliation:** Syncs Salesforce and Google Sheets data seamlessly.
    
- **AI-Powered Analysis:** Uses Google Gemini to interpret gaps and suggest resolutions.
    
- **Batch Processing:** Built-in rate control to handle large datasets without hitting **API** limits.
    
- **Real-time Alerts:** Direct notifications to Slack for immediate visibility.
    
- **Audit Trail:** Automatically updates Salesforce records with AI-generated audit notes.

## Workflow Structure

### 1\. Revenue Intake & Opportunity Fetch

The process begins by retrieving all opportunities from **Salesforce** where the stage is set to Closed Won.

### 2\. Batch Processing & Rate Control

To ensure stability, the workflow uses a **Split in Batches** node and a **Wait** node. This prevents hitting **API** rate limits during high-volume processing.

### 3\. Billing Lookup & Revenue Comparison

The workflow fetches the corresponding billing record from **Google Sheets** (using Deal ID or Account ID) and calculates the mathematical difference:

> **Gap** = Expected Revenue (Salesforce) - Actual Billed (Sheets)

### 4\. AI Revenue Gap Analysis & Parsing

Data is sent to **Google Gemini AI** to:

- Classify the **Severity** (Low, Medium, High).
    
- Identify the likely **Root Cause**.
    
- Provide **Recommended Actions**.
    
- The output is then parsed into structured **JSON** for downstream nodes.

### 5\. Gap Validation & Filtering

An **IF Condition** checks if the revenue gap is greater than zero. If no gap is found, the workflow ends for that record to avoid false alarms.

### 6\. Alerting & Logging

- **Slack:** Sends a formatted message containing the deal name, gap amount, and AI recommendations.
    
- **Salesforce:** Updates the Opportunity description with a detailed audit discrepancy note.

##  Setup Instructions

### Prerequisites

1. **n8n Instance:** A running version of n8n.
    
2. ****API** Credentials:**
    
    *   **Salesforce:** OAuth2 or Access Token.
    
    *   **Google Sheets:** Service Account or OAuth2.
    
    *   **Google Gemini:** **API** Key.
    
    *   **Slack:** Bot Token.

### Installation

1. **Copy the Workflow:** Copy the **JSON** code from this workflow.
    
2. **Import to n8n:** In your n8n canvas, press Ctrl+V (or Cmd+V) to paste the nodes.
    
3. **Configure Credentials:** Open each node (Salesforce, Google Sheets, Gemini, Slack) and select your pre-configured credentials.
    
4. **Set Trigger:** Configure the **Manual Trigger** for testing or add a **Schedule** node to run the audit daily/weekly.

##  Node List

- **Salesforce Node:** Fetch *Closed Won* opportunities.
    
- **Split In Batches:** Manage execution flow.
    
- **Wait Node:** **API** rate limit delay.
    
- **Google Sheets Node:** Lookup billing data.
    
- **Code Node:** Compare revenue totals.
    
- **Google Gemini Node:** Analyze gap and generate insights.
    
- **Slack Node:** Post alerts to the finance channel.
    
- **Salesforce Update Node:** Log audit notes to the **CRM**.

> **Note:** Ensure that the Deal IDs in Salesforce match the identifiers used in your Google Sheets billing tracker for accurate lookups.