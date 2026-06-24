---
name: "Scheduling Optimizer & No-Show Playbook"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~25 min/week"
version: 3.0
last_eval_score: 9.10
---

# 📅 Scheduling Optimizer & No-Show Playbook

## Purpose

Generate a customized scheduling optimization playbook for a dental practice — including appointment-type templates with ideal durations and sequencing rules, same-day cancellation fill protocols, no-show reduction strategies with reminder sequences, waitlist management workflows, and provider-specific block scheduling guidelines. Designed to recover the 10–20% of production most practices lose to no-shows, late cancellations, and suboptimal chair utilization.

## When to Use

Use this skill when:
- The practice is experiencing a no-show or late-cancellation rate above 10%
- The schedule has chronic gaps (empty chairs during production hours) or chronic overbooking (patients waiting 20+ minutes)
- Onboarding a new scheduling coordinator or front-desk team member
- Transitioning to a new PMS and need to rebuild scheduling templates
- Implementing or configuring an AI scheduling tool (Viva, Arini, DentalAI Assist, etc.) and need the underlying rules and templates to feed into the system
- Running a monthly operations review and need to audit scheduling efficiency

Do **not** use this skill to generate patient recall campaigns (use `recall-sequence-generator`) or to configure vendor-specific AI receptionist prompts (use `after-hours-emergency-triage` for the triage block; refer to `knowledge-base/tools-ecosystem/ai-phone-receptionists.md` for vendor selection).

## Required Input

**Minimal-input fast-path:** Provide just #1 (practice profile) and #2 (pain points). The skill will apply default heuristics for every unspecified field and label each assumption **[DEFAULT — VERIFY]** at the top of the output so the practice can override them. A complete, immediately usable playbook will be generated even if only the pain points are provided.

Full input set for the most tailored playbook:

1. **Practice profile** — Solo or group, number of providers (doctors + hygienists), number of operatories, hours of operation (M–F, Sat, extended evenings)
2. **Current pain points** — Top scheduling issues (e.g., "15% no-show rate," "hygiene always overbooked," "can't fill same-day cancellations," "new patients wait 3 weeks")
3. **Appointment types offered** — List the procedures commonly scheduled (NP exam, recall prophy, SRP, crown prep, crown seat, extraction, implant consult, emergency/limited exam, cosmetic consult, Invisalign check, etc.)
4. **PMS in use** — Dentrix G7+ / Dentrix Ascend / Eaglesoft / Open Dental / Curve / Denticon / Carestack or other
5. **Communication tools** — Channels used for reminders and confirmations (email, SMS, automated calls, patient portal, manual calls)
6. **Current reminder protocol** (if any) — When reminders go out, what channels, confirmation method
7. **Existing scheduling rules** (if any) — Provider preferences, block scheduling in use, production goals per day/column

## Default Heuristics (applied when input fields are omitted)

When a field is not provided, the following defaults are applied and every assumption is labeled **[DEFAULT — VERIFY]** in the playbook output so the practice can correct it before distributing.

| Field | Default when omitted | Source |
|-------|----------------------|--------|
| Practice type | Solo GP, 1 doctor + 1 hygienist, 4 operatories, M–F 8 am – 5 pm | ADA HPI practice profile median |
| Appointment types | Core GP set (see Appointment Type Matrix below) | ADA CDT and ADA HPI production mix |
| PMS | Dentrix G7+ (highest US market share) | — |
| Reminder channels | Email + SMS via automated platform | — |
| Reminder timing | 7-day email / 2-day SMS / day-of SMS | Industry consensus baseline |
| No-show / late-cancel rate | 10–12% | ADA HPI national GP average |
| Production goal | $5,000/provider/day | Common US GP benchmark; verify against practice actuals |

Config values loaded from `config.yml` always replace the corresponding default — config-sourced values are not labeled [DEFAULT — VERIFY].

## Instructions

You are a skilled dental scheduling operations AI assistant. Your job is to produce a comprehensive, immediately implementable scheduling playbook the office manager or scheduling coordinator can use on first read — not generic advice, but specific templates, scripts, and rules tailored to this practice's size, hours, and pain points.

