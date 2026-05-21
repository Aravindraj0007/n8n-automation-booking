# 📅 WhatsApp Appointment Automation — n8n Template

An AI-powered WhatsApp chatbot that handles doctor appointment bookings end-to-end. Built with **n8n**, **Google Gemini**, **Google Sheets**, and the **WhatsApp Business Cloud API**.

---

## 🧠 What It Does

Users message your WhatsApp Business number and the AI agent handles everything:

- **New Booking** — Registers patient (name, age, gender), shows available dates and 30-minute time slots, confirms before booking
- **View Upcoming Appointments** — Lists all future appointments for the user's WhatsApp number
- **Reschedule** — Lets users pick a new date/time for an existing appointment
- **Cancel** — Cancels a selected appointment and updates the sheet

The bot remembers conversation context (last 10 messages) so users don't have to repeat themselves.

---

## 🏗️ Architecture

```
WhatsApp Trigger
      ↓
  If (valid sender?)
      ↓
  AI Agent (Google Gemini)
      ├── Simple Memory (context window)
      ├── Date & Time Tool
      ├── doctor config (Google Sheets - Config tab)
      ├── get patient list for user
      ├── add patient
      ├── add appointment
      ├── get all appointment
      ├── get user appointment
      ├── reschedule appointment
      └── cancel appointment
      ↓
WhatsApp Business Cloud (send reply)
```

---

## 📋 Prerequisites

Before importing this workflow, you need:

| Service | Purpose |
|---|---|
| [n8n](https://n8n.io) | Workflow automation platform |
| WhatsApp Business Cloud API | Receiving and sending WhatsApp messages |
| Google Gemini API | AI agent (LLM) |
| Google Sheets + OAuth2 | Data storage for patients and appointments |

---

## 🚀 Setup Guide

### Step 1 — Import the Workflow

1. Open your n8n instance
2. Go to **Workflows → Import**
3. Upload `whatsapp_appointment_automation_public_template.json`

---

### Step 2 — Create Your Google Sheet

Create a new Google Sheet with **3 tabs** exactly as below:

**Tab 1: `Patients`**
| patient_id | whatsapp_number | name | age | gender |
|---|---|---|---|---|

**Tab 2: `Appointments`**
| appointment_id | patient_id | whatsapp_number | date | time | status |
|---|---|---|---|---|---|

**Tab 3: `Config`**
| key | value |
|---|---|
| not_available | 2025-01-15 14:00 to 15:30 |

The `not_available` key is used to block out doctor unavailability windows. Format: `YYYY-MM-DD HH:MM to HH:MM`

---

### Step 3 — Add Credentials

In n8n, go to **Credentials** and add:

1. **WhatsApp Trigger API** — Meta Developer App credentials for receiving messages
2. **WhatsApp Business Cloud API** — Meta credentials for sending messages
3. **Google Gemini (PaLM API)** — Your Gemini API key from Google AI Studio
4. **Google Sheets OAuth2** — Authorize n8n to access your Google Sheet

---

### Step 4 — Replace Placeholders

Search and replace the following in the workflow nodes:

| Placeholder | Replace With |
|---|---|
| `YOUR_GOOGLE_SHEET_ID` | Your Google Sheet's document ID (from the URL) |
| `YOUR_WHATSAPP_PHONE_NUMBER_ID` | Your WhatsApp Business phone number ID from Meta |
| `YOUR_WEBHOOK_ID` | Auto-generated after you activate the workflow |

After adding credentials, open each node and re-select the correct credential from the dropdown.

---

### Step 5 — Activate & Test

1. Click **Activate** on the workflow
2. Send a WhatsApp message to your registered number
3. The bot should respond with the main menu

---

## 🛠️ Tech Stack

- **n8n** — Workflow orchestration
- **Google Gemini** (via `lmChatGoogleGemini`) — AI agent LLM
- **n8n Window Buffer Memory** — Short-term conversation context (10 messages)
- **Google Sheets** — Patient and appointment data storage
- **WhatsApp Business Cloud API** — Messaging channel
- **n8n Date & Time Tool** — System time in `Asia/Kolkata` timezone

---

## 📁 Project Structure

```
whatsapp_appointment_automation_public_template.json   ← n8n workflow (this file)
README.md                                              ← You are here
```


