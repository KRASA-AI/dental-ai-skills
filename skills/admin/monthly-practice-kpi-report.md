---
name: "Monthly Practice KPI Report"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~90 min/month"
version: 2.1
last_eval_score: 9.60
---

# Monthly Practice KPI Report

## Purpose

Turn raw practice-management exports (Dentrix G7+, Dentrix Ascend, Eaglesoft, Open Dental, Curve Dental, Denticon, Carestack) into a one-page monthly KPI narrative that an owner-dentist, office manager, or DSO regional director can read in five minutes and act on. Covers the canonical dental KPIs — production, collections, net collection %, AR aging, new-patient count, case acceptance, hygiene reappointment, broken-appointment / no-show rate, open-chair utilization, provider-level production — with month-over-month trend, year-over-year comparison, an explicit reconciliation check, and specific, named action items tied to the numbers. Produces PMS-specific export-path instructions so the office manager pulls the right report on first run rather than hunting through their PMS menus.

## When to Use

Use this skill when:
- Closing the books at month-end and producing the owner/partner dashboard
- Preparing a monthly leadership-team review packet
- Running a DSO regional or multi-site comparison
- Onboarding a new office manager who needs a baseline performance snapshot
- Diagnosing a revenue drop or scheduling problem month-over-month
- Building the standing input for the quarterly leadership offsite

Do **not** use as a substitute for a CPA's financial statement, a tax filing, or a valuation report.

## Required Input

**Minimal-input fast-path:** Provide just #1 (reporting period). The skill applies default heuristics for every unspecified field and labels each assumption **[DEFAULT — VERIFY]** in a Section 0 Defaults Summary at the top of the output. A complete one-pager is produced even from a single-field input — the practice can then re-run with full PMS exports for a refined version.

Full input set for the most tailored report:

1. **Reporting period** — Calendar month closing date, prior-month comparison, prior-year-same-month comparison
2. **PMS export data** — Production by provider (dentist and hygienist), collections, adjustments, write-offs, refunds, AR aging buckets (0–30, 31–60, 61–90, 91+), new patient count, scheduled-vs-completed (no-show/broken), hygiene reappointment rate, treatment-plan presented vs. accepted $
3. **Practice targets** — The owner's benchmarks (or national benchmarks from ADA Health Policy Institute, Levin Group, Dental Intelligence averages): e.g., collection % target 98%+, no-show rate target < 5%, hygiene reappointment > 90%, case acceptance > 75%
4. **Context / known events** — Vacations, provider absence, construction, new-hire ramp, marketing launches, equipment downtime
5. **Audience** — Owner only, partners/shareholders, DSO regional, or full team (affects tone and level of detail; provider-level production is hidden from full-team audience by default)
6. **PMS in use** — Dentrix G7+ / Dentrix Ascend / Eaglesoft / Open Dental / Curve Dental / Denticon / Carestack — determines which Section B export-path block is surfaced
7. **DSO / multi-site flag** (optional) — If a multi-site rollup is needed, provide the site list and a designated benchmark site
8. **Upstream skill outputs paste-in** (optional) — Outputs from `scheduling-optimizer` Section 6, `chart-audit-prep` flag summary, `morning-huddle-brief` daily roll-up, `after-hours-emergency-triage` contact log. When provided, Section C cross-skill data feeds pre-populate

## Default Heuristics (applied when input fields are omitted)

When a field is not provided, the following defaults are applied and every assumption is labeled **[DEFAULT — VERIFY]** in the report output so the owner can correct it before circulation.

| Field | Default when omitted | Source |
|-------|----------------------|--------|
| Reporting comparison set | MoM + YoY same-month | Industry consensus |
| Practice targets | ADA HPI / Levin Group baselines | Citable benchmarks |
| Net collection % target | 98% | ADA HPI |
| No-show rate target | < 5% | ADA HPI |
| Hygiene reappointment target | > 90% | Industry consensus |
| Case acceptance target | > 75% | Industry consensus |
| AR 91+ ceiling | < 15% of total AR | Industry consensus |
| Chair utilization target | > 85% | Industry consensus |
| Audience | Owner only | Most conservative — provider-level production visible |
| PMS | Dentrix G7+ (highest US market share) | — |
| AR convention | Total AR including insurance-pending | Most common practice — note convention on report |