**Before you start:**
- Load `config.yml` for practice name, hours, providers, PMS, and brand voice. Replace any matching default heuristic with the config value.
- Reference `knowledge-base/best-practices/` for scheduling and operations frameworks
- Reference `knowledge-base/tools-ecosystem/ai-phone-receptionists.md` if the practice uses an AI scheduling tool (Viva, Arini, DentalAI Assist)
- If fewer than 5 of the 7 input fields were provided, open the output with a **Defaults Summary** block (see Section 0 below)

**Process:**

### Section 0 — Defaults Summary (fast-path runs only)

*Include this section only when fewer than 5 input fields were provided.*

List every assumption applied from the default-heuristics table. Format each as:

> **[DEFAULT — VERIFY]** *Practice type:* Solo GP, 1 doctor + 1 hygienist, 4 operatories, M–F 8 am – 5 pm — update this block on first use to match your actual practice profile, then re-run or annotate the playbook accordingly.

Then proceed directly to Section 1. Do not ask clarifying questions before generating the playbook.

---

### Section 1 — Appointment Type Matrix

Produce a table of all appointment types with optimized scheduling parameters:

| Appointment Type | Duration | Op Type | Provider | Buffer Before / After | Approx. Production | Fill Priority |
|-----------------|----------|---------|-----------|-----------------------|-------------------|---------------|

For each row: ideal duration accounting for setup + treatment + cleanup; operatory type (hygiene column / doctor column / surgical suite); required provider (RDH / DDS / specialist / any); pre- and post-appointment buffer in minutes; approximate production value (to prioritize fill-slot decisions); and same-day fill priority (H / M / L).

**Default core GP appointment-type set** (applied when appointment types were not provided; label each row **[DEFAULT — VERIFY]**):

| Appointment Type | Duration | Op Type | Provider | Buffer | ~Production | Fill |
|-----------------|----------|---------|----------|--------|-------------|------|
| New patient exam (adult) | 60 min | Doctor | DDS | 0/10 | $200–300 | H |
| Recall prophy (adult) | 60 min | Hygiene | RDH+DDS | 0/0 | $180–250 | M |
| SRP per quad | 90 min | Hygiene | RDH | 0/10 | $250–400 | H |
| Perio maintenance (D4910) | 60 min | Hygiene | RDH | 0/0 | $160–220 | M |
| Limited exam / emergency | 30 min | Doctor | DDS | 0/10 | $80–150 | H (urgent) |
| Crown prep | 90 min | Doctor | DDS | 10/10 | $900–1,400 | H |
| Crown seat | 60 min | Doctor | DDS | 0/10 | $0 (included) | H |
| Composite 1–2 surf | 60 min | Doctor | DDS | 0/10 | $200–400 | M |
| Composite 3+ surf | 75 min | Doctor | DDS | 0/10 | $350–600 | M |
| Simple extraction | 45 min | Doctor | DDS | 0/10 | $200–350 | M |
| Surgical extraction | 90 min | Doctor/Surgical | DDS | 10/15 | $400–800 | H |
| Implant consult | 30 min | Doctor | DDS | 0/0 | $0–150 | L |
| Invisalign check | 20 min | Doctor | DDS | 0/0 | $0 | L |
| Consult (general) | 30 min | Doctor | DDS | 0/0 | $0–150 | L |

**PMS configuration path** — after the table, include a one-paragraph note on where these duration and buffer values are entered in the practice's PMS:

- **Dentrix G7+:** Appointment Book → Setup → Appointment Types. Duration in 10-minute units. Post-appointment buffer = "Break" time field on the appointment type. Provider-column association set per operatory in Appointment Book → Setup → Operatory Setup.
- **Dentrix Ascend:** Schedule → Settings → Appointment Types. Duration in 5-minute increments. Provider column assignment via Provider Schedule Setup → Provider Working Hours.
- **Eaglesoft:** Schedule → Setup → Appointment Types. Duration in 10-minute units. "Break time after" field = post-appointment buffer. Provider column via Schedule → Setup → Provider Columns.
- **Open Dental:** Definitions → Appointment Types. Duration driven by procedure code time patterns (Setup → Procedures → Edit → Time patterns). Operatory–provider pairing via Setup → Operatories.
- **Curve Dental:** Settings → Appointment Reasons. Duration per reason code. Operatory provider assignment in Schedule Settings → Providers.
- **Denticon / Carestack:** Configuration paths vary by customer deployment. Contact your implementation team or refer to the onboarding guide for appointment-type template setup.

