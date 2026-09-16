# Healthcare and Clinical Documentation

**Read when:** the product handles protected health information, clinical trial data, or
is subject to HIPAA, ICH E6 GCP, or 21 CFR Part 11. EDC, eTMF, CTMS, EHR, and patient
portals all qualify.
**Skip if:** the product is not regulated. None of this applies and it is pure token cost.
**Pairs with:** `standards.md` 20.8 (the PHI section), `screenshots.md` 6 (synthetic data
only).
**Cost:** ~1.7k tokens.

Covers PHI-aware writing, regulatory context, clinical terminology, role-gated content,
audit trail documentation, dynamic forms, translatable content, and the anti-patterns
specific to this domain.

---

## Section 18: Healthcare and Clinical Trial Documentation

Documentation for clinical trial management systems requires additional care beyond
standard software documentation. This section covers PHI-awareness, regulatory context,
clinical terminology, and audit trail documentation.

### PHI-Aware Writing

Protected Health Information (PHI) must never appear in generated documentation. This
applies to examples, screenshots, code samples, and any illustrative data.

**Rules for all documentation generated from clinical systems:**

- Use synthetic placeholder values in all examples: "Patient #1042", "Jane D.", "Study ABC-001", "Site 01"
- Never use real patient names, dates of birth, medical record numbers, or study IDs
- Never embed real data from the system in generated documentation
- Screenshot instructions must specify: "Use test data, not real patient data, for screenshots"

**PHI sensitivity notice.** Include this block at the top of any guide that involves patient data:

```markdown
> **Data sensitivity.** This module contains protected health information (PHI).
> Access is logged and audited. Do not share screenshots or exports outside of
> authorized channels.
```

### Regulatory Context

Clinical trial management systems operate under regulatory frameworks that affect
what must be documented and how.

**ICH E6(R2) GCP (Good Clinical Practice).** Requires audit trails for all data
entered or modified in a clinical trial system. Any module that logs user actions
(creation, modification, deletion of records) must include documentation covering:

- What actions are logged
- How long logs are retained
- Who can view logs
- How to export logs for regulatory inspection

**21 CFR Part 11.** US FDA requirement for electronic records in clinical trials.
Systems subject to Part 11 must document:

- User authentication requirements
- Electronic signature workflows
- Audit trail access and content
- System access controls and who manages them

If the system is HIPAA-covered, include a HIPAA section in the admin guide covering:
access controls, audit log review procedures, and breach response steps.

### Clinical Terminology Standards

Use terms exactly as they appear in the system's interface and PRDs. Clinical
terminology is precise and regulated. Do not paraphrase.

| Write this | Not this |
|---|---|
| Investigational medicinal product (IMP) | Drug, medicine, medication |
| Disposition | Removal, use |
| Protocol | Study plan, trial plan |
| Participant or subject | Patient (unless the system uses "patient") |
| Adverse event | Side effect, reaction |
| Visit | Appointment (unless the system uses "appointment") |
| Site | Clinic, hospital, location |

The system's CLAUDE.md, i18n files, and PRDs are the authoritative source for
correct terminology. Use them. If a term in the code differs from a term in the PRD,
flag it in the gap report and use the PRD term in documentation.

### Role-Gated Documentation

Clinical trial systems have complex permission models. Do not document features in
a guide for an audience that cannot access them.

**Before writing any section, check:**

1. What role is required to perform this action? (Read the Policy file)
2. Is this action scoped to a site, a study, or the global system?
3. Are there actions that look the same but have different permissions? (e.g., "view" vs "view all sites")

**Generate a permissions table for every module:**

| Action | Minimum Role | Scope |
|---|---|---|
| View patient list | Site Staff | Own site only |
| Add a patient | Site Coordinator | Own site only |
| View all sites | Sponsor | Read-only |
| Delete a patient record | Site Admin | Own site only |
| View audit log | Site Admin, Sponsor | Scoped to their access |

### Audit Trail Documentation

Any module that uses activity logging (Spatie ActivityLog in Laravel, or equivalent)
must include an audit trail section. Generate this section by reading the model's
activity log configuration and the audit controller if present.

Template:

```markdown
## Audit trail

Every change to [module name] is logged automatically. The audit trail records:

- Who made the change (user name and role)
- What changed (field name, old value, new value)
- When the change was made (timestamp)
- From which site the change was made

To view the audit trail:

1. Go to [Module] in the main navigation.
2. Click **Audit Log** in the top-right corner.
3. Use the filters to narrow by date range, user, or record.

Audit logs are retained for [X years] in accordance with [regulatory standard].
Contact your system administrator to export logs for regulatory inspection.
```

### Dynamic Forms Documentation

If the system uses a schema-driven form engine (forms table -> elements -> field types),
generate field documentation from the element definitions rather than from hardcoded UI.

**Per-element documentation pattern:**

```markdown
### [Element label]

**Field type:** [text / dropdown / date / checkboxes / ...]
**Required:** [Yes / No]
**Options:** [List values if dropdown or radio]
**Rules:** [Validation constraints from the element config]
**Notes:** [Any conditional logic: "Appears only when [parent field] is [value]"]
```

For tabular elements (L3 forms in the Standard Forms module), document the table
as a section with its own field list. Explain that rows can be added and removed.

### Translatable Content

If the system supports multiple languages (i18n), note in the admin guide which
fields are translatable and how to manage translations. Do not hardcode English
strings in documentation examples - use the i18n key path and the English label
together so the reader can find the field regardless of their language setting.

---

## Section 19: Anti-Patterns Specific to Healthcare Documentation

| Anti-pattern | What to do instead |
|---|---|
| Real patient data in examples | Always use synthetic placeholder values |
| Undifferentiated guide for all roles | Split by role; check Policy files |
| Documenting a feature behind a feature flag as if it is live | Note "Available when [flag] is enabled" |
| Using generic software terms for clinical concepts | Use the exact clinical term from the PRD or i18n file |
| No PHI notice on patient-facing sections | Add the PHI sensitivity block at the top |
| Audit trail described as optional | Audit trails are mandatory in GCP-regulated systems; say so |
| Screenshots with real study or patient data | Replace with synthetic data or use text descriptions |
| One guide covering all study phases | Split by phase: pre-screening, enrollment, active, completed |

---
