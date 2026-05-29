# Example 6: Clinical Trial Management SaaS - Laravel + Vue.js

## Context

A healthcare SaaS company runs a multi-tenant clinical trial management platform built
on Laravel (PHP) with a Vue.js frontend. The system manages patients, appointments,
drug inventory, study protocols, consult workflows, and dynamic clinical forms across
multiple research sites.

The engineering team stores PRDs, URDs, and functional requirements in their chosen
tools (Linear, Confluence, Jira, Notion, or others). All code lives in Git.
No user-facing documentation exists. The team wants to generate documentation for the
entire platform - either by module, by feature, or as a complete one-shot run.

Stack: Laravel 10 (PHP), Vue.js v5, PostgreSQL, Sanctum auth, Spatie ActivityLog,
TanStack Table (frontend), shadcn-vue components.

---

## Generation Modes

Choose the mode that fits the current goal:

| Mode | When to Use | Output |
|---|---|---|
| Full system scan | First-time documentation of the entire platform | One doc per module + overview |
| Module-by-module | Documenting one functional area completely before moving on | Per-audience docs for that module |
| Feature-by-feature | A specific feature just shipped and needs docs | One feature guide + gap report |
| PRD-driven | A PRD file exists and needs to be reconciled against code | User manual + gap report |

---

## Mode 1: Full System Scan

Use this mode to generate documentation for every module in the platform at once.

### Step 1: Inventory all modules

```
Read routes/api.php and routes/V5/api.php.
Group endpoints by controller prefix.
Produce a module inventory:

Module               Controllers                         Audiences
---------------------------------------------------------------------------
Patient Management   PatientController, PatientCenterCtrl  Staff, Admin
Appointments         AppointmentController, Sched...       Staff, Admin, QI
Drug Inventory       DrugInventory*, Disposition*          Staff, Admin, Sponsor
Study Management     Study*, Protocol*, Roadmap*           Staff, Admin, Sponsor
Consults             ConsultController, ConsultStatus*     Staff, Clinician
Dynamic Forms        DynamicFormController, FormBuilder*   Admin
Pre-Screenings       StudyPreScreening*, Eligibility*      Staff
Sites & Roles        SiteController, RoleController        Admin
Reports              ReportController, Cohort*, Survival*  Admin, QI, Sponsor
Lists & Translations ListController, Translation*          Admin
```

### Step 2: For each module, run the ingestion sequence

See `references/architecture.md` - "Laravel / PHP Stack Ingestion" for the complete
sequence. Key steps:

1. Map routes -> controller methods
2. Read FormRequests for validation rules and permissions
3. Read Service classes for business logic
4. Read Eloquent Models for data shape
5. Read Policy files for permission matrix
6. Read Pest feature tests as behavioral specification
7. Read any linked PRD files

### Step 3: Generate per-module, per-audience documentation

For each module, generate:
- `docs/<module>/overview.md` - What this module does, who uses it, key concepts
- `docs/<module>/staff-guide.md` - Task-oriented guide for site staff
- `docs/<module>/admin-guide.md` - Configuration and admin tasks
- `docs/<module>/permissions.md` - Permission matrix for all actions in this module

Output is always Markdown. All files drop directly into Docusaurus, Mintlify,
GitBook, or any Markdown-based docs portal.

---

## Mode 2: Module-by-Module (Drug Inventory Example)

This example shows how to generate complete documentation for the Drug Inventory module.

### Ingestion

```
Routes: GET/POST /drug-inventory/*, /dispositions/*, /inventory-checks/*
Controllers:
  - DrugInventoryController (shipments, receipts)
  - DispositionController (dispensing, destruction, returns)
  - InventoryCheckController (physical count reconciliation)
  - DrugInventoryAuditController (audit log with activity trail)
  - DrugInventoryShipmentController (manifest line items)

Service: app/Services/DrugInventory/InventoryBalanceCalculator.php
  - Balance = sum(manifest items) - sum(dispositions) - sum(linked medications)
  - Balance is never stored; always calculated on read

Models:
  - DrugInventory (shipments and receipts)
  - Disposition (dispensing, destruction, transfer, return)
  - InventoryCheck (physical count events)

Policy: app/Policies/DrugInventoryPolicy.php

Tests: tests/Feature/DrugInventory/
  - Read every it() block as a documented behavior

Spec: Fetch PRD/URD from your spec tool (Linear, Confluence, Jira, Notion, etc.)
      Fetch linked issues/stories from your issue tracker for acceptance criteria
```

### Audiences for Drug Inventory

| Audience | What they do | Doc needed |
|---|---|---|
| Site staff (dispensing) | Receive shipments, dispense to patients, perform counts | staff-guide.md |
| Site administrator | Approve dispositions, view full audit log, generate reports | admin-guide.md |
| Sponsor (read-only) | View balance and audit trail across all sites | sponsor-guide.md |