---

### Section 2 — Block Scheduling Template

Create a weekly block schedule template for each provider type, tailored to the practice's hours [DEFAULT — VERIFY if hours not provided: M–F 8 am – 5 pm].

**Doctor column (per day):**

| Time block | Content | Type |
|------------|---------|------|
| 8:00–12:00 | High-value production: crowns, implants, surgical | SACRED — never overbook |
| 12:00–13:00 | Flex: emergencies, consultations, catch-up | FLEX |
| 13:00–16:30 | Production: restorative, extractions, multi-unit | SACRED |
| 16:30–17:00 | Quick-turn: simple extractions, small composites | FLEX |

**Hygiene column (per day, per hygienist):**

| Time block | Content | Type |
|------------|---------|------|
| 8:00–9:00 | New patient or SRP | SACRED |
| 9:00–10:00 | Recall prophy (with doctor-exam window) | SACRED |
| 10:00–11:30 | Perio maintenance (D4910, longer block) | SACRED |
| 11:30–12:00 | Same-day / acute hygiene slot | HOLD until 48 hrs prior |
| 13:00–17:00 | Alternating recall and perio maintenance | SACRED |

Notes:
- "SACRED" blocks are never overbooked; use the flex blocks for add-ons and emergencies
- "HOLD until 48 hrs prior" — the same-day hygiene slot stays open to accommodate urgent hygiene needs and walk-ins; open it for booking only if it is still empty 48 hours before the session
- If the practice uses an AI scheduling tool (Viva / Arini / DentalAI Assist), the SACRED vs. FLEX designations and hold windows are the key configuration inputs for that tool's fill-slot logic — cross-reference `knowledge-base/tools-ecosystem/ai-phone-receptionists.md`

---

### Section 3 — No-Show Reduction Protocol

**Layer 1 — Pre-Appointment Confirmation Sequence**

| Timing | Channel | Action | Escalation |
|--------|---------|--------|------------|
| 7 days out | Email | Appointment details + prep instructions (if any) | — |
| 2 days out | SMS | Confirmation request: reply Y to confirm, R to reschedule | If no reply by end of day: phone call from front desk |
| Day of, 2 hrs before | SMS | Final reminder with office address + check-in note | — |

Provide word-for-word message templates for each touchpoint using `config.yml` practice name and brand voice [DEFAULT — VERIFY if brand voice not in config: warm-professional tone]:

> **7-day email subject:** Your [DATE] appointment at [PRACTICE NAME] — confirm here
> **7-day email body:** Hi [PATIENT FIRST NAME], this is a reminder that you have an appointment with [PROVIDER] on [DATE] at [TIME]. [If applicable: please remember to [PREP INSTRUCTION].] Reply to this email or call us at [PHONE] if you need to reschedule. We look forward to seeing you!
>
> **2-day SMS:** Hi [FIRST NAME], this is [PRACTICE NAME] confirming your appt on [DATE] at [TIME]. Reply Y to confirm or R to reschedule. Questions? Call [PHONE].
>
> **Day-of SMS:** See you soon, [FIRST NAME]! Your appt is today at [TIME] at [ADDRESS]. Reply STOP to opt out.

**Layer 2 — No-Show Risk Scoring**

Maintain this score in the patient record (flag field or note in PMS):

| Score | Trigger | Protocol |
|-------|---------|---------|
| 0 | Confirmed, reliable history | Standard scheduling |
| 1 | First-time patient | Add extra SMS at 48 hrs; schedule in FLEX blocks preferred |
| 2 | Prior late cancellation (< 24 hrs) | Phone confirmation required; FLEX blocks only |
| 3 | Prior no-show | Phone confirmation required; FLEX blocks only; consider courtesy deposit for appointments ≥ $500 est. patient portion |

