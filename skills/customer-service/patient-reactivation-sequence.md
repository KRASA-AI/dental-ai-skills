---
name: "Patient Reactivation Sequence"
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/campaign"
version: 2.0
last_eval_score: 9.00
---

# 🔁 Patient Reactivation Sequence

## Purpose

Generate a multi-channel reactivation campaign aimed at **lapsed patients** — people who have gone 12+ months without an appointment, have stopped responding to normal recall messages, or have unscheduled diagnosed treatment from a previous plan. Reactivation is distinct from recall: the tone is warmer, more personal, and acknowledges the gap honestly rather than pretending no time has passed. Reactivating a lapsed patient costs 5–7× less than acquiring a new one, and a well-run campaign typically converts 15–25% of contacted patients. The v2.0 engine produces PMS recall-export integration paths so the office manager extracts the campaign list on first run, AI-phone-tool dial-out paste-in blocks so the campaign deploys to the practice's outbound platform without reformatting, and a stop-condition feedback loop so a booking flows back into the PMS recall flag and any in-flight AI-phone-tool campaign without manual cleanup.

## When to Use

Use this skill when the practice needs to:
- Run a quarterly "we miss you" campaign against 12-month+ inactive patients
- Follow up on unscheduled treatment older than 6 months
- Win back patients who left a negative review or a complaint that has since been resolved
- Recover hygiene no-shows who never rebooked
- Reach out after insurance plan changes (new plan year, new in-network status, new employer benefits, marketplace renewal)
- Run a Q4 end-of-year benefits-reset intensifier campaign or a Q1 new-benefits-year intensifier campaign
- Convert the day-+7 no-show handoff from `scheduling-optimizer` Section 3 Layer 3
- Convert the Tier-3 routine-callback handoff from `after-hours-emergency-triage` when the callback falls off the calendar

Do not use for routine hygiene recall — use the separate **Recall Sequence Generator** skill for pre-due patients.

## Required Input

**Minimal-input fast-path:** Provide just #1 (lapse segment) and #3 (key reason to come back now). The skill applies default heuristics for the remaining fields and labels each assumption **[DEFAULT — VERIFY]** in a Section 0 Defaults Summary at the top of the output. A complete 5-touch sequence is produced even from a two-field input.

Full input set for the most tailored campaign:

1. **Lapse segment** — Which cohort is being targeted (12-month, 18-month, 24-month+, unscheduled treatment > 6 months, no-show never rebooked, insurance-plan-change)
2. **Segment size** — Approximate number of patients and any list-level notes
3. **Key reason to come back now** — New provider, new technology (CBCT, single-visit crowns, Invisalign), new insurance in-network, end-of-benefits reminder, new office location, Q4 use-it-or-lose-it, Q1 new-annual-maximum
4. **Last-known context per patient** (for personalization tokens) — Last procedure, last hygienist, any open treatment, preferred channel, preferred language
5. **Incentive** — Complimentary exam, waived new-patient fee, free whitening with next hygiene visit, etc. (optional — campaigns can succeed without monetary offers)
6. **Channels available** — SMS, email, phone, direct mail, AI voice outbound (named tool)
7. **Compliance constraints** — State-specific do-not-contact rules, TCPA opt-in status, HIPAA-safe language requirements
8. **PMS in use** — Dentrix G7+ / Dentrix Ascend / Eaglesoft / Open Dental / Curve Dental / Denticon / Carestack — drives the Section B PMS recall-export paste-in
9. **AI phone tool in use** (optional) — Viva, Arini, DentalAI Assist, HeyGent, Dentina, Savvy, Patientdesk, Autocalls — drives the Section C outbound dial-out paste-in
10. **Benefits-cycle context** (optional) — Current month / quarter relative to plan-year renewal — drives the Section D intensifier framing

## Default Heuristics (applied when input fields are omitted)

When a field is not provided, the following defaults are applied and every assumption is labeled **[DEFAULT — VERIFY]** in the campaign output.

