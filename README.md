# Power Platform — Complete, Practical Tutorial (Markdown)

> **Goal:** Learn Power BI, Power Apps, Dataverse, Power Pages, Power Automate, Copilot Studio/AI Builder, and security/governance.
> **Style:** Simple English. Short steps. Lots of examples.
> **What you will build:** a small business solution for *Leave Requests* (end-to-end).

---

## Table of Contents

- [Power Platform — Complete, Practical Tutorial (Markdown)](#power-platform--complete-practical-tutorial-markdown)
  - [Table of Contents](#table-of-contents)
  - [What is Microsoft Power Platform?](#what-is-microsoft-power-platform)
  - [Setup \& Environments](#setup--environments)
  - [Dataverse (Core Data Layer)](#dataverse-core-data-layer)
    - [Key Concepts (with fixes to notes)](#key-concepts-with-fixes-to-notes)
    - [Minimal Build Steps](#minimal-build-steps)
    - [Exercises](#exercises)
  - [Power Apps](#power-apps)
    - [Canvas Apps](#canvas-apps)
    - [Model-driven Apps](#model-driven-apps)
  - [Power Automate](#power-automate)
  - [Power BI](#power-bi)
  - [Power Pages](#power-pages)
  - [Copilot Studio \& AI Builder (AI Hub)](#copilot-studio--ai-builder-ai-hub)
    - [Copilot Studio (formerly Power Virtual Agents)](#copilot-studio-formerly-power-virtual-agents)
    - [AI Builder / AI Hub](#ai-builder--ai-hub)
  - [Security \& Governance](#security--governance)
    - [Roles \& Access](#roles--access)
    - [Dataverse Security Layers](#dataverse-security-layers)
    - [DLP — Data Loss Prevention](#dlp--data-loss-prevention)
    - [Conditional Access \& Managed Environments](#conditional-access--managed-environments)
  - [ALM: Solutions, Managed vs Unmanaged, Pipelines, DevOps](#alm-solutions-managed-vs-unmanaged-pipelines-devops)
  - [Integration \& Data (Dataflows, Gateways, Connectors)](#integration--data-dataflows-gateways-connectors)
  - [Admin \& Monitoring](#admin--monitoring)
  - [End-to-End Project: Leave Request](#end-to-end-project-leave-request)
  - [Cheat Sheets (Power Fx, Flow expressions, Power Query M)](#cheat-sheets-power-fx-flow-expressions-power-query-m)
    - [Power Fx (Canvas)](#power-fx-canvas)
    - [Power Automate Expressions](#power-automate-expressions)
    - [Power Query M (Dataflows/Power BI)](#power-query-m-dataflowspower-bi)
  - [Troubleshooting — Common Errors](#troubleshooting--common-errors)
  - [Glossary](#glossary)
  - [Extra Notes (from your draft, corrected \& merged)](#extra-notes-from-your-draft-corrected--merged)
    - [Your Next Steps](#your-next-steps)

---

## What is Microsoft Power Platform?

A low-code platform to build apps, automations, websites, and analytics:

* **Dataverse:** Secure cloud database for business data.
* **Power Apps:** Build apps (Canvas or Model-driven).
* **Power Automate:** Automate tasks and workflows.
* **Power BI:** Reports and dashboards.
* **Power Pages:** Secure external website/portal.
* **Copilot Studio / AI Builder:** Add AI to your apps and flows.

**When to use what**

* Need a quick form or mobile app → **Canvas app**.
* Need standard data screens (lists, forms, charts) with security by roles → **Model-driven app** on **Dataverse**.
* Need process automation or approvals → **Power Automate**.
* Need reporting and KPIs → **Power BI**.
* Need a public or partner site → **Power Pages**.
* Need chat or AI skills → **Copilot Studio** and **AI Builder**.

---

## Setup & Environments

1. Go to **Power Platform Admin Center** → create environments:

   * **Dev** (makers build here)
   * **Test** (UAT)
   * **Prod** (live)
2. Enable **Dataverse** in Dev/Test/Prod if you will use it.
3. Give users **Security Roles** (see [Security](#security--governance)).
4. Plan **ALM** (Application Lifecycle Management):

   * Build in **Dev** → export **Solution** → import to **Test/Prod** (managed).

> **Tip:** Start small. One environment for Dev, one for Prod is OK at first.

---

## Dataverse (Core Data Layer)

Dataverse stores tables, columns, relationships, and rules.

### Key Concepts (with fixes to notes)

* **Solutions:** Containers to move components between environments.

  * **Managed** (locked) → ideal for **Prod/Test**.
  * **Unmanaged** (editable) → use in **Dev**.
* **Ownership (table setting):**

  * **Organisation-owned:** permissions at organisation level.
  * **User or Team-owned:** record-level security by user/team.
* **Business Rules** (not “Business Roles”): set rules for form/field logic (show/hide, set values).
* **Workflows & Actions (classic):** real-time or background processes.

  * Today, **Power Automate** is recommended for most automation.
  * **Actions** can be reusable operations with inputs/outputs (called from apps/flows).
* **Dataflows:** get data from sources (Excel, SQL, Azure, etc.), transform in **Power Query**, load to Dataverse.
* **Gateway:** needed for on-premises data refresh (files/DBs on your network).

### Minimal Build Steps

1. **Create a Solution** in Dev (e.g., `LeaveSolution`).
2. **Create Tables**, e.g., `LeaveRequest`, `Employee`, `Manager`.
3. Define **columns**, **relationships**, and **views/forms**.
4. Add **Business Rules** for simple logic (e.g., if Type = “Sick”, make `Note` required).
5. Add **Dataflows** if you need to import data.

### Exercises

* Create a `LeaveRequest` table with columns:

  * `Employee` (lookup), `FromDate` (date), `ToDate` (date),
    `Type` (choice), `Reason` (text), `Status` (choice: Draft/Pending/Approved/Rejected).
* Add a **Business Rule:** if `FromDate` > `ToDate` → show error.
* Import employees from Excel using a **Dataflow**.

---

## Power Apps

### Canvas Apps

* Drag-and-drop UI. Use **Power Fx** formulas (Excel-like).
* Data sources: Dataverse, SharePoint, SQL, Excel, etc.
* **Components:** reusable UI units (like React components). You can export/import.
  Others can use, but may not edit inside your component (black-box idea).

**Quick Start**

1. `make.powerapps.com` → **Apps** → **New app** → **Canvas**.
2. Add **Data** → choose **Dataverse** table `LeaveRequest`.
3. Insert **Edit Form**, set **DataSource** = `LeaveRequest`.
4. Use formulas for behaviour, e.g.:

   ```powerfx
   // Save button
   SubmitForm(EditForm1);
   Notify("Saved", NotificationType.Success)
   ```
5. Publish new version: **File → Save → Publish** (or App list → … → **Details** → **Versions**).

**Tips**

* You can set formulas on properties (size, visible, color, etc.).
* Use **Copilot** to create screens or write formulas faster.

### Model-driven Apps

* Auto-generated UI based on **Dataverse** tables.
* Strong security, good for complex data and processes.

**Create Steps**

1. Create tables (already done above).
2. Ensure each table has a **Main form** and at least one **View**.
3. **Apps → New app → Model-driven**. Add **Areas**, **Groups**, **Sub-areas** (tables, dashboards, charts).
4. Add **Dashboards/Charts** using table data.
5. Publish app.

**Notes**

* Model-driven apps work **only with Dataverse**.
* You can embed a **Canvas app** in a Model-driven form (advanced).

**Exercise**

* Build a Model-driven app named `LeaveAdmin`. Include:

  * `LeaveRequest` views, forms, charts (e.g., requests by status).
  * A dashboard that shows pending approvals.

---

## Power Automate

* Build flows (like Zapier, but deep with Microsoft).
* Share flows as **co-owner** (edit/run) or **read-only**.
* **Custom connectors**: wrap your API; can be certified for public use.

**Common Patterns**

* **Approval Flow:**
  Trigger: *When a row is added in Dataverse* → **Start and wait for an approval** → set `Status` based on approve/reject → notify by email/Teams.
* **Business Process Flow** (BPF):
  Multi-stage process for a table record (e.g., Draft → Pending → Approved).

**Desktop (RPA):** record UI actions on your PC (open apps, copy/paste, etc.).

**Process Mining & Task Mining**

* Discover how a process really works. Show bottlenecks.
* Use **Process Mining templates** to start.

**Exercise**

* Build a flow: When `LeaveRequest.Status` changes to *Pending*, send approval to the manager.
  On approve → set to *Approved*, else *Rejected*. Send email to requestor.

---

## Power BI

> *Your note “Workspaces vs Apps” belongs to Power BI.*

* **Workspace:** place to build and share content with your team.
* **App:** a packaged, read-only view of reports/dashboards published **from a workspace** for a wider audience. Changes in the workspace show in the **App** only **after you publish the app** again.

**Key Terms (Service)**

* **Dataset / Semantic model** (in Microsoft Fabric), **Report**, **Dashboard**, **Workspace**, **App**, **Dataflow** (Power Query online), **Gateway** (for on-prem data).

**Quick Steps**

1. Build report in **Power BI Desktop** (connect to Dataverse or SQL).
2. Publish to a **Workspace**.
3. Create an **App** from that workspace for end users.
4. Set **Refresh** schedules (Gateway if on-premises).

**Exercise**

* Create a report “Leave KPIs”:

  * Cards: Pending count, Approved this month.
  * Bar chart: Requests by Type.
  * Line chart: Requests over time.
    Publish and create a simple App.

---

## Power Pages

* Public or partner-facing website with secure login (Azure AD, external users).
* Works well with **Dataverse**.

**Quick Steps**

1. `make.powerpages.microsoft.com` → **Create** (choose a template).
2. Connect pages and forms to **Dataverse** tables (e.g., submit a Leave Request).
3. Set **Web roles** and **Table permissions** for who can view/edit records.
4. Theme and publish.

**Exercise**

* Add a “Submit Leave” page bound to `LeaveRequest` table.
* Add a “My Requests” list for logged-in user.

---

## Copilot Studio & AI Builder (AI Hub)

### Copilot Studio (formerly Power Virtual Agents)

* Build a chatbot/agent with topics and generative answers.
* Add a **knowledge source** (website, files) for Q\&A.
* Expose the bot in Teams, web, or your site.

**Exercise**

* Create a “Leave Helper” bot:

  * Answers: leave policy, how to request.
  * Action: create a leave request by calling a Power Automate flow.

### AI Builder / AI Hub

* Ready-to-use AI models (no code). Examples:

  * **Document processing** (extract fields from invoices/bills).
  * **Business card reader**.
  * **Object detection**.
  * **Text classification** and **sentiment**.

**Use in Apps/Flows**

* In Canvas apps: **Insert → AI Builder** control.
* In Flows: **AI Builder** actions (e.g., “Extract information from documents”).

---

## Security & Governance

### Roles & Access

* **Security Roles** (in environment):

  * **Basic User:** create/use own records.
  * **Delegate:** act for another user.
  * **System Customizer:** can customize and see data for their tables.
  * **System Administrator:** full control.
  * **Environment Admin / Maker:** admin or maker rights at environment level (maker can create apps/flows, not see all data by default).

### Dataverse Security Layers

* **Business Units & Teams**: structure your org and group users.
* **User/Team-owned tables**: record-level permissions (own, business unit, org).
* **Column-level security**: lock sensitive columns.
* **Auditing**: track changes to tables/fields.
* **Hierarchy security**: managers can see subordinates’ records.

### DLP — Data Loss Prevention

* Set in **Power Platform Admin Center → Data policies**.
* Put connectors into:

  * **Business** (allowed for internal/business data)
  * **Non-Business** (separate group)
  * **Blocked** (not allowed)
* **Rule:** a connector in Business **cannot** share data with one in Non-Business.
* You **cannot block** core Microsoft connectors like SharePoint, Dataverse, etc.

### Conditional Access & Managed Environments

* Use **Conditional Access** (Azure AD) — control by user, device, location.
* **Managed Environments**: extra governance, sharing controls, and limits for large orgs.

**Gateway Security**

* On-premises data refresh uses **On-premises data gateway**.
* Keep gateway hosts secure and updated.

---

## ALM: Solutions, Managed vs Unmanaged, Pipelines, DevOps

* **Build** in **Unmanaged Solution** in Dev.
* **Export** from Dev as:

  * **Managed** for Test/Prod (locked)
  * **Unmanaged** only for moving Dev-like work (not recommended to import unmanaged into Prod)
* **Pipelines for Power Platform** (no/low-code ALM):

  * Create pipeline → stages for **Dev → Test → Prod** → deploy the **solution** through stages.
* **DevOps** (advanced):

  * Power Platform CLI (`pac`), Git repos, build/release with Azure DevOps or GitHub Actions.
  * Use **ALM Accelerator** (from CoE) if needed.

**Exercise**

* Package all app, flows, tables into a **Solution**.
* Move Dev → Test → Prod with a **Pipeline**.

---

## Integration & Data (Dataflows, Gateways, Connectors)

* **Dataflows** (Power Apps or Power BI) use **Power Query** to transform and load data.
* **Gateway** for on-prem data refresh.
* **Connectors**:

  * Standard: SharePoint, Outlook, Teams, SQL, etc.
  * **Custom connector**: wrap your REST API (OAuth/API key).
* **Virtual Tables**: read external data in Dataverse without copying it (advanced).

**Exercise**

* Build a Dataflow that imports employees from a network file share (via Gateway) into Dataverse nightly.

---

## Admin & Monitoring

* **Power Platform Admin Center**:

  * Environment analytics, capacity, **Maker** activity.
  * **Data policies** (DLP), **Data integration**, **Billing**.
* **Solutions Health**: check missing dependencies before export.
* **Flow run history**, **App Monitor** (Canvas) for performance issues.
* **Audit logs** for security reviews.

---

## End-to-End Project: Leave Request

**Goal:** One solution that covers app, automation, report, and portal.

1. **Dataverse**

   * Tables: `Employee`, `Manager`, `LeaveRequest`.
   * Columns: as in Dataverse exercise.
   * Business Rule: `FromDate` ≤ `ToDate`.

2. **Model-driven app (LeaveAdmin)**

   * Views: *My Team’s Pending*, *All Approved*.
   * Chart: Requests by Type, by Month.

3. **Canvas app (LeaveMobile)**

   * Form to create a request.
   * Gallery: *My Requests*.
   * Button: `SubmitForm(EditForm1)` + `Notify()`.

4. **Power Automate**

   * Trigger: When `LeaveRequest` created or `Status` changes to *Pending*.
   * Send manager **Approval**.
   * Update `Status` and email result.

5. **Power BI**

   * Dataset from Dataverse `LeaveRequest`.
   * Report with KPIs, trend, breakdown by type.
   * Publish to workspace → publish **App**.

6. **Power Pages**

   * “Submit Leave” page (Dataverse form).
   * “My Requests” list.
   * Table permissions for signed-in users.

7. **Security**

   * Roles: `Employee`, `Manager`, `HR Admin`.
   * DLP: block risky connectors (e.g., Twitter) for this environment.

8. **ALM**

   * Put all components into **Solution** `LeaveSolution`.
   * Use **Pipeline** to deploy Dev → Test → Prod.

---

## Cheat Sheets (Power Fx, Flow expressions, Power Query M)

### Power Fx (Canvas)

```powerfx
// Filter my requests
Filter(LeaveRequest, Employee.Email = User().Email)

// Validate date
If(DateValue(FromDateInput.Text) > DateValue(ToDateInput.Text),
   Notify("From date must be before To date", NotificationType.Error)
);

// Show control only for managers
Visible: "Manager" in User().Email // simple demo; prefer role check via Dataverse

// Patch (create) record
Patch(LeaveRequest,
      Defaults(LeaveRequest),
      { Employee: LookUp(Employee, Email = User().Email),
        FromDate: DatePickerFrom.SelectedDate,
        ToDate: DatePickerTo.SelectedDate,
        Type: DropdownType.Selected.Value,
        Status: "Pending" }
);
```

### Power Automate Expressions

```text
coalesce(triggerOutputs()?['body/Reason'], 'No reason provided')
equals(items('Apply_to_each')?['Status'], 'Approved')
formatDateTime(utcNow(), 'yyyy-MM-dd')
if(greater(item()?['Days'], 10), 'Long', 'Short')
```

### Power Query M (Dataflows/Power BI)

```m
let
  Source = Excel.Workbook(File.Contents("C:\HR\employees.xlsx"), true),
  Table  = Source{[Item="Employees",Kind="Table"]}[Data],
  Clean  = Table.TransformColumnTypes(Table,{{"StartDate", type date}}),
  Keep   = Table.SelectRows(Clean, each [Active] = true)
in
  Keep
```

---

## Troubleshooting — Common Errors

* **“Connection refused [http://localhost](http://localhost) … kubernetes”**
  Ensure you target the right cluster/URL. For Power Platform, check **Dataverse** connection and environment.
* **Flow keeps failing on approval:**
  Check the approver’s email, permissions, and if the row still exists. Look at **Run history** details.
* **App changes not visible to users:**
  You saved but did not **Publish** the latest version.
* **Power BI report not updating:**
  Configure **Refresh** and **Gateway** if data is on-premises.
* **Cannot import solution to Prod:**
  Export as **Managed**. Fix missing dependencies before export.

---

## Glossary

* **Dataverse:** cloud database for business apps.
* **Solution:** package of components to move between environments.
* **Managed/Unmanaged:** locked vs editable solution.
* **Canvas app:** pixel-perfect app with Power Fx.
* **Model-driven app:** data-first app from Dataverse tables.
* **Dataflow:** data import with Power Query.
* **Gateway:** bridge for on-prem data.
* **DLP:** Data Loss Prevention policies for connectors.
* **BPF:** Business Process Flow (multi-stage path on a record).
* **AI Builder:** ready AI models for apps/flows.
* **Copilot Studio:** build chat/AI agents.

---

## Extra Notes (from your draft, corrected & merged)

* **Power BI Workspaces vs Apps:** workspaces are for building; **Apps** are for publishing to many users.
* **Dataverse “Business Rules”, not “Roles”:** use to set/clear/require fields and show/hide on forms.
* **Real-time Workflows/Actions:** still exist, but **Power Automate** is preferred for most cases.
* **Dataflows (not “DataFlaws”):** import/transform/load data into Dataverse or Power BI.
* **Gateway:** needed for refresh of local files or on-prem databases.
* **Publish versions:** after you edit a Canvas app, **Publish** so users get the new version.
* **Copilot:** you can use Copilot in many places — in home, when creating tables/apps, and even inside apps.

---

### Your Next Steps

1. Create Dev/Test/Prod environments.
2. Build the **Leave Request** solution in Dev (tables, apps, flows).
3. Add a small **Power BI** report.
4. Publish a **Power Pages** site for external submission.
5. Set **DLP**, roles, and **Pipeline** to move to Prod.

If you want, say **“export this as a file”**, and I’ll give you a downloadable `.md` copy.