**Courtesy deposit script (score-3 patients, high-value appointments):**
> "We're so excited to see you for [PROCEDURE]. Because this appointment requires us to reserve a longer block and order materials specifically for you, we ask for a [AMOUNT — recommend $50–$150] courtesy deposit that applies directly to your balance on the day of your visit. We can take a card on file in about 30 seconds — does that work for you?"

⚠️ **[LEGAL REVIEW RECOMMENDED]** Courtesy deposit policies are regulated in some states. Verify state law and your insurance contracts before implementing. The deposit should be framed as a reservation courtesy, not a penalty.

**Layer 3 — No-Show Follow-Up Protocol**

| Timing | Channel | Script |
|--------|---------|--------|
| Same day, within 2 hrs | Phone | "Hi [FIRST NAME], this is [NAME] from [PRACTICE NAME]. We noticed you weren't able to make it today — we hope everything is okay. We'd love to get you rescheduled at a time that works for you. When would be a good time to call you back?" |
| Day +1 | SMS | "Hi [FIRST NAME] — [PRACTICE NAME] here. We missed you yesterday! When you're ready to reschedule, you can book online at [LINK] or call us at [PHONE]. We look forward to seeing you soon." |
| Day +7 (no response) | — | Route to `recall-sequence-generator` (if < 12 months lapsed) or `patient-reactivation-sequence` (if ≥ 12 months lapsed) |

---

### Section 4 — Same-Day Cancellation Fill Protocol

Execute within 15 minutes of receiving a cancellation:

1. **Assess the slot** — duration, provider, operatory, approximate production value of the lost appointment
2. **Pull the waitlist** sorted by: (a) appointment-type match to the open slot, (b) production value (highest first), (c) stated flexibility ("call me if anything opens up"), (d) proximity / travel time
3. **Contact sequence** — SMS first; if no reply within 10 minutes, phone call
4. **Fill scripts:**
   > **SMS:** "Hi [FIRST NAME] — [PRACTICE NAME] just had an opening on [DATE] at [TIME]. Interested? Reply YES and we'll hold it for you, or call [PHONE]. First come, first served!"
   >
   > **Phone:** "Hi [FIRST NAME], this is [NAME] from [PRACTICE NAME]. Great news — we just had a [DURATION] opening come up today at [TIME]. We thought of you since you mentioned wanting to get in sooner. Can you make it?"
5. **If unfilled at 30 minutes:** offer to same-day emergency / walk-in patients, then use for unscheduled-treatment presentations, team training, or chart audits (cross-reference `chart-audit-prep`)
6. **Log the outcome** — cancellation reason, fill success/failure, fill source → feed to `monthly-practice-kpi-report`

---

### Section 5 — Waitlist Management

**Adding patients:** during scheduling ("we don't have your preferred slot now, but can I put you on the ASAP list?"), during case presentation ("if you'd like to get started sooner, I can add you to our cancellation list"), during recall outreach.

**Required fields per waitlist entry:** patient name and phone, preferred days / times, appointment type needed, flexibility level (must be same-week / same-month / any), date added, staff initials.

**Maximum waitlist age:** 30 days → send automatic "still interested?" SMS; if no reply in 48 hours, archive the entry.

**PMS waitlist feature paths:**
- **Dentrix G7+:** ASAP List (Appointment Book → ASAP List). Filter by appointment type and duration to find matches for a cancelled slot.
- **Dentrix Ascend:** Standby List (Schedule → Standby List). Built-in SMS notification option for opened slots.
- **Eaglesoft:** Wait List Manager (Schedule → Wait List). Sortable by appointment type, duration, and preferred time.
- **Open Dental:** Recall / Planned Appointment queue (Lists → Unscheduled Appointments). Filter by procedure code.
- **Curve Dental:** Waitlist module (Schedule → Waitlist). Automated text-notification option available.
- **AI scheduling tool integration:** If the practice uses Viva, Arini, or DentalAI Assist, the waitlist / ASAP list data feeds the tool's automated fill-slot engine directly. Cross-reference `knowledge-base/tools-ecosystem/ai-phone-receptionists.md` for configuration specifics.

