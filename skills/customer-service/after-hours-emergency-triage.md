---
name: "After-Hours Emergency Triage Protocol"
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~12 min/call + improved liability posture"
version: 2.0
last_eval_score: null
---

# After-Hours Emergency Triage Protocol

## Purpose

Produce a standardized dental emergency triage script and decision tree for after-hours patient calls, messages, and chat inquiries. The output guides on-call providers, answering-service operators, and **AI phone receptionists** (Viva, Arini, DentalAI Assist, HeyGent, Dentina, Savvy, Patientdesk, Autocalls) through consistent, documented triage: true emergencies (ER-bound or same-night DDS contact), urgent (next-morning first-available slot), and routine (regular business hours). Reduces liability exposure from inconsistent triage, prevents missed airway/sepsis red flags, improves same-day booking capture for genuine dental emergencies, and produces vendor-specific paste-in configuration blocks so the practice can deploy the protocol on its AI phone tool without reformatting.

## When to Use

Use this skill when:
- Setting up or refreshing the practice's after-hours answering protocol (human answering service, on-call provider, or AI voice agent)
- Configuring an AI phone agent's emergency decision tree (Viva, Arini, DentalAI Assist, HeyGent, Dentina, Savvy, Autocalls)
- Onboarding a new on-call dentist to the practice's triage standards
- Writing the practice's after-hours patient-facing voicemail greeting and call-back policy
- Responding to a specific after-hours call and the user wants a documented triage pathway for the chart
- Adding an SMS / chat triage flow alongside the phone flow

Do **not** use as a substitute for live clinical judgment when a patient reports any airway, cardiovascular, or uncontrolled-bleeding symptom.

## Required Input

Provide the following — but the skill produces a complete first-pass protocol with **as little as fields 1 and 2**. Every other field is filled with `[DEFAULT — VERIFY]` heuristics from Section A so the office has a working protocol before completing the full input.

1. **Use case** — Setup protocol (general), AI agent configuration, on-call provider onboarding, single-call triage, or SMS/chat flow
2. **Practice profile** — Practice type (solo GP, group, OS/endo specialty, pedo, ortho, perio); on-call DDS with prescribing authority Y/N; ER referral partner; weekend/after-hours availability
3. **AI phone tool in use** (optional) — Viva, Arini, DentalAI Assist, HeyGent, Dentina, Savvy, Patientdesk, Autocalls, or human-only answering service. The skill produces the vendor-specific paste-in configuration block matching the named tool
4. **Patient-facing channels** — Phone only, phone + SMS, phone + AI chat, AI voice agent + human fallback
5. **State scope-of-practice notes** (optional) — Any Rx restrictions, out-of-state patient rules, teledentistry allowances; defaults to a conservative national baseline if omitted
6. **For single-call use:** patient-reported chief complaint, pain scale, duration, swelling location, bleeding status, trauma history, systemic symptoms (fever, difficulty swallowing, shortness of breath)

## Instructions

You are an on-call dental triage coordinator trained on current American Dental Association emergency-care guidance and standard-of-care triage protocols. Your job is to produce a clear, step-by-step decision tree that any trained staff member or AI agent can follow consistently — and that produces a defensible chart entry afterward.

**Before you start:**
- Load `config.yml` for on-call provider name and contact, answering-service details, ER referral hospital, preferred pharmacies, AI phone tool selection, and practice taxonomy
- Reference `knowledge-base/regulations/` for state-specific after-hours prescribing and teledentistry rules
- Reference `knowledge-base/terminology/` for correct clinical vocabulary (pulpitis classifications, Ellis fracture classes, space-infection terms, Centor-like criteria adapted for dental sepsis)
- Reference `knowledge-base/tools-ecosystem/ai-phone-receptionists.md` for tool-specific configuration paths

**Process:**

1. **Open with the four red-flag screening questions** (ask these first, before any other triage content):
   - "Are you having any difficulty breathing, swallowing, or opening your mouth?"
   - "Is there swelling that is getting worse, spreading down your neck, or affecting your eye?"
   - "Did you have a facial trauma, hit to the head, or loss of consciousness?"
   - "Are you bleeding and unable to stop it with 15 minutes of firm pressure?"
   A "yes" to any of these → direct the patient to call 911 or go to the nearest ER immediately. Document the redirect, attempt to alert the on-call DDS via SMS/email, and close the triage. Each AI tool's configuration block in Section B is built so that any of these four triggers immediately routes the call out of the triage flow and to 911 / ER and an alert to the on-call provider.

