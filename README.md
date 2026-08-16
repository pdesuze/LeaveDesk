# LeaveDesk

**A single-file HR leave management web app.** No build step, no backend, no dependencies — just open one HTML file and manage your team's time off.

[![License: MIT](https://img.shields.io/badge/License-MIT-3f6de0)](LICENSE)
[![Runs anywhere](https://img.shields.io/badge/Runs-Anywhere-0f8a8a)](#)
[![Version](https://img.shields.io/badge/version-1.0.0-182233)](#)

---

## Overview

LeaveDesk is a complete leave-management system that runs entirely in the browser. All data — employees, requests, leave types, policies, and even the audit trail — lives in `localStorage` and can be exported to / imported from a portable `.json` file.

It ships with a demo workspace and three sign-in roles, so you can explore the full workflow out of the box: **employees** request leave, **managers** approve their team's requests, and **HR** administers everything.

---

## ✨ Features

### 🔐 Roles & Sign-in
| Role | Capabilities |
|---|---|
| **Employee** | Request leave, view own balances, cancel pending requests, browse the team calendar |
| **Manager** | Everything above **+** approve/reject requests from direct reports, view team balances & reports |
| **HR admin** | Full administration: team CRUD, leave types, policies, holidays, reports, audit log, import/export |

### 🗓️ Leave Management
- **Sex-restricted leave types** — Maternity (female-only) and Paternity (male-only), plus Annual, Sick, Personal, and Unpaid.
- **Half-days & hourly leave** — request full days, half-days (0.5), or custom hours per day.
- **Public holidays** — declare company holidays; they're skipped in day calculations and shown on the calendar.
- **Accrual & carryover** — per-type rules: annual vs. monthly accrual, join-date pro-ration, and capped carryover into the next year.
- **Attachments** — attach supporting documents (e.g. doctor's notes, ≤2 MB) to any request.
- **Approval delegation** — when a manager is on leave, their delegate can approve requests on their behalf.

### 📊 Dashboard & Reports
- **Interactive dashboard** — clickable stat tiles (Pending approvals, On leave today, Days booked, Active team) that drill into detail views.
- **Five report types** — Overview, Balances, Trends, Departments, and Absence Register.
- **Export to PDF** — clean, print-optimised documents with headings and data tables (not screenshots).
- **Export to CSV** — every report also downloads as spreadsheet-ready CSV.

### 🧾 Governance
- **Full audit log** — every request decision, employee edit, policy change, and password reset is recorded with actor, timestamp, and details.
- **Light & dark themes** — respects your OS preference, switchable anytime, persisted per browser.

### 💾 Data & Portability
- **Autosave** — every change is saved to `localStorage` automatically.
- **JSON export/import** — back up or move your entire workspace via a single `.json` file (drag-and-drop supported).

---

## 📸 Screenshots

<!-- Replace these with your own screenshots once captured -->
<!-- ![Dashboard](docs/screenshots/dashboard.png) -->
<!-- ![Reports](docs/screenshots/reports.png) -->

> _Screenshots coming soon. Run the app and capture the Dashboard, Reports, and Calendar views._

---

## 🚀 Quick Start

1. **Download or clone** the repository.
2. **Open `leavedesk.html`** in any modern browser (double-click it, or drag it into a browser window).
3. **Sign in** with one of the demo accounts below.

That's it — no server, no install, no build.

### Demo Accounts

All demo accounts use the password **`demo123`**.

| Username | Password | Role |
|---|---|---|
| `chloe` | `demo123` | HR admin |
| `bruno` | `demo123` | Manager |
| `amara` | `demo123` | Employee |
| `dmitri` | `demo123` | Employee |
| `farah` | `demo123` | Employee |
| `jonah` | `demo123` | Employee |

> **Locked out?** Click **"Reset all passwords to demo123"** on the sign-in screen, or use **Settings → Danger zone → Reset passwords** when signed in.

---

## 🧭 Usage by Role

### As an Employee
1. Go to **My Leave** and click **Request leave**.
2. Pick a leave type, date range, and duration (full / half / hours).
3. Optionally add a reason and an attachment, then submit.
4. Track the request status and your remaining balance on the same screen.

### As a Manager
1. Open **Approvals** to see pending requests from your direct reports.
2. Review each request's projected balance, then **Approve** or **Reject**.
3. If you'll be away, HR can assign a **delegate approver** to your reports.

### As HR
1. **Team** — add/edit people, set roles, managers, delegates, and sign-in credentials.
2. **Requests** — review every request across the company.
3. **Reports** — generate and export PDF/CSV reports.
4. **Settings** — manage leave types, policies, holidays, and the audit log.
5. **Data & backup** — export/import the workspace as JSON.

---

## 🗂️ Data & Backup

All data is stored under a single `localStorage` key and mirrored to the `.json` you export. The schema is versioned:

```jsonc
{
  "meta":      { "app": "LeaveDesk", "version": 5, "savedAt": "…" },
  "settings":  { "companyName": "…", "year": 2025, "holidays": […], "leaveTypes": [ … ] },
  "employees": [ { "id", "name", "sex", "role", "managerId", "delegateManagerId", "username", … } ],
  "requests":  [ { "id", "employeeId", "typeId", "from", "to", "days", "durationType", "attachments", "status", … } ],
  "audit":     [ { "id", "at", "actorName", "action", "after", … } ]
}
