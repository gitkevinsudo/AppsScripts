# 🧠 Discord + Google Apps Script Automations

This repository contains a collection of **Google Apps Script** tools that integrate with **Discord Webhooks** and **Google Sheets** — built to power fun community features like daily questions, raffles, Bible prompts, and member engagement systems.

All scripts use the same core design:
- **Google Sheets** as a data store
- **Apps Script triggers** for scheduling
- **Discord Webhooks** for posting
- Lightweight, serverless architecture (no bot hosting required)

---

## 📚 Table of Contents
| App | Description | Status |
|-----|--------------|--------|
| [QOTD Bot](https://github.com/gitkevinsudo/AppsScripts/blob/main/QOTD%20BOT)| Posts a random daily question from a Google Sheet | ✅ Active |
| [Bible QOTD Bot](https://github.com/gitkevinsudo/AppsScripts/blob/main/ChristianQOTD) | Posts a random Bible-related question or verse | ✅ Active |
| [Random Drawing Bot](https://github.com/gitkevinsudo/AppsScripts/blob/main/RandomDrawing)| Runs weighted raffles for community check-ins | ✅ Active |
| (Coming Soon) Suggestion Box | Collects member feedback | ⏳ Planned |

---

## ✳️ Shared Setup Requirements

All scripts follow the same general setup process:

1. **Create a Google Sheet**
   - Tabs and columns vary per app (see each section below).
2. **Add Script**
   - Open **Extensions → Apps Script** and paste the code.
   - Add your webhook
3. **Deploy as a Web App** (if it has interactive links)
   - Execute as: **Me**
   - Access: **Anyone with the link**
4. **Add Triggers**
   - Set daily or weekly schedules as described in each section.
5. **Authorize** when prompted.

---

## 💬 QOTD Bot

**Purpose:** Posts a random question from a Google Sheet to a Discord channel once a day.

**Sheet Tabs:**
- `Questions`: Column A = question text  
- `Settings`: includes webhook + timezone

**Key Functions:**
- `postQuestionToDiscord()`: picks a random non-empty question
- `postNextQOTD()`: main daily trigger
- Auto-skips blank rows and reshuffles as needed

**Trigger Setup:**
- `postNextQOTD` → Time-driven → Every day 9 AM

**Setup Script for Google Sheets **
- BETA: Setups the QOTD google sheet before adding the QOTD Bot App Script [Setup Script](https://github.com/gitkevinsudo/AppsScripts/blob/main/QOTD_setup). **NOTE: you will have to copy and paste the QOTD App Script after you run this**

---

## 📖 Bible QOTD Bot

**Purpose:** Posts a daily faith-related question or verse.

**Sheet Tabs:**
- `BibleQuestions`: Column A = prompt
- `Settings`: webhook + timezone
- Optional: `Verses` for random verse pairing

**Key Functions:**
- `postBibleQuestion()`: posts daily prompt
- Supports multiple webhooks (for different channels)

**Trigger Setup:**
- `postBibleQuestion` → Every day 9 AM

---

## 🎟️ Random Drawing Bot

**Purpose:** Runs weighted raffles (e.g., “Check-In Raffle Winner”) using Sheet data.

**Sheet Tabs:**
| Name | Entries |
|------|----------|
| USER A | 5 |
| USER B | 2 |

**Key Function:**
- `runWeightedRaffle()`: builds pool based on entries, selects winner, and posts to Discord

**Webhook Fields:**
- `username`: `"🎟️ Raffle Bot"`
- `avatar_url`: *(your image link)*

**Enhancements:**
- Retry logic on Discord webhook errors
- Option to send results to multiple channels

**Trigger Setup:**
- `runWeightedRaffle` → Time-driven → Weekly / Monthly (depending on event)

---

## 🧩 Future Bots (Ideas)
| Idea | Description |
|------|--------------|
| 🎯 Verse of the Week | Posts weekly verse challenge |
| 💡 Suggestion Box | Collects member ideas via Forms |
| 🎮 Trivia | Pulls random trivia from Sheet |
| 🙏 Prayer Tracker | Logs prayer requests, posts updates |
| 🧠 Memory Verse Game | Hides words progressively to test recall |

---
