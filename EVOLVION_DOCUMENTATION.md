# 🎯 Evolvion — Full Project Documentation

> **Project Name:** Evolvion Quiz Competition Platform  
> **Organization:** SSDSSC  
> **Type:** Real-time Live Quiz Show Web Application  
> **Tech Stack:** Node.js · Express · Socket.io · HTML/CSS/JS · Firebase  

---

## 📖 Table of Contents

1. [What is Evolvion?](#1-what-is-evolvion)
2. [Project Structure](#2-project-structure)
3. [How the Competition Works](#3-how-the-competition-works)
4. [Each Folder Explained](#4-each-folder-explained)
5. [Pages & Screens](#5-pages--screens)
6. [Technical Architecture](#6-technical-architecture)
7. [Data & Files](#7-data--files)
8. [Game Phases & Flow](#8-game-phases--flow)
9. [Features In Detail](#9-features-in-detail)
10. [Technology Stack](#10-technology-stack)
11. [How to Run](#11-how-to-run)

---

## 1. What is Evolvion?

**Evolvion** is a fully custom-built **live quiz competition platform** made for a school/college-level inter-house or inter-team quiz competition. It is like a digital, real-time version of a traditional quiz show — but powered by modern web technology.

Instead of using paper buzzers and manual scoring, everything runs through a local network (Wi-Fi). The admin controls the show from a laptop, the big screen (projector/TV) shows the questions and scores, and the audience participates on their own phones in real-time.

### 🏆 Competition Structure

The competition runs in **three stages**:

```
Knockouts  ──►  Semifinals (Semis)  ──►  Finals
```

Each stage has its **own completely separate server** — they run independently and don't interfere with each other.

---

## 2. Project Structure

```
evolvion final network/
│
├── 📁 knockouts/          ← Knockout round server
│   ├── server.js          ← Node.js backend
│   ├── public/            ← Web pages (admin, team, scoreboard)
│   ├── questions.json     ← Question bank
│   ├── scores.json        ← Team scores
│   ├── users.json         ← Team login accounts
│   └── package.json
│
├── 📁 semis/              ← Semifinal round server  (MOST ADVANCED)
│   ├── server.js          ← Node.js backend (1509 lines!)
│   ├── public/            ← Web pages
│   ├── questions.json     ← Question bank (bilingual)
│   ├── scores.json        ← Team scores
│   ├── users.json         ← Team login accounts
│   ├── semis.state.json   ← Live game state (auto-saved)
│   ├── audience.scores.json     ← Audience member scores
│   ├── audience.streaks.json    ← Audience answer streaks
│   ├── audience.responses.log.json  ← Full audience answer log
│   ├── board.config.json   ← Scoreboard layout & sound settings
│   ├── ads.library.json    ← Sponsor advertisement images
│   ├── firebase-service-account.json  ← Firebase credentials
│   └── package.json
│
├── 📁 final/              ← Finals round server
│   ├── server.cjs         ← Node.js backend
│   ├── public/            ← Web pages (admin, team)
│   ├── data/finals.json   ← Persistent game data
│   └── package.json
│
├── 📁 final2/             ← Backup/alternate finals server
│   ├── server.cjs
│   ├── public/
│   └── data/
│
├── 📁 portal/             ← Audience entry landing page
│   ├── index.html         ← "Open Audience Web" button
│   └── key.html           ← Key/access page
│
├── package.json           ← Root-level package (shared)
├── knockouts.rar          ← Archived backup of knockouts
└── portal.rar             ← Archived backup of portal
```

---

## 3. How the Competition Works

### 👥 The Three Types of Users

| Role | Device | Page They Open |
|------|--------|---------------|
| **Admin / Host** | Laptop (backstage) | `admin.html` |
| **Teams (Blue & Red)** | Tablet / Laptop on stage | `team.html` |
| **Audience** | Their personal phones | `scoreboard.html` or `evo.html` |

### 🎮 The Flow of a Question

```
Admin selects question
        │
        ▼
[Reading Phase]  ──► Timer countdown, question shown (no options yet)
        │
        ▼
[Asking Phase]   ──► All options A/B/C/D revealed, timer starts
        │
        ├──► Teams submit answers via their tablet
        │
        ├──► Audience submits answers via their phones
        │
        ▼
[Revealed Phase] ──► Admin reveals correct answer
        │
        ▼
[Scoring]        ──► Points awarded, scoreboard updates live
        │
        ▼
[Next Question]  ──► Repeat
```

---

## 4. Each Folder Explained

### 📁 `knockouts/` — Knockout Round

- **Purpose:** The **first round** of the competition. Multiple teams compete, weaker teams are eliminated.
- **Complexity:** Simpler server compared to semis.
- **Pages:** `admin.html`, `team.html`, `scoreboard.html`
- **Port:** Runs on its own port (separate from other rounds)
- **Key files:**
  - `server.js` — Backend with Express + Socket.io
  - `questions.json` — MCQ questions for this round
  - `users.json` — Team accounts (hashed passwords)
  - `scores.json` — Current team scores

---

### 📁 `semis/` — Semifinal Round ⭐ (Most Feature-Rich)

- **Purpose:** The **second round**, the most advanced stage of the platform.
- **Complexity:** The largest server (1,509 lines of code), with the most features.
- **Port:** Runs independently
- **Special features unique to semis:**
  - ✅ **Audience participation** — audience members answer on their phones and compete
  - ✅ **Audience streaks** — consecutive correct answers give streak bonuses
  - ✅ **Audience leaderboard** — top audience members tracked
  - ✅ **Advertisements** — sponsor images shown between questions
  - ✅ **Firebase integration** — cloud backup / integration
  - ✅ **ATA (Ask The Audience)** — teams can ask the audience for help (like "Phone a Friend")
  - ✅ **Follow-up questions** — special second-chance questions
  - ✅ **Sound effects (SFX)** — configurable volume per event (buzzer, correct, wrong, timer beeps...)
  - ✅ **Board layout config** — choose Blue team on left or right side

**Real schools competing in the last saved state:**
- 🔵 Blue Team: **ANANDA** College
- 🔴 Red Team: **ISIPATHANA** College

---

### 📁 `final/` — Finals Round

- **Purpose:** The **grand finale** of the competition — two finalist teams face off.
- **Key features:**
  - ✅ **Video questions** — questions can have a video intro (up to 200MB MP4/WebM)
  - ✅ **Video controls** — admin can play, pause, skip videos
  - ✅ **Video preloading** — admin can preload the next video in advance
  - ✅ **Image questions** — questions can include images
  - ✅ **Buzzer system** — admin manually triggers blue/red buzzer on screen
  - ✅ **Reading mode** — text read aloud before options appear
  - ✅ **Bilingual toggle** — switch English/Sinhala on the fly
  - ✅ **Team logos** — custom PNG/JPG logos for each team
  - ✅ **Score delta controls** — add/subtract points manually
- **Server file:** `server.cjs` (CommonJS format)
- **Data:** Stored in `data/finals.json`

---

### 📁 `final2/` — Alternate Finals

- **Purpose:** A backup or secondary finals server (same structure as `final/`)
- **Use case:** Could be used for a second finals match or as a recovery copy

---

### 📁 `portal/` — Audience Entry Page

- **Purpose:** A simple, beautiful landing page for audience members.
- **What it does:** Shows a "Open Audience Web" button that redirects to the live quiz URL.
- **URL hardcoded:** `http://172.20.10.2:3000/scoreboard.html` (local network IP)
- **Design:** Dark purple/violet gradient, Poppins font, glassmorphism card

---

## 5. Pages & Screens

### `admin.html` — Admin Control Panel

> **Who uses it:** The quiz master / event organizer (backstage, not shown to audience)

**What it can do:**
- 🔐 Login with password (hashed & secure)
- 📋 View all questions in a card grid
- ➕ Add new questions (English + Sinhala text, options A/B/C/D, correct answer, time limit, marks, reading time, image, video)
- ✏️ Edit existing questions
- 🗑️ Delete questions
- ▶️ Start a question (triggers it on the team screen)
- ⏭️ Skip reading phase
- 👁️ Reveal the correct answer
- 🔇 End the question
- 🏆 Manually set/add/subtract scores
- 🔴🔵 Trigger buzzer for Blue or Red team
- 📺 Upload and control videos
- 🖼️ Upload images for questions
- 🏷️ Set team names and logos
- 🌐 Toggle bilingual (English/Sinhala) mode
- 📖 Toggle reading mode on/off

---

### `team.html` — Team Display Screen

> **Who uses it:** Shown on a big screen (TV/projector) for the competing teams

**What it shows:**
- Team names and logos (Blue vs Red)
- Live scores for both teams
- The current question (English and/or Sinhala)
- Answer options A / B / C / D
- Countdown timer (animated)
- Which team has answered
- Correct/wrong answer reveal
- Buzzer activation animation

---

### `scoreboard.html` (Semis) — Audience Live View

> **Who uses it:** Audience members on their phones

**What it shows:**
- Live question (with timer)
- The audience member can tap an answer (A/B/C/D)
- Their own score and streak
- Overall audience leaderboard
- Advertisements / sponsor images between questions
- Team scores (Blue vs Red)

---

### `evo.html` (Semis) — Audience Login Page

> **Who uses it:** Audience members — they enter their name here to join

- Name entry form
- Joins the competition as an "audience player"

---

### `cloud.html` (Semis) — Cloud/Firebase Screen

- Shows Firebase-synced data (possibly for cloud display or backup checking)

---

### `portal/index.html` — Public Portal

- A polished landing page with a glowing purple button
- Redirects audience to the live quiz URL on the local network

---

## 6. Technical Architecture

### Network Architecture (Local Wi-Fi)

```
                    [ Wi-Fi Router / Hotspot ]
                            │
            ┌───────────────┼───────────────┐
            │               │               │
    [Admin Laptop]   [Team Tablets]  [Audience Phones]
    (runs servers)   (open team.html) (open scoreboard.html)
```

All devices must be on the **same Wi-Fi network**. The server runs on the host laptop and is accessed via the laptop's local IP address (e.g. `192.168.x.x` or `172.20.10.x`).

---

### Socket.io Namespaces (Semis)

Socket.io uses separate "channels" (namespaces) for each type of user:

| Namespace | Users |
|-----------|-------|
| `/host` | Admin panel |
| `/team` | Team display screen |
| `/audience` | Audience phones |

This means the admin can send commands only to teams, or only to the audience, or broadcast to everyone — with full control.

---

### Finals Namespaces

| Namespace | Users |
|-----------|-------|
| `/final-admin` | Admin panel (requires token login) |
| `/final-board` | Team/display screen |

---

### Authentication System

| User Type | Method |
|-----------|--------|
| **Admin** | Password → PBKDF2 hashed (100,000 iterations, SHA-512) → Token issued |
| **Teams** | Username + Password → Hashed → Session token (12-hour TTL) |
| **Audience** | Just a name (no password needed) → Audience session token |

Session tokens are stored in `sessions.json` and auto-expire after 12 hours.

---

## 7. Data & Files

### JSON Data Files (Semis)

| File | Purpose |
|------|---------|
| `questions.json` | All MCQ questions with English + Sinhala text, options, correct answer, time limit, marks |
| `users.json` | Team login accounts (username → hashed password + optional logo URL) |
| `scores.json` | Current team scores |
| `sessions.json` | Active login tokens for admin, teams, and audience |
| `semis.state.json` | The **complete live game state** — what phase the game is in, who answered, which questions were played, etc. |
| `audience.scores.json` | Individual scores for each audience member |
| `audience.streaks.json` | Streak data per audience member (current streak, max streak) |
| `audience.responses.log.json` | Full log of every audience answer ever submitted (101KB!) |
| `board.config.json` | Scoreboard layout (blue left/right) and all SFX volume levels |
| `ads.library.json` | Sponsor advertisement images (SLT Mobitel sponsor seen!) |
| `admin.json` | Admin password hash |
| `live.state.json` | Simple live/active state flag |

---

### Question Format

Each question in `questions.json` looks like this:

```json
{
  "text": "Full question in English",
  "textEn": "Question in English",
  "textSi": "ප්‍රශ්නය සිංහලෙන්",
  "options": {
    "A": "Option A text",
    "B": "Option B text",
    "C": "Option C text",
    "D": "Option D text"
  },
  "correct": "A",
  "timeLimit": 90,
  "normalMark": 10,
  "readingTime": 10,
  "imageUrl": null,
  "videoUrl": null
}
```

**Fields explained:**
- `textEn` / `textSi` — Question in English and Sinhala
- `correct` — The right answer: `"A"`, `"B"`, `"C"`, or `"D"`
- `timeLimit` — How many seconds teams have to answer
- `normalMark` — Points awarded for correct answer
- `readingTime` — Seconds given to just READ the question before options appear
- `imageUrl` — Optional image attached to the question
- `videoUrl` — Optional video intro for the question (Finals only)

---

### Sound Effects Configuration (`board.config.json`)

The scoreboard plays real sound effects! Each event has its own volume level:

| Event | Sound |
|-------|-------|
| `start` | Question starts |
| `answeredBlue` | Blue team submitted answer |
| `answeredRed` | Red team submitted answer |
| `ata` | "Ask The Audience" triggered |
| `reveal` | Correct answer revealed |
| `correct` | Team got it right ✅ |
| `wrong` | Team got it wrong ❌ |
| `score` | Score updated |
| `reading` | Reading phase music |
| `idle` | Background idle music |
| `beep3/2/1` | Countdown beeps (3, 2, 1 seconds left) |
| `timeup` | Time's up! |

---

## 8. Game Phases & Flow

### Semis Game Phases

The `semis.state.json` file tracks which **phase** the game is in at all times:

```
idle        ← No question active, waiting
   │
   ▼
reading     ← Question text shown, NO options yet. Reading time counts down.
   │
   ▼
asking      ← Options shown, main timer counting down. Teams & audience answer.
   │
   ▼
locked      ← Timer expired. No more answers accepted. Admin decides what to do.
   │
   ▼
revealed    ← Admin reveals the correct answer. Scores updated.
   │
   ▼
askFollowup ← Optional: A follow-up question given to a team
   │
   ▼
idle        ← Back to start, next question
```

### Finals Game Phases

```
idle      ← Waiting
   │
   ▼
video     ← A video intro plays before the question
   │
   ▼
reading   ← Question text shown without options
   │
   ▼
asking    ← Full question with options and timer
   │
   ▼
revealed  ← Answer revealed
   │
   ▼
idle
```

---

## 9. Features In Detail

### 🔔 Buzzer System (Finals)
- Admin presses **Blue Buzzer** or **Red Buzzer** button
- The screen instantly shows a flashing buzzer animation for that team
- Admin can clear the buzzer and re-trigger it

### 📺 Video Questions (Finals)
- Admin uploads a video file (MP4, WebM, OGG — up to 200MB)
- Before the question, the video plays on the team screen
- Admin can **play**, **pause**, and **skip** the video remotely
- Admin can **preload** the next video in advance so it's ready instantly

### 🤔 ATA — Ask The Audience (Semis)
- During a question, a team can use their "Ask The Audience" lifeline (one use per team)
- Audience members on their phones vote on the answer
- Live poll results (A/B/C/D percentages) are displayed on screen

### 📈 Audience Streaks (Semis)
- If an audience member answers correctly multiple times in a row, they build a **streak**
- Streak bonuses give extra points
- Maximum streak per person is tracked

### 🖼️ Advertisements (Semis)
- Admin can push sponsor ads (images) to the audience screens between questions
- Ads auto-hide after a set time
- The sponsor seen: **SLT Mobitel** (Sri Lanka's national telecom)

### 🌐 Bilingual Mode
- Toggle between English only, Sinhala only, or **both languages at once**
- Questions show in both `textEn` (English) and `textSi` (Sinhala)
- Options also shown bilingually

### 📖 Reading Mode
- When enabled, the question appears for a set "reading time" (e.g. 10 seconds)
- During reading time, the answer options are **hidden**
- This gives teams time to understand the question before the clock starts

### 🏷️ Team Logos
- Each team (Blue/Red) can have a custom logo image
- Logos appear on the scoreboard and team screen
- Uploaded as base64 image data

### 🔒 Secure Authentication
- Passwords stored as `PBKDF2 + SHA-512 + salt` hashes (industry standard)
- Session tokens with 12-hour expiry
- Admin tokens stored in memory + `sessions.json`
- Timing-safe password comparison (prevents timing attacks)

---

## 10. Technology Stack

| Category | Technology | Version | Purpose |
|----------|-----------|---------|---------| 
| **Runtime** | Node.js | Latest | Server-side JavaScript |
| **Web Framework** | Express | v4/v5 | HTTP API + static file serving |
| **Real-time** | Socket.io | v4.7-4.8 | Live bidirectional communication |
| **File Uploads** | Multer | - | Video/image file uploads (Finals) |
| **Cloud** | Firebase Admin SDK | v12.7 | Firebase integration (Semis) |
| **Dev Server** | Nodemon | v3.1 | Auto-restart on file changes |
| **Frontend** | HTML5 + CSS3 + JS | - | All UI pages |
| **Fonts** | Poppins (Google Fonts) | - | Portal & audience pages |
| **Math Rendering** | LaTeX (`\[...\]` syntax) | - | Math question options (fractions, etc.) |
| **Data Storage** | JSON files | - | No database — flat file storage |
| **Security** | Node.js `crypto` | Built-in | PBKDF2 hashing, token generation |

---

## 11. How to Run

### Starting the Semis Server

```powershell
# Navigate to the semis folder
cd "d:\Web Projects\ssdssc\evolvion final network\semis"

# Install dependencies (first time only)
npm install

# Start the server
node server.js
```

Then open in browser:
- **Admin panel:** `http://localhost:PORT/admin.html`
- **Team screen:** `http://localhost:PORT/team.html`
- **Audience:** `http://YOUR_IP:PORT/scoreboard.html`

---

### Starting the Finals Server

```powershell
cd "d:\Web Projects\ssdssc\evolvion final network\final"

npm install

node server.cjs
```

Default port: `3000`  
Default admin password: `admin123` (change immediately in production!)

---

### Starting the Knockouts Server

```powershell
cd "d:\Web Projects\ssdssc\evolvion final network\knockouts"

npm install

node server.js
```

---

### The Portal

The `portal/index.html` file is just a static HTML file. Open it directly in a browser — no server needed. It links to the audience URL on the local network.

---

## 📝 Summary

| Aspect | Detail |
|--------|--------|
| **Project** | Evolvion — live quiz competition platform |
| **Organization** | SSDSSC |
| **Rounds** | Knockouts → Semis → Finals |
| **Teams seen** | ANANDA College vs ISIPATHANA College |
| **Languages** | English + Sinhala (bilingual) |
| **Audience** | Live participation via phones |
| **Real-time** | Yes — instant updates via WebSockets |
| **Data** | JSON files (no external database) |
| **Auth** | Secure hashed passwords + session tokens |
| **Sponsor** | SLT Mobitel |
| **Biggest file** | `semis/server.js` — 1,509 lines |
| **Most code** | Semis round (most features) |

---

*Documentation generated from source code analysis of the Evolvion project.*