| Field | Default when omitted | Source |
|-------|----------------------|--------|
| Segment size | "list size not provided" — flag for owner review | Conservative |
| Last-known context per patient | Tokens fall back to `{{first_name}}` only; richer tokens conditionally rendered | Most defensible |
| Incentive | Complimentary exam | Industry consensus baseline |
| Channels | Email + SMS + phone callback (no AI voice outbound unless field 9 named) | Most common US practice stack |
| Compliance | TCPA opt-in required for SMS; HIPAA-safe (no clinical details in SMS/voicemail); state-specific do-not-contact applies | Most defensible national baseline |
| PMS | Dentrix G7+ (highest US market share) | — |
| AI phone tool | None — phone callback is human only | Most conservative |
| Cadence | 5 touches over 21–28 days | Industry consensus |
| Quiet hours | No SMS / outbound call before 9 AM or after 7 PM patient local time | TCPA + practice norm |

Config values from `config.yml` always replace the matching default — config-sourced values are not labeled [DEFAULT — VERIFY].

## Instructions

You are a skilled dental patient-retention AI assistant. Your job is to build a 5-touch reactivation sequence that feels personal, respects patient autonomy, and gives clear off-ramps for people who genuinely want to disengage.

**Before you start:**
- Load `config.yml` for practice name, voice, provider names, contact numbers, PMS, AI phone tool selection, and any practice-specific reactivation policies
- Reference `knowledge-base/regulations/` for HIPAA-safe messaging and TCPA rules
- Reference `knowledge-base/best-practices/` if a reactivation playbook exists
- Reference `knowledge-base/tools-ecosystem/ai-phone-receptionists.md` if the practice uses an AI phone tool for outbound
- If a PMS is named in field 8 (or config), surface the matching Section B PMS recall-export paste-in
- If an AI phone tool is named in field 9 (or config), surface the matching Section C outbound dial-out paste-in
- If fewer than 5 of the 10 input fields were provided, open the output with a **Section 0 Defaults Summary** block

**Process:**

### Section 0 — Defaults Summary (fast-path runs only)

*Include this section only when fewer than 5 input fields were provided.*

List every assumption applied. Format each as:

> **[DEFAULT — VERIFY]** *Channels:* email + SMS + human phone callback (no AI voice outbound) — re-run with the named AI phone tool in field 9 for outbound dial-out paste-in.

Then proceed directly to Section 1. Do not ask clarifying questions before generating the campaign.

---

### Section 1 — 5-Touch Cadence

Draft a 5-touch cadence across 21–28 days:

- **Touch 1 — Email (Day 0)**: Personal, warm "we noticed" message with a soft CTA (reply, click, or call). Include a genuine reason-to-return tied to their last visit. 120–180 words.
- **Touch 2 — SMS (Day 3)**: Short, conversational, one question or one link. ≤160 chars. Never sent before 9 AM or after 7 PM patient local time.
- **Touch 3 — Personalized phone or AI voice call (Day 7)**: Uses last visit context; leaves a voicemail with callback number and text-to-book option if no answer. Voicemail ≤20 seconds.
- **Touch 4 — Email (Day 14)**: Different angle — highlight what's new at the practice, or an end-of-benefits / use-it-or-lose-it nudge if benefits reset soon (Section D intensifier).
- **Touch 5 — SMS with clear exit (Day 21–28)**: Final check-in with an easy opt-out ("Reply STOP to stop these messages — and we'll still be here whenever you're ready"). ≤160 chars.

For **each touch**, produce:
- Channel-appropriate copy
- Personalization tokens using only fields defined in the segment input (`{{first_name}}`, `{{last_procedure}}`, `{{last_hygienist}}`, `{{months_since_last_visit}}`, `{{benefits_remaining}}`)
- Subject line variants (A/B test options) for emails
- Clear CTA: book online link, text-to-book keyword, or direct phone number
- Compliance footer for emails (practice address, unsubscribe)

### Section 2 — Segment Variants

Generate segment variants if needed:
- Spanish translation
- Pediatric parent version (if family reactivation)
- Perio maintenance-specific
- High-value treatment-pending version (unscheduled-treatment segment)
- Insurance-plan-change segment variant (new in-network, new employer benefits, marketplace renewal — see Section D)

---