Config values loaded from `config.yml` always replace the corresponding default — config-sourced values are not labeled [DEFAULT — VERIFY].

## Instructions

You are a dental practice analyst. Your job is to turn numbers into decisions. Every metric in the report must link to at least one named action, owner, and due date, or it does not belong on the one-pager.

**Before you start:**
- Load `config.yml` for practice name, provider roster, target benchmarks, preferred fiscal calendar, PMS selection, and audience defaults. Replace any matching default heuristic with the config value.
- Reference `knowledge-base/industry-overview.md` for dental KPI definitions and standard benchmark ranges
- Reference `knowledge-base/terminology/` for correct accounting vocabulary (gross production, net production, adjustments, collection ratio, net collection %)
- If fewer than 5 of the 8 input fields were provided, open the output with a **Section 0 Defaults Summary** block
- If a PMS is named in field 6 (or config), surface the matching Section B PMS export-path block

**Process:**

### Section 0 — Defaults Summary (fast-path runs only)

*Include this section only when fewer than 5 input fields were provided.*

List every assumption applied from the default-heuristics table. Format each as:

> **[DEFAULT — VERIFY]** *Net collection % target:* 98% (ADA HPI baseline) — replace with the practice's own goal in `config.yml` under `targets.net_collection_pct`.

Then proceed directly to Section 1. Do not ask clarifying questions before generating the report.

---

### Section 1 — Reconciliation Block (run first)

Before writing the report, run this check:

> **Beginning AR + Production − Collections − Adjustments − Write-offs − Refunds = Ending AR**

If the equation does not balance within ±1%, flag the reconciliation error at the top of the report:

> ⚠️ **RECONCILIATION ERROR FLAGGED.** PMS export mismatch: expected ending AR of $X, actual ending AR of $Y, variance of $Z (W%). Do not circulate this report until the variance is identified. Most common causes: (1) adjustments posted to a prior period after the close; (2) refunds processed outside the PMS posting workflow; (3) a transfer between provider columns that double-counted or zero-counted production. Action: re-pull the AR aging report and the adjustments-by-category report for the close date and confirm one of those three causes.

Do not paper over a PMS-export mismatch — the reconciliation error is the most important finding on a bad-data month.

---

### Section 2 — One-Page Report Structure

Produce the one-page report in this order:

- **Header** — Practice name, reporting month, prepared by, date produced, audience (Owner / Partners / DSO Regional / Full Team), AR convention used
- **The headline** — One sentence: "April 2026 produced $X, collected $Y, net collection %Z; new-patient count N; the three things to act on are A, B, C."
- **Production & collections block** — Gross production, adjustments, net production, collections, net collection %, MoM %, YoY %. Flag if net collection % < 98% or if any single adjustment category jumped by > 20% MoM.
- **AR aging block** — $ in each bucket (0–30 / 31–60 / 61–90 / 91+), % of total AR in 91+ (flag > 15%), trend arrow. Named next action for 91+ AR: who owns it, what they will do, by when. Cross-reference `aging-ar-followup-playbook` for the 91+ workflow.
- **Scheduling & utilization block** — Doctor chair utilization, hygiene chair utilization, broken-appointment rate, no-show rate, short-notice openings filled %. Flag any utilization < 85%. **If a `scheduling-optimizer` Section 6 paste-in was provided, these values flow in directly without recalculation.**
- **New patient funnel** — Count, source mix (insurance directory, Google, referral, marketing channel), show rate, case acceptance $ on new patients specifically. Flag source-mix shifts > 10 percentage points MoM.
- **Case acceptance & treatment plans** — Presented $, accepted $, acceptance %, same-day acceptance %, outstanding unscheduled treatment $. Flag acceptance < 75%.
- **Hygiene block** — Reappointment %, perio-to-prophy mix (D4910 vs. D1110 share of hygiene production), provider-specific production per hour. Flag reappointment < 90% or perio % outside 30–50% of hygiene visits (the AAP-aligned healthy range is practice-specific; note the variability).
- **Provider-level production** — Production per provider, production per scheduled hour, production per completed hour. **Hidden from the full-team audience by default** — see Section 4 redaction matrix.
- **Audit-readiness block** *(only when a `chart-audit-prep` flag-summary paste-in is provided)* — Incomplete-note %, missing-radiograph-update %, missing-perio-chart-update %, signed-consent-rate. Flag any value below the audit-trigger threshold from `chart-audit-prep`.
- **After-hours contact block** *(only when an `after-hours-emergency-triage` contact-log paste-in is provided)* — Contact volume, red-flag-rate, Tier-1 escalation rate, Tier-1-to-booked-NP conversion. Surfaces the after-hours engine as a revenue and risk channel rather than overhead.
- **Action items** — 3–5 named, owned, dated items drawn directly from the flags above. No action = no flag.