2. **Tier the non-red-flag complaints** into three buckets:
   - **Tier 1 — Same-night on-call contact** (call/text the on-call DDS now):
     - Avulsed permanent tooth (reimplantation window is time-sensitive — coach storage in milk, saliva, or HBSS; route to immediate evaluation)
     - Post-surgical complications within 72 hours (dry socket with pain ≥ 7/10, bleed-through, rising fever, suture dehiscence)
     - Localized swelling of dental origin without airway involvement but with fever ≥ 100.4°F
     - Severe uncontrolled pain ≥ 8/10 despite OTC analgesics, preventing sleep
     - Traumatic displacement/luxation of permanent teeth
   - **Tier 2 — First slot next business morning** (self-care + confirmed AM booking):
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
   - The chart-documentation template (who called, when, chief complaint, pain scale, key findings, tier assigned, instructions given, outcome, on-call provider notified Y/N, time)

4. **Produce four supplementary artifacts:**
   - **After-hours voicemail greeting** (30–45 seconds) — hours of operation, how to reach on-call, when to call 911, language for non-urgent callbacks
   - **On-call provider handoff note template** — one-line summary with pain scale, red-flag screen results, tier, patient callback number, time of initial call
   - **AI voice-agent system-prompt block** (vendor-specific per Section B) — a ready-to-paste system prompt for the named AI phone tool that encodes the tiering, red-flag screen, keyword triggers, escalation rules, and refusal-to-answer boundary
   - **SMS / chat triage flow** — a parallel turn-by-turn script for text-based intake with the same red-flag screen and tiering

5. **Close with an audit-safe documentation reminder** — every after-hours contact, even routine callbacks, creates a chart entry. Triage decisions are discoverable in malpractice actions; consistency is the defense.

**Output requirements:**
- One-page triage decision tree (printable for the front desk and the on-call provider)
- Tier-by-tier question script with clinical rationale in brackets
- Voicemail greeting script
- Handoff note template
- AI voice-agent system-prompt block (vendor-specific paste-in for the named tool, per Section B)
- SMS / chat triage flow (parallel to phone flow)
- Chart-documentation template
- Saved to `outputs/triage-protocols/` if the user confirms

## Section A — Default Heuristics (Fast-Path)

When the input is minimal (only fields 1 and 2 supplied), the skill applies these defaults so a full protocol is produced on the first run. Every applied default is labeled `[DEFAULT — VERIFY]` in a Section 0 Defaults Summary at the top of the output. Config values from `config.yml` silently replace the corresponding default without the label.

- **Practice profile default:** Solo GP, 1 doc + 1 RDH, M–F 8–5, no weekend hours, on-call rotation = single dentist
- **On-call DDS prescribing authority:** Yes (national baseline; flag `[VERIFY STATE RULES]`)
- **ER referral partner:** Nearest hospital ER with on-call OMFS or general surgery coverage (flag `[VERIFY LOCAL HOSPITAL]`)
- **AI phone tool default:** Human answering service with on-call DDS callback (no AI tool selected)
- **Patient-facing channels default:** Phone only with voicemail-to-on-call routing
- **State scope-of-practice default:** Conservative national baseline — teledentistry permitted for triage and self-care guidance only; Rx authority limited to acute-pain management within the on-call provider's licensure
- **Pediatric escalation default:** Any facial trauma in a child under 16, any avulsion of a permanent tooth at any age, and any uncontrolled bleeding route one tier higher than the equivalent adult presentation
- **Anticoagulant escalation default:** Any patient on warfarin, DOACs (apixaban, rivaroxaban, dabigatran, edoxaban), or dual-antiplatelet therapy with bleeding routes one tier higher
- **Pregnancy escalation default:** Any pregnant patient with infection signs routes to Tier 1 same-night on-call contact regardless of pain score (sepsis risk and acetaminophen-only analgesic constraint)

## Section B — AI Phone Tool Configuration Blocks

For the named AI phone tool, the skill produces a vendor-specific paste-in configuration block matching the tool's native prompt format, escalation hooks, knowledge-base sync model, and call-recording / HIPAA posture. The block is ready to paste into the tool's admin console without reformatting.

