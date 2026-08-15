# 🗓️ LeaveDesk

**A single-file, role-based HR leave management web app.**

LeaveDesk runs entirely in the browser as one HTML file. It ships with a full sign-in layer (employees, managers, HR admins), sex-aware leave types (maternity, paternity, sick, annual…), an approval workflow, an interactive dashboard, and a portable JSON backup model — so the whole workspace moves with you between machines.

![License: MIT](https://img.shields.io/badge/License-MIT-3f6de0)
![Status: Ready](https://img.shields.io/badge/Status-Ready-22a05d)
![Runs: Anywhere](https://img.shields.io/badge/Runs-Anywhere-182233)

---

## ✨ Features

### 🔐 Role-based access
Three sign-in roles, each with their own nav and permissions:

| Role | Capabilities |
|------|-------------|
| **Employee** | Request leave, view own balances, cancel pending requests, browse team calendar |
| **Manager** | Everything above + approve/reject requests from their direct reports |
| **HR admin** | Full system: dashboard, all requests, team CRUD, leave types, import/export, sign-in accounts |

### 🧬 Sex-aware leave types
Leave types can be gated by the sex recorded on the employee profile. The app ships with:

- **Maternity Leave** (90 days, women only)
- **Paternity Leave** (14 days, men only)
- Plus Annual, Sick, Personal and Unpaid — all fully editable

When an employee's profile doesn't match a type's restriction, that type is disabled in the request form with a clear hint, and mismatches are flagged in approval queues.

### 📊 Interactive dashboard (HR)
Four stat tiles that do something useful when clicked:

- **Pending approvals** → jumps to the Requests tab, filtered to pending
- **On leave today** → opens a modal listing everyone out today (drill-down to profiles)
- **Days booked · FY** → analytics modal: totals by leave type (with share bars) and by employee
- **Active team** → jumps to the Team tab

### 👤 People profiles
Click any name anywhere (dashboard queue, team table, request row, calendar chip) to open a wide profile modal showing:
- Role, department, manager, sex, join date
- FY stats (approved, pending, next leave)
- Per-type balance cards with animated usage bars
- Full history split into **currently on leave / upcoming / past**

### 🌓 Light & dark themes
A sun/moon toggle in the header. Preference is persisted, defaults to your OS setting, and every generated element (pills, calendar chips, balance bars, badges) adapts automatically.

### 💾 Portable JSON workspace
Everything — people, requests, leave types, sign-in accounts, passwords — lives in one `.json` file.
- **Export** downloads a timestamped backup
- **Import** via button or drag-and-drop restores the entire workspace
- Autosaves to `localStorage` on every change

### ⚙️ Everything else
- Modal back-stack (modals-in-modals step back cleanly)
- Animated count-up tiles, animated balance bars, reveal-on-load panels
- Calendar with per-day pills, weekend shading, monthly totals by type
- Per-person allowance overrides
- Passwords stored salted + hashed (never plaintext)
- Last HR admin can't be demoted/deleted (no lockouts)

---

## 🚀 Getting started

There is no build step. Just:

1. Save [`leavedesk.html`](leavedesk.html)
2. Open it in any modern browser

On first launch the app seeds a sample workspace with a demo team. Sign in with any of the accounts below (password is the same for all of them).

### 🧪 Default demo accounts

| Username | Password | Role |
|----------|----------|------|
| `chloe` | `demo123` | HR admin |
| `bruno` | `demo123` | Manager |
| `amara` | `demo123` | Employee |
| `farah` | `demo123` | Employee |
| `dmitri` | `demo123` | Employee |
| `jonah` | `demo123` | Employee |

To start from a blank slate, use **Settings & Data → Start fresh** — the setup screen will prompt you to create the first HR admin.

---

## 📦 Data model

Everything is stored under one key (`leavedesk.v1`) in `localStorage` and mirrored into the exported `.json`:

```jsonc
{
  "meta": { "app": "LeaveDesk", "version": 3, "savedAt": "ISO-8601" },
  "settings": {
    "companyName": "string",
    "year": 2026,
    "weekStartsMonday": true,
    "leaveTypes": [
      { "id": "annual", "name": "Annual Leave", "color": "#hex",
        "allowance": 20, "countsWeekends": false, "sexRestriction": null }
    ]
  },
  "employees": [
    { "id": "uuid", "name": "...", "sex": "female|male|''",
      "role": "employee|manager|hr", "managerId": "uuid|null",
      "username": "string", "pwHash": "hex", "pwSalt": "string",
      "overrides": { "annual": 23 } }
  ],
  "requests": [
    { "id": "uuid", "employeeId": "uuid", "typeId": "annual",
      "from": "YYYY-MM-DD", "to": "YYYY-MM-DD", "days": 5,
      "status": "pending|approved|rejected", "reason": "..." }
  ]
}
