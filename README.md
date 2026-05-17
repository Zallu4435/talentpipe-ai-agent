# 🤖 Telegram AI Career Agent (n8n Orchestration)

<p align="center">
  <strong>Autonomous AI Telegram Bot for Job Extraction & Conversational Intelligence</strong>
</p>

<p align="center">
  Production-grade n8n automation system combining conversational AI memory,
  intelligent job scraping, and structured data extraction into a single
  Telegram interface powered by Gemini, Redis, and Jina AI.
</p>

<p align="center">
  <strong>Core Modes</strong><br>
  💬 AI Assistant &nbsp;&nbsp;•&nbsp;&nbsp; 🔍 Job Extraction Pipeline
</p>

<p align="center">
  <img src="https://img.shields.io/badge/n8n-Workflow-FF6C37?style=for-the-badge&logo=n8n&logoColor=white" />

  <img src="https://img.shields.io/badge/Telegram-Bot-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white" />

  <img src="https://img.shields.io/badge/Google_Gemini-AI-8E75B2?style=for-the-badge&logo=google&logoColor=white" />

  <img src="https://img.shields.io/badge/Redis-Memory-DC382D?style=for-the-badge&logo=redis&logoColor=white" />
</p>

---

# 📌 Overview

The **Telegram AI Career Agent** is a production-grade automation system built entirely in **n8n**.

It combines conversational AI, intelligent job scraping, structured data extraction, and persistent memory into a single Telegram-powered interface.

The system acts as a dual-purpose AI agent:

- 💬 AI Conversational Assistant
- 🔍 Automated Job Extraction Engine

Designed for:

- AI-assisted Telegram workflows
- Job opportunity aggregation
- Structured web data extraction
- Persistent conversational memory
- Admin-controlled automation systems

---

# ✨ Key Features

## 💬 Conversational AI Engine

- Context-aware AI chat using Google Gemini
- Redis-backed persistent memory
- Multi-turn conversational handling
- Telegram-native interactions
- Structured AI response formatting

---

## 🔎 Intelligent Job Extraction Pipeline

- Automatic URL detection from Telegram messages
- Web scraping using Jina AI Reader (`r.jina.ai`)
- Markdown conversion pipeline
- AI-powered structured JSON extraction
- Output validation layer
- Automatic Google Sheets insertion

---

## 🧠 Smart Intent Routing

Automatically routes incoming Telegram messages:

| Input Type | Workflow |
|------------|-----------|
| Plain Text | Conversational AI Pipeline |
| URL | Job Extraction Pipeline |

Built entirely using conditional orchestration inside n8n.

---

## 🔐 Authentication & Access Control

- New user onboarding workflow
- Redis-based user state management
- Rate limiting protection
- Admin approval system
- Telegram inline approval buttons
- Pending → Approved access state machine

---

# 🏗️ System Architecture

```text
Telegram Trigger
        │
        ├── Intent Router
        │        ├── Text → Gemini Chat + Redis Memory
        │        └── URL → Jina Scraper → Gemini Parser → Sheets
        │
        └── Auth Layer (Redis + Admin Approval)
```

---

# 🚀 Core Workflows

## 💬 Conversational Pipeline

```text
Telegram Message
        ↓
Load Redis Memory
        ↓
Google Gemini
        ↓
Generate Response
        ↓
Telegram Reply
```

### Features

- Context retention
- Memory persistence
- AI conversation continuity
- Structured response generation

---

## 🔍 Job Extraction Pipeline

```text
Telegram URL
        ↓
Jina AI Scraper
        ↓
Markdown Conversion
        ↓
Gemini Structured Extraction
        ↓
JSON Validation
        ↓
Google Sheets Storage
        ↓
Telegram Confirmation
```

### Extracted Fields

- Job Title
- Company Name
- Location
- Salary
- Experience
- Skills
- Application URL
- Posted Date
- Description

---

## 🔐 Authentication Workflow

```text
New User Message
        ↓
Redis User Check
        ↓
Rate Limiter
        ↓
Admin Approval Request
        ↓
Inline Button Approval
        ↓
User Activation
```

---

# ⚙️ Tech Stack

| Layer | Technology |
|------|-------------|
| Automation | n8n |
| Messaging | Telegram Bot API |
| AI Engine | Google Gemini |
| Memory Layer | Redis |
| Web Scraping | Jina AI |
| Data Storage | Google Sheets |

---

# 📦 Quick Start

## 1️⃣ Clone Repository

```bash
git clone <your-repo-url>
cd telegram-ai-career-agent
```

---

## 2️⃣ Import Workflow

1. Open n8n
2. Create a new workflow
3. Import `workflow.json`

---

## 3️⃣ Configure Credentials

Add the following credentials inside n8n:

- Telegram Bot API
- Google Gemini API
- Redis
- Google Sheets OAuth

---

## 4️⃣ Activate Workflow

Turn the workflow **ON** and start messaging your Telegram bot.

---

# 🌐 Web Scraping Layer

The system uses Jina AI Reader for extracting readable web content.

## Endpoint

```text
https://r.jina.ai/http://target-url.com
```

## n8n Expression

```javascript
https://r.jina.ai/{{ $('Telegram Trigger').item.json.message.text.trim() }}
```

---

# 🧠 Workflow Highlights

✅ Dual AI architecture (Chat + Extraction)  
✅ Redis-backed persistent memory  
✅ Fully orchestrated inside n8n  
✅ AI-powered structured extraction  
✅ Telegram-native experience  
✅ Admin-controlled onboarding  
✅ Safe JSON validation pipeline  
✅ Intelligent workflow routing  

---

# 📂 Recommended Project Structure

```text
telegram-ai-career-agent/
│
├── workflows/
│   └── workflow.json
│
├── docs/
│   └── architecture.png
│
├── screenshots/
│   ├── telegram-chat.png
│   ├── admin-approval.png
│   ├── sheets-output.png
│   └── workflow-overview.png
│
├── .env.example
├── README.md
└── LICENSE
```

---

# 🔐 Security Considerations

- Store secrets using n8n credentials
- Enable Redis authentication
- Restrict Telegram admin IDs
- Add request validation
- Implement webhook security
- Use rate limiting for abuse prevention

---

# 📸 Recommended Screenshots

Add screenshots for:

- Telegram AI conversation
- Job extraction flow
- Admin approval interface
- Google Sheets results
- Full n8n workflow overview

---

# 🔮 Future Improvements

- PostgreSQL migration
- Vector memory system
- Semantic job search
- Multi-language support
- Web dashboard
- Analytics & monitoring
- Job alert subscriptions
- Resume parsing integration

---

# 🛠️ Possible Enhancements

## AI Features

- Tool calling
- Function agents
- Multi-agent workflows
- Vector retrieval memory

## Infrastructure

- Docker deployment
- Queue workers
- Webhook scaling
- Monitoring dashboards

## Data Layer

- PostgreSQL
- Supabase
- Pinecone / Weaviate
- ElasticSearch

---

# 📜 License

MIT License

---

# ⭐ Support

If you found this project useful:

- ⭐ Star the repository
- 🍴 Fork the project
- 🚀 Share improvements
- 🤝 Contribute workflows

---

# 👨‍💻 Author

Built with ❤️ using:

- n8n
- Telegram Bot API
- Google Gemini
- Redis
- Jina AI
- Google Sheets