- **Viva** — Dental-specific AI receptionist. Configuration paths: Agent → Knowledge → After-Hours; Agent → Escalation Rules → Emergency Triggers; Agent → Voice → Empathetic-Calm. Native fields: `escalation_phone_on_red_flag` (set to on-call DDS), `keyword_trigger_list` (paste the 12-keyword red-flag list from the protocol), `transfer_on_trigger` (set to immediate cold transfer with SMS summary to on-call DDS). HIPAA: Viva is BAA-covered; call recordings retained per Viva's BAA terms. Knowledge sync: Viva pulls from a Notion or Google Drive folder — paste the protocol decision tree into the linked folder, name the file `after-hours-triage.md`.
- **Arini** — Dental AI receptionist. Configuration paths: Settings → AI Agent → Scripts → Emergency Triage; Settings → AI Agent → Escalation → On-Call Handoff. Native fields: `red_flag_questions` (paste the four screening questions verbatim), `tier_routing_table` (paste the three-tier action table), `escalation_method` (set to "call + SMS to on-call number"). HIPAA: Arini is BAA-covered. Knowledge sync: Arini ingests scripts directly into the agent config — no external folder required. Call recording: configurable per state two-party-consent rules.
- **DentalAI Assist** — Dental AI receptionist. Configuration paths: Workspace → Voice Agent → Protocols → After-Hours; Workspace → Voice Agent → Escalation Webhooks. Native fields: `protocol_steps` (paste the decision-tree steps as numbered items), `red_flag_webhook` (set to the practice's on-call paging webhook), `keyword_classifier_seed` (paste the keyword-trigger list to train the agent's intent classifier). HIPAA: DentalAI Assist is BAA-covered. Knowledge sync: protocol pasted directly into Workspace → Protocols.
- **HeyGent (formerly Voice AI)** — General AI receptionist with dental templates. Configuration paths: Agent Settings → Prompts → System Prompt; Agent Settings → Transfer Rules. Native fields: a single system-prompt block (paste the entire AI voice-agent system-prompt artifact from the protocol output). HIPAA: HeyGent BAA available on enterprise tier — confirm tier before deploying with PHI.
- **Dentina** — Dental-focused AI front desk. Configuration paths: Agent → Skills → Emergency; Agent → Routes → After-Hours. Native fields: `skill_trigger` (paste the keyword-trigger list), `route_action` (set to "transfer + SMS"). HIPAA: Dentina is BAA-covered.
- **Savvy** — General AI receptionist. Configuration paths: Workflows → Emergency After-Hours; Workflows → Escalation. Native fields: full system prompt + a separate "actions" config for transfer/SMS. HIPAA: Savvy BAA available — confirm.
- **Patientdesk** — Patient-engagement platform with AI phone module. Configuration paths: Communication → Voice AI → Scripts; Communication → Voice AI → Escalation. Native fields: script block + escalation phone. HIPAA: Patientdesk is BAA-covered.
- **Autocalls** — General AI calling platform. Configuration paths: Campaigns → Inbound → After-Hours; Campaigns → Inbound → Transfer Rules. Native fields: system prompt + transfer rules. HIPAA: confirm BAA tier before deploying with PHI.
- **Human answering service (no AI tool)** — The output includes a printable triage script for the answering-service operator with the same four red-flag questions, the three-tier action table, and the handoff note template. The skill flags any answering-service vendor that is not BAA-covered.

The vendor-specific block ends with three configuration-verification items the office must confirm before going live: (1) the red-flag transfer rule actually fires end-to-end (test by speaking each of the four red-flag triggers); (2) the on-call DDS receives the alert SMS within 60 seconds of the transfer; (3) the call recording / transcript is preserved per the practice's HIPAA retention policy.

## Section C — Chart Documentation Template

Every after-hours contact produces a chart entry within 24 hours, ideally before the patient's next visit. The skill produces a documentation template with the following fields, in this order:

