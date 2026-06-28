---
title: "Suvarix — Personal Finance Tracker for Indian Investors"
date: "2026-06-27"
slug: "suvarix"
tags: [vue, tauri, rust, sqlite, fintech]
description: "An offline-first desktop app for tracking Indian equity, mutual funds, and more — all data stays on your device."
---

## What is it?

Suvarix is a privacy-first desktop app for Indian investors who want a single place to track all their assets, liabilities, income, and expenses — without handing data to any cloud service.

Every rupee of data lives in a local SQLite database on your machine. Nothing is ever sent to a server.

<!-- ![Suvarix dashboard demo](demo.gif) -->

---

## Is this for you?

If you track Zerodha holdings in one tab, SIP history in another, and FDs in a spreadsheet — Suvarix puts it all in one place, offline, no accounts needed.

---

## Why I built it

Most personal finance tools for Indian investors are either web apps that store your data on their servers, or spreadsheets that break when you add one more asset class. I wanted something that:

- Works fully offline
- Supports the full Indian asset landscape (Zerodha, MF Central CAS, FD, PPF, EPF, gold, real estate)
- Has a clean, fast UI with proper INR formatting (₹1.5Cr, ₹30L — not ₹15,000,000)
- Runs as a native desktop app, not a browser tab

---

## Key Features

**Portfolio tracking**
- Equity (NSE/BSE) — import from Zerodha or add manually
- Mutual funds — import from MF Central CAS (Summary + Detailed PDF)
- Fixed Deposits, PPF, EPF, Real Estate, Gold, Crypto, Insurance

**Net worth dashboard**
- Asset allocation donut chart
- Monthly income vs. expenses bar chart
- Net worth history snapshots

**Liabilities**
- Loan amortisation schedule (principal vs. interest per EMI)
- Credit card tracking

**Goals**
- Set financial targets (Home, Retirement, Emergency Fund, etc.) and track progress against your total portfolio

**Privacy**
- Master password + auto-lock
- AES-GCM encryption for broker credentials stored in SQLite
- No telemetry — all diagnostic data is local and opt-in to export

---

## Screenshots

{{< figure src="screenshot-dashboard.png" alt="Net worth dashboard with asset allocation chart" >}}

{{< figure src="screenshot-equity.png" alt="Portfolio view showing equity holdings" >}}
{{< figure src="screenshot-mf.png" alt="Portfolio view showing mutual fund holdings" >}}

{{< figure src="screenshot-goals.png" alt="Goals tracking with progress toward financial targets" >}}

---

## Tech Stack

| Layer | Tech |
|---|---|
| Frontend | Vue 3, Pinia, Vue Router, PrimeVue 4, Chart.js |
| Desktop shell | Tauri 2 (Rust + WebView2) |
| Backend | Rust (Tauri commands) |
| Database | SQLite via sqlx (WAL mode, 11 migrations) |
| Build | Vite, pnpm |

The data flow is strictly one-way: the Vue frontend never touches SQLite directly. All reads and writes go through Tauri `invoke()` calls mapped to typed Rust commands.

---

## Try Suvarix

Beta is available for Windows, macOS, and Linux. No account needed. Your data never leaves your machine.

[Download Beta](https://github.com/rajkumarGosavi/suvarix/releases) · [View Source on GitHub](https://github.com/rajkumarGosavi/suvarix)
