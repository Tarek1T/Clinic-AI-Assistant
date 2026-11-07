<div align="center">

# Clinic AI Assistant

**A bilingual (Arabic / English) AI virtual receptionist for a medical complex.**
It answers patient questions from the clinic's own data, books / changes / cancels appointments in a real database, and gives the owner a password-protected dashboard.

![Python](https://img.shields.io/badge/Python-3-3776AB?logo=python&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-Frontend%20%2B%20Backend-FF4B4B?logo=streamlit&logoColor=white)
![Groq](https://img.shields.io/badge/LLM-Groq%20(gpt--oss--120b)-F55036)
![FAISS](https://img.shields.io/badge/RAG-FAISS-0467DF)
![SQLite](https://img.shields.io/badge/DB-SQLite-003B57?logo=sqlite&logoColor=white)
![Tests](https://img.shields.io/badge/tests-114%2F114%20passed-brightgreen)
![Languages](https://img.shields.io/badge/UI-English%20%7C%20Arabic%20(RTL)-blue)

![Patient chat]
<img width="1920" height="1040" alt="Clinic AI Assistant — Mozilla Firefox 11_7_2026 4_28_24 PM" src="https://github.com/user-attachments/assets/2594b329-e86f-430b-8380-4971e73c7e75" />
</div>

---

## Table of Contents

1. [Overview](#overview)
2. [Key Features](#key-features)
3. [Screenshots](#screenshots)
4. [System Architecture](#system-architecture)
5. [Everything Built in This Project](#everything-built-in-this-project)
6. [Booking Rules](#booking-rules)
7. [Guardrails, Privacy & Security](#guardrails-privacy--security)
8. [Data](#data)
9. [Testing & Evaluation](#testing--evaluation)
10. [Tech Stack](#tech-stack)
11. [Repository Structure](#repository-structure)
12. [Getting Started](#getting-started)
13. [Known Limitations](#known-limitations)
14. [Roadmap](#roadmap)

---

## Overview

**Al-Islah Medical Complex (Mansoura)** runs 25 clinical units. Reception staff answer the same questions about doctors, fees, services and policies all day, and manage appointments by phone and in person.

**Clinic AI Assistant** automates this work. Patients chat in **Arabic or English** (including Egyptian dialect and Arabic digits). The assistant:

- answers **only from the complex's own data** (Excel workbook + PDF patient guide) using RAG,
- routes a symptom ("my knee hurts") to the right clinic,
- shows each doctor's **real weekly sessions**, tells the fee, and **books, reschedules or cancels** appointments in a SQLite database,
- gives the owner a **password-protected dashboard** for bookings, doctors, fees, sessions, documents and reports.

> **Design principle:** the LLM *proposes* (intent + fields) while **validated tools and a database do the real work**. A booking is never invented by the model — only the appointment agent, through the scheduling tools, can confirm one.

---

## Key Features

| Area | What it does |
|---|---|
| **Patient chat** | Natural bilingual conversation with memory, suggested questions, Confirm / Cancel buttons and cited sources |
| **RAG** | Multilingual embeddings + FAISS: an Arabic question finds English data |
| **Smart booking** | Per-doctor sessions, turn numbers, availability filters (time of day, before/after, doctor), reschedule and cancel |
| **Symptom routing** | English / Arabic / Egyptian-dialect symptom words → the right department |
| **Guardrails** | Prompt-injection blocking, emergency redirect, no medical advice, numeric faithfulness check, no unauthorised booking claims |
| **Owner dashboard** | Overview charts, bookings (filter / export / cancel), editable doctors, fees & sessions, knowledge base upload, daily AI report, password change |
| **Automation** | Confirmation / cancellation / change notifications (simulated SMS outbox) and an LLM-summarised daily schedule report |
| **i18n** | Full English and Arabic UI with right-to-left layout |
| **Evaluation** | 114 automated tests across 14 categories — 100 % pass |

---

## Screenshots

### Patient interface

| Home / suggested questions | Chat with collapsed sidebar |
|:---:|:---:|
| ![Patient chat home]<img width="1920" height="1040" alt="Clinic AI Assistant — Mozilla Firefox 11_7_2026 4_28_24 PM" src="https://github.com/user-attachments/assets/62194cb7-7b31-48a9-91ab-a8c9b88633a7" />
 | ![Patient chat, collapsed sidebar]<img width="1920" height="1040" alt="Clinic AI Assistant — Mozilla Firefox 11_7_2026 4_29_50 PM" src="https://github.com/user-attachments/assets/387a063b-8f39-412f-ad74-94786175e9a6" />

The sidebar switches between **English / العربية** and between **Patient chat** and **Clinic dashboard**. The welcome screen offers one-click suggested questions (e.g. *Which clinic should I visit for knee pain?*).

### Owner dashboard (password protected)

#### 1 · Overview
KPIs (bookings today, upcoming, cancelled, patient messages, documents) and charts for patient requests by type, bookings by department and bookings by doctor.

![Dashboard overview]<img width="1920" height="1040" alt="Clinic AI Assistant — Mozilla Firefox 11_7_2026 4_29_00 PM" src="https://github.com/user-attachments/assets/1bc2953e-07d5-4d3c-951b-bb7841bc7151" />

#### 2 · Bookings
Date and status filters, a table with the weekday next to the date, the doctor session window, the patient's **turn number**, price snapshot, CSV export, and cancellation by booking code.

![Dashboard bookings]<img width="1920" height="1040" alt="Clinic AI Assistant — Mozilla Firefox 11_7_2026 4_29_12 PM" src="https://github.com/user-attachments/assets/1496af3e-bae2-4dcc-a0f2-2978e42f7dc7" />

#### 3 · Doctors, fees and sessions
The owner edits doctors (English / Arabic names), consultation fees and weekly sessions directly; the booking tools read these tables live.

![Dashboard doctors and fees]<img width="1920" height="1040" alt="Clinic AI Assistant — Mozilla Firefox 11_7_2026 4_29_20 PM" src="https://github.com/user-attachments/assets/d67c6f24-1ddf-41a2-ace3-abe253ff3b94" />

#### 4 · Knowledge base
Upload `.xlsx`, `.pdf`, `.txt` or `.md` files, or import a shared Google Sheet. Every file is chunked and indexed for retrieval, and each document can be removed.

![Dashboard knowledge base]<img width="1920" height="1040" alt="Clinic AI Assistant — Mozilla Firefox 11_7_2026 4_29_28 PM" src="https://github.com/user-attachments/assets/2f285046-e310-45aa-a4a7-f328508bb904" />

#### 5 · Reports
Daily schedule per date, CSV download, an **AI-generated summary**, and the notifications log (simulated SMS).

![Dashboard reports]<img width="1920" height="1040" alt="Clinic AI Assistant — Mozilla Firefox 11_7_2026 4_29_34 PM" src="https://github.com/user-attachments/assets/16fb3a71-e0ed-4059-a2f5-d113d04fe1c8" />

#### 6 · Security
Change the owner password (minimum 8 characters). Passwords are stored as salted PBKDF2 hashes.

![Dashboard security]<img width="1920" height="1040" alt="Clinic AI Assistant — Mozilla Firefox 11_7_2026 4_29_40 PM" src="https://github.com/user-attachments/assets/57304bc8-29d5-4e1c-a6ed-7cdcc1386be1" />

---

## System Architecture

```text
Patient message
      │
 Input guardrail      empty · too long · prompt injection · emergency words
      │
 Router agent         small model returns JSON (intent + fields); rules validate it
      │
      ├─► greeting · question · symptom · small talk
      │       └─► Retrieval (FAISS) ‖ Facts (DB)  ──► Answer agent (large model) ──► Output guardrail
      │
      └─► availability · book · cancel · reschedule · confirm
              └─► Appointment agent ──► Scheduling tools ──► SQLite
                  (a booking is never invented by the model)
      │
 Memory agent (last messages + summary) ──► conversation log (phone numbers masked)
```

### Design decisions

- **The answer model talks naturally** (greets back, routes a symptom to the right clinic, refuses unrelated topics) but **only from the clinic data** it is given. It cannot confirm a booking.
- **Every doctor has their own sessions.** A booking is for the doctor's session (e.g. 8:00 PM – 10:30 PM) and gets a **turn number**; the patient comes during the session.
- **Tools validate every action:** open session, free place, 14 days ahead, valid phone, one booking per phone per department per day. A **unique database index** makes double booking impossible.
- **No unknown answers:** if the answer is not in the data, the assistant refers the patient to the complex's phone number. A **numeric check** blocks any number that is not in the data.
- **Retrieval and facts run in parallel**; the FAISS index is rebuilt only when stored chunks change; there is no router call for simple yes/no replies.

### Models

| Role | Model | Notes |
|---|---|---|
| Answer agent | `openai/gpt-oss-120b` | temperature 0.3 |
| Router + memory summaries | `openai/gpt-oss-20b` | temperature 0 |
| Backup | `llama-3.3-70b-versatile` | automatic fallback on rate limit / error |
| Embeddings | `paraphrase-multilingual-MiniLM-L12-v2` | runs locally, no GPU needed |

---

## Everything Built in This Project

The notebook generates a small Python package, `clinic_ai`, plus a Streamlit app. Each module is one part of the system.

### 1. Core modules (`clinic_ai/`)

| Module | What was built |
|---|---|
| `config.py` | Central settings: model names, temperatures, chunk size/overlap, retrieval Top-K and min score, memory window, input limit, login lockout, clinic time zone (`Africa/Cairo`) |
| `database.py` | SQLite schema with **10 tables** (settings, departments, doctors, sessions, days off, documents, chunks, bookings, messages, notifications) + a **partial unique index** on confirmed bookings (`doctor, date, time`) so double booking is impossible, even under concurrent requests. PBKDF2 salted password hashing, phone normalisation, PII masking, dashboard statistics |
| `nlu.py` | Rule-based language layer that **validates and completes** what the LLM extracts: Arabic & English dates, times (with before/after filters), Egyptian phone numbers, booking codes, names, yes/no, department aliases, emergency and medical keywords, prompt-injection patterns. Handles Arabic digits and normalises Arabic letters/diacritics |
| `feature_engineering.py` | Deterministic feature layer: Arabic↔English vocabulary bridge, symptom → department mapping (English, Arabic, Egyptian dialect), entity extraction, and RAG-query enrichment using only matched clinic vocabulary (never invents facts) |
| `i18n.py` | Complete **English and Arabic** UI/assistant texts (stored as unicode escapes so the notebook stays ASCII), date/time formatting with Arabic day names |
| `tools.py` | **Scheduling tools** — the only code that reads/writes bookings: slot grid from weekly sessions, free-slot search with filters, session windows, turn numbers, booking, cancellation (24 h rule), rescheduling, patient-name and phone validation |
| `automation.py` | `NotificationAgent` (confirmation / cancellation / change messages to an outbox), `ReportAgent` (daily schedule table + LLM summary), dashboard bookings table |
| `loader.py` | Data retrieval: Excel workbook → database, PDF/TXT reading, **overlapping word chunking**, Google Sheets import, LLM summarisation of documents, and **auto-generated Patient Guide PDF** (ReportLab) |
| `rag.py` | `EmbeddingAgent` (multilingual sentence-transformers), `VectorDBAgent` (FAISS inner-product over L2-normalised vectors = cosine), `RetrievalAgent` (thread-safe, rebuilds index only on data change) |
| `llm.py` | Groq client with **model fallback list**, retry rounds, reasoning-token budget handling, and **phone-number masking before any text leaves the server** |
| `guardrails.py` | Input guardrail, emergency detection, output guardrail (emoji removal, unsafe medical claims, unauthorised booking claims, numeric faithfulness check) |
| `agents.py` | The multi-agent workflow: `MemoryAgent`, `RouterAgent`, `AppointmentAgent`, `AnswerAgent` and the `ClinicAssistant` orchestrator, which records a **timed trace of every step** |

### 2. Streamlit application (`app.py`)

- One app = front end + back end, **English / Arabic with RTL layout**.
- **Patient Chat** — conversation, suggested questions, Confirm/Cancel buttons for pending changes, answer sources, agent-workflow trace.
- **Clinic Dashboard** — 6 tabs: Overview · Bookings · Doctors, fees & sessions · Knowledge base · Reports · Security.
- Login protection: **5 failed attempts lock the login for 60 seconds**.
- Widget labels never change with language, so Streamlit keeps widget state when switching language.
- Public access through **ngrok** while the notebook is running.

### 3. Data work (`Clinic_AI_Final_Data.xlsx`)

The supplied clinic data was cleaned, completed and documented in a `Data_Audit` sheet:

- Restored phone numbers that had lost their leading zero.
- Unified inconsistent department names/ids into **one Departments table (D01–D25)** covering all 33 specialties.
- Created a **Doctors** sheet (37 doctors, one row per doctor) and gave **every session its own doctor**; overlapping sessions now belong to different doctors.
- Added missing fees and realistic weekly sessions for bookable departments; merged a duplicated *General Clinic* fee into *Family Medicine*; fixed a mis-filed pediatric physiotherapy session.
- Kept *Psychiatry* as **"coming soon"** (not bookable), matching the official policy.
- Added English/Arabic **aliases** so patient wording maps to a department.
- Added booking limits (14 days ahead, 60 min notice) and labelled official facts vs. demo values.
- Sheets: `Clinic`, `Departments`, `Doctors`, `Schedule`, `Specialties`, `Services`, `Devices`, `Offers`, `Policies`, `FAQ`, `Rights`, `Payment`, `Data_Audit`.

### 4. Knowledge base

| Source | Result |
|---|---|
| Excel workbook | **156 chunks**, 25 departments, 37 doctors, **158 weekly sessions** |
| Auto-generated PDF patient guide | **24 chunks** (Policies + FAQ + Rights + Payment) |
| Admin uploads | `.xlsx`, `.pdf`, `.txt`, `.md`, Google Sheets link |

### 5. Testing & evaluation

A 114-test suite on an **isolated test database** (real data restored afterwards), with a fixed clock for reproducible scheduling tests. See [Testing & Evaluation](#testing--evaluation).

### 6. Project requirements coverage

| # | Requirement | Implementation |
|---|---|---|
| 1 | Data retrieval from PDFs, Google Sheets, APIs, databases | Excel workbook, PDF patient guide, Google Sheets link, SQLite queried live by the scheduling tools |
| 2 | LLM processing & summarisation | Small model → JSON intent/fields; large model → grounded answers; document and daily-schedule summaries |
| 3 | Automated actions | Booking / cancellation / change notifications; daily report |
| 4 | Workflow orchestration | Guardrail → router → retrieval ‖ facts → answer / appointment agent → output guardrail → memory, with a timed trace |
| 5 | Presentation & explanation | Architecture above + presentation guide inside the notebook |
| 6 | Testing & evaluation | 114 automated tests (reliability, accuracy, efficiency) |
| + | Extra | Streamlit front/back end, Arabic & English UI, password-protected dashboard |

---

## Booking Rules

| Rule | Value |
|---|---|
| Booking window | up to **14 days** ahead |
| Minimum notice | **60 minutes** before the slot |
| Cancellation / change | not allowed inside **24 hours** of the slot |
| Slot length | **15 minutes**, up to **12 patients per session** |
| Per patient | one booking per phone, per department, per day |
| Phone validation | Egyptian mobile format |
| Double booking | blocked by a unique DB index (tested with 8 simultaneous requests) |
| Days off | owner-defined; a day off removes all slots |

---

## Guardrails, Privacy & Security

- **Input guardrail:** rejects empty input, input over 500 characters, control characters, and prompt-injection patterns (English + Arabic).
- **Emergency detection:** redirects emergency messages instead of chatting.
- **Output guardrail:** removes emojis; blocks diagnoses/prescriptions and unsafe dosage claims; blocks the answer model from claiming a booking; **every number of 3+ digits must exist in the clinic context**.
- **Administrative information only** — the assistant never gives medical advice.
- **Privacy:** phone numbers are masked in logs and **before text is sent to the Groq API**.
- **Security:** salted PBKDF2 password hash, login lockout, SQL-injection text handled safely, password hash hidden from settings views.

---

## Data

The workbook **`Clinic_AI_Final_Data.xlsx`** is the data source the notebook loads. It is **not included in this repository** — place it at `clinic_data/Clinic_AI_Final_Data.xlsx` (or upload it when the notebook asks).

> Official facts and **demo values** (doctor names, fees, weekly timetable, online booking) are labelled separately in the `Data_Audit` sheet. The official complex books in person only.

---

## Testing & Evaluation

**114 / 114 tests passed (100 %)** — 99 deterministic + 15 LLM-dependent.
Tests run on a separate database; categories and results:

| Category | Tests | Passed |
|---|:---:|:---:|
| Automation | 3 | 3 |
| Dashboard | 4 | 4 |
| Data | 9 | 9 |
| Feature Engineering | 2 | 2 |
| Information | 18 | 18 |
| Language | 4 | 4 |
| Memory | 5 | 5 |
| Privacy | 2 | 2 |
| Retrieval | 6 | 6 |
| Safety | 6 | 6 |
| Scheduling | 26 | 26 |
| Security | 3 | 3 |
| Validation | 5 | 5 |
| Workflow | 21 | 21 |
| **Total** | **114** | **114** |

| Dependency | Tests | Passed |
|---|:---:|:---:|
| Deterministic (DB, scheduling, rules, security) | 99 | 99 |
| LLM-dependent | 15 | 15 |

### Latency (end-to-end, milliseconds)

| Request type | Requests | Avg ms | Max ms |
|---|:---:|:---:|:---:|
| availability | 8 | 1612 | 6474 |
| blocked (guardrail) | 1 | 12 | 12 |
| book | 31 | 2888 | 7365 |
| cancel | 1 | 6699 | 6699 |
| confirm | 2 | 38 | 43 |
| decline | 1 | 6554 | 6554 |
| emergency | 1 | 12 | 12 |
| greeting | 4 | 2405 | 7483 |
| info | 20 | 1050 | 8049 |
| medical | 3 | 1220 | 1230 |
| other | 2 | 5777 | 10547 |
| reschedule | 1 | 6571 | 6571 |

Errors raised inside the assistant: **none**.

> A 100 % score applies to the executed test set, not to every possible patient question. Bookings use templates and database tools without the answer model, so they are fast; other messages need one router call and one answer call.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Python |
| UI | Streamlit (RTL support for Arabic) |
| LLMs | Groq API — `gpt-oss-120b`, `gpt-oss-20b`, `llama-3.3-70b-versatile` |
| Embeddings | `sentence-transformers` (multilingual MiniLM) |
| Vector search | FAISS (`faiss-cpu`) |
| Database | SQLite |
| Documents | `pypdf`, `reportlab`, `openpyxl`, `pandas` |
| Hosting (demo) | Google Colab + `pyngrok` |

---

## Repository Structure

```text
Clinic-AI-Assistant/
├── Clinic_AI_Assistant.ipynb   # Full project: code, data loading, tests, app launcher
├── requirements.txt
├── README.md
├── .gitignore
└── 01-...08-*.jpg              # Interface screenshots used in this README
```

Running the notebook generates (at runtime):

```text
clinic_ai/                      # config · database · nlu · feature_engineering · i18n ·
│                               # tools · automation · loader · rag · llm · guardrails · agents
app.py                          # Streamlit application
clinic_data/                    # workbook, generated Patient_Guide.pdf, clinic.db
```

---

## Getting Started

### Option A — Google Colab (recommended, no GPU needed)

1. Get a free **Groq API key** at [console.groq.com](https://console.groq.com).
2. Open `Clinic_AI_Assistant.ipynb` in Colab and add the key as a Colab secret named **`GROQ_API_KEY`** (or paste it when asked).
3. Create a free [ngrok](https://ngrok.com) account and copy your authtoken.
4. Upload `Clinic_AI_Final_Data.xlsx` when prompted (or place it in `clinic_data/`).
5. Run all cells from top to bottom. The last cells print the public website URL.

### Option B — Local

```bash
git clone https://github.com/<your-username>/Clinic-AI-Assistant.git
cd Clinic-AI-Assistant
pip install -r requirements.txt
export GROQ_API_KEY="your_key_here"
```

Run the notebook cells in section 2 to generate `clinic_ai/` and `app.py` (remove the `google.colab` upload/secret lines), then:

```bash
streamlit run app.py
```

### Demo owner login

The demo dashboard password is `clinic-demo-2026` (set in the notebook, section 4). **Change it from the Security tab** before any real use.

---

## Known Limitations

- The official complex books **in person only** — online booking, doctor names, fees and the weekly timetable are **project demo values**.
- Scanned PDFs without a text layer are not read.
- SQLite suits a demo; use PostgreSQL for production.
- Patient messages go to the Groq API (phone numbers are masked first).
- SMS and email are **simulated** (no provider connected).
- The assistant provides administrative information only — **never medical advice**.

---

## Roadmap

- PostgreSQL for production
- WhatsApp channel
- Real doctor calendars and names from the complex
- Payment link
- Evaluation set built from real patient questions

---

<div align="center">

**Clinic AI Assistant** — built with Python, Streamlit, Groq, FAISS and SQLite.

</div>
