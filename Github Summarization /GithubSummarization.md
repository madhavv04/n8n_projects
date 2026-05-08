# GitHub Release Digest Automation

A fully automated GitHub Release Monitoring & AI Summarization workflow built using [n8n](https://n8n.io?utm_source=chatgpt.com), [GitHub API](https://docs.github.com/en/rest?utm_source=chatgpt.com), [Groq AI](https://groq.com?utm_source=chatgpt.com), [Google Sheets](https://www.google.com/sheets/about/?utm_source=chatgpt.com), and [Gmail API](https://developers.google.com/gmail/api?utm_source=chatgpt.com).

This workflow automatically:

* Tracks GitHub users and organizations
* Detects newly published releases
* Filters releases from the last 24 hours
* Uses AI to summarize release notes
* Sends a professional email digest
* Handles errors automatically
* Maintains logs in Google Sheets

---

# 🚀 Project Overview

This automation runs every day at **9:00 AM** and checks GitHub repositories for new releases.

If new releases are found:

* AI generates a clean summary
* HTML email digest is created
* Email is sent to configured recipients
* Success logs are stored

If no releases are found:

* A “No Releases Today” email is sent

If any error occurs:

* Admins receive an instant alert email
* Error details are logged automatically

---

# 🧠 Who Is This For?

This project is useful for:

* Development teams
* CTOs & Engineering Managers
* Open-source maintainers
* Product teams
* DevOps teams
* Tech communities
* Companies tracking dependencies/tools

---

# ⚙️ Tech Stack

| Technology                                                                   | Purpose                          |
| ---------------------------------------------------------------------------- | -------------------------------- |
| [n8n](https://n8n.io?utm_source=chatgpt.com)                                 | Workflow automation              |
| [GitHub REST API](https://docs.github.com/en/rest?utm_source=chatgpt.com)    | Fetch repositories & releases    |
| [Groq AI](https://groq.com?utm_source=chatgpt.com)                           | AI-powered release summarization |
| [Google Sheets](https://www.google.com/sheets/about/?utm_source=chatgpt.com) | Configuration + logs storage     |
| [Gmail API](https://developers.google.com/gmail/api?utm_source=chatgpt.com)  | Email delivery                   |
| JavaScript                                                                   | Data transformation inside n8n   |

---

# 🔄 Workflow Architecture

## 1. Schedule Trigger

The workflow starts automatically every day at **9:00 AM**.

### Trigger Node

* `Every Day at 9 AM`

---

# 📂 Google Sheets Structure

The system uses multiple Google Sheet tabs.

---

## 1. `users` Sheet

Stores GitHub usernames.

| username  |
| --------- |
| madhavv04 |

---

## 2. `orgs` Sheet

Stores GitHub organizations to monitor.

| org    | active |
| ------ | ------ |
| my-org | yes    |

---

## 3. `config` Sheet

Stores email recipients.

| email                                       |
| ------------------------------------------- |
| [info@example.com](mailto:info@example.com) |

---

## 4. `success_logs` Sheet

Stores successful execution logs.

| timestamp | workflow | status | release_count |
| --------- | -------- | ------ | ------------- |

---

## 5. `error_logs` Sheet

Stores workflow errors.

| timestamp | failed_node | error_message |
| --------- | ----------- | ------------- |

---

# 🔍 Workflow Flow Explanation

---

# Phase 1 — Configuration Loading

The workflow first loads:

* GitHub users
* GitHub organizations
* Email recipients

### Nodes Used

* `.env`
* `Read GitHub Users`
* `Read GitHub Orgs`
* `Read Config (Emails)`

---

# Phase 2 — Build GitHub API Calls

The workflow dynamically creates API URLs for:

* GitHub users
* GitHub organizations

### Example APIs

```bash
https://api.github.com/users/{username}/repos
https://api.github.com/orgs/{org}/repos
```

### Node Used

* `Build API Call List`

---

# Phase 3 — Fetch Repositories

The workflow fetches:

* All repositories
* Removes duplicate repositories

### Nodes Used

* `Fetch All Repos`
* `Flatten & Deduplicate Repos`

---

# Phase 4 — Fetch Releases

The workflow fetches releases for every repository.

### Example API

```bash
https://api.github.com/repos/{owner}/{repo}/releases
```

### Node Used

* `Fetch Releases Per Repo`

---

# Phase 5 — Filter Last 24 Hours

Only releases published within the last 24 hours are kept.

### Node Used

* `Filter Last 24h Releases`

---

# Phase 6 — AI Summarization

If releases exist:

* All release notes are aggregated
* Sent to Groq AI
* AI generates clean HTML summaries

### AI Model Used

* `llama-3.1-8b-instant`

### Node Used

* `Groq Summarize`

---

# ✉️ Email Generation

The workflow builds professional HTML email digests.

Email contains:

* Repository name
* Release version
* Release notes link
* AI-generated summary

### Node Used

* `Build HTML Email`

---

# 📬 Email Delivery

Emails are sent using Gmail integration.

### Nodes Used

* `Send Release Digest`
* `Send No-Releases Email`

---

# 🚨 Error Handling System

A completely separate error workflow handles failures.

If any node crashes:

* Error email is generated
* Admins are notified instantly
* Error is logged into Google Sheets

### Error Flow Nodes

* `Error Trigger`
* `Build Error Email`
* `Send Error Alert Email`
* `Log Error to Google Sheets`

---

# 📊 Logging System

The workflow stores:

## Success Logs

* Timestamp
* Email type
* Release count
* Subject
* Execution ID

## Error Logs

* Failed node
* Error message
* Execution ID
* Workflow name

---

# 📨 Example Email Output

The email digest contains:

* Professional GitHub-style UI
* AI summarized release notes
* Clean HTML formatting
* Direct release links

Example:

```html
owner/repo
Version: v2.1.0
What's New:
Added authentication support and performance improvements.
```

---

# 🛠️ Setup Guide

---

# Step 1 — Import Workflow

Import the JSON workflow into [n8n](https://n8n.io?utm_source=chatgpt.com).

---

# Step 2 — Configure Credentials

Add these credentials inside n8n:

## Required Credentials

| Credential           | Purpose               |
| -------------------- | --------------------- |
| Google Sheets OAuth2 | Read/write sheet data |
| Gmail OAuth2         | Send emails           |
| GitHub PAT           | Access GitHub API     |
| Groq API Key         | AI summarization      |

---

# Step 3 — Create Google Sheets

Create required tabs:

* `config`
* `users`
* `orgs`
* `success_logs`
* `error_logs`

---

# Step 4 — Add GitHub Users & Orgs

Fill sheets with:

* GitHub usernames
* Organizations to monitor

---

# Step 5 — Activate Workflow

Enable the workflow inside n8n.

The automation will now run daily at 9 AM.

---

# 🔐 Important Notes

## GitHub Repository Visibility

Repositories should preferably be:

* Public repositories

Private repositories require:

* Proper GitHub PAT permissions

---

# 📈 Features

✅ Fully automated
✅ AI-powered summarization
✅ Daily email digests
✅ Error notifications
✅ Success & failure logging
✅ Google Sheets driven configuration
✅ Dynamic GitHub repo tracking
✅ Duplicate repo handling
✅ Clean HTML email UI
✅ Scalable architecture

---

# 🧩 Future Improvements

Possible upgrades:

* Slack notifications
* Discord integration
* Telegram alerts
* Multi-language summaries
* Weekly digest mode
* PDF report export
* Dashboard analytics
* Priority-based releases
* Repository categories
* Release sentiment analysis

---

# 📂 Main Workflow Sections

| Section        | Purpose                  |
| -------------- | ------------------------ |
| Trigger        | Starts workflow daily    |
| Config Loader  | Loads sheet data         |
| GitHub Fetcher | Fetches repos & releases |
| Release Filter | Filters recent releases  |
| AI Summarizer  | Generates summaries      |
| Email Builder  | Creates HTML emails      |
| Logging System | Stores logs              |
| Error Handler  | Handles failures         |

---

# 👨‍💻 Developed Using

* [n8n Workflow Automation](https://n8n.io?utm_source=chatgpt.com)
* [GitHub REST API](https://docs.github.com/en/rest?utm_source=chatgpt.com)
* [Groq AI API](https://groq.com?utm_source=chatgpt.com)
* [Google Sheets API](https://developers.google.com/sheets/api?utm_source=chatgpt.com)
* [Gmail API](https://developers.google.com/gmail/api?utm_source=chatgpt.com)

---

# 📌 Final Summary

This project is a production-style AI automation workflow that continuously monitors GitHub releases, summarizes them intelligently using AI, sends professional email digests, and maintains full operational logging with automatic error handling.

It is designed to be:

* Easy to configure
* Scalable
* Team-friendly
* Low maintenance
* Business ready