### Generated: docs/drug-inventory/overview.md

```markdown
# Drug Inventory

The Drug Inventory module tracks investigational medication from the moment a shipment
arrives at the site through every dispensing event, physical count, and final disposition.

## Key concepts

**Shipment.** A delivery of investigational drug from the sponsor to a site. Each shipment
contains one or more manifest items specifying the drug, quantity, lot number, and expiration date.

**Receipt.** Confirmation that the site has physically received a shipment and verified its contents.

**Disposition.** Any event that removes drug from the available balance: dispensing to a patient,
returning to the sponsor, transferring to another site, or destroying expired stock.

**Inventory check.** A physical count of all drug on hand at a site, used to reconcile the
calculated balance against actual stock.

**Balance.** The quantity of each drug currently available at the site. Balance is always
calculated from the full history of receipts, dispensing events, and dispositions. It is
never stored as a single number - it is always computed.

## Who uses this module

- **Site staff** receive shipments, dispense drug, and perform physical counts.
- **Site administrators** approve dispositions, manage the audit log, and generate reports.
- **Sponsors** view balances and audit trails across all sites in read-only mode.

## Related topics

- [Receive a shipment](./staff-guide.md#receive-a-shipment)
- [Dispense drug to a patient](./staff-guide.md#dispense-drug)
- [Perform an inventory count](./staff-guide.md#inventory-count)
- [View the audit log](./admin-guide.md#audit-log)
```

### Generated: docs/drug-inventory/staff-guide.md (excerpt)

```markdown
# Drug Inventory: Site Staff Guide

## Receive a shipment

When a drug shipment arrives at your site, record its receipt to add the quantity to
your site balance.

### Before you begin

- You need the Pharmacy Staff or Site Coordinator role.
- The shipment must already exist in the system, created by the sponsor or a site admin.

### Steps

1. Go to **Drug Inventory** in the main navigation.
2. Click the shipment you want to receive.
3. Verify the manifest items match the physical delivery:
   - Drug name and lot number
   - Quantity received
   - Expiration date
4. Click **Confirm Receipt**.
5. Enter the date the shipment physically arrived at the site.
6. Click **Save**.

The shipment status changes to Received and the quantity is added to your site balance.

### If the delivery does not match the manifest

If any item differs from the manifest (wrong quantity, wrong lot, damaged packaging),
do not confirm receipt. Contact your site administrator and the sponsor before proceeding.

## Dispense drug to a patient

Record a dispensing event when you give investigational drug to a study participant.

### Before you begin

- You need the Pharmacy Staff or Site Coordinator role.
- The patient must be active on a protocol that authorizes this drug.

### Steps

1. Go to **Drug Inventory**.
2. Click **New Disposition**.
3. Select **Dispensed to Patient** as the disposition type.
4. Search for and select the patient.
5. Enter the quantity dispensed and the lot number used.
6. Enter the date of dispensing.
7. Click **Save**.

The dispensed quantity is subtracted from the site balance immediately.
```

---

## Mode 3: Feature-by-Feature (Appointment Scheduling Example)

Use this mode when a specific feature just shipped and needs documentation.
This example uses the appointment scheduling workflow.

### Step 1: Identify all sources for this feature

```
Spec sources:
  PRD: "Appointment Form Parity and Follow-up Flow Improvements"
       (fetch from your spec tool: Linear, Confluence, Jira, Notion, etc.)
  Issues: Epic and linked issues/stories for the appointment milestone
          (fetch from your issue tracker using the appropriate MCP connector)
          (fetch via Linear MCP - extract acceptance criteria per issue)

Code sources:
  AppointmentController
  AppointmentDashboardController
  AppointmentExportController
  ScheduleBlockController
  WorkflowSettingController
  DB migration: new columns (order_date, booking_date, results_shared_date,
                preliminary_report_date, final_report_date, appointment_kept,
                clinic_completion_note, cancellation_reason_id)

Tests: tests/Feature/Appointments/
```

### Step 2: Run the PRD-to-manual workflow

See `examples/05-prd-to-manual.md` for the full PRD reconciliation workflow.

For the appointment feature:

```json
{
  "feature": "Appointment Scheduling with Follow-up Flow",
  "prd_user_stories": 40,
  "shipped_behaviors": [
    "Form parity: all standard appointment fields on schedule page",
    "Order Date and Booking Date fields with today() default",
    "Urgency and Specialty fields with conditional subtypes",
    "Follow-up mini form pre-populated from parent appointment",
    "Parent-child relationship badges on appointment list",
    "Timeline tab on patient detail page",
    "Cancelled status with cancellation reason fields"
  ],
  "reconcile_against_code": "Read AppointmentController methods + DB migration for new columns"
}
```

### Step 3: Generate the user manual

Output: `docs/appointments/schedule-appointment.md`

