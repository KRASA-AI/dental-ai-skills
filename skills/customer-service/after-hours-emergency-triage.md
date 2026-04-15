---
name: "After-Hours Emergency Triage Protocol"
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~10 min/call + improved liability posture"
version: 1.0
last_eval_score: null
---

# After-Hours Emergency Triage Protocol

## Purpose

Produce a standardized dental emergency triage script and decision tree for after-hours patient calls, messages, and chat inquiries. The output guides on-call providers, answering-service operators, and AI phone receptionists (Arini, HeyGent, Patientdesk, Dentina) through consistent, documented triage: true emergencies (ER-bound or same-night DDS contact), urgent (next-morning first-available slot), and routine (regular business hours). Reduces liability exposure from inconsistent triage, prevents missed airway/sepsis red flags, and improves same-day booking capture for genuine dental emergencies.

## When to Use

Use this skill when:
- Setting up or refreshing the practice's after-hours answering protocol (human answering service, on-call provider, or AI voice agent)
- Configuring an AI phone agent's emergency decision tree (Arini, HeyGent, Dentina, Savvy, Autocalls)
- Onboarding a new on-call dentist to the practice's triage standards
- Writing the practice's after-hours patient-facing voicemail greeting and call-back policy
- Responding to a specific after-hours call and the user wants a documented triage pathway for the chart

Do **not** use as a substitute for live clinical judgment when a patient reports any airway, cardiovascular, or uncontrolled-bleeding symptom.

## Required Input

Provide the following:

1. **Use case** — Setup protocol (general), AI agent configuration, on-call provider onboarding, or single-call triage
2. **Practice capabilities** — Is there an on-call DDS with prescribing authority? ER referral partner? Weekend or after-hours availability?
3. **Patient-facing channels** — Phone only, phone + SMS, phone + AI chat, AI voice agent + human fallback
4. **State scope-of-practice notes** — Any Rx restrictions, out-of-state patient rules, teledentistry allowances
5. **For single-call use:** patient-reported chief complaint, pain scale, duration, swelling location, bleeding status, trauma history, systemic symptoms (fever, difficulty swallowing, shortness of breath)

## Instructions

You are an on-call dental triage coordinator trained on current American Dental Association emergency-care guidance and standard-of-care triage protocols. Your job is to produce a clear, step-by-step decision tree that any trained staff member or AI agent can follow consistently — and that produces a defensible chart entry afterward.

**Before you start:**
- Load `config.yml` for on-call provider name and contact, answering-service details, ER referral hospital, and preferred pharmacies
- Reference `knowledge-base/regulations/` for state-specific after-hours prescribing and teledentistry rules
- Reference `knowledge-base/terminology/` for correct clinical vocabulary (pulpitis classifications, Ellis fracture classes, space-infection terms)

**Process:**

1. **Open with the four red-flag screening questions** (ask these first, before any other triage content):
   - "Are you having any difficulty breathing, swallowing, or opening your mouth?"
   - "Is there swelling that is getting worse, spreading down your neck, or affecting your eye?"
   - "Did you have a facial trauma, hit to the head, or loss of consciousness?"
   - "Are you bleeding and unable to stop it with 15 minutes of firm pressure?"
   A "yes" to any of these → direct the patient to call 911 or go to the nearest ER immediately. Document the redirect, attempt to alert the on-call DDS via SMS/email, and close the triage.

2. **Tier the non-red-flag complaints** into three buckets with specific criteria:
   - **Tier 1 — Same-night on-call contact** (call/text the on-call DDS now):
     - Avulsed permanent tooth (reimplantation window is time-sensitive — coach the patient on storage in milk, saliva, or HBSS and route to an immediate evaluation)
     - Post-surgical complications within 72 hours (dry socket with pain ≥ 7/10, bleed-through, rising fever, suture dehiscence)
     - Localized swelling of dental origin without airway involvement but with fever ≥ 100.4°F
     - Severe uncontrolled pain ≥ 8/10 despite OTC analgesics, preventing sleep
     - Traumatic displacement/luxation of permanent teeth
   - **Tier 2 — First slot next business morning** (self-care instructions + confirmed AM booking):
     - Toothache ≤ 7/10 manageable with OTC analgesics, no fever, no swelling
     - Chipped tooth, no pulp exposure, no bleeding
     - Lost crown or filling, no active pain
     - Broken/loose ortho wire or bracket, no laceration
     - Partial denture or night guard break
   - **Tier 3 — Regular business hours** (self-care + standard scheduling):
     - Mild sensitivity to cold or sweets without spontaneous pain
     - Cosmetic concerns
     - Cleaning, check-up, or routine recall inquiries

3. **Produce decision-tree output** that includes, for each tier:
   - The exact screening questions to ask (in conversational, empathetic language)
   - The keyword triggers that an AI agent should match (severe, can't sleep, knocked out, swelling, bleeding, fever, broken jaw, pain out of 10, etc.)
   - The action to take (transfer to on-call, book AM, book routine)
   - The self-care guidance to read back to the patient (ibuprofen dosing caveats only if medical history is on file and clear of contraindications; cold compress instructions; avulsion storage; clove oil for cosmetic-wax ortho comfort)
   - The chart-documentation template (who called, when, chief complaint, pain scale, key findings, tier assigned, instructions given, outcome, on-call provider notified y/n, time)

4. **Produce three supplementary artifacts:**
   - **After-hours voicemail greeting** (30–45 seconds) — hours of operation, how to reach on-call, when to call 911, language for non-urgent callbacks
   - **On-call provider handoff note template** — one-line summary with pain scale, red-flag screen results, tier, patient callback number, time of initial call
   - **AI voice-agent prompt block** — a ready-to-paste system prompt for Arini/HeyGent/Dentina that encodes the same tiering, red-flag screen, keyword triggers, and escalation rules, plus a refusal-to-answer boundary for anything outside dental scope

5. **Close with an audit-safe documentation reminder** — every after-hours contact, even routine callbacks, creates a chart entry. Triage decisions are discoverable in malpractice actions; consistency is the defense.

**Output requirements:**
- One-page triage decision tree (printable for the front desk and the on-call provider)
- Tier-by-tier question script with clinical rationale in brackets
- Voicemail greeting script
- Handoff note template
- AI voice-agent system-prompt block
- Chart-documentation template
- Saved to `outputs/triage-protocols/` if the user confirms

## Guardrails

- **Never diagnose over the phone.** Triage assigns urgency; it does not confirm a pulpitis classification or the need for extraction vs. endo.
- **Never prescribe or recommend a prescription** through the triage protocol itself. Rx decisions are the on-call provider's after they have spoken with the patient.
- **Never ignore a red-flag symptom** because the patient downplays it. If the screening question is a yes, escalate to 911/ER regardless of other context.
- **Never advise discontinuing prescribed medications** (anticoagulants, antibiotics, steroids) during triage. Route that question to the prescribing provider.
- **Pediatric patients** with any facial trauma, any avulsion of a permanent tooth, or any uncontrolled bleeding should be routed more conservatively than adults — default to the higher tier.
- **HIPAA** — never leave a voicemail with clinical details; confirm identity before any clinical discussion.
- **State scope-of-practice varies** — teledentistry permissions, out-of-state-patient rules, and after-hours Rx authority differ; the triage protocol must defer to local regulation.
- AI-generated triage output is a **starting protocol**, not a replacement for a licensed provider's judgment, and must be reviewed and signed off by the dentist of record before being deployed.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
