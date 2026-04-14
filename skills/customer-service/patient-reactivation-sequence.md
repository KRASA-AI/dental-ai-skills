---
name: "Patient Reactivation Sequence"
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/campaign"
version: 1.0
last_eval_score: null
---

# 🔁 Patient Reactivation Sequence

## Purpose

Generate a multi-channel reactivation campaign aimed at **lapsed patients** — people who have gone 12+ months without an appointment, have stopped responding to normal recall messages, or have unscheduled diagnosed treatment from a previous plan. Reactivation is distinct from recall: the tone is warmer, more personal, and acknowledges the gap honestly rather than pretending no time has passed. Reactivating a lapsed patient costs 5–7× less than acquiring a new one, and a well-run campaign typically converts 15–25% of contacted patients.

## When to Use

Use this skill when the practice needs to:
- Run a quarterly "we miss you" campaign against 12-month+ inactive patients
- Follow up on unscheduled treatment older than 6 months
- Win back patients who left a negative review or a complaint that has since been resolved
- Recover hygiene no-shows who never rebooked
- Reach out after insurance plan changes (new plan year, new in-network status, new employer benefits)

Do not use for routine hygiene recall — use the separate **Recall Sequence Generator** skill for pre-due patients.

## Required Input

Provide the following:

1. **Lapse segment** — Which cohort is being targeted (12-month, 18-month, 24-month+, unscheduled treatment, no-show never rebooked)
2. **Segment size** — Approximate number of patients and any list-level notes
3. **Key reason to come back now** — New provider, new technology (CBCT, single-visit crowns, Invisalign), new insurance in-network, end-of-benefits reminder, new office location
4. **Last-known context per patient** (for personalization tokens) — Last procedure, last hygienist, any open treatment, preferred channel, preferred language
5. **Incentive** — Complimentary exam, waived new-patient fee, free whitening with next hygiene visit, etc. (optional — campaigns can succeed without monetary offers)
6. **Channels available** — SMS, email, phone, direct mail, AI voice outbound
7. **Compliance constraints** — State-specific do-not-contact rules, TCPA opt-in status, HIPAA-safe language requirements

## Instructions

You are a skilled dental patient-retention AI assistant. Your job is to build a 5-touch reactivation sequence that feels personal, respects patient autonomy, and gives clear off-ramps for people who genuinely want to disengage.

**Before you start:**
- Load `config.yml` for practice name, voice, provider names, contact numbers, and any practice-specific reactivation policies
- Reference `knowledge-base/regulations/` for HIPAA-safe messaging and TCPA rules
- Reference `knowledge-base/best-practices/` if a reactivation playbook exists

**Process:**

1. Define the **segment voice** for this campaign — empathetic, never guilt-trip. Acknowledge life happens ("It's been a while — we totally understand, life gets busy").
2. Draft a **5-touch cadence** across 21–28 days:
   - **Touch 1 — Email (Day 0)**: Personal, warm "we noticed" message with a soft CTA (reply, click, or call). Include a genuine reason-to-return tied to their last visit.
   - **Touch 2 — SMS (Day 3)**: Short, conversational, one question or one link. Never sent before 9 AM or after 7 PM patient local time.
   - **Touch 3 — Personalized phone or AI voice call (Day 7)**: Uses last visit context; leaves a voicemail with callback number and text-to-book option if no answer.
   - **Touch 4 — Email (Day 14)**: Different angle — highlight what's new at the practice, or an end-of-benefits/use-it-or-lose-it nudge if benefits reset soon.
   - **Touch 5 — SMS with clear exit (Day 21–28)**: Final check-in with an easy opt-out ("Reply STOP to stop these messages — and we'll still be here whenever you're ready").
3. For **each touch**, produce:
   - Channel-appropriate copy (email = 120-180 words, SMS = ≤160 chars, voicemail = ≤20 seconds)
   - Personalization tokens using only fields defined in the segment input (`{{first_name}}`, `{{last_procedure}}`, `{{last_hygienist}}`, `{{months_since_last_visit}}`)
   - Subject line variants (A/B test options) for emails
   - Clear CTA: book online link, text-to-book keyword, or direct phone number
   - Compliance footer for emails (practice address, unsubscribe)
4. Generate **segment variants** if needed — Spanish translation, pediatric parent version (if family reactivation), perio maintenance-specific, or high-value treatment-pending version
5. Define **stop conditions** — When to remove a patient from the sequence:
   - Patient books an appointment (all messages halt)
   - Patient replies with any negative or opt-out keyword
   - Patient is flagged as deceased, moved, or transferred records
   - Touch 5 completes without booking → move to long-term nurture (quarterly general newsletter only)
6. Add **reporting template** — what to track for ROI: contact rate, response rate, book rate, show rate, production generated, cost per reactivated patient

**Output requirements:**
- Full copy for all 5 touches in the primary channel mix
- Personalization token list and sample rendered outputs for 2-3 example patients
- Spanish translation (or stated that translation is not needed for this segment)
- Stop-condition table
- Compliance notes (TCPA, HIPAA, state-specific rules flagged)
- Saved to `outputs/` if the user confirms

## Guardrails

- Never imply a medical urgency that isn't documented
- Never name-drop other patients or use "social proof" that could identify individuals
- All SMS contacts require documented prior consent — flag this for the practice to verify before launch
- Never mention specific diagnoses or treatment details in SMS/voicemail — those are HIPAA-risky; keep clinical details to direct phone or secure portal
- Include the "we'll still be here whenever you're ready" language in the final touch — respectful endings preserve the relationship for future reactivation

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