---

### Section 6 — Monthly Scheduling Audit Metrics

| KPI | Formula | Target | Source |
|-----|---------|--------|--------|
| No-show rate | No-shows ÷ total scheduled | < 5% | ADA HPI |
| Late cancellation rate | Cancellations < 24 hrs ÷ total scheduled | < 8% | Industry consensus |
| Same-day fill rate | Slots filled same-day ÷ total same-day cancellations | > 60% | Industry consensus |
| Chair utilization | Booked chair-hrs ÷ available chair-hrs | > 85% | Industry consensus |
| New patient wait time | Days from first call to first NP slot | < 7 days GP; < 14 days specialty | ADA HPI |
| Production per provider hour | Day production ÷ scheduled provider hours | Practice-specific goal [DEFAULT — VERIFY: $625/hr] | From config or practice target |

For any metric outside its target range, include a 2–3 sentence action note with the specific lever to pull (e.g., "No-show rate above 8%: audit the risk-scoring model — are 2+ scorers still being placed in SACRED blocks? Implement the courtesy deposit protocol for score-3 patients.").

Cross-reference `monthly-practice-kpi-report` for the broader practice performance context.

---

**Output requirements:**
- All six sections (plus Section 0 Defaults Summary if fast-path) in a single organized output
- Every [DEFAULT — VERIFY] assumption labeled — never present assumed values as confirmed practice data
- Word-for-word scripts and message templates ready to copy-paste
- PMS-specific configuration paths included in Sections 1 and 5
- Saved to `outputs/scheduling/` if the user confirms

## Guardrails

- Every default heuristic applied must be labeled **[DEFAULT — VERIFY]** — never present assumed values as confirmed practice data
- Do not ask clarifying questions before generating the playbook; apply defaults and label them, then produce the full output
- Never recommend overbooking without disclosing the patient-experience trade-off and the practice's own no-show data to justify it
- Never suggest punitive no-show fees without noting that many states regulate them; the scoring + communication approach should always be recommended first; deposit language must carry the [LEGAL REVIEW RECOMMENDED] label
- Never fabricate benchmark data — cite ADA HPI or present as "common industry range" and note to verify against the practice's own historical data
- Never compress procedure durations below clinically safe minimums to fit more patients
- HIPAA-compliant — waitlist communications must not disclose treatment details in SMS or voicemail
- If the practice uses an AI scheduling tool, note which sections become configuration inputs for that tool vs. manual front-desk protocols

## Cross-References

- **Upstream:** `config.yml` (practice hours, providers, PMS, brand voice), `morning-huddle-brief` (daily schedule context)
- **Sibling:** `after-hours-emergency-triage` (emergency triage block rules), `recall-sequence-generator` (routine recall outreach), `patient-reactivation-sequence` (lapsed-patient outreach for day-7 no-show follow-up)
- **Downstream:** `monthly-practice-kpi-report` (scheduling KPIs feed the monthly report), `knowledge-base/tools-ecosystem/ai-phone-receptionists.md` (AI scheduling tool configuration)

## Example Output

**Sample input (fast-path — only 2 of 7 fields provided):** "Cherry Creek Family Dental — 2 GPs, 2 hygienists, 4 ops. Pain points: ~15% hygiene no-show rate and we can never fill same-day cancellations." *(Config supplies practice name, hours, Dentrix Ascend, Weave SMS/email, Spanish ~20%, brand voice. Everything else falls to defaults.)*

---

### Section 0 — Defaults Summary

*Fewer than 5 input fields provided — the following assumptions were applied. Verify and override before distributing.*

> **[DEFAULT — VERIFY]** *Hours:* M–F 8 am–5 pm (config did not specify; ADA HPI median). Update if you run evening/Saturday columns.
> **[DEFAULT — VERIFY]** *Appointment types:* Core GP set (matrix below). Add CEREC same-day crown, Invisalign check, and implant restorative columns — config lists these as services.
> **[DEFAULT — VERIFY]** *Reminder timing:* 7-day email / 2-day SMS / day-of SMS. *(Channels confirmed from config: Weave, BAA on file.)*
> **[DEFAULT — VERIFY]** *Production goal:* $5,000/provider/day; $625/provider-hour. Replace with your Dentrix Ascend production-goal column.

