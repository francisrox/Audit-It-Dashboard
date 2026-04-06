# Diggibyte IT Audit Dashboard

> **IT Infrastructure & Security Audit Visualization Tool**
> Built by Diggibyte Technologies Private Limited

---

## Quick Start

1. Download all files into a **single folder**
2. Open `1_login.html` in your browser
3. Login with: **admin** / **diggibyte@2025**
4. Read the About page, then click **Enter Dashboard**
5. Click **Enter Data**, fill in survey responses, click **Generate Charts**
6. Click **Generate Report** for a full audit report

> **Important:** All 3 HTML files must be in the same folder. Do not rename them.

---

## Files in This Package

| File | Purpose |
|------|---------|
| `1_login.html` | Login page — start here |
| `2_about.html` | About Diggibyte Technologies |
| `3_dashboard.html` | Main audit dashboard |
| `README.md` | This file |
| `DOCUMENTATION.md` | Full technical documentation |
| `OVERVIEW.md` | Process summary and explanation |

---

## Requirements

- A modern web browser (Chrome, Edge, Firefox, Safari)
- Internet connection (for Chart.js and Google Fonts from CDN)
- No installation, no server, no database needed

---

## Features

- **Secure Login** — Session-based authentication
- **Company About Page** — Diggibyte profile and leadership team
- **30 Audit Questions** across 6 sections
- **Interactive Charts** — Pie, Doughnut, Bar, Gauge
- **Section Pages** — Each audit section has its own dedicated page
- **Auto Report** — Detailed report with warnings and recommendations
- **Print to PDF** — Export report via browser print dialog
- **Dark / Light Mode** — Toggle from any page
- **Responsive** — Works on desktop, tablet, and mobile

---

## Audit Sections

| # | Section | Questions |
|---|---------|-----------|
| 1 | Governance & Compliance | 5 |
| 2 | IT Infrastructure & Cloud Systems | 5 |
| 3 | Data Security & Protection | 5 |
| 4 | Access Control & Identity Management | 5 |
| 5 | Monitoring & Incident Management | 5 |
| 6 | Analytics & Dashboard Governance | 5 |

---

## How to Use

### Step 1 — Login
Open `1_login.html` and enter your credentials.

### Step 2 — About Page
Review company information. Click **Enter Dashboard** to proceed.

### Step 3 — Enter Survey Data
Click the **Enter Data** button. A modal opens with 6 tabs (one per section).
Enter the number of respondents for each option, then click **Generate Charts**.

### Step 4 — Explore Charts
- The **Dashboard Summary** shows 3 KPI cards and section overview cards
- Click any section card (or use the sidebar) to view its 5 charts
- Use the **← Back** button to return to the summary

### Step 5 — Generate Report
Click **Generate Report** (header or sidebar). The report shows:
- Executive summary with key metrics
- Section-by-section breakdown with bar charts
- Color-coded warnings for critical gaps
- Prioritized recommendations

Click **Print / Save PDF** to export.

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Page shows JavaScript code as text | Use the latest fixed `3_dashboard.html` |
| Cannot click "Enter Data" | Ensure you are viewing in Chrome or Edge (not IE) |
| Charts not showing | Check internet connection (Chart.js loads from CDN) |
| Data lost after refresh | This is expected — data is session-only, not stored |
| Login redirects loop | Clear browser cache and sessionStorage, reopen `1_login.html` |
| Fonts look wrong | Check internet connection for Google Fonts |

---

## Contact

**Diggibyte Technologies Private Limited**
- Phone: +91-8110889199
- Email: Info@diggibyte.com | sales@diggibyte.com
- Website: https://diggibyte.com
- Location: Bengaluru, Karnataka, India

---

*© 2025 Diggibyte Technologies Private Limited. All rights reserved.*
