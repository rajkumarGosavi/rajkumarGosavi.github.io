---
title: "Suvarix"
date: "2026-07-15"
description: "An offline-first, encrypted personal-finance tracker for Indian investors — every rupee stays on your device."
tags: [tauri, rust, react, desktop, fintech]
draft: false
---

<p align="center">
  <img src="https://raw.githubusercontent.com/rajkumarGosavi/suvarix/main/src-tauri/icons/128x128%402x.png" alt="Suvarix" width="96" height="96">
</p>

<h1 align="center">Suvarix</h1>

<p align="center"><strong>Your money, on your machine.</strong><br>
An offline-first personal finance tracker for Indian investors — every rupee stays on your device, encrypted.</p>

<p align="center">
  <a href="https://github.com/rajkumarGosavi/suvarix/releases/latest" target="_blank" rel="noopener noreferrer"><strong>⬇️ Download latest</strong></a> ·
  <a href="https://github.com/rajkumarGosavi/suvarix/blob/main/USER_GUIDE.md" target="_blank" rel="noopener noreferrer">User Guide</a> ·
  <a href="https://github.com/rajkumarGosavi/suvarix/blob/main/PRIVACY.md" target="_blank" rel="noopener noreferrer">Privacy</a> ·
  <a href="https://github.com/rajkumarGosavi/suvarix/blob/main/EULA.md" target="_blank" rel="noopener noreferrer">Licence / EULA</a>
</p>

---

## Demo

<p align="center">
  <video controls preload="metadata" width="100%" poster="shot-dashboard-1.png" style="max-width:900px;border-radius:10px;border:1px solid rgba(128,128,128,.25);">
    <source src="demo.mp4" type="video/mp4">
    Your browser can't play embedded video. <a href="demo.mp4">Download the demo (MP4)</a> instead.
  </video>
</p>

## What it is

Suvarix tracks your whole net worth in one place — **9 asset classes** (equity, mutual funds, FDs, PPF/EPF, gold, crypto, bonds, real estate, insurance), income & expenses, loans & credit cards, net-worth history, allocation, and a 0–100 **Financial Health Score**.

- 🇮🇳 **Built for India** — ₹ Cr/L formatting, Indian tax rules, Indian brokers.
- 🔌 **Pull your holdings** — connect Zerodha, Upstox, or Angel One; import MF Central CAS / Groww CSV / bank statements (HDFC, ICICI).
- 🎯 **Stay on track** — goals, budgets, bill & FD-maturity reminders, savings streaks.
- ☁️ **Optional sync** — you own the folder (Dropbox / Drive / OneDrive); the snapshot is encrypted end-to-end.

## Offline & encrypted — the whole point

- **No account. No cloud backend. No telemetry.** There is no server to send your data to — because there isn't one.
- **Encrypted at rest** with SQLCipher (AES-256). Your master password is the key; it's never stored and never leaves the device.
- **Broker credentials get a second AES-256-GCM layer** on top, keyed by your master password.
- **Your data leaves only when you tell it to** — to a broker's own API, to fetch public prices, or into your own cloud folder as an encrypted file the developer can't read.

