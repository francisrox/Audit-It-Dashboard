# Diggibyte Technologies — IT Audit Dashboard
## Technical Documentation

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [File Structure](#2-file-structure)
3. [Technology Stack](#3-technology-stack)
4. [File Descriptions](#4-file-descriptions)
   - 4.1 Login Page (1_login.html)
   - 4.2 About Page (2_about.html)
   - 4.3 Dashboard Page (3_dashboard.html)
5. [Authentication Flow](#5-authentication-flow)
6. [Data Entry & Survey Schema](#6-data-entry--survey-schema)
7. [Chart Types & Rendering](#7-chart-types--rendering)
8. [Report Generation](#8-report-generation)
9. [Navigation System](#9-navigation-system)
10. [Theme System](#10-theme-system)
11. [Responsive Design](#11-responsive-design)
12. [Browser Compatibility](#12-browser-compatibility)
13. [Known Limitations](#13-known-limitations)
14. [Customization Guide](#14-customization-guide)

---

## 1. Project Overview

The **Diggibyte IT Infrastructure & Security Audit Dashboard** is a fully client-side, single-page web application built to visualize and report IT infrastructure and security audit data collected from organizational surveys.

It enables IT auditors and managers to:
- Enter manual survey response counts across 30 audit questions
- Visualize results as interactive charts (pie, doughnut, bar, gauge)
- Generate detailed audit reports with recommendations
- Navigate between 6 audit sections independently

The application requires no server, database, or backend — it runs entirely in a web browser from local HTML files.

---

## 2. File Structure

```
diggibyte-audit/
├── 1_login.html        # Login / authentication page (Entry point)
├── 2_about.html        # Company about page (Shown after login)
├── 3_dashboard.html    # Main audit dashboard (Core application)
├── README.md           # Setup and usage guide
├── DOCUMENTATION.md    # This file — technical reference
└── OVERVIEW.md         # Process summary and explanation
```

> All files must reside in the **same folder** for inter-page navigation to work correctly.

---

## 3. Technology Stack

| Component         | Technology                          |
|-------------------|-------------------------------------|
| Markup            | HTML5                               |
| Styling           | CSS3 (Custom Properties / Variables)|
| Scripting         | Vanilla JavaScript (ES5 compatible) |
| Charts            | Chart.js v4.4.1 (CDN)              |
| Fonts             | Google Fonts — Syne, JetBrains Mono |
| Session Storage   | Browser sessionStorage API          |
| No frameworks     | No React, Vue, Angular, or jQuery   |
| No build tools    | No npm, webpack, or bundlers needed |

---

## 4. File Descriptions

### 4.1 Login Page — `1_login.html`

**Purpose:** First point of entry. Validates user credentials before granting access.

**Key Features:**
- Animated background orbs (CSS radial gradients + keyframes)
- Card slide-up animation on load
- Username and password fields with icon indicators
- Shake animation on failed login attempt
- Stores auth token in `sessionStorage` on success
- Redirects to `2_about.html` on successful login
- Auto-redirects to `2_about.html` if already authenticated

**Credentials (default):**
- Username: `admin`
- Password: `diggibyte@2025`

**To change credentials:** Edit the login check condition in the JavaScript section:
```javascript
if (u === 'admin' && p === 'diggibyte@2025') {
```

---

### 4.2 About Page — `2_about.html`

**Purpose:** Displays Diggibyte Technologies company information after login, before entering the dashboard.

**Key Features:**
- Auth-guarded — redirects unauthenticated users to login
- Displays logged-in username in the header
- Company hero section with animated stats
- Sections: Who We Are, Vision & Mission, Industries, Services, Leadership Team, Contact
- "Enter Dashboard" CTA button navigates to `3_dashboard.html`
- Logout button clears session and returns to login

**Data Source:** All company content is hardcoded from Diggibyte's official website (https://diggibyte.com/).

---

### 4.3 Dashboard Page — `3_dashboard.html`

**Purpose:** The main application — audit data entry, chart visualization, and report generation.

**Key Features:**
- Auth-guarded — redirects to login if not authenticated
- Sidebar navigation with 6 audit sections + actions
- Dashboard Summary page with 3 KPI cards and section overview cards
- Each audit section opens as a separate page (not scrolled)
- 30 charts across 6 sections (pie, doughnut, bar, gauge)
- Modal-based data entry with 6 tabbed sections
- Auto-generated detailed audit report with recommendations
- Print / Save PDF functionality for reports
- Dark/Light theme toggle
- Responsive layout for mobile and tablet
- All JavaScript is wrapped in IIFE for scope safety
- No inline onclick attributes — all events wired via addEventListener

---

## 5. Authentication Flow

```
User opens 1_login.html
        │
        ▼
Enters username + password
        │
        ├── INVALID → Show error, clear password field, shake animation
        │
        └── VALID → sessionStorage.setItem('db_auth', 'true')
                            sessionStorage.setItem('db_user', username)
                                    │
                                    ▼
                            Redirect to 2_about.html
                                    │
                                    ▼
                    User clicks "Enter Dashboard"
                                    │
                                    ▼
                            3_dashboard.html
                    (checks sessionStorage on load)
                                    │
                    ┌───────────────┴───────────────┐
                    │ Not authenticated             │ Authenticated
                    ▼                               ▼
            Redirect to 1_login.html        Load dashboard
```

**Session Storage Keys:**
- `db_auth` — value `"true"` when authenticated
- `db_user` — stores the logged-in username

**Logout:** Clears both keys and redirects to `1_login.html`.

---

## 6. Data Entry & Survey Schema

The dashboard covers **30 questions** across **6 audit sections**.

### Section Structure

Each section is defined in the `SCHEMA` array with:
- `id` — unique section identifier
- `title` — display name
- `questions` — array of question objects

### Question Object Properties

| Property  | Description                             |
|-----------|-----------------------------------------|
| `id`      | Unique question identifier (e.g. `q_cf`)|
| `label`   | Display name of the question            |
| `type`    | Chart type: `pie`, `doughnut`, `bar`, `gauge` |
| `opts`    | Array of response option labels         |
| `isGauge` | Boolean — true for gauge charts only    |
| `max`     | Maximum value for gauge (default: 5)    |

### Survey Sections Summary

| Section | Title                              | Questions |
|---------|------------------------------------|-----------|
| S1      | Governance & Compliance            | 5         |
| S2      | IT Infrastructure & Cloud Systems  | 5         |
| S3      | Data Security & Protection         | 5         |
| S4      | Access Control & Identity Mgmt     | 5         |
| S5      | Monitoring & Incident Management   | 5         |
| S6      | Analytics & Dashboard Governance   | 5         |

### Data Entry Modal

- Opens via "Enter Data" button or sidebar link
- 6 tabs — one per audit section
- Each question shows options with a number input box aligned right
- Number inputs are properly separated from labels (alignment fixed)
- Data persists in the `surveyData` JavaScript object during the session
- "Sample Data" button loads realistic demo values for testing

---

## 7. Chart Types & Rendering

### Chart.js Integration

Charts are rendered using **Chart.js v4.4.1** loaded from Cloudflare CDN:
```
https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js
```

### Chart Type Details

| Type     | Used For                          | Notes                              |
|----------|-----------------------------------|------------------------------------|
| Pie      | Distribution/proportion questions | Shows legend below, hover offset   |
| Doughnut | Certification distribution        | Same as pie, with center cutout    |
| Bar      | Frequency/count questions         | Horizontal x-axis, rounded corners |
| Gauge    | Maturity rating (single score)    | Half-doughnut, color-coded by score|

### Gauge Color Coding

| Score Range | Color  | Meaning  |
|-------------|--------|----------|
| 4.0 – 5.0   | Green  | Strong   |
| 3.0 – 3.9   | Amber  | Moderate |
| 0.0 – 2.9   | Red    | Weak/Critical |

### Lazy Rendering

Charts are rendered **only when their section page is viewed** to improve performance. Each time a section is navigated to, its charts are re-rendered via `setTimeout(fn, 60ms)` to ensure the DOM is ready.

---

## 8. Report Generation

The report is generated entirely client-side by iterating over `surveyData`.

### Report Structure

1. **Header** — Company name, generation date/time, prepared by
2. **Executive Summary** — 4 key metrics (maturity score, cloud adoption, encryption, MFA)
3. **Per-Section Analysis** — For each of the 6 sections:
   - Per-question data with text bar charts
   - Top response indicator
   - Context-specific warnings (⚠) where critical gaps detected
4. **Overall Recommendations** — Priority-ranked action items
5. **Footer** — Contact details

### Warning Triggers

| Question            | Warning Condition                          |
|---------------------|--------------------------------------------|
| Cybersecurity Framework | Any org with "None" selected           |
| Compliance Frequency    | Any "Never" responses                  |
| Encryption              | Any "None" or "Partial" responses      |
| Backup Frequency        | Any "No Backup" responses              |
| Authentication Method   | Any "Password Only" responses          |
| Vulnerability Scanning  | Any "Never" responses                  |
| Incident Response Team  | Any "None" responses                   |
| Log Monitoring          | Any "Not Monitored" responses          |

### Print / PDF

The "Print / Save PDF" button opens a new window with clean formatted report content and triggers the browser's print dialog. Users can select "Save as PDF" in the print dialog.

---

## 9. Navigation System

### Page-Based Navigation (SPA Pattern)

The dashboard uses a **single-page application pattern** without a router:
- All section pages are `<div class="pg">` elements
- Active page has class `active` (visible via CSS `display:block`)
- All other pages have `display:none` (CSS controlled)
- Navigation fires `showPage(id)` which toggles `active` class

### Section Cards (Summary Page)

After data is entered, the Summary page shows clickable overview cards for each section. Clicking a card navigates directly to that section's page.

### Back Button

Each section page has a "← Back" button that returns to the Dashboard Summary page.

---

## 10. Theme System

The dashboard supports **Dark** and **Light** themes using CSS Custom Properties.

### Toggle Behavior

- Theme button in the header toggles `data-theme` attribute on `<html>`
- `[data-theme="light"]` overrides the default dark color variables
- Charts are re-rendered after theme toggle to apply new background/tick colors

### Color Variables

| Variable     | Dark Mode   | Light Mode  |
|--------------|-------------|-------------|
| `--bg`       | `#0a0e1a`   | `#f0f4fb`   |
| `--card`     | `#141c2e`   | `#ffffff`   |
| `--text`     | `#e8edf5`   | `#0f172a`   |
| `--border`   | `#1e2d45`   | `#d0daea`   |
| `--primary`  | `#2563eb`   | `#2563eb`   |
| `--orange`   | `#f97316`   | `#f97316`   |

---

## 11. Responsive Design

| Breakpoint    | Behavior                                           |
|---------------|----------------------------------------------------|
| > 900px       | Full sidebar + main layout (side-by-side)          |
| ≤ 900px       | Sidebar hidden, hamburger menu button shown        |
| ≤ 500px       | Summary cards stack to single column               |

### Mobile Sidebar

- Hamburger (☰) button appears on screens ≤ 900px
- Clicking opens sidebar with a dark backdrop overlay
- Clicking backdrop or any nav item closes sidebar automatically

---

## 12. Browser Compatibility

| Browser           | Support Status    | Notes                              |
|-------------------|-------------------|------------------------------------|
| Google Chrome     | ✅ Full support   | Recommended                        |
| Microsoft Edge    | ❌ Not supported  |                                    |
| Mozilla Firefox   | ✅ Full support   |                                    |
| Safari            | ✅ Full support   |                                    |
| Internet Explorer | ❌ Not supported  | ES6+ features used                 |

### Edge-Specific Fixes Applied

The previous version had issues in Microsoft Edge because:
1. Inline `onclick` attributes conflicted with Edge's strict Content Security Policy
2. Email addresses in HTML were being mangled by Cloudflare's email obfuscation proxy
3. Template literals caused issues in some Edge versions

**Fixes applied in v2:**
- All event handlers moved to `addEventListener()` (no inline onclick)
- All email addresses removed from HTML (only in JS strings)
- Special characters use HTML entities (`&amp;`, `&middot;`, etc.)
- Entire JS wrapped in IIFE `(function(){ ... })()` for scope safety
- ES5-compatible syntax used throughout

---

## 13. Known Limitations

1. **No data persistence** — Data is lost on page refresh (held in JS variable, not localStorage)
2. **No backend** — Cannot save reports to a database or send emails
3. **Single user** — Credentials are hardcoded; no multi-user support
4. **Online required** — Chart.js and fonts are loaded from CDN (needs internet)
5. **Session only** — sessionStorage clears when the browser tab is closed

---

## 14. Customization Guide

### Change Login Credentials

In `1_login.html`, find:
```javascript
if (u === 'admin' && p === 'diggibyte@2025') {
```
Change to your desired username and password.

### Add a New Audit Question

In `3_dashboard.html`, find the SCHEMA array and add to the relevant section's `questions` array:
```javascript
{id:'q_new', label:'New Question Label', type:'bar', opts:['Option A','Option B','Option C']}
```

### Add a New Audit Section

Add a new object to the `SCHEMA` array:
```javascript
{id:'s7', title:'New Section Title', questions:[ /* questions */ ]}
```
Also add a nav item and icon in `ICONS` array.

### Change Company Branding

Update the SVG logo, company name text, and tagline in all three HTML files.

### Make Data Persistent

To save data across refreshes, replace `var surveyData = {}` with:
```javascript
var surveyData = JSON.parse(localStorage.getItem('db_survey') || '{}');
```
And after each update, add:
```javascript
localStorage.setItem('db_survey', JSON.stringify(surveyData));
```

---

*Documentation version: 1.0 | Diggibyte Technologies | 2025*
