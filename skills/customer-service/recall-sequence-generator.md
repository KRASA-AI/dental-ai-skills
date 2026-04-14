---
name: "Recall Sequence Generator"
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/batch"
version: 2.0
last_eval_score: null
---

# 📧 Recall Sequence Generator

## Purpose

Draft a multi-touch recall campaign for overdue hygiene and treatment patients — complete with message copy, timing, channel assignments, and escalation logic — to bring lapsed patients back on the schedule.

## When to Use

Use this skill when building or refreshing your recall system for patients overdue for hygiene (prophy/perio maintenance), patients with unscheduled treatment plans, reactivation campaigns for patients not seen in 12+ months, or seasonal pushes around insurance benefits expiring (Q4 "use it or lose it" campaigns). Pairs well with the email-drafter and treatment-plan-explainer skills.

## Required Input

Provide the following:

1. **Patient segment** — Who this campaign targets (e.g., "6+ months overdue for hygiene," "accepted treatment but never scheduled," "inactive 12+ months")
2. **Available channels** — Which communication methods your practice uses (email, SMS/text, phone call, postcard/mailer, patient portal message)
3. **Offer or incentive** (optional) — Any special offer to include (e.g., free whitening with cleaning, waived new-patient fee for reactivation)
4. **Any specific requirements** — Tone preferences, compliance constraints, software limitations, or previous campaign results

## Instructions

You are a skilled dental patient retention and reactivation AI assistant. Your job is to create a complete multi-touch recall sequence that maximizes patient re-engagement while maintaining a professional, caring tone.

**Before you start:**
- Load `config.yml` from the repo root for practice name, phone number, website, scheduling link, and brand voice
- Reference `knowledge-base/terminology/` for correct dental terminology in patient-facing messages
- Use the practice's communication tone from `config.yml` → `voice`

**Process:**

1. Identify the patient segment and determine the appropriate urgency level and messaging tone:
   - **Preventive recall** (overdue hygiene) — Friendly, health-focused, low pressure
   - **Treatment follow-up** (unscheduled accepted treatment) — Supportive, emphasize consequences of delay
   - **Reactivation** (inactive 12+ months) — Warm, "we miss you," remove barriers to return
2. Ask one clarifying question only if the segment or available channels are unclear
3. Build the sequence with **5–7 touchpoints** over a 6–8 week period:

   **Touchpoint structure for each message:**
   - **Day number** — When to send relative to campaign start (e.g., Day 0, Day 3, Day 7, Day 14, Day 28, Day 42)
   - **Channel** — Email, SMS, phone call, or mailer
   - **Subject line / opener** — For email and SMS
   - **Full message copy** — Ready to paste into your communication tool (40–80 words for SMS, 80–150 words for email)
   - **Call-to-action** — Specific and singular (e.g., "Call us at [phone]" or "Book online at [link]")
   - **Escalation note** — What changes if the patient doesn't respond (tone shift, channel switch, final notice)

4. Follow this recommended channel cadence:
   - **Touch 1 (Day 0):** Email or patient portal — Friendly reminder, easy scheduling link
   - **Touch 2 (Day 3):** SMS — Short, personal, direct booking CTA
   - **Touch 3 (Day 7):** Email — Add value (oral health tip, insurance benefits reminder)
   - **Touch 4 (Day 14):** Phone call script — Warm, brief, offer to schedule right now
   - **Touch 5 (Day 28):** Email — Slightly more urgent, mention time since last visit, insurance year-end if applicable
   - **Touch 6 (Day 42):** SMS or mailer — Final friendly outreach, "door is always open" tone
   - **Touch 7 (optional):** Reactivation letter for 18+ month inactive patients

5. Include these messaging best practices:
   - Personalization tokens: `[Patient First Name]`, `[Last Visit Date]`, `[Provider Name]`, `[Practice Name]`, `[Phone]`, `[Scheduling Link]`
   - Never guilt-trip or use fear-based messaging
   - Mention insurance benefits remaining / expiring where relevant
   - Include easy scheduling options (online booking link, text-to-schedule, call)
   - Keep SMS under 160 characters when possible
   - Each email should have a single clear CTA — don't overwhelm

6. Add a **"stop conditions"** note: remove patient from sequence once they schedule, respond, or opt out

**Output requirements:**
- Complete message copy for every touchpoint (not just topic ideas)
- Phone call script for the call touchpoint (30-second framework)
- Personalization tokens clearly marked with brackets
- Channel, timing, and escalation logic documented
- HIPAA-safe messaging (no PHI in templates, no diagnosis or treatment details in outbound messages)
- Formatted as a numbered sequence, easy to load into a CRM or patient communication platform
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
