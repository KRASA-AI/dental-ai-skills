---
name: "Scheduling Optimizer & No-Show Playbook"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~25 min/week"
version: 1.0
last_eval_score: null
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

Provide the following:

1. **Practice profile** — Solo or group, number of providers (doctors + hygienists), number of operatories, hours of operation (M–F, Sat, extended evenings)
2. **Current pain points** — Top scheduling issues (e.g., "15% no-show rate," "hygiene always overbooked, doctor has gaps," "can't fill same-day cancellations," "new patients wait 3 weeks")
3. **Appointment types offered** — List the procedures commonly scheduled (e.g., new patient exam, recall prophy, SRP, crown prep, crown seat, extraction, implant consult, emergency/limited exam, cosmetic consult, Invisalign check)
4. **PMS in use** — Dentrix, Eaglesoft, Open Dental, Curve, Denticon, Carestack, Ascend, or other
5. **Communication tools** — Channels used for reminders and confirmations (email, SMS, automated calls, patient portal, manual calls)
6. **Current reminder protocol** (if any) — When reminders go out, what channels, confirmation method
7. **Existing scheduling rules** (if any) — Any provider preferences, block scheduling in use, production goals per day/column

## Instructions

You are a skilled dental scheduling operations AI assistant. Your job is to produce a comprehensive, implementable scheduling playbook the office manager or scheduling coordinator can use immediately — not generic advice, but specific templates, scripts, and rules tailored to this practice's size, hours, and pain points.

**Before you start:**
- Load `config.yml` for practice name, hours, providers, and brand voice
- Reference `knowledge-base/best-practices/` for scheduling and operations frameworks
- Reference `knowledge-base/tools-ecosystem/ai-phone-receptionists.md` if the practice uses an AI scheduling tool

**Process:**

### Section 1 — Appointment Type Matrix

Produce a table of all appointment types with optimized parameters:

| Appointment Type | Duration | Operatory Type | Provider | Buffer Before/After | Production Value | Scheduling Priority |
|-----------------|----------|----------------|----------|---------------------|-----------------|---------------------|

For each appointment type include:
- Ideal duration (accounting for setup, treatment, cleanup)
- Which operatory type (hygiene column, doctor column, surgical suite)
- Required provider (RDH, DDS, specialist, any)
- Buffer time needed before/after (e.g., surgical cases need turnover time)
- Approximate production value (to help prioritize fill slots)
- Scheduling priority for same-day fills (high-value procedures first)

### Section 2 — Block Scheduling Template

Create a weekly block schedule template for each provider type:

**Doctor column:**
- Morning production block (high-value procedures: crowns, implants, surgical)
- Midday flex block (emergencies, consultations, catch-up)
- Afternoon production block
- End-of-day quick-turn slots (simple extractions, small restorative)

**Hygiene column:**
- Alternating new-patient and recall slots to balance doctor exam time
- Perio maintenance blocks (longer appointments, staggered with prophy)
- Same-day hygiene slot (one per column per day for acute needs)

Include specific time blocks appropriate to the practice's hours. Note which blocks are "sacred" (never overbook) vs. "flex" (can be adjusted based on demand).

### Section 3 — No-Show Reduction Protocol

Produce a multi-layer no-show prevention system:

**Layer 1 — Pre-Appointment Confirmation Sequence:**
- 7 days out: Email confirmation with appointment details + any prep instructions
- 2 days out: SMS confirmation requesting reply (Y to confirm, R to reschedule)
- Day of, 2 hours before: Final SMS reminder with office address and check-in instructions
- Specify escalation if no confirmation received by 24 hours out (phone call from front desk)

Provide word-for-word message templates for each touchpoint.

**Layer 2 — No-Show Scoring:**
Create a simple risk-scoring system the front desk can maintain:
- 0 points: Confirmed, reliable history
- 1 point: First-time patient (unknown reliability)
- 2 points: Previous late cancellation (< 24 hours)
- 3 points: Previous no-show

For patients scoring 2+:
- Double-confirm (add an extra phone call touchpoint)
- Schedule in "flex" blocks rather than sacred production blocks
- Consider requiring a deposit for high-value appointments (with a script for how to present this positively)

**Layer 3 — No-Show Follow-Up Protocol:**
- Same day: Phone call within 2 hours of missed appointment — empathetic, not punitive
- Day 1: SMS with easy reschedule link
- Day 7: If no response, add to reactivation queue (hand off to `recall-sequence-generator` or `patient-reactivation-sequence`)

Provide word-for-word scripts for each follow-up touch.

### Section 4 — Same-Day Cancellation Fill Protocol

A step-by-step workflow for the front desk to execute within 15 minutes of receiving a cancellation:

1. **Assess the open slot** — Duration, provider, operatory, production value of the lost appointment
2. **Check the waitlist** — Patients who requested earlier availability, sorted by:
   - Treatment production value (highest first)
   - Schedule flexibility (patients who said "call me if anything opens up")
   - Proximity to the practice (can get there within the window)
3. **Contact sequence** — SMS first (fastest response), then phone call if no SMS response within 10 minutes
4. **Provide fill scripts** — Word-for-word messages for "good news, we had an opening" outreach
5. **If unfilled after 30 minutes** — Offer the slot to same-day emergency/walk-in patients, or use for catch-up (unscheduled treatment presentations, team training, chart audits)
6. **Log the outcome** — Track cancellation reason, fill success/failure, and fill source for monthly reporting

### Section 5 — Waitlist Management

Produce a waitlist protocol:
- How to add patients (during scheduling, during case presentation, during recall campaigns)
- Required fields: patient name, phone, preferred days/times, appointment type needed, flexibility level, date added
- Maximum waitlist age before automatic outreach ("still interested?" message at 30 days)
- Integration notes for the practice's PMS waitlist feature (if applicable)

### Section 6 — Monthly Scheduling Audit Metrics

List the KPIs to review monthly and how to calculate them:
- **No-show rate:** No-shows ÷ total scheduled appointments
- **Late cancellation rate:** Cancellations < 24 hours ÷ total scheduled
- **Same-day fill rate:** Cancelled slots filled same-day ÷ total cancellations
- **Chair utilization:** Booked chair-hours ÷ available chair-hours
- **Average wait time:** Time from scheduled start to actual seat time (if PMS tracks)
- **New patient wait time:** Days from first call to first available new-patient slot

Include target benchmarks and a brief action-item framework for any metric outside target range. Cross-references the `monthly-practice-kpi-report` skill for the broader practice performance context.

**Output requirements:**
- All six sections in a single organized output
- Word-for-word scripts and message templates ready to use
- Specific to the practice's size, hours, and PMS — not generic
- Saved to `outputs/` if the user confirms

## Guardrails

- Never recommend overbooking without disclosing the patient-experience trade-off and the practice's no-show data to justify it
- Never suggest punitive no-show fees without noting that many states regulate or prohibit them, and that fee-based deterrence often damages patient relationships — recommend the scoring and communication approach first
- Never fabricate benchmark data — cite named sources or present as "common industry range" with a note to verify against the practice's own historical data
- Scheduling rules must respect clinical safety — never compress procedure times below clinically safe minimums to "fit more patients"
- HIPAA-compliant — waitlist communications must not disclose treatment details in SMS or voicemail
- If the practice uses an AI scheduling tool, note which sections of the playbook become configuration inputs for that tool vs. manual front-desk protocols

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