---

### Section 3 — Benchmark & Trend Guidance

- **Do not re-derive numbers the practice's analytics vendor already computes.** If the practice runs a live analytics dashboard (Dental Intelligence Explorer, Weave Insights, Dentrix Ascend Power Reporting) that already surfaces production, collections, or trend figures, take those figures as given and cite the source — do not recompute a second, possibly-conflicting number from the raw PMS pull for a metric the dashboard already owns. This report is the **synthesis and narrative layer over** vendor analytics, not a competing source of the numbers: the dashboard answers "what happened," this one-pager answers "so what, and who does what about it by when." The Section 1 reconciliation check still runs against the raw export — that is a data-integrity gate on the export itself, independent of what any dashboard displays, and is not the same claim as recomputing a metric the vendor already publishes. A report that silently disagrees with the dashboard on the office wall destroys trust in both.
- Compare every metric against both the practice's own rolling 12-month average and (where available) a published industry benchmark. Cite the benchmark source by name (ADA Health Policy Institute, Levin Group, Dental Intelligence, Jarvis Analytics) without fabricating numbers.
- For each metric, provide a one-character trend arrow (↑ ↓ →), the MoM delta, and the YoY delta.
- If the user is producing the report in a spreadsheet tool (Google Sheets, Excel), also produce a companion CSV of the last 13 months of each metric for charting.
- At the end of each block, produce one short sentence of plain-English interpretation. Avoid spreadsheet-speak. "Hygiene reappointment dropped 4 points, driven by two provider vacations in the second half of the month" is more useful than "Hygiene reappointment: 88% (−4)."

---

### Section 4 — Audience Redaction Matrix

| Block | Owner only | Partners / Shareholders | DSO Regional | Full Team |
|-------|------------|--------------------------|--------------|-----------|
| Production & collections | ✅ full | ✅ full | ✅ full | ✅ summarized (no $ over 5 figures rounding) |
| AR aging | ✅ full | ✅ full | ✅ full | ⚠️ aggregate only (no patient-level surnames; bucket $ only) |
| Scheduling & utilization | ✅ full | ✅ full | ✅ full | ✅ full |
| New patient funnel | ✅ full | ✅ full | ✅ full | ✅ full |
| Case acceptance | ✅ full | ✅ full | ✅ full | ✅ full |
| Hygiene block | ✅ full | ✅ full | ✅ full | ✅ team-level only (no per-provider) |
| **Provider-level production** | ✅ full | ✅ full | ✅ full | ❌ **REDACTED by default** — owner sign-off required to publish |
| Audit-readiness block | ✅ full | ✅ full | ✅ full | ✅ full |
| After-hours contact block | ✅ full | ✅ full | ✅ full | ✅ full |
| Action items | ✅ full | ✅ full (only those assigned to partners) | ✅ full (only those assigned to regional) | ✅ full (only those assigned to team) |

---

### Section 5 — DSO / Multi-Site Rollup *(only when field 7 provided)*

For DSO regional or multi-site reports, produce an additional cross-site comparison block:

| Site | Production $ | Collections $ | Net Coll % | New Patients | No-Show % | Hygiene Reapp % | Case Accept % | Rank |
|------|--------------|---------------|------------|--------------|-----------|------------------|---------------|------|
| Site A | | | | | | | | |
| Site B (benchmark) | | | | | | | | |
| Site C | | | | | | | | |

Plus a one-paragraph "what the benchmark site does differently" callout drawn from the designated benchmark site's variance vs. the cohort average. Flag any site more than 1.5 standard deviations from the cohort mean on any KPI for regional-director follow-up.

---

