# 📋 EVOLVION — Complete Event Runbook
> **Keep this file private. Contains all passwords.**  
> Last updated from source code analysis — July 2026

---

## 🗂️ TABLE OF CONTENTS
1. [All Passwords & Accounts](#1-all-passwords--accounts)
2. [Fresh Start — Reset Everything](#2-fresh-start--reset-everything)
3. [How to Start Each Server](#3-how-to-start-each-server)
4. [What Each Person Opens](#4-what-each-person-opens)
5. [Event Day Flow](#5-event-day-flow)
6. [Troubleshooting](#6-troubleshooting)
7. [Pre-Event Checklist](#7-pre-event-checklist)

---

## 1. All Passwords & Accounts

### 🔐 Admin Passwords (to control the quiz)

| Round | Admin Login URL | Password |
|-------|----------------|----------|
| **Knockouts** | `http://YOUR_IP:3000/admin.html` | `admin123` ⚠️ |
| **Semis** | `http://YOUR_IP:3000/admin.html` | `admin123` ⚠️ |
| **Finals** | `http://YOUR_IP:3000/admin.html` | `admin123` ⚠️ |

> ⚠️ **All servers are using the default password `admin123`.**  
> Change this before the event — see Section 2 for instructions.

---

### 🏫 Knockouts — Team Accounts

> These teams log in at `http://YOUR_IP:3000/team.html`

| Team Name | Username | Password |
|-----------|----------|----------|
| Royal College | `royal` | *(hashed — unknown, must reset)* |
| Ananda College | `ananda` | *(hashed — unknown, must reset)* |
| S. Thomas' | `thomas` | *(hashed — unknown, must reset)* |
| Thurstan College | `thurstan` | *(hashed — unknown, must reset)* |
| Isipathana | `isipathana` | *(hashed — unknown, must reset)* |
| Kingswood | `kingswood` | *(hashed — unknown, must reset)* |
| Nalanda | `Nalanda` | *(hashed — unknown, must reset)* |
| Rathnavali | `rathnavali` | *(hashed — unknown, must reset)* |
| Maliyadeva | `maliyadeva` | *(hashed — unknown, must reset)* |
| Peters | `peters` | *(hashed — unknown, must reset)* |

> **Note:** Passwords are stored as secure hashes. If you don't remember the original passwords,  
> you must delete each team and re-add them with a new password via the Admin panel.  
> See Section 2 — Reset Procedure.

---

### 🏫 Semis — Team Accounts

| Team Name | Username | Password |
|-----------|----------|----------|
| Royal College | `ROYAL` | *(hashed — must reset if forgotten)* |
| Kingswood | `KINGSWOOD` | *(hashed — must reset if forgotten)* |
| Ananda College | `ANANDA` | *(hashed — must reset if forgotten)* |
| Isipathana | `ISIPATHANA` | *(hashed — must reset if forgotten)* |

> Last match state: **ANANDA (Blue)** vs **ISIPATHANA (Red)**  
> Last scores: ANANDA = 30 pts, ISIPATHANA = 50 pts — **RESET BEFORE NEW EVENT**

---

### 🏆 Finals — Team Accounts

The Finals server does NOT have team logins.  
Teams are set by the admin directly in the admin panel using **team names and logos only**.  
No passwords needed for teams in Finals.

---

## 2. Fresh Start — Reset Everything

> **Do this before EVERY event or round to clear old data.**

---

### 🔄 KNOCKOUTS — Full Reset

Open PowerShell and run these commands:

```powershell
cd "d:\Web Projects\ssdssc\evolvion final network\knockouts"

# Reset scores to zero
'{}' | Out-File -Encoding utf8 scores.json

# Clear live question state
'{"currentQuestionIndex":-1,"questionStartAt":null,"currentQuestion":null,"endsAt":null}' | Out-File -Encoding utf8 live.state.json

# Clear all sessions (force all tablets to re-login)
'{"admin":[],"team":{}}' | Out-File -Encoding utf8 sessions.json

# Clear redzone state
'{"enabled":false}' | Out-File -Encoding utf8 redzone.state.json
```

Then **re-add your teams** via Admin Panel → Teams section with fresh passwords.

---

### 🔄 SEMIS — Full Reset

```powershell
cd "d:\Web Projects\ssdssc\evolvion final network\semis"

# Reset team scores
'{}' | Out-File -Encoding utf8 scores.json

# Reset audience scores
'{}' | Out-File -Encoding utf8 audience.scores.json

# Reset audience streaks
'{}' | Out-File -Encoding utf8 audience.streaks.json

# Backup old log then clear it
Copy-Item audience.responses.log.json audience.responses.log.BACKUP.json
'[]' | Out-File -Encoding utf8 audience.responses.log.json

# Clear all sessions
'{"admin":[],"team":{},"audience":{}}' | Out-File -Encoding utf8 sessions.json

# Reset game state to idle
'{"blueUser":null,"redUser":null,"phase":"idle","endsAt":null,"questionIndex":-1,"answered":{"blue":false,"red":false},"selected":{"blue":null,"red":null},"correct":null,"awarded":false,"started":[],"readingMode":true,"bilingual":true,"poll":{"active":false,"caller":null,"qid":null,"counts":{"A":0,"B":0,"C":0,"D":0},"total":0,"correct":null},"ask":{"blue":{"used":false},"red":{"used":false}},"askLock":{"blue":false,"red":false},"askFollowup":{"active":false,"side":null}}' | Out-File -Encoding utf8 semis.state.json

# Clear live state
'{"currentQuestionIndex":-1,"questionStartAt":null,"currentQuestion":null,"endsAt":null}' | Out-File -Encoding utf8 live.state.json
```

---

### 🔄 FINALS — Full Reset

```powershell
cd "d:\Web Projects\ssdssc\evolvion final network\final"

# Reset finals data (keeps questions, resets scores and team info)
'{"questions":[],"blueName":"Blue","redName":"Red","blueLogo":null,"redLogo":null,"scores":{"blue":0,"red":0},"played":[]}' | Out-File -Encoding utf8 data\finals.json
```

---

### 🔑 How to Change Admin Password

The admin password is hashed — you cannot read or edit it in a file directly.

**Steps to reset it:**
```powershell
# Example for semis — repeat for knockouts and final
cd "d:\Web Projects\ssdssc\evolvion final network\semis"

# Delete the old password file
Remove-Item admin.json

# Start the server — it recreates admin.json with "admin123"
node server.js

# NOW: open admin panel → login with "admin123"
# Go to Settings → Change Password → set your new password
# Stop server (Ctrl+C) then start again → new password is active
```

> Repeat for `knockouts\admin.json` and `final\admin.json`

---

## 3. How to Start Each Server

> **Run ONE server at a time.** Each round has its own separate server.

### ▶️ Start Knockouts Server
```powershell
cd "d:\Web Projects\ssdssc\evolvion final network\knockouts"
node server.js
```
**Runs on:** `http://YOUR_IP:3000`

---

### ▶️ Start Semis Server
```powershell
cd "d:\Web Projects\ssdssc\evolvion final network\semis"
node server.js
```
**Runs on:** `http://YOUR_IP:3000`

---

### ▶️ Start Finals Server
```powershell
cd "d:\Web Projects\ssdssc\evolvion final network\final"
node server.cjs
```
**Runs on:** `http://YOUR_IP:3000`

---

### 🌐 How to Find Your IP Address
```powershell
ipconfig
```
Look for **IPv4 Address** under your Wi-Fi adapter.  
Example output: `IPv4 Address. . . : 172.20.10.2`  
That number is your `YOUR_IP`.

---

## 4. What Each Person Opens

> Replace `YOUR_IP` with your actual IP (from `ipconfig`)

### 🥊 Knockouts Round

| Role | Device | URL |
|------|--------|-----|
| Admin (you) | Your laptop | `http://YOUR_IP:3000/admin.html` |
| Scoreboard / Projector | Any screen | `http://YOUR_IP:3000/team.html` |
| Each competing team | Tablet | `http://YOUR_IP:3000/team.html` → login |
| Audience | Phone / tablet | Not needed in knockouts |

---

### 🎯 Semis Round

| Role | Device | URL |
|------|--------|-----|
| Admin (you) | Your laptop | `http://YOUR_IP:3000/admin.html` |
| Scoreboard / Projector | Any screen | `http://YOUR_IP:3000/team.html` |
| Blue team tablet | Tablet | `http://YOUR_IP:3000/team.html` → login |
| Red team tablet | Tablet | `http://YOUR_IP:3000/team.html` → login |
| Audience (join first) | Phone | `http://YOUR_IP:3000/evo.html` → enter name |
| Audience (quiz view) | Phone | `http://YOUR_IP:3000/scoreboard.html` |

---

### 🏆 Finals Round

| Role | Device | URL |
|------|--------|-----|
| Admin (you) | Your laptop | `http://YOUR_IP:3000/admin.html` |
| Scoreboard / Projector | Any screen | `http://YOUR_IP:3000/team.html` |
| Audience | Phone | `http://YOUR_IP:3000/scoreboard.html` |

> **No team login in finals.** Admin sets team names from the admin panel.

---

## 5. Event Day Flow

```
════════════════════════════════════════
BEFORE THE EVENT STARTS
════════════════════════════════════════

  1. Run reset commands (Section 2) for the current round
  2. Start server: node server.js
  3. Run: ipconfig → note your IP address
  4. Open admin.html on your laptop → verify login works
  5. Connect projector → open team.html on projector browser
  6. Generate QR code for audience URL:
     → Go to https://qr.io in your browser
     → Enter: http://YOUR_IP:3000/evo.html
     → Project the QR code on screen
  7. Test one full question before audience joins

════════════════════════════════════════
KNOCKOUTS ROUND
════════════════════════════════════════

  1. Admin → Users → Add each competing team (username + password)
  2. Give each team their username + password on a slip of paper
  3. Teams open team.html on their tablet → login
  4. Admin starts questions from the admin panel
  5. Scoreboard on projector updates live
  6. After all questions → Admin → Announce Top 4
  7. Note which teams advance to Semis
  8. Stop server: Ctrl+C

════════════════════════════════════════
SEMIS ROUND
════════════════════════════════════════

  1. Start semis server: cd semis → node server.js
  2. Admin → Users → Add advancing teams
  3. Audience scans QR code → enters their name on evo.html
  4. Admin → Set Pair → assign Blue and Red team for the match
  5. Admin runs questions one by one
  6. Between questions → push sponsor ads to audience phones
  7. Teams can use "Ask The Audience" lifeline once
  8. After all matches → note the two finalists
  9. Stop server: Ctrl+C

════════════════════════════════════════
FINALS ROUND
════════════════════════════════════════

  1. Start finals server: cd final → node server.cjs
  2. Admin → Teams → Set Blue team name + logo
  3. Admin → Teams → Set Red team name + logo
  4. Admin runs questions (some may have video intros)
  5. Admin uses Buzzer buttons for Blue/Red
  6. Admin adjusts scores manually if needed
  7. Winner announced! 🏆
  8. Stop server: Ctrl+C

════════════════════════════════════════
AFTER THE EVENT
════════════════════════════════════════

  1. Press Ctrl+C to stop the server
  2. Back up the data folders to a USB drive
  3. Done!
```

---

## 6. Troubleshooting

### ❌ Tablets cannot connect
- Confirm all tablets are on the **same Wi-Fi** as your laptop
- Re-run `ipconfig` — IP address may have changed if you reconnected
- Test: on your laptop, open `http://localhost:3000` — if it loads, server is running
- Tell tablets to open `http://YOUR_IP:3000` (just the base URL) to confirm connection

### ❌ Server suddenly stops / crashes
```powershell
# Just restart it
node server.js
```
State is auto-saved to JSON files — tablets reconnect and pick up where they left off.

### ❌ Team cannot login
- Password might be wrong → Admin Panel → Delete that user → Re-add with new password
- Username is case-sensitive! `ANANDA` ≠ `ananda`

### ❌ Scores are wrong
- Admin Panel → Scores → manually set any team to any number
- Or: reset all to zero from the Scores panel

### ❌ Timer is stuck / question won't end
- Admin Panel → End Question button
- If that fails → restart server (state restores automatically)

### ❌ Admin password not working
```powershell
Remove-Item admin.json    # delete the password file
node server.js            # restart — default "admin123" is restored
```
Login with `admin123` → immediately set a new password.

### ❌ Audience sees blank screen
- Make sure they opened `evo.html` FIRST and entered their name
- Then go to `scoreboard.html`
- If still blank: refresh the page

### ❌ 40+ tablets slow to respond
- Reduce Wi-Fi interference — switch hotspot to 5GHz if available
- Close any other apps on your laptop
- Restart the server — it clears memory

---

## 7. Pre-Event Checklist

```
TECHNICAL
─────────────────────────────────────────────
□ Laptop charged and charger plugged in
□ Wi-Fi hotspot running (laptop hotspot or router)
□ Correct round's server started
□ Admin login verified
□ Projector connected showing team.html
□ Questions loaded and checked in admin panel
□ Scores reset to zero for all teams
□ Sessions cleared (fresh login required for all)
□ QR code generated for audience URL

CONTENT
─────────────────────────────────────────────
□ All team accounts created with known passwords
□ Team logos uploaded (if applicable)
□ All questions entered and reviewed
□ Sound effects tested (admin → SFX panel)
□ Sponsor ads uploaded (if semis)

ON THE DAY
─────────────────────────────────────────────
□ Run one full test question before live audience joins
□ Passwords written on slips of paper for each team
□ Someone assigned to help audience members scan QR code
□ Spare tablet/phone ready as backup
□ Know the Ctrl+C → node server.js restart procedure
```

---

## ⚡ Quick Reference Card
> *Print this and keep it at the event*

```
┌─────────────────────────────────────────────────┐
│           EVOLVION QUICK REFERENCE               │
├─────────────────────────────────────────────────┤
│ YOUR IP:      _________________________          │
│               (run: ipconfig in PowerShell)      │
├─────────────────────────────────────────────────┤
│ ADMIN:        http://______:3000/admin.html      │
│ TEAM SCREEN:  http://______:3000/team.html       │
│ AUDIENCE:     http://______:3000/scoreboard.html │
│ AUDIENCE JOIN:http://______:3000/evo.html        │
├─────────────────────────────────────────────────┤
│ ADMIN PASSWORD:   admin123  ← CHANGE THIS!       │
├─────────────────────────────────────────────────┤
│ START KNOCKOUTS:                                 │
│   cd knockouts  →  node server.js               │
│                                                  │
│ START SEMIS:                                     │
│   cd semis      →  node server.js               │
│                                                  │
│ START FINALS:                                    │
│   cd final      →  node server.cjs              │
├─────────────────────────────────────────────────┤
│ STOP SERVER:      Ctrl + C                       │
│ FIND IP:          ipconfig                       │
│ SERVER CRASH:     just run node server.js again  │
└─────────────────────────────────────────────────┘
```

---

*Evolvion Event Runbook — SSDSSC | Confidential*