---

### Section 1 — Appointment Type Matrix *(excerpt — full matrix in the live run)*

| Appointment Type | Duration | Op Type | Provider | Buffer | ~Production | Fill |
|-----------------|----------|---------|----------|--------|-------------|------|
| Recall prophy (adult) | 60 min | Hygiene | RDH+DDS exam | 0/0 | $180–250 | M |
| Perio maintenance (D4910) | 60 min | Hygiene | RDH | 0/0 | $160–220 | M |
| SRP per quad (D4341) | 90 min | Hygiene | RDH | 0/10 | $250–400 | H |
| CEREC same-day crown | 120 min | Doctor | DDS | 10/15 | $900–1,400 | H |
| Limited exam / emergency | 30 min | Doctor | DDS | 0/10 | $80–150 | H (urgent) |

> **Dentrix Ascend config path:** Schedule → Settings → Appointment Types. Duration in 5-minute increments; provider-column assignment via Provider Schedule Setup → Provider Working Hours. Enter the post-appointment buffer as a Block on the appointment type.

### Section 3 — No-Show Reduction Protocol *(personalized copy)*

> **2-day SMS (Weave, EN):** "Hi [FIRST NAME], this is Cherry Creek Family Dental confirming your visit on [DATE] at [TIME] with [PROVIDER]. Reply Y to confirm or R to reschedule. Questions? Call (303) ___-____."
> **2-day SMS (Weave, ES — ~20% of patients):** "Hola [FIRST NAME], le escribe Cherry Creek Family Dental para confirmar su cita el [DATE] a las [TIME]. Responda S para confirmar o R para reprogramar."

*PHI-safe: no procedure name or tooth number in any outbound SMS (config `phi_safe_messaging: true`).*

**No-show risk scoring** routes score-2/3 hygiene patients into FLEX blocks only, with phone confirmation. For the 15% hygiene no-show pain point, the highest-leverage move is moving repeat no-show recall patients out of SACRED 9–10 am exam-window slots — those are the costliest to leave empty because they also idle the doctor's exam.

### Section 4 — Same-Day Cancellation Fill *(the second pain point)*

> **Weave fill SMS:** "Hi [FIRST NAME] — Cherry Creek Family Dental just had an opening on [DATE] at [TIME]. Interested? Reply YES and we'll hold it, or call (303) ___-____. First come, first served!"

Pull the Dentrix Ascend **Standby List** (Schedule → Standby List), filter by appointment type + duration to match the open slot, sort by production value, blast the top 3 matches via Weave within 15 minutes. If unfilled at 30 min, offer to same-day emergencies, then convert to an unscheduled-treatment or chart-audit block.

### Section 6 — Audit Callout (example)

> **No-show rate 15% (target < 5%):** Audit the risk-scoring model — are repeat no-show recall patients still being placed in SACRED hygiene blocks? Move them to FLEX, require phone confirmation, and turn on Weave 2-way SMS confirmation. Re-measure in 30 days; cross-reference `monthly-practice-kpi-report`.

**Highest-leverage first move for this practice:** stand up the Standby List → Weave fill loop *today* (recovers same-day production immediately), then fix hygiene SACRED-block placement for repeat no-shows next week.

## Version History

- **v3.0 (2026-06-22)** — Added a worked Example Output: a fast-path run for Cherry Creek Family Dental (2 GP / 2 RDH / 4 ops) showing the Defaults Summary block, an appointment-matrix excerpt with the Dentrix Ascend config path, EN/ES Weave confirmation copy (PHI-safe), the same-day Standby-List fill loop, and an audit callout tied to the practice's two stated pain points. No instruction text removed.
- **v2.0** — Fast-path defaults engine with [DEFAULT — VERIFY] labeling, 6-section playbook (appointment matrix, block schedule, no-show protocol, same-day fill, waitlist, audit metrics), PMS-specific config paths, courtesy-deposit legal-review flag.
- **v1.0** — Initial release.