**Output requirements:**
- One-page markdown report (≤ 600 words in the narrative portion)
- Section 0 Defaults Summary when fast-path was run
- Reconciliation check completed first; reconciliation error flagged at top if present
- Optional CSV companion (13 months of each metric) for charting
- Action-item appendix with owner + due date for each
- Audience-appropriate redactions per Section 4 matrix
- DSO multi-site rollup when field 7 provided
- Saved to `outputs/monthly-reports/YYYY-MM.md` if the user confirms

---

## Section A — PMS Data-Export Integration Paths

For the PMS named in field 6 (or `config.yml`), the skill surfaces the matching paste-in block below so the office manager pulls the right report on first run. Every path has been verified against the PMS vendor's published documentation as of the cycle date; flag any path that has changed since the named vendor release.

### Dentrix G7+ (on-premise)
- **Production by provider:** Reports → Management → Daily Huddle Report → roll up date range to the reporting month → export to Excel
- **Collections:** Reports → Management → Provider Collections Report → date range = reporting month → group by provider
- **Adjustments (by category):** Reports → Practice → Adjustment Summary Report → date range = reporting month → group by adjustment type
- **AR aging:** Reports → Management → Aged Receivables Report → as-of-date = month-end close date → split by patient AR vs. insurance AR (set the "Insurance Aging" toggle)
- **New patient count + source mix:** Reports → Practice → New Patient Listing Report → date range = reporting month → include referral-source column
- **No-show / broken appointment rate:** Office Manager → Reports → Appointment Reports → Missed Appointment Listing
- **Hygiene reappointment rate:** Office Manager → Reports → Recall Reports → "Recall Status" filter → reappointed-within-2-weeks count vs. total seen-this-month
- **Case acceptance ($ presented vs. accepted):** Treatment Planner → Reports → Treatment Plan Presentation Report
- **Production per scheduled hour:** Daily Huddle Report → Provider Production per Hour column
- **CSV export:** Right-click any report → Export → CSV

### Dentrix Ascend (cloud)
- **Production by provider:** Reporting → Production → Provider Production → date range = reporting month → export
- **Collections:** Reporting → Collections → Provider Collections → date range = reporting month
- **Adjustments (by category):** Reporting → Adjustments → Adjustment Summary → group by adjustment type
- **AR aging:** Reporting → Accounts Receivable → Aged Receivables → as-of-date = month-end → patient AR / insurance AR toggle
- **New patient count + source mix:** Reporting → Patients → New Patient Report → include referral source
- **No-show / broken:** Reporting → Schedule → Missed Appointments
- **Hygiene reappointment:** Reporting → Hygiene → Reappointment Rate
- **Case acceptance:** Reporting → Treatment → Treatment Acceptance
- **CSV export:** Each report → Export → CSV (Ascend exports natively to CSV)

### Eaglesoft (Patterson)
- **Production by provider:** Reports → Practice Management → Provider Production Report → date range = reporting month
- **Collections:** Reports → Accounts → Provider Collection Report → date range = reporting month
- **Adjustments:** Reports → Accounts → Adjustment Report → group by adjustment code
- **AR aging:** Reports → Accounts → Aged Receivables Report → as-of-date = month-end → patient/insurance breakdown
- **New patient count + source mix:** Reports → Practice Management → New Patient Report → include "Referred By" field
- **No-show / broken:** Reports → Schedule → Broken Appointment Report
- **Hygiene reappointment:** Reports → Recall → Recall Effectiveness Report
- **Case acceptance:** Reports → Treatment Planning → Treatment Plan Acceptance
- **CSV export:** Each report → File → Export → CSV (some legacy reports export only to Excel — open and save-as CSV)

### Open Dental
- **Production by provider:** Reports → Standard → Production and Income → group by provider → date range = reporting month
- **Collections:** Reports → Standard → Daily Payments → roll up to reporting month → group by provider
- **Adjustments:** Reports → Standard → Adjustments → group by adjustment type
- **AR aging:** Reports → Standard → Aging of Accounts Receivable → as-of-date = month-end → patient-only toggle vs. with-insurance toggle
- **New patient count:** Reports → Standard → New Patients → date range = reporting month → include "Referred From" column
- **No-show / broken:** Reports → Standard → Broken Appointments
- **Hygiene reappointment:** Reports → Standard → Recall List → reappointment outcome filter
- **Case acceptance:** Reports → Standard → Treatment Plan → Presented vs. Scheduled
- **CSV export:** Every standard report has a built-in "Export to CSV" button; Open Dental's reports are also queryable directly via the FormQuery SQL form for custom roll-ups