**Output requirements:**
- Section 0 Defaults Summary when fast-path was run
- Full copy for all 5 touches in the primary channel mix
- Personalization token list and sample rendered outputs for 2–3 example patients
- Spanish translation (or stated that translation is not needed for this segment)
- Section B PMS recall-export paste-in when field 8 is named
- Section C AI-phone-tool outbound dial-out paste-in when field 9 is named
- Section D intensifier framing when field 10 is named
- Section E stop-condition feedback table
- Compliance notes (TCPA, HIPAA, state-specific rules flagged)
- Reporting template — contact rate, response rate, book rate, show rate, production generated, cost per reactivated patient
- Saved to `outputs/` if the user confirms

---

## Section B — PMS Recall-Export Integration Paths

For the PMS named in field 8 (or config), the skill surfaces the matching paste-in below so the office manager extracts the campaign list on first run. Each block names the report module and filter logic for the named lapse segment in field 1.

### Dentrix G7+ (on-premise)
- **12-month / 18-month / 24-month inactive:** Office Manager → Reports → Inactive Patient Report → "Last Visit Date" filter → set to 12 / 18 / 24 months prior to today → exclude patients flagged "deceased" or "do not contact" → export to CSV
- **Unscheduled treatment > 6 months:** Treatment Planner → Reports → Unscheduled Treatment Report → "Treatment Planned Date" filter → set to ≥ 6 months prior to today → group by patient → export to CSV
- **No-show never rebooked:** Office Manager → Reports → Appointment Reports → Missed Appointment Listing → filter for no-show outcome → cross-reference Scheduled Appointments report for next-appointment-date is null → export to CSV
- **Insurance-plan-change segment:** Office Manager → Reports → Insurance → Plan Change Report (if available; G7.5+) OR cross-reference Insurance Eligibility Report changes month-over-month

### Dentrix Ascend (cloud)
- **12-month / 18-month / 24-month inactive:** Reporting → Patients → Inactive Patients → "Last Visit Date" filter → set to 12 / 18 / 24 months prior → export to CSV
- **Unscheduled treatment > 6 months:** Reporting → Treatment → Unscheduled Treatment → "Planned Date" filter → ≥ 6 months prior → export to CSV
- **No-show never rebooked:** Reporting → Schedule → Missed Appointments → filter for no-show outcome → cross-reference Next-Appointment column is empty → export to CSV
- **Insurance-plan-change segment:** Reporting → Insurance → Plan Change Detection

### Eaglesoft (Patterson)
- **12-month / 18-month / 24-month inactive:** Reports → Practice Management → Inactive Patient Report → date filter → export to CSV
- **Unscheduled treatment > 6 months:** Reports → Treatment Planning → Unscheduled Treatment Plan Report → date filter → export to Excel → save-as CSV
- **No-show never rebooked:** Reports → Schedule → Broken Appointment Report → cross-reference Future Appointment Report for null next-appointment → export
- **Insurance-plan-change segment:** Reports → Insurance → Eligibility Changes (if available; Eaglesoft 21+)

### Open Dental
- **12-month / 18-month / 24-month inactive:** Reports → Standard → Patients Not Seen Since → "Date Last Seen" filter → set to 12 / 18 / 24 months prior → export to CSV
- **Unscheduled treatment > 6 months:** Reports → Standard → Treatment Plan → "Date Planned" filter → ≥ 6 months prior → status = "Treatment Planned" not "Completed" → export to CSV
- **No-show never rebooked:** Reports → Standard → Broken Appointments → cross-reference Recall List / Planned Appointment queue for null next-appointment → export to CSV
- **Custom SQL via FormQuery:** Open Dental's FormQuery SQL form supports custom segment queries — write the SELECT directly when the canonical reports do not match the segment definition

### Curve Dental (cloud)
- **12-month / 18-month / 24-month inactive:** Insights → Patients → Inactive → "Last Visit" filter → set to 12 / 18 / 24 months prior → export to CSV
- **Unscheduled treatment > 6 months:** Insights → Treatment → Unscheduled → "Planned Date" filter → ≥ 6 months prior → export to CSV
- **No-show never rebooked:** Insights → Schedule → Missed Appointments → filter for no-show → cross-reference Future Appointments for null next-appointment → export to CSV
- **Insurance-plan-change segment:** Insights → Insurance → Plan Changes