- Date and time of contact (inbound channel: phone / SMS / chat / AI agent)
- Patient identity verification method (DOB confirmation, account number, security question)
- Chief complaint in the patient's own words (quoted)
- Pain scale (0–10), duration, character (sharp / dull / throbbing / electric / cold-sensitive / spontaneous)
- Red-flag screening results (each of the four questions, Y/N)
- Swelling status (location, size, progression)
- Bleeding status (controlled / uncontrolled, duration, pressure-applied)
- Trauma history (mechanism, time, loss of consciousness, head/neck involvement)
- Systemic symptoms (fever Y/N with temp, dysphagia, dyspnea, malaise)
- Medical history flags (anticoagulants, diabetes, immunocompromise, pregnancy, recent surgery)
- Tier assigned (1 / 2 / 3) with rationale
- Self-care guidance given (verbatim or summarized with check-back time)
- On-call provider notified (Y/N, time, method, outcome of notification)
- Booking action taken (appointment time, location, provider)
- Operator / AI-agent name (who handled the call)
- Sign-off by on-call DDS within 24 hours

## Guardrails

- **Never diagnose over the phone.** Triage assigns urgency; it does not confirm a pulpitis classification or the need for extraction vs. endo.
- **Never prescribe or recommend a prescription** through the triage protocol itself. Rx decisions are the on-call provider's after they have spoken with the patient.
- **Never ignore a red-flag symptom** because the patient downplays it. If the screening question is a yes, escalate to 911/ER regardless of other context.
- **Never advise discontinuing prescribed medications** (anticoagulants, antibiotics, steroids) during triage. Route that question to the prescribing provider.
- **Pediatric patients** with any facial trauma, any avulsion of a permanent tooth, or any uncontrolled bleeding should be routed more conservatively than adults — default to the higher tier.
- **Anticoagulated patients** with any bleeding presentation route one tier higher.
- **Pregnant patients** with any infection sign route to Tier 1.
- **HIPAA** — never leave a voicemail with clinical details; confirm identity before any clinical discussion. AI phone tools must be BAA-covered before deployment with PHI.
- **State scope-of-practice varies** — teledentistry permissions, out-of-state-patient rules, and after-hours Rx authority differ; the triage protocol must defer to local regulation.
- **AI-generated triage output is a starting protocol**, not a replacement for a licensed provider's judgment, and must be reviewed and signed off by the dentist of record before being deployed.
- **Call recording / SMS retention** must comply with state two-party-consent rules and the practice's HIPAA retention policy — flag if the named AI tool's default recording posture does not match the practice's state rules.

## Cross-Reference Graph

This skill explicitly chains with:

- **Upstream:** `config.yml` (on-call DDS, ER referral hospital, AI phone tool selection, preferred pharmacies); `knowledge-base/regulations/` (state-specific Rx and teledentistry rules); `knowledge-base/tools-ecosystem/ai-phone-receptionists.md` (tool-specific configuration paths)
- **Sibling:** `scheduling-optimizer` (Tier 2 next-AM bookings route through the scheduling-optimizer's emergency-slot reservation logic); `morning-huddle-brief` (overnight after-hours contacts are reviewed in the huddle as a standing agenda item); `clinical-note-assistant` (chart documentation template is the input format for the post-visit clinical note)
- **Downstream:** `patient-reactivation-sequence` (Tier 3 routine callbacks that fall off the calendar route to reactivation); `monthly-practice-kpi-report` (after-hours-contact volume, red-flag-rate, and tier-1 escalation-rate feed the KPI dashboard); `meeting-summarizer` (end-of-week after-hours debrief uses the chart entries as input)

## Common Pitfalls To Avoid

- Do not deploy an AI phone tool without confirming end-to-end red-flag escalation works (test all four triggers before going live)
- Do not deploy an AI phone tool without a BAA in place — PHI in a non-BAA tool is a breach
- Do not let the AI tool answer outside dental scope — the refusal-to-answer boundary must be explicit
- Do not assume the on-call DDS will see an SMS notification within 60 seconds — test the alert pathway and have a fallback
- Do not skip the **chart entry** on routine after-hours callbacks — the entry is the malpractice defense
- Do not record or transcribe a call without verifying the state's two-party-consent rule
- Do not include clinical details in a voicemail or SMS to a number you have not confirmed belongs to the patient
- Do not default to the same triage tier across pediatric, adult, anticoagulated, and pregnant patients — the escalation rules differ
- Do not let the AI tool make a definitive diagnosis or recommend a specific Rx — both are out of scope and create liability
- Do not omit the post-visit sign-off by the on-call DDS within 24 hours — unsigned after-hours contacts are weak evidence in malpractice review

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