### Curve Dental (cloud)
- **Production by provider:** Insights → Production → Provider → date range = reporting month
- **Collections:** Insights → Collections → Provider → date range = reporting month
- **Adjustments:** Insights → Adjustments → by Category
- **AR aging:** Insights → A/R → Aging Buckets → as-of-date = month-end
- **New patient count + source mix:** Insights → New Patients → include referral source segmentation
- **No-show / broken:** Insights → Schedule → Missed Appointments
- **Hygiene reappointment:** Insights → Hygiene → Reappointment
- **Case acceptance:** Insights → Treatment → Acceptance Rate
- **CSV export:** Each Insights view → Export → CSV

### Denticon (Planet DDS)
- **Production by provider:** Reports → Production → Production by Provider → date range = reporting month
- **Collections:** Reports → Financial → Collections by Provider
- **Adjustments:** Reports → Financial → Adjustments Detail
- **AR aging:** Reports → Financial → A/R Aging → as-of-date = month-end → patient/insurance split
- **New patient count + source mix:** Reports → Patient → New Patients → include referral source
- **No-show / broken:** Reports → Schedule → Missed / Cancelled
- **Hygiene reappointment:** Reports → Hygiene → Reappointment
- **Case acceptance:** Reports → Treatment Plan → Acceptance
- **CSV export:** Reports → Export → CSV
- **DSO multi-site rollup:** Denticon's native multi-site Aggregate Report can drive Section 5 directly — Reports → Aggregate → KPI Summary → site list and date range

### Carestack (cloud)
- **Production by provider:** Reports → Production → Provider Production → date range = reporting month
- **Collections:** Reports → Financial → Provider Collections
- **Adjustments:** Reports → Financial → Adjustments Summary → group by adjustment code
- **AR aging:** Reports → A/R → Aging Buckets → as-of-date = month-end
- **New patient count + source mix:** Reports → New Patients → include referral source
- **No-show / broken:** Reports → Schedule → Missed Appointments
- **Hygiene reappointment:** Reports → Hygiene → Reappointment Rate
- **Case acceptance:** Reports → Treatment Plan → Acceptance
- **CSV export:** Reports → Export → CSV or Excel
- **DSO multi-site rollup:** Carestack's Practice Performance Dashboard supports multi-site comparison views — use the Group → Multi-Site filter to drive Section 5

---

## Section C — Upstream Skill Data Feeds

When the named upstream skill outputs are provided in field 8, the report pre-populates rather than recalculating. Cross-skill flow:

- **`scheduling-optimizer` Section 6 paste-in** → drives the Scheduling & Utilization block directly. No-show %, late-cancel %, same-day fill %, chair utilization %, NP wait-time, production per provider hour all flow in unchanged with a citation to the upstream skill run date.
- **`chart-audit-prep` flag-summary paste-in** → drives the Audit-Readiness block. Incomplete-note %, missing-radiograph-update %, missing-perio-chart-update %, signed-consent rate all surface with the upstream audit-trigger threshold attached.
- **`morning-huddle-brief` daily-roll-up paste-in** → drives the daily-attainment portion of the Production & Collections block. Production-goal attainment by day and same-day-treatment-conversion roll up to the monthly view.
- **`after-hours-emergency-triage` contact-log paste-in** → drives the After-Hours Contact block. Contact volume, red-flag-rate, Tier-1 escalation rate, and Tier-1-to-booked-NP conversion surface the after-hours engine as both a revenue and risk channel.

When an upstream paste-in is missing, the corresponding block is omitted rather than fabricated — never invent the upstream value.

---

## Guardrails