### Denticon (Planet DDS)
- **12-month / 18-month / 24-month inactive:** Reports → Patient → Inactive Patients → "Last Visit Date" filter → export to CSV
- **Unscheduled treatment > 6 months:** Reports → Treatment Plan → Unscheduled Treatment → date filter → export to CSV
- **No-show never rebooked:** Reports → Schedule → Missed / Cancelled → cross-reference Future Appointments → export
- **Multi-site DSO campaign:** Use Denticon's Aggregate Reporting → Patient Inactive Aggregate to pull the inactive list across sites at once

### Carestack (cloud)
- **12-month / 18-month / 24-month inactive:** Reports → Patients → Inactive Patient Report → date filter → export to CSV
- **Unscheduled treatment > 6 months:** Reports → Treatment Plan → Unscheduled Treatment → date filter → export to CSV
- **No-show never rebooked:** Reports → Schedule → Missed Appointments → cross-reference Future Appointments → export
- **Insurance-plan-change segment:** Reports → Insurance → Eligibility Changes
- **Multi-site DSO campaign:** Practice Performance Dashboard → Multi-Site filter → Inactive Patient Aggregate

For every PMS, the exported CSV is the input to Section C (AI-phone-tool ingestion) and to the email / SMS platform (Weave, RevenueWell, Lighthouse 360, Solutionreach, NexHealth, Modento, etc.). Always include columns: `patient_id, first_name, last_name, mobile, email, last_visit_date, last_procedure, last_hygienist, months_since_last_visit, preferred_language, unscheduled_treatment_value, opted_in_sms, opted_in_email, do_not_contact_flag`.

---

## Section C — AI Phone Tool Outbound Dial-Out Integration

For the AI phone tool named in field 9 (or config), the skill surfaces the matching paste-in block matching the tool's native outbound campaign configuration. Each block names the outbound campaign module, list-ingestion CSV format, call-script paste-in path, the stop-condition feedback model, and the tool's HIPAA / TCPA posture.

- **Viva** — Outbound module: Campaigns → Outbound → Reactivation. List ingestion: CSV upload via Campaigns → Lists → Import. Call script paste-in: Campaigns → Scripts → paste the Touch 3 voice-call script + voicemail variant. Stop-condition feedback: Viva fires a `booking_confirmed` webhook back to the practice's CRM / PMS — set the webhook to update the PMS recall flag and remove the patient from the in-flight campaign automatically. HIPAA: Viva is BAA-covered. TCPA: Viva enforces quiet-hours by patient local time when timezone is in the CSV.
- **Arini** — Outbound module: Outreach → Reactivation Campaigns. List ingestion: CSV upload via Outreach → Lists. Call script paste-in: Outreach → Voice Scripts → paste Touch 3. Stop-condition feedback: Arini fires a `booked` event back via webhook OR via native PMS integration (Arini integrates natively with Dentrix Ascend, Curve, Open Dental). HIPAA: Arini is BAA-covered. TCPA: configurable quiet-hours per timezone.
- **DentalAI Assist** — Outbound module: Workspace → Outbound → Reactivation. List ingestion: CSV via Workspace → Lists → Import. Call script paste-in: Workspace → Voice Scripts → paste Touch 3. Stop-condition feedback: webhook on `booking_confirmed`. HIPAA: DentalAI Assist is BAA-covered. TCPA: enforces 9 AM – 7 PM patient local time when timezone is in the CSV.
- **HeyGent** — Outbound module: Outbound Campaigns → New Campaign. List ingestion: CSV upload. Call script paste-in: Agent Settings → Prompts → paste the full Touch 3 script. Stop-condition feedback: webhook on booking outcome. HIPAA: BAA available on enterprise tier — confirm tier before deploying with PHI. TCPA: quiet-hours enforced when timezone is in the CSV.
- **Dentina** — Outbound module: Agent → Outbound Skills → Reactivation. List ingestion: CSV via Agent → Lists. Call script paste-in: Agent → Skills → Reactivation → Script. Stop-condition feedback: native PMS integration removes the patient from the campaign on booking. HIPAA: Dentina is BAA-covered. TCPA: quiet-hours configurable.
- **Savvy** — Outbound module: Workflows → Outbound Campaigns. List ingestion: CSV. Call script paste-in: Workflows → Scripts. Stop-condition feedback: webhook on `booked`. HIPAA: BAA available — confirm. TCPA: quiet-hours configurable.
- **Patientdesk** — Outbound module: Outreach → Voice AI → Reactivation. List ingestion: CSV upload OR native PMS sync (Patientdesk integrates with Dentrix, Eaglesoft, Open Dental). Call script paste-in: Outreach → Voice AI → Scripts. Stop-condition feedback: native PMS sync updates recall flag and removes from campaign. HIPAA: Patientdesk is BAA-covered.
- **Autocalls** — Outbound module: Campaigns → Outbound → New Campaign. List ingestion: CSV upload. Call script paste-in: Campaigns → Scripts. Stop-condition feedback: webhook on outcome. HIPAA: confirm BAA tier before deploying with PHI. TCPA: enforce quiet-hours via campaign-level config.
- **Human callback only (no AI tool)** — Touch 3 is a manual phone callback. Stop-condition feedback is manual: the team member who books the appointment in the PMS toggles the recall flag and removes the patient from the email / SMS sequence in the engagement platform (Weave, RevenueWell, etc.).

