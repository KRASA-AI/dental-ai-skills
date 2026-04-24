# AI Phone Receptionists & Front-Desk Agents — Dental Tools Ecosystem

A landscape note maintained by the landscape-monitor. These are third-party vendors that sit upstream of several skills in this repo (particularly `after-hours-emergency-triage`, `patient-reactivation-sequence`, `new-patient-welcome-kit`). Many practices configure these tools using prompts; several of the skills in this repo can be used to *generate* the system-prompt blocks these tools ingest.

## Category definition

An AI phone receptionist is a voice + SMS agent that answers inbound calls 24/7, books/reschedules/cancels directly against the PMS calendar, fills cancellation slots from a waitlist, handles new-patient intake (name, DOB, insurance, chief complaint), and routes dental emergencies per a configured triage decision tree.

## Active vendors (as of 2026-04-24)

- **Arini** — Dental AI receptionist, voice + SMS, PMS integrations. Positioned as "Sophie from Arini Dental." Emphasizes 24/7 scheduling + emergency triage.
- **Viva AI** — Positions as a full AI operating system for dental practices (receptionist + outbound recall/reactivation campaigns), multilingual in 100+ languages with automatic detection.
- **HeyGent** — Claims training on 700,000+ hours of dental conversations; customizable voice personality; no-code prompt builder.
- **Patientdesk** — AI booking system for dental practices, focus on booking conversion.
- **Dentina** — AI dental receptionist.
- **Revenue Ring AI** — AI receptionist marketed on no-hold-time and booking capture.
- **Savvy Agents** — No-code dental AI phone agent builder.
- **Autocalls** — AI voice agents marketed as dental-specific.
- **Zaha** — Reported in coverage with 95% call answer rate and 40% booking lift.
- **Newo** — AI front-desk automation.
- **Rondah AI** — AI receptionist for dental practices.
- **JustCall** — General-purpose AI voice agent with dental vertical packaging.
- **AgentZap / aireceptionistdental.com / Aira** — Additional entrants surfaced in 2026 category roundups.

## How this repo interacts with these tools

1. `after-hours-emergency-triage` produces a ready-to-paste system-prompt block for any of the above vendors — tiered triage, keyword triggers, red-flag escalation, chart-note template.
2. `new-patient-welcome-kit` produces the welcome-email + pre-visit checklist that the AI agent can SMS after a successful booking.
3. `patient-reactivation-sequence` produces the outbound-campaign script that the AI agent can run on lapsed-patient lists.
4. `review-responder` (shared) can be used after the AI agent closes a positive interaction and the patient leaves a Google review.

## Evaluation criteria when selecting a vendor

- PMS integration depth (read/write calendar vs. read-only; supported PMS list)
- HIPAA BAA and data residency
- Voice latency and interruption handling
- Emergency-triage configurability and escalation SLA
- SMS + voice coverage vs. voice only
- Transfer to live staff vs. voicemail handoff
- Transcription + chart-note output for the PMS
- Pricing model (per-minute, per-booking, per-seat)

## Ambient clinical-voice (adjacent category)

Separate from phone receptionists but often confused with them. The ambient-scribe category has consolidated rapidly through 2026-Q1–Q2 and now has several production-scale entrants:

- **Pearl Voice (April 2026 launch)** — Ambient voice AI suite for the operatory. Multi-speaker ambient transcription, voice-enabled perio charting in real time, procedure-specific templates. Early adopters reporting ~60 min/day/provider time savings.
- **Videa Voice Notes (launched October 2025; enterprise deployments expanding April 2026)** — First AI-powered ambient scribe purpose-built for dentistry per VideaHealth. Smart Ambient Mode (multi-speaker) and Quick Dictation Mode (free-form speech), SOAP-structured output. Enterprise deployment at Emergency Dental of America (April 2026) demonstrates DSO-scale adoption. Vendor claims ~10 hrs/provider/week savings, up to ~$117K annual revenue per dentist, 95% first-pass completion.
- **Overjet Voice (April 2026 global GA)** — Voice-powered clinical documentation; Overjet acquired DentalBee to accelerate this capability and brought it to general availability. Now paired in-product with Overjet's imaging AI.
- **Bola AI** — Voice Perio and Voice Restorative structured voice-command charting plus ambient AI Scribe. Vendor claims 10,000+ users and 3M+ charts completed. Verified Dentrix partnership, authorized Eaglesoft integration, bridge integration with Open Dental. Also announced Voice Perio integrations with Open Dental and Dentrix Enterprise.
- **Denti.AI Voice Perio** — Hands-free periodontal charting, speaker diarization in real time, writes to Dentrix, Eaglesoft, or Open Dental.
- **DentScribe** — Patent-pending AI Voice Perio Charting (PR Newswire April 2026).
- **Dentrix Ascend Voice** — Voice dictation integrated natively into Dentrix Ascend.
- **Alta AI** — Dental voice AI entrant.
- **SoapnotesAI** — General SOAP-note generator with dental-specific templates.

These are in-operatory documentation tools and integrate with `clinical-note-assistant` rather than with the receptionist skills. They are upstream inputs to the PMS and downstream from the patient encounter. Evaluation criteria for the practice choosing among them: PMS write-back depth (one-click to chart vs. batch review), perio-specific vocabulary coverage (probe depths, bleeding, suppuration, mobility, furcation, recession all captured by voice), multi-speaker vs. single-speaker mode, provider template customization, BAA and PHI handling of the raw audio, and per-provider or per-location pricing.

## Notable 2026 partnerships and enterprise deployments

- **Imagen Dental Partners × Overjet (April 2026)** — Exclusive AI partnership across Imagen's 120+ locations in 17 states. Scale signal: DSOs are starting to standardize on a single imaging-AI vendor rather than piloting multiple.
- **Mortenson Dental Partners × Overjet IRIS (2026)** — 147 practices; ~1M patients annually.
- **Dental Care Alliance × Overjet (2026)** — Historic rollout of AI.
- **Emergency Dental of America × VideaHealth Voice Notes (April 2026)** — Network-wide voice-scribe deployment.

These deployments matter for the repo because they signal the DSO category is moving past pilot into production. Skills that depend on provider workflow (pre-visit intake summary, clinical-note assistant, morning huddle brief) should assume AI-scribe output is increasingly the default upstream input, not a future consideration.

---

*This file is maintained by the landscape-monitor scheduled task. Vendor names and capabilities change frequently; verify current state before recommending a specific vendor to a practice.*
