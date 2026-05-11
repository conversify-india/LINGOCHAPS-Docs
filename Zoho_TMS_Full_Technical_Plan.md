# 🛠️ Zoho TMS: Full Technical Implementation Plan

This document contains the "Source of Truth" for how the Lingo Chaps TMS is configured in Zoho Projects. Use this if you ever need to rebuild the system or troubleshoot automation.

---

## 1. Task Custom Fields
The following fields must be active in the "Standard Layout":
*   **Source Language:** Picklist (List of target languages)
*   **Target Language:** Picklist (List of target languages)
*   **Upwork Contract URL:** URL Field
*   **Upwork Freelancer Cost:** Currency (USD)
*   **Internal Notes:** Multi-line text

## 2. Workflow Automation
**Rule Name:** `Client Delivery Notification`
*   **Trigger:** Task Status updated to `Awaiting Client Approval`.
*   **Recipients:** `Project Client` (Primary) and `Task Owner` (Secondary/Internal).
*   **Email Template:** `Client Delivery Template`.

## 3. Status Lifecycle
The status order must be maintained as follows:
1.  **Open** (Initial)
2.  **Hiring on Upwork**
3.  **In Translation**
4.  **Review in Progress**
5.  **Awaiting Client Approval** (Triggers Email)
6.  **Completed & Invoiced**
7.  **On Hold / Delayed / Cancelled**

## 4. Revision Tracker (Issue Tracker)
Renamed from "Issues" to **"Revision Requests"**.
*   **Field: Revision Type**
    *   Translation/Accuracy
    *   Grammar/Spelling
    *   Formatting/Layout
    *   Client Preference (Stylistic)
    *   Missing Content

## 5. Security & Roles
*   **Manager Profile:** Full edit access to all tasks and costs.
*   **Contractor Profile:** Limited to editing "Both" (Tasks assigned to them or owned by them). Cannot see internal budget fields.

---

## 6. Freelancer Cost Tracker Report

Replaces the manual Excel cost tracker. This report is saved under **Reports tab → Task Basic Reports → Freelancer Cost Tracker**.

### Report Configuration:
*   **Module:** Task
*   **Chart Type:** Table (first icon — horizontal lines)
*   **X-AXIS:** Owner (the freelancer assigned to the task)
*   **Y-AXIS:** Sum → `Upwork Freelancer Cost` (custom field)
*   **Group By:** None

### What It Shows:
For each freelancer (Owner), it displays the **total Upwork cost** paid across all their assigned tasks.

| Owner (Freelancer) | Total Upwork Cost |
|---|---|
| Freelancer A | $150.00 |
| Freelancer B | $60.00 |
| **Total Spend** | **$210.00** |

### ⚠️ Important:
The report only shows data if the `Upwork Freelancer Cost` field is filled in on each task. Empty fields = blank report.

---

## 7. Upwork-to-Zoho Data Entry Workflow (Replaces Excel)

Every time a freelancer is hired on Upwork, follow these steps to log the data in Zoho — **no Excel needed**.

### Step-by-Step:
1.  **Post & hire** the freelancer on Upwork (as normal)
2.  Go to **Zoho Projects → relevant Project → Tasks tab**
3.  Open the relevant task (or create a new one)
4.  Fill in the following fields:
    *   **Assignee / Owner:** Select the freelancer's name (if they are a Portal User)
    *   **Upwork Contract URL:** Paste the direct link to the Upwork contract
    *   **Upwork Freelancer Cost:** Enter the agreed contract amount in USD
    *   **Source Language / Target Language:** Set the language pair
5.  Change the task **Status** to `Hiring on Upwork`
6.  When the freelancer accepts and starts work → update status to `In Translation`

### Why This Replaces Excel:
*   **Single entry point:** Data entered once in Zoho, never in a separate spreadsheet
*   **Auto-calculated totals:** The Freelancer Cost Tracker report sums all costs automatically
*   **Audit trail:** Every cost is tied to a specific task, project, and Upwork contract URL
*   **Month-end reporting:** Filter the report by date range to get monthly Upwork spend instantly

---
*Created on 2026-05-05 for Lingo Chaps Operations.*
