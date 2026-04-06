# Diggibyte IT Audit Dashboard — Process Overview

## Complete Explanation of What This System Does and How It Works

---

## What Is This System?

The **Diggibyte IT Infrastructure & Security Audit Dashboard** is a web-based tool designed for IT auditors and managers at Diggibyte Technologies or their clients to visualize the results of an IT security survey.

Instead of reading raw numbers from a spreadsheet, users can enter survey response counts and instantly see them transformed into interactive charts, graphs, and a comprehensive audit report — all inside a browser, with no installation required.

---

## The Problem It Solves

When an organization conducts an IT audit using a Google Form or paper survey, they collect data like:

> "How many organizations use ISO 27001 as their cybersecurity framework?"
> Answer: ISO 27001 = 4, NIST = 3, COBIT = 2 ...

This raw data is hard to interpret. The dashboard solves this by:
1. Accepting these counts through a simple form
2. Converting them into visual charts automatically
3. Analyzing patterns and flagging risks
4. Generating a complete written report with recommendations

---

## How the System Is Structured

The system is split into **3 separate HTML pages** that work as a connected flow:

```
1_login.html  →  2_about.html  →  3_dashboard.html
   (Login)         (About)           (Dashboard)
```

Each page is a self-contained HTML file. They communicate using the browser's `sessionStorage`, which acts like a temporary pass — when you log in, a pass is created. Each subsequent page checks for this pass before showing content. If the pass doesn't exist, you're sent back to login.

---

## The Complete User Flow

### Phase 1 — Authentication (1_login.html)

When you open the system, the login page is the first thing you see.

You enter your **username** and **password**. The system checks these against the stored credentials. If correct, it:
- Creates a session pass (`sessionStorage.setItem('db_auth', 'true')`)
- Remembers your username for display later
- Smoothly redirects you to the About page

If the credentials are wrong, the form shakes and shows an error message. The password field is cleared for security.

**Why sessionStorage?** Unlike cookies or localStorage, sessionStorage is automatically wiped when the browser tab is closed. This means every new session requires a fresh login — appropriate for a confidential audit tool.

---

### Phase 2 — Company Introduction (2_about.html)

After login, you land on the About page. This serves two purposes:

1. **Branding** — Presents Diggibyte Technologies' profile, services, leadership team, and contact details. It establishes context for who is running this audit system.

2. **Gateway** — Provides a clear path into the dashboard. You can read through the company information and then click **Enter Dashboard** when ready.

The About page also performs an **auth check** on load. If someone tries to navigate directly to `2_about.html` without logging in, they're immediately redirected to `1_login.html`.

---

### Phase 3 — The Dashboard (3_dashboard.html)

This is the core of the system. It has three main functional areas:

#### Area 1: Dashboard Summary

The first page you see shows:
- **3 KPI Cards** at the top: Security Score, Cloud Adoption Rate, Data Protection Level
- **6 Section Overview Cards**: Clickable cards showing each audit section — click one to go directly to that section's charts

These KPI values are automatically calculated from the data you enter:
- **Security Score** = Cybersecurity maturity rating converted to percentage
- **Cloud Adoption** = Percentage of respondents using any cloud platform
- **Data Protection** = Percentage with full encryption enabled

#### Area 2: Audit Section Pages

Each of the 6 audit sections has its **own dedicated page** — when you click a section, you see only that section's 5 charts. No other sections are shown, making it clean and focused.

Each section page has:
- A heading with section number and icon
- A **← Back** button to return to the summary
- 5 charts visualizing the 5 questions in that section

Charts are only rendered when you visit that section (lazy loading), so the dashboard stays fast.

#### Area 3: Data Entry Modal

Clicking **Enter Data** opens a popup window with 6 tabs — one per section. Each tab shows all 5 questions for that section. Each question shows its response options with a number input box next to each option.

You type in how many survey respondents chose each option. For example:
```
Cybersecurity Framework Distribution:
  ISO 27001  [  4  ]
  NIST       [  3  ]
  COBIT      [  2  ]
  CIS        [  2  ]
  Multiple   [  3  ]
  None       [  1  ]
```

When you click **Generate Charts**, all charts update instantly. You can also load **Sample Data** to see how the dashboard looks with pre-filled realistic values.

---

## The 30 Audit Questions

The system covers the following areas through 30 specific questions:

