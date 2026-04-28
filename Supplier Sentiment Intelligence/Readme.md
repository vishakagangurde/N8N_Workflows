# Supplier Sentiment Intelligence

This n8n workflow automatically monitors incoming supplier emails and transforms them into actionable insights using AI. It analyzes sentiment, detects relationship risks, and generates professional engagement strategies to ensure proactive supplier management.

##  Overview
The workflow monitors a Gmail inbox, filters out noise (newsletters/spam), and uses Gemini AI to evaluate the tone and urgency of supplier communications. If a high-risk interaction is detected, the system applies a "cool-off" delay, suggests a response strategy, alerts the team via Slack, and logs all data to Google Sheets.

##  Setup Steps
- **Connect Accounts:** Configure credentials for Gmail, Gemini AI, Slack, and Google Sheets within your n8n instance.
- **Prepare Data Storage:** Create a Google Sheet with columns for Date, Sender, Sentiment, Risk Level, and Strategy to store interaction history.
- **Add Email Trigger:** Set up the Gmail Trigger node to monitor your inbox for new supplier messages.
- **Filter & Structure Data:** Use a Filter node to remove irrelevant content and a Set node to organize the sender, subject, and body.
- **Add AI Analysis:** Configure the Gemini AI node to analyze the text for sentiment, intent, and urgency.
- **Detect Risk:** Implement logic to identify "At Risk" relationships based on negative sentiment or high urgency.
- **Generate Strategy:** Use AI to draft a professional response and a recommended action plan (e.g., "Schedule a call").
- **Notify & Log:** Map the output to a Slack node for real-time alerts and an Append to Sheet node for tracking.

##  Workflow Components
1. **Email Intake & Filtering**
   - *Gmail Trigger:* Watches for incoming mail.
   - *Filter Valid Supplier Emails:* Uses logic to exclude newsletters, auto-replies, and unsubscribe-related content.
2. **AI Analysis & Insight Extraction**
   - *Structure Email Data:* Formats the raw email into clean fields for the AI.
   - *Analyze Sentiment & Intent:* Uses LLMs to determine if the supplier is frustrated, inquiring, or urgent.
   - *Extract & Normalize Insights:* Converts AI prose into structured data fields (Sentiment Score, Risk Level).
3. **Risk Detection & Delay Handling**
   - *Detect Supplier Risk:* An IF node that branches based on the "Risk" flag.
   - *Cool-off Period:* Applies a timed delay for high-risk emails to allow for internal review before automated actions occur.
4. **Strategy & Communication**
   - *Generate Engagement Strategy:* AI produces a specific action plan and a drafted reply.
   - *Build Slack Alert Message:* Formats a concise summary for internal teams.
   - *Send Alert to Team:* Dispatches the notification to a dedicated Slack channel.
5. **Logging & History Tracking**
   - *Log Interaction History:* Appends all data, including the AI-generated strategy, to a Google Sheet for long-term audit trails and trend analysis.

##  Prerequisites
- n8n (Self-hosted or Cloud)
- Google Cloud Console (For Gmail and Google Sheets API access)
- Google AI (Gemini) API Key
- Slack Webhook or App Token

Disclaimer: This workflow is designed to assist in supplier management. Always review AI-generated response drafts before sending them to external partners to ensure accuracy and brand alignment.