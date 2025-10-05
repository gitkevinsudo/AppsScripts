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
| [Check-In Bot](https://github.com/gitkevinsudo/AppsScripts/blob/main/dailycheckin) | Tracks daily user check-ins and streaks | 🆕 v1.0.0 |
| (Coming Soon) Suggestion Box | Collects member feedback | ⏳ Planned |

---

## ✳️ Shared Setup Requirements

All scripts follow the same general setup process:

1. **Create a Google Sheet**
   - Tabs and columns vary per app (see each section below).
2. **Add Script**
   - Open **Extensions → Apps Script** and paste the code.
3. **Configure Settings tab**
   - Always include:
     | key | value |
     |------|-------|
     | DISCORD_WEBHOOK | *(your Discord webhook URL)* |
4. **Deploy as a Web App** (if it has interactive links)
   - Execute as: **Me**
   - Access: **Anyone with the link**
5. **Add Triggers**
   - Set daily or weekly schedules as described in each section.
6. **Authorize** when prompted.

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

## ✅ Check-In Bot (v1.0.0)

**Purpose:** Daily community check-in system that tracks streaks, posts leaderboards, and celebrates consistency.

**Version:** v1.0.0  
**Date Created:** October 2025  
**Author:** Runningpico

### Sheet Tabs

#### 1. `Users`
| userId | username | currentStreak | bestStreak | lastCheckin |
|--------|-----------|---------------|-------------|--------------|

#### 2. `Checkins`
| timestamp | date | userId | username | source |
|------------|------|--------|-----------|--------|

#### 3. `Settings`
| key | value |
|------|-------|
| DISCORD_WEBHOOK | *your webhook URL* |
| TIMEZONE | America/New_York |
| WEBAPP_URL | *added after deployment* |
| DAILY_POST_HOUR | 9 |
| LEADERBOARD_DAY | SUN |

---

### Script Overview
| Function | Description |
|-----------|-------------|
| `postDailyCheckin()` | Posts daily check-in link |
| `postWeeklyLeaderboard()` | Posts weekly leaderboard |
| `postPersonalizedLinksInThread()` | Posts custom user links |
| `doGet()` | Web App endpoint (handles user clicks) |
| `recordCheckin()` | Core logic to update streaks |
| `adminBackfillStreaksFromLog()` | Rebuilds streaks from history |
| `htmlResponse()` | Returns a styled confirmation page (includes version footer) |

---

### Setup Steps

1. Create the three tabs (`Users`, `Checkins`, `Settings`)
2. Paste the full `v1.0.0` Apps Script
3. Set your **project timezone**
4. Deploy as **Web App**
   - Execute as: *Me*  
   - Access: *Anyone with the link*
5. Copy the **Web App URL** → paste into `Settings!WEBAPP_URL`
6. Test by running `postDailyCheckin`
7. Add triggers:
   - `postDailyCheckin` → Daily at 9 AM
   - `postWeeklyLeaderboard` → Weekly on Sunday at 10 AM
   - *(optional)* `postPersonalizedLinksInThread` → Daily 9:05 AM

**Footer:** Every page displays  
> `Discord Check-In Bot — v1.0.0`

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

## 🛠️ Shared Utilities

Each script follows the same code pattern for webhooks:
```js
function postToDiscord(payload) {
  const options = {
    method: 'post',
    contentType: 'application/json',
    payload: JSON.stringify(payload),
    muteHttpExceptions: true
  };
  UrlFetchApp.fetch(WEBHOOK_URL, options);
}
