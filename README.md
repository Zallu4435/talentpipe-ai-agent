# 🤖 Telegram AI Career Agent (n8n Orchestration)

![n8n](https://img.shields.io/badge/n8n-Workflow-FF6C37?style=for-the-badge&logo=n8n&logoColor=white)
![Telegram](https://img.shields.io/badge/Telegram-Bot-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)
![Google Gemini](https://img.shields.io/badge/Google_Gemini-AI-8E75B2?style=for-the-badge&logo=google&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-Memory-DC382D?style=for-the-badge&logo=redis&logoColor=white)

An automated, intelligent dual-purpose Telegram bot built entirely within **n8n**. This workflow acts as both a conversational AI assistant (with memory) and a robust data extraction pipeline that scrapes job postings, parses them using LLMs, and saves the structured data to a database.

## ✨ Core Features

*   **Intelligent Intent Routing:** Automatically detects whether a user is sending a standard chat message or a URL, routing the data to the appropriate processing pipeline.
*   **AI Conversational Agent:** Uses Google Gemini and Redis-backed chat memory to maintain context in conversations.
*   **Bypass Scraper Blocking:** Integrates **Jina AI (`r.jina.ai`)** to convert heavy, JavaScript-loaded web pages into clean Markdown, bypassing standard bot protections (like 451/403 errors).
*   **LLM Data Parsing:** Instructs Gemini to read scraped Markdown and extract specific JSON key-value pairs (Company, Title, Salary, Skills).
*   **Automated Database Entry:** Validates the AI's output and automatically appends successful job scrapes into Google Sheets.
*   **Admin Approval System:** Features an onboarding flow with rate-limiting and inline Telegram callback buttons for admin approval of new users.

---

## 🏗️ System Architecture

The n8n workflow is divided into three primary micro-pipelines:

1.  **Auth & Onboarding Layer:** 
    *   Intercepts incoming Telegram messages.
    *   Checks Redis for rate limits and queries the database for user status.
    *   Routes new users to a pending state and alerts the Admin via inline callback buttons for approval/rejection.
2.  **The Extraction Pipeline (True Branch):** 
    *   Triggered when the Intent Router detects an `http` string.
    *   `Jina Web Scraper` -> `Gemini Brain (JSON Extraction)` -> `Validation Router` -> `Google Sheets` -> `Telegram Success Notification`.
3.  **The Conversational Pipeline (False Branch):**
    *   Triggered on standard text input.
    *   Loads history from Redis -> Processes via Gemini -> Returns formatted response to Telegram.

---

## 🚀 Quick Start & Installation

Because this is an n8n orchestration project, installation is as simple as importing the JSON blueprint into your own environment.

### Prerequisites
*   A running instance of [n8n](https://n8n.io/) (Self-hosted or Cloud).
*   A Telegram Bot Token (via BotFather).
*   A Google Cloud Project (for Gemini API and Google Sheets API).
*   A Redis instance (for chat memory and rate limiting).

### Setup Instructions

1.  **Clone the repository:**
    ```bash
    git clone [Your Repository URL]
    ```
2.  **Import the Workflow:**
    *   Open your n8n dashboard.
    *   Create a new workflow.
    *   Click the **"..."** menu in the top right -> **Import from File**.
    *   Select the `workflow.json` file from this repository.
3.  **Configure Credentials:**
    Once imported, n8n will prompt you to connect your credentials. You will need to add:
    *   `Telegram API`
    *   `Google Gemini API`
    *   `Google Sheets OAuth2`
    *   `Redis Server Details`
4.  **Activate:** Toggle the workflow to **Active** and send a message to your Telegram bot!

---

## 🛠️ Configuration Details

### Web Scraper Setup (Jina AI)
To ensure the scraper bypasses Cloudflare/bot protections, the HTTP Request node is configured to prepend the Jina Reader API to the user's input:
```javascript
[https://r.jina.ai/](https://r.jina.ai/){{ $('Telegram Trigger').item.json.message.text.trim() }}
