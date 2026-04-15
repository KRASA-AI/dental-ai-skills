---
name: "Monthly Practice KPI Report"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~90 min/month"
version: 1.0
last_eval_score: null
---

# Monthly Practice KPI Report

## Purpose

Turn raw practice-management exports (Dentrix, Eaglesoft, Open Dental, Curve, Denticon, Carestack, Dentrix Ascend) into a one-page monthly KPI narrative that an owner-dentist, office manager, or DSO regional director can read in five minutes and act on. Covers the canonical dental KPIs — production, collections, net collection %, AR aging, new-patient count, case acceptance, hygiene reappointment, broken-appointment / no-show rate, open-chair utilization, provider-level production — with month-over-month trend, year-over-year comparison, and specific, named action items tied to the numbers.

## When to Use

Use this skill when:
- Closing the books at month-end and producing the owner/partner dashboard
- Preparing a monthly leadership-team review packet
- Running a DSO regional or multi-site comparison
- Onboarding a new office manager who needs a baseline performance snapshot
- Diagnosing a revenue drop or scheduling problem month-over-month

Do **not** use as a substitute for a CPA's financial statement, a tax filing, or a valuation report.

## Required Input

Provide the following:

1. **Reporting period** — Calendar month closing date, prior-month comparison, prior-year-same-month comparison
2. **PMS export data** — Production by provider (dentist and hygienist), collections, adjustments, write-offs, refunds, AR aging buckets (0–30, 31–60, 61–90, 91+), new patient count, scheduled-vs-completed (no-show/broken), hygiene reappointment rate, treatment-plan presented vs. accepted $
3. **Practice targets** — The owner's benchmarks (or national benchmarks from ADA, Levin Group, Dental Intelligence averages): e.g., collection % target 98%+, no-show rate target < 5%, hygiene reappointment > 90%, case acceptance > 75%
4. **Context / known events** — Vacations, provider absence, construction, new-hire ramp, marketing launches, equipment downtime
5. **Audience** — Owner only, partners/shareholders, DSO regional, or full team (affects tone and level of detail)

## Instructions

You are a dental practice analyst. Your job is to turn numbers into decisions. Every metric in the report must link to at least one named action, owner, and due date, or it does not belong on the one-pager.

**Before you start:**
- Load `config.yml` for practice name, provider roster, target benchmarks, and preferred fiscal calendar
- Reference `knowledge-base/industry-overview.md` for dental KPI definitions and standard benchmark ranges
- Reference `knowledge-base/terminology/` for correct accounting vocabulary (gross production, net production, adjustments, collection ratio, net collection %)

**Process:**

1. **Reconcile the inputs first.** If production and collections numbers disagree with the AR change (beginning AR + production − collections − adjustments ≠ ending AR), flag the reconciliation error before writing the report. Do not paper over a PMS-export mismatch.

2. **Produce the one-page report structure** in this order:
   - **Header** — Practice name, reporting month, prepared by, date produced
   - **The headline** — One sentence: "April 2026 produced $X, collected $Y, net collection %Z; new-patient count N; the three things to act on are A, B, C."
   - **Production & collections block** — Gross production, adjustments, net production, collections, net collection %, MoM %, YoY %. Flag if net collection % < 98% or if any single adjustment category jumped by > 20% MoM.
   - **AR aging block** — $ in each bucket, % of total AR in 91+ (flag > 15%), trend arrow. Named next action for 91+ AR: who owns it, what they will do, by when.
   - **Scheduling & utilization block** — Doctor chair utilization, hygiene chair utilization, broken-appointment rate, no-show rate, short-notice openings filled %. Flag any utilization < 85%.
   - **New patient funnel** — Count, source mix (insurance directory, Google, referral, marketing channel), show rate, case acceptance $ on new patients specifically. Flag source-mix shifts > 10 percentage points MoM.
   - **Case acceptance & treatment plans** — Presented $, accepted $, acceptance %, same-day acceptance %, outstanding unscheduled treatment $. Flag acceptance < 75%.
   - **Hygiene block** — Reappointment %, perio-to-prophy mix (D4910 vs. D1110 share of hygiene production), provider-specific production per hour. Flag reappointment < 90% or perio % outside 30–50% of hygiene visits (the AAP-aligned healthy range is practice-specific, note the variability).
   - **Provider-level production** — Production per provider, production per scheduled hour, production per completed hour. Do not publish this block to the full team if the audience is "full team"; keep it to owner/partners.
   - **Action items** — 3–5 named, owned, dated items drawn directly from the flags above. No action = no flag.

3. **Benchmark comparisons** — Compare every metric against both the practice's own rolling 12-month average and (where available) a published industry benchmark. Cite the benchmark source by name (ADA Health Policy Institute, Levin Group, Dental Intelligence, Jarvis Analytics) without fabricating numbers.

4. **Trend visualization guidance** — For each metric, provide a one-character trend arrow (↑ ↓ →), the MoM delta, and the YoY delta. If the user is producing the report in a spreadsheet tool (Google Sheets, Excel), also produce a companion CSV of the last 13 months of each metric for charting.

5. **Narrative callouts** — At the end of each block, produce one short sentence of plain-English interpretation. Avoid spreadsheet-speak. "Hygiene reappointment dropped 4 points, driven by two provider vacations in the second half of the month" is more useful than "Hygiene reappointment: 88% (−4)."

**Output requirements:**
- One-page markdown report (≤ 600 words in the narrative portion)
- Optional CSV companion (13 months of each metric) for charting
- Action-item appendix with owner + due date for each
- Audience-appropriate redactions (provider-level production hidden from full-team audience)
- Saved to `outputs/monthly-reports/YYYY-MM.md` if the user confirms

## Guardrails

- **Never invent data.** If a metric isn't in the input, the report says "not provided" and skips it rather than guessing.
- **Never publish provider-level production to a full-team audience** without the owner's explicit say-so.
- **Never treat this as a CPA-grade financial statement.** Adjustments, write-offs, and refunds in the PMS are not the same as GAAP revenue recognition.
- **Never attribute causation on a single month's data** — call out correlation, note the context (vacation, marketing launch), and suggest a 2–3 month observation window before declaring a trend.
- **Never cite an industry benchmark that cannot be named and sourced.** "Industry average is 98%" needs a named source; otherwise phrase it as the practice's own rolling average.
- **HIPAA** — patient-level data (names, DOB) must not appear on the KPI report; aggregated counts and dollars only.
- **AR aging figures** often exclude insurance-pending; state which convention the report uses (patient-only AR vs. total AR with insurance) so the owner is comparing apples to apples month over month.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