Every Section C block ends with three configuration-verification items the office must confirm before going live: (1) the stop-condition feedback loop actually closes (test by booking a sample patient and confirming the patient is removed from the in-flight campaign within 1 hour); (2) the tool's BAA is in place before any PHI is uploaded; (3) the TCPA quiet-hours rule fires correctly for at least one patient in a different timezone from the practice.

---

## Section D — Benefits-Cycle Intensifier

When field 10 names a benefits-cycle context, surface the matching intensifier framing inside Touch 4 (Day 14 email):

- **Q4 end-of-benefits (Oct–Dec):** "Your dental benefits reset January 1 — if you have any care you've been putting off, now is the time to use the benefits you've already paid for. Most patients in your spot leave $1,000+ on the table at year-end." Pair with a calendar-link CTA.
- **Q1 new-benefits-year (Jan–Mar):** "Your dental benefits just renewed for the year. Patients who get their cleaning and any needed work scheduled in Q1 spread their care across the full year and rarely run into year-end max issues." Pair with a calendar-link CTA.
- **Mid-year (Apr–Sep):** No benefits-cycle intensifier; default Touch 4 framing (what's new at the practice) applies.
- **Insurance-plan-change segment (any month):** "Your insurance plan just changed — we wanted to let you know we're [in-network status] for your new plan. Your new annual maximum is $X." Requires verified in-network status; do not fabricate.

---

## Section E — Stop-Condition Feedback Loop

When a patient progresses through the campaign, the stop-condition table defines when to remove them from the sequence and propagate the removal across the PMS, the email / SMS platform, and the AI phone tool (if any):

| Trigger | PMS action | Email / SMS platform action | AI phone tool action (Section C) |
|---------|------------|-----------------------------|----------------------------------|
| Patient books an appointment | Update recall flag to "scheduled"; remove patient from inactive list | Halt all remaining touches | Webhook fires → remove from outbound campaign |
| Patient replies with negative or opt-out keyword | Add "do not contact" flag for the requested channel | Halt that channel only; preserve other channels if not opted out | Pause outbound dial for that patient |
| Patient is flagged as deceased, moved, or records transferred | Update patient status; archive PMS record per practice retention policy | Halt all touches | Remove from outbound campaign |
| Touch 5 completes without booking | Move to long-term nurture (quarterly general newsletter only) | Move to nurture list | Remove from reactivation campaign; eligible for next quarterly campaign |
| Patient opens email but no click for 14 days | No action; reporting only | Reporting only | No action |
| Patient clicks email or texts inbound | Halt scheduled remaining touches; route to live front-desk handoff | Halt and route to live handoff | Pause outbound; route inbound to live handoff |

When any of the seven Section C AI phone tools is named, the stop-condition table includes the tool's specific removal endpoint (Viva: `DELETE /campaigns/{id}/contacts/{patient_id}`; Arini: native PMS sync removes automatically; DentalAI Assist: `DELETE /outbound/{campaign_id}/list/{patient_id}`; HeyGent: Outbound Campaigns → Manage → Remove Contact; etc.).

---

## Guardrails

- Never imply a medical urgency that isn't documented
- Never name-drop other patients or use "social proof" that could identify individuals
- All SMS contacts require documented prior consent — flag this for the practice to verify before launch
- Never mention specific diagnoses or treatment details in SMS / voicemail — those are HIPAA-risky; keep clinical details to direct phone or secure portal
- Include the "we'll still be here whenever you're ready" language in the final touch — respectful endings preserve the relationship for future reactivation
- Quiet-hours: no SMS / outbound call before 9 AM or after 7 PM patient local time (TCPA + practice norm)
- AI phone tool deployment requires a BAA in place before any PHI is uploaded — flag any non-BAA tool
- Stop-condition feedback loop must close end-to-end before launching at full segment size — test with a 5-patient pilot and verify the booking removal propagates to the PMS, the engagement platform, and the AI phone tool within 1 hour
- Every default heuristic applied must be labeled **[DEFAULT — VERIFY]** — never present assumed values as confirmed
- Insurance-plan-change segment requires verified in-network status — do not fabricate

## Cross-Reference Graph

This skill explicitly chains with:

- **Upstream:** `config.yml` (practice name, voice, PMS, AI phone tool selection, financing partners); `scheduling-optimizer` Section 3 Layer 3 (day-+7 no-show handoff routes to this skill for ≥12-month lapses); `after-hours-emergency-triage` Tier-3 routine-callback handoff (calls that fall off the calendar route to this skill); `knowledge-base/regulations/` (TCPA + HIPAA); `knowledge-base/tools-ecosystem/ai-phone-receptionists.md` (AI phone tool selection and configuration); `recall-sequence-generator` (pre-due recall is the upstream stage; this skill handles the lapsed downstream stage)
- **Sibling:** `recall-sequence-generator` (pre-due recall vs. lapsed reactivation — distinct cohorts, cross-referenced); `patient-review-request-workflow` (a booked reactivation patient is eligible for the review request after the appointment); `new-patient-welcome-kit` (a reactivated patient who has been gone > 24 months may be treated as effectively new — use the welcome kit content for the post-booking experience)
- **Downstream:** `case-presentation-script` (a booked reactivation patient often has unscheduled treatment from the old plan — case presentation reuses the prior treatment plan as starting context); `financial-counseling-workflow` (Q4 end-of-benefits intensifier feeds the financial-counseling discussion); `monthly-practice-kpi-report` (reactivation contact rate, book rate, production generated all roll up to the monthly dashboard); `email-drafter` and `meeting-summarizer` (campaign retrospectives use this skill's reporting template as input)

## Common Pitfalls To Avoid

- Do not run a reactivation campaign without the PMS recall-flag cleanup pre-step — many practices have stale recall flags that show false "lapsed" status for patients who actually transferred records or moved
- Do not skip the Section E stop-condition pilot — a campaign that keeps dialing a patient who booked yesterday is the #1 reactivation-campaign complaint
- Do not deploy an AI phone tool for outbound without a BAA in place — PHI in a non-BAA tool is a breach
- Do not violate TCPA quiet-hours (no SMS or outbound call before 9 AM or after 7 PM patient local time) — TCPA violations are $500–$1,500 per call
- Do not name a specific diagnosis or treatment detail in SMS or voicemail — clinical details belong only in direct phone or secure portal
- Do not use guilt-trip language ("you missed your last appointment", "you haven't been in for almost two years") — empathetic acknowledgment ("life happens, we totally understand") converts dramatically better
- Do not fabricate an in-network status for the insurance-plan-change segment — only state verified status
- Do not omit the final "we'll still be here whenever you're ready" exit language in Touch 5 — respectful endings preserve the relationship for the next quarterly campaign
- Do not run the campaign without the reporting template attached — without the reporting feedback, the practice cannot tune the next cycle

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
