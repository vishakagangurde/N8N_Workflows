# News Intelligence Workflow

This workflow automatically fetches the latest news articles from an external **API** and transforms them into structured, AI-powered insights. It is designed to be a reliable and scalable system for monitoring news, regulatory changes, and industry trends.

---

## How It Works

The workflow retrieves news based on a defined query, cleans and filters the data, and selects the most relevant articles. Each article is processed individually using a batch loop to ensure controlled execution and avoid **API** rate limits.

The system stores raw article data in **Google Sheets**, then uses an **AI model (Gemini/OpenAI)** to generate a concise summary and identify key regulatory changes. The AI output is formatted and updated back into the same Google Sheet for easy tracking and analysis.

---

## Setup Steps

### 1. Prepare Google Sheets

Create a Google Sheet with the following column headers:
- **Title**
- **Source**
- **Published Date**
- **Content**
- **Summary**
- **Key Changes**

### 2. Configure Variables

Set the following parameters within the workflow configuration:
- ****API** Key:** Your NewsAPI credentials.
- **Query:** The keyword or topic you want to track.
- **Article Limit:** Maximum number of articles to process per run.
- **Sheet ID:** The unique ID of your Google Sheet.

### 3. Fetch & Process Data

- ****HTTP** Request:** Connects to NewsAPI to fetch articles.
- **Code Node:** Preprocesses, normalizes, and cleans the raw data.
- **Limit Node:** Restricts the workflow to the Top N articles.

### 4. AI Analysis

- **AI Agent:** Uses Gemini or OpenAI to analyze the article text.
- **Output:** Generates a concise summary and extracts key regulatory changes.
- **Formatting:** A Code node ensures the AI output is structured correctly before being saved.

### 5. Storage & Error Handling

- **Google Sheets:** Appends raw data and updates processed insights.
- **Batching:** Uses `SplitInBatches` and `Wait` nodes to manage rate limits.
- **Error Handling:** Collects failures from any node, formats the details, and sends an email alert after a short delay.

---

## Workflow Nodes Overview

| Section | Description |
| :--- | :--- |
| **Config & Fetching** | Centralizes API keys and fetches latest news based on keywords. |
| **Data Processing** | Cleans and structures raw articles into a consistent format. |
| **Control & Batching** | Limits article count and processes them one-by-one for stability. |
| **AI Processing** | Extracts insights, summaries, and regulatory changes. |
| **Formatting & Storage** | Saves original and AI-generated data into Google Sheets. |
| **Error Handling** | Monitors for failures and sends automated email alerts. |

---

> **Note:** Ensure your Google Sheets and NewsAPI credentials are properly authenticated in the n8n credentials manager before executing the workflow.