# AI Phone Receptionists & Front-Desk Agents — Dental Tools Ecosystem

A landscape note maintained by the landscape-monitor. These are third-party vendors that sit upstream of several skills in this repo (particularly `after-hours-emergency-triage`, `patient-reactivation-sequence`, `new-patient-welcome-kit`). Many practices configure these tools using prompts; several of the skills in this repo can be used to *generate* the system-prompt blocks these tools ingest.

## Category definition

An AI phone receptionist is a voice + SMS agent that answers inbound calls 24/7, books/reschedules/cancels directly against the PMS calendar, fills cancellation slots from a waitlist, handles new-patient intake (name, DOB, insurance, chief complaint), and routes dental emergencies per a configured triage decision tree.

## Active vendors (as of 2026-04-14)

- **Arini** — Dental AI receptionist, voice + SMS, PMS integrations. Positioned as "Sophie from Arini Dental." Emphasizes 24/7 scheduling + emergency triage.
- **HeyGent** — Claims training on 700,000+ hours of dental conversations; customizable voice personality; no-code prompt builder.
- **Patientdesk** — AI booking system for dental practices, focus on booking conversion.
- **Dentina** — AI dental receptionist.
- **Revenue Ring AI** — AI receptionist marketed on no-hold-time and booking capture.
- **Savvy Agents** — No-code dental AI phone agent builder.
- **Autocalls** — AI voice agents marketed as dental-specific.
- **Zaha** — Reported in coverage with 95% call answer rate and 40% booking lift.
- **Newo** — AI front-desk automation.

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

Separate from phone receptionists but often confused with them:

- **Pearl Voice (April 2026 launch)** — Ambient clinical voice AI suite for the operatory, not the front desk.
- **Denti.AI Voice Perio** — Hands-free periodontal charting.

These are in-operatory documentation tools and integrate with `clinical-note-assistant` rather than with the receptionist skills.

---

*This file is maintained by the landscape-monitor scheduled task. Vendor names and capabilities change frequently; verify current state before recommending a specific vendor to a practice.*