Key sections:
- Schedule a new appointment (step-by-step)
- Schedule an appointment with Order Date and Booking Date
- Complete an appointment
- Update report dates (Radiology/Imaging)
- Schedule a follow-up appointment
- Understand the appointment timeline
- Cancel an appointment

Each section follows the task-oriented template:
context -> prerequisites -> numbered steps -> expected outcome

### Step 4: Generate the gap report

Output: `docs/appointments/gap-report-appointment-v1.md`

Any user stories in the PRD that are not confirmed by code evidence appear in the
gap report. Each gap creates a Linear issue automatically.

---

## Mode 4: PRD-Driven (Fetch from Any Spec Tool)

ADUMAS fetches specs from wherever the team stores them - Linear, Confluence, Jira,
Notion, Docmost, GitHub, Asana, ClickUp, or any other tool. The workflow is identical
regardless of which tools are used; only the MCP connector changes.

### Source discovery - always ask first

Before fetching anything, ask the user:
- "Where do your PRDs and URDs live?" (e.g. Linear documents, Confluence pages, Jira epics, Notion databases, GitHub wiki, Asana projects)
- "Where are acceptance criteria tracked?" (may be the same tool or a separate issue tracker)

Then use the appropriate MCP connector. See `references/mcps.md` for the full connector
list and `SKILL.md` for the supported sources table.

### Reconciliation workflow

For each PRD:

1. Fetch the PRD/URD from the team's spec tool: extract goals, user stories, acceptance criteria
2. Fetch linked issues/stories from the issue tracker: extract per-story acceptance criteria and edge cases
3. Map to code: identify controllers, models, routes, and DB migrations
4. Run the Specification Reconciler: compare spec against code behavior
5. Generate the user manual from what shipped
6. Generate the gap report from what was specified but not shipped
7. Create tracked issues for each gap in the team's issue tracker

### Feature-to-source mapping for patients-api

| Feature Area | Spec document (your tool) | Issues (your tracker) | Controllers |
|---|---|---|---|
| Appointment scheduling | "Appointment Form Parity and Follow-up" | Appointments epic/milestone | AppointmentController, WorkflowSettingController |
| Specialty-driven clinical forms | "Specialty-Driven Clinical Forms" | Forms epic/milestone | DynamicFormController, FormElementController |
| Panel invitation | "Panel Invitation Confirmation" | Pre-screening epic/milestone | StudyPreScreeningController |
| Appointment chains | "Appointment Chains and Follow-up View" | Appointments epic/milestone | AppointmentController, PatientController |

---

## Healthcare Domain Considerations

When generating documentation for a clinical trial management system, apply these rules:

### PHI-aware language

Never include real patient identifiers, real study data, or real site names in generated
documentation examples. Use placeholder values:

- Patient: "Patient #1042" or "Jane D." - never a real name
- Study: "Study ABC-001" - never a real protocol number with sponsor identity
- Site: "Site 01" - never a named hospital

### Audience-specific PHI warnings

Every user-facing guide that involves patient data should include a note:

```markdown
> **Data sensitivity.** This page contains protected health information (PHI).
> Do not share screenshots or data exports outside of authorized channels.
> Access to this module is logged and audited.
```

### Role-gated feature documentation

Do not document features in a guide for an audience that cannot access those features.
Check the Policy file for each resource. If Site Staff cannot access the audit log,
do not include audit log instructions in the staff guide.

### Audit trail documentation

Any module that uses Spatie ActivityLog (DrugInventory, Appointments, PatientProtocol)
should include a section explaining what is logged and how long logs are retained.
Clinical trial regulations (ICH E6 GCP) require audit trail documentation.

### Terminology standards

Use the exact terms from the system's i18n files and PRDs. Do not paraphrase clinical terms.
If the system calls it "disposition", do not call it "removal" in the documentation.
Clinical trial terminology is precise and regulated.

---

## Output File Structure

For the complete patients-api documentation set:

```
docs/
  overview/
    platform-overview.md
    getting-started.md
    roles-and-permissions.md
  patients/
    overview.md
    staff-guide.md
    admin-guide.md
  appointments/
    overview.md
    schedule-appointment.md
    complete-appointment.md
    follow-up-appointments.md
    appointment-timeline.md
    admin-guide.md
  drug-inventory/
    overview.md
    staff-guide.md
    admin-guide.md
    sponsor-guide.md
    audit-log.md
  studies/
    overview.md
    protocol-management.md
    participant-enrollment.md
    admin-guide.md
  pre-screenings/
    overview.md
    eligibility-workflow.md
    enrollment.md
  consults/
    overview.md
    consult-workflow.md
  forms/
    overview.md
    dynamic-forms-guide.md
    form-builder-guide.md
  reports/
    overview.md
    cohort-reports.md
    export-guide.md
  admin/
    site-configuration.md
    user-management.md
    roles-and-permissions.md
    lists-and-values.md
    translations.md
```

All files are Markdown. Drop them into any Markdown-based documentation portal.