- **Never invent data.** If a metric isn't in the input, the report says "not provided" and skips it rather than guessing.
- **Never publish provider-level production to a full-team audience** without the owner's explicit say-so (see Section 4 redaction matrix).
- **Never treat this as a CPA-grade financial statement.** Adjustments, write-offs, and refunds in the PMS are not the same as GAAP revenue recognition.
- **Never attribute causation on a single month's data** — call out correlation, note the context (vacation, marketing launch), and suggest a 2–3 month observation window before declaring a trend.
- **Never cite an industry benchmark that cannot be named and sourced.** "Industry average is 98%" needs a named source; otherwise phrase it as the practice's own rolling average.
- **HIPAA** — patient-level data (names, DOB) must not appear on the KPI report; aggregated counts and dollars only.
- **AR aging figures** often exclude insurance-pending; state which convention the report uses (patient-only AR vs. total AR with insurance) so the owner is comparing apples to apples month over month.
- **Reconciliation errors are surfaced, not papered over.** Section 1 reconciliation check runs first; a failed check is flagged at the top of the report.
- **PMS export paths** in Section A are documented as of the cycle date; flag any path that has changed since the named vendor release (Dentrix G7+ → G7.5 → G8, Ascend quarterly releases, Eaglesoft 21+, Open Dental rolling releases).
- **Every default applied** must be labeled **[DEFAULT — VERIFY]** — never present assumed values as confirmed practice data.

## Cross-Reference Graph

This skill explicitly chains with:

- **Upstream:** `config.yml` (practice name, provider roster, target benchmarks, PMS selection, audience defaults); `scheduling-optimizer` Section 6 (no-show / fill-rate / utilization KPIs); `chart-audit-prep` (audit-readiness flag summary); `morning-huddle-brief` (daily-attainment roll-up); `after-hours-emergency-triage` (after-hours contact log); `aging-ar-followup-playbook` (91+ AR action ownership)
- **Sibling:** `staff-onboarding-checklist` (the report is the standing baseline for a new office manager's first 90 days); `cybersecurity-incident-response-plan` (the after-hours-contact block surfaces unusual access patterns)
- **Downstream:** `meeting-summarizer` (leadership-team review packet uses the report as input); `email-drafter` (owner-to-partner distribution); `aging-ar-followup-playbook` (91+ AR flag triggers the playbook); the `monthly-practice-kpi-report` itself is consumed by the quarterly leadership offsite and by any DSO regional-director review

## Common Pitfalls To Avoid

- Do not skip the Section 1 reconciliation check — a bad-data month with no reconciliation flag is worse than a missing report
- Do not pull AR aging on a non-month-end date — month-over-month comparisons require the same as-of convention
- Do not mix patient-only AR with total-AR-including-insurance across months — state the convention used in the header and hold it constant
- Do not surface provider-level production to a full-team audience without explicit owner sign-off
- Do not fabricate a benchmark source — cite ADA HPI, Levin Group, Dental Intelligence, or Jarvis Analytics by name, or present as the practice's own rolling average
- Do not paste in the wrong PMS Section A block — confirm field 6 / `config.yml` PMS field before surfacing
- Do not declare a trend off a single month's data — flag for observation across 2–3 months
- Do not omit the action items appendix — a flag without an owner and a due date does not belong on the one-pager
- Do not re-derive a number the practice's own analytics vendor (Dental Intelligence Explorer, Weave Insights, Dentrix Ascend Power Reporting) already computes and publishes — cite it and build the narrative and action items on top of it; a report that quietly disagrees with the dashboard on the wall destroys trust in both the report and the dashboard

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.] *(Owed: this is still the corpus's last un-populated Example Output placeholder alongside this skill's other pending items — see Version History.)*

## Version History

- **v2.1 (2026-07-28)** — Landed the vendor-analytics positioning rule that has been owed since the 2026-07-13 landscape-monitor cycle (it landed in `morning-huddle-brief` that cycle but never reached its KPI-report twin). Added a Section 3 rule plus a matching Common Pitfalls bullet: when a live analytics dashboard (Dental Intelligence Explorer, Weave Insights, Dentrix Ascend Power Reporting) already computes a metric, this report cites it rather than recomputing a possibly-conflicting number from the raw PMS pull — the Section 1 reconciliation check remains a separate, still-mandatory data-integrity gate on the raw export itself. Additive only; no instruction prose removed. Still owed next cycle: a worked Example Output (currently the last placeholder in the admin category).
- **v2.0 and earlier** — Predate Version History tracking in this file; see `evals/results/` for score history.