Full details: **[Privacy Policy →](https://github.com/rajkumarGosavi/suvarix/blob/main/PRIVACY.md)**

## Download

| Platform | File | Get it |
|---|---|---|
| **Windows 10/11** | `.msi` installer | [Download](https://github.com/rajkumarGosavi/suvarix/releases/latest) |
| **macOS** | `.dmg` (Apple Silicon / Intel) | [Download](https://github.com/rajkumarGosavi/suvarix/releases/latest) |
| **Linux** | `.AppImage` / `.deb` | [Download](https://github.com/rajkumarGosavi/suvarix/releases/latest) |
| **Android** | `.apk` (sideload) | [Download](https://github.com/rajkumarGosavi/suvarix/releases/latest) |

> The desktop app auto-updates itself once installed — every release is signed with the developer's update key and verified before it's applied.
> For Android you will need to install new updates manually as of now.

## "Is this safe? Windows warned me."

Short answer: **yes**, and here's the honest why.

Windows **SmartScreen** (and macOS Gatekeeper) show an "unknown publisher" warning for any app that hasn't yet paid for an OS code-signing certificate. It's a *reputation* prompt, **not** a virus detection. New independent apps all start here until trust builds up.

**To install on Windows:** on the SmartScreen dialog click **More info → Run anyway**.

**To install on macOS:** because the app isn't notarized yet, Gatekeeper blocks the first launch. Either:

- **Right-click** (or Control-click) the app in Applications → **Open** → **Open** again in the dialog; or
- if macOS says the app is *"damaged / can't be opened"*, clear the download quarantine flag once in Terminal:

  ```bash
  xattr -dr com.apple.quarantine /Applications/Suvarix.app
  ```

  Then open it normally. This only tells macOS you trust *this* file — it doesn't disable Gatekeeper.

Why you can trust it anyway:

- **Nothing phones home.** No telemetry, no analytics server, no account. Your finances never touch the internet unless *you* connect a broker or enable sync.
- **Every update is cryptographically signed** with a minisign key and verified by the app before installing — a tampered update is rejected automatically.
- **Verify your download** against the checksums published on each [release](https://github.com/rajkumarGosavi/suvarix/releases/latest) if you want to be sure the file wasn't altered in transit.
- **OS code signing is on the roadmap**, which will remove the SmartScreen prompt entirely.

## Screenshots

### Dashboard — net worth, health score & allocation

A single glance at net worth, a 0–100 Financial Health Score with the next best action, your emergency-fund progress, and an asset-allocation donut across all 9 classes.

<p align="center">
  <img src="shot-dashboard-1.png" alt="Dashboard — net worth, financial health score and emergency fund" width="48%">
  <img src="shot-dashboard-2.png" alt="Dashboard — investor journey and asset-allocation donut" width="48%">
</p>

### Portfolio — 9 asset classes in one place

Equity, mutual funds, FDs/RDs, bonds, PPF/EPF/NPS, real estate, gold, crypto and insurance — each with live P&L. Add a holding by symbol with ISIN lookup, or pull it from your broker.

<p align="center">
  <img src="shot-portfolio-equity.png" alt="Portfolio — equity holdings with live P&amp;L" width="48%">
  <img src="shot-portfolio-mf.png" alt="Portfolio — mutual fund holdings" width="48%">
  <img src="shot-portfolio-fd.png" alt="Portfolio — fixed deposits and RDs" width="48%">
  <img src="shot-portfolio-add-equity.png" alt="Add equity dialog with ISIN lookup" width="48%">
</p>

### Money in & out — transactions, income & expenses

A full ledger with buys, SIPs, dividends, EMIs and everyday spends — import a bank statement or add by hand. Monthly income-vs-expense trends and category breakdowns show where the money goes.

<p align="center">
  <img src="shot-transactions.png" alt="Transactions ledger with type, category and amount" width="48%">
  <img src="shot-income-expenses-1.png" alt="Income and expenses — monthly trend chart" width="48%">
  <img src="shot-income-expenses-2.png" alt="Income and expenses — category breakdown" width="48%">
</p>

### Goals & milestones

Track goals against your live portfolio value, and celebrate net-worth milestones from ₹1 Lakh to ₹10 Crore as you cross them.

<p align="center">
  <img src="shot-goals.png" alt="Goals — progress toward targets based on portfolio value" width="48%">
  <img src="shot-reminders-milestones.png" alt="Net-worth milestones, achieved and upcoming" width="48%">
</p>

### Liabilities & debt payoff

Loans and credit cards in one view, with an avalanche/snowball payoff planner that shows your debt-free date and the interest you'd save.

<p align="center">
  <img src="shot-liabilities.png" alt="Liabilities — loans, credit cards and debt payoff planner" width="70%">
</p>

### Never miss a bill — reminders & calendar

Upcoming bills, recurring transactions you can apply to the ledger in one click, and a calendar that lays out EMIs, card payments, FD maturities and goal dates.

<p align="center">
  <img src="shot-reminders-bills.png" alt="Reminders — upcoming bills with due dates" width="48%">
  <img src="shot-reminders-recurring.png" alt="Recurring transactions due to apply" width="48%">
  <img src="shot-calendar.png" alt="Calendar view of bills, EMIs and maturities" width="70%">
</p>

### Connect your brokers & import statements

Link Zerodha, Upstox or Angel One with your own API keys, or import CSVs and CAS files — the credentials stay encrypted on your device.

<p align="center">
  <img src="shot-data-sources-1.png" alt="Data Sources — connect Zerodha and Upstox" width="48%">
  <img src="shot-data-sources-2.png" alt="Data Sources — Angel One and CSV import" width="48%">
</p>

### Security & settings

Everything sits behind a master password. Auto-lock, encrypted backups, and optional encrypted sync to a folder you control.

<p align="center">
  <img src="shot-unlock.png" alt="Locked screen — master password to unlock" width="48%">
  <img src="shot-settings-1.png" alt="Settings — security, auto-lock and appearance" width="48%">
  <img src="shot-settings-2.png" alt="Settings — backup, restore and encrypted sync" width="48%">
</p>

## FAQ

**Do you see my data?** No. There's no backend. Everything is local and encrypted; even the optional cloud-sync file is encrypted with a password only you hold.

**What if I forget my master password?** It can't be recovered — not even by the developer. Keep a backup and remember it.

**Is it free?** Yes, free to use. It is not open-source; source is proprietary. See the [EULA](https://github.com/rajkumarGosavi/suvarix/blob/main/EULA.md).

**Investment advice?** None. Suvarix shows *your* numbers for information only. The developer is **not** a SEBI-registered adviser.

---

<p align="center"><sub>Suvarix is provided "as is", without warranty. Not financial, tax, or investment advice. © 2026.</sub></p>