### Section 1: Governance & Compliance (5 questions)
Examines what cybersecurity frameworks and compliance certifications are in use, how often compliance reports are generated, the maturity of data governance policies, and the overall cybersecurity maturity rating.

### Section 2: IT Infrastructure & Cloud Systems (5 questions)
Covers where data is stored (on-premise vs cloud), which cloud platforms are used, what analytics tools are deployed, data migration strategies, and how dashboard access is controlled.

### Section 3: Data Security & Protection (5 questions)
Assesses encryption coverage, backup frequency, data retention policies, data quality monitoring practices, and which data governance tools are in use.

### Section 4: Access Control & Identity Management (5 questions)
Reviews the access control model (RBAC, ABAC, etc.), authentication methods (MFA, SSO, passwords), how often access is reviewed, who can modify dashboards, and whether user activity is logged.

### Section 5: Monitoring & Incident Management (5 questions)
Looks at monitoring tools in use (SIEM, Splunk, etc.), how frequently logs are monitored, how incidents are reported, the structure of incident response teams, and vulnerability scan frequency.

### Section 6: Analytics & Dashboard Governance (5 questions)
Examines data sources feeding dashboards, how frequently dashboards refresh, AI/ML usage in analytics, who is responsible for data validation, and how often dashboards are audited.

---

## How Charts Are Generated

Once you enter data and click "Generate Charts":

1. The system reads the values from every input field
2. For each question, it creates a Chart.js chart of the appropriate type:
   - **Pie chart** → proportional distribution (e.g., framework adoption)
   - **Doughnut chart** → same as pie, used for certifications
   - **Bar chart** → counts per category (e.g., backup frequency)
   - **Gauge chart** → single score displayed as a half-circle (maturity rating)
3. Charts are color-coded using a consistent 10-color palette
4. Hovering over chart sections shows exact values and percentages
5. Charts adapt to Dark/Light mode when you toggle the theme

---

## How the Report Works

Clicking **Generate Report** produces a structured written report built from your data:

1. **It calculates key metrics** — maturity score as %, cloud adoption %, encryption %, MFA %
2. **It processes each question** — sorts responses by count, shows text bar charts
3. **It flags risks automatically** — checks specific conditions like:
   - Any organization with no cybersecurity framework → Warning
   - Any organization with no backup → Critical warning
   - Password-only authentication → Warning
   - No vulnerability scanning → Warning
4. **It generates recommendations** — based on the overall scores and gaps found
5. **It outputs a formatted report** — with color-coded text (orange headings, amber warnings, green positives)

The **Print / Save PDF** button opens a clean print-friendly version of the report that you can save as PDF using your browser's print dialog.

---

## Security Considerations

- **No data leaves the browser** — everything is processed locally
- **Session-based auth** — session clears when tab closes
- **No data stored on disk** — data exists only in RAM during the session
- **Credentials are hardcoded** — suitable for internal use; should be updated before broader deployment

---

## What Happens When You Logout

Clicking Logout from any page:
1. Removes `db_auth` and `db_user` from sessionStorage
2. Redirects to `1_login.html`
3. Any unsaved survey data is lost (since it was only in memory)

---

## Summary of Key Design Decisions

| Decision | Reason |
|----------|--------|
| 3 separate HTML files | Cleaner code separation; each page has a clear purpose |
| sessionStorage for auth | More secure than localStorage; clears on tab close |
| No inline onclick attributes | Compatibility with Edge's Content Security Policy |
| HTML entities for special chars | Prevents issues with email obfuscation proxies |
| Vanilla JavaScript only | No framework dependencies; works anywhere |
| Lazy chart rendering | Only render charts when a section is viewed; improves speed |
| IIFE (module pattern) | Prevents variable leaks to global scope |
| Section-page navigation | Each section gets full attention; not buried in a long scroll |

---

## For Future Development

If this system were to be extended, the following enhancements would be considered:

1. **Backend integration** — Store survey data in a database (e.g., Firebase, Supabase)
2. **Multi-user login** — A real authentication system with hashed passwords
3. **Data export** — Export entered data as CSV or JSON
4. **Historical comparison** — Compare audit results over multiple time periods
5. **Email report** — Send the generated report via email directly
6. **Import from Google Forms** — Auto-populate data from a CSV export
7. **Custom branding** — Allow clients to configure their own logo and company name

---

*Process Overview Document v1.0 | Diggibyte Technologies | 2025*
*For technical details, refer to DOCUMENTATION.md*
