# Gold-Silver Ratio Tracker with Opportunity Alerts

This n8n workflow monitors real-time gold and silver prices, calculates the Gold-Silver ratio, and generates automated investment signals (Buy Gold, Buy Silver, or Hold) based on custom thresholds.

##  Overview
The workflow fetches live market data from GoldAPI, computes the current ratio, and evaluates it against predefined logic. If a threshold is met, it sends a structured email alert containing market insights and investment interpretation.

##  Setup Steps
- **Configure Master Node:** Set up the Configuration node with your GoldAPI key, alert email, currency, and threshold values for buy signals.
- **Connect API Access:** Ensure your GoldAPI key is valid and added correctly for both gold (XAU) and silver (XAG) requests.
- **Add Trigger:** Use the Manual Trigger for testing or replace it with a Cron node for automated periodic execution (e.g., daily or hourly).
- **Fetch Market Data:** The workflow uses HTTP Request nodes to fetch real-time gold and silver prices.
- **Format Price Data:** API responses are mapped to extract `goldPrice` and `silverPrice` fields.
- **Merge & Calculate Ratio:** Both price streams are combined, and the Gold-Silver ratio is calculated using a Code node.
- **Apply Signal Logic:** IF nodes compare the ratio against thresholds:
  - Ratio > upper threshold → Buy Silver
  - Ratio < lower threshold → Buy Gold
  - Otherwise → Hold
- **Generate Signal Output:** A Code node assigns the signal type and enriches it with a detailed interpretation.
- **Send Email Alert:** The Gmail node sends a formatted report including prices, ratio, and analysis.
- **Monitor & Adjust:** Track signals over time and adjust thresholds in the Configuration node as market conditions change.

##  Workflow Components
1. **Trigger & Configuration**
   - Initializes the workflow and sets global variables (API keys, thresholds, currency, and recipient email).
2. **Market Data Fetching**
   - *Gold Price Fetching:* Fetches real-time XAU prices and formats them.
   - *Silver Price Fetching:* Fetches real-time XAG prices and formats them.
3. **Logic & Analysis**
   - *Data Merge & Ratio Calculation:* Combines the data and computes the formula:
     ```
     Ratio = Silver Price / Gold Price
     ```
   - *Buy Silver Condition:* Triggers if silver is undervalued compared to gold.
   - *Buy Gold Condition:* Triggers if gold is undervalued compared to silver.
4. **Output & Alerts**
   - *Signal Assignment:* Structures the final advice (Buy/Hold) with market insights.
   - *Final Processing & Alert:* Aggregates all data and sends a detailed email alert.

##  Prerequisites
- n8n (Self-hosted or Cloud)
- GoldAPI Key (Free or Paid tier)
- Gmail Credentials (Configured within n8n for email alerts)

> Disclaimer: This workflow is for informational purposes only and does not constitute financial advice. Always perform your own due diligence before making investment decisions.