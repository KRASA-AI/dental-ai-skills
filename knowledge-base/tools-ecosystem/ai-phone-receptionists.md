# AI Phone Receptionists & Front-Desk Agents — Dental Tools Ecosystem

A landscape note maintained by the landscape-monitor. These are third-party vendors that sit upstream of several skills in this repo (particularly `after-hours-emergency-triage`, `patient-reactivation-sequence`, `new-patient-welcome-kit`). Many practices configure these tools using prompts; several of the skills in this repo can be used to *generate* the system-prompt blocks these tools ingest.

## Category definition

An AI phone receptionist is a voice + SMS agent that answers inbound calls 24/7, books/reschedules/cancels directly against the PMS calendar, fills cancellation slots from a waitlist, handles new-patient intake (name, DOB, insurance, chief complaint), and routes dental emergencies per a configured triage decision tree.

## Active vendors (as of 2026-05-11)

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
- **Aron** — Originally launched February 20, 2026 as an AI-powered marketing/growth platform for dental practices. Late-April / early-May 2026 platform expansion adds AI Receptionist (24/7 inbound capture, after-hours coverage), Insurance Verification, and Patient Recall (re-engaging inactive patients with automated cadence) — repositioning Aron as a vertical-stack front-office suite rather than a marketing-only tool. Differentiator vs. pure receptionist vendors: bundles inbound-call handling with the marketing-and-acquisition layer that drives the inbound calls in the first place; the same patient record flows from first ad click through booking, intake, recall.
- **JAZA** — AI-powered front office operating system positioned as the standard-setting layer that "replaces manual, inconsistent work with automated structure" across multi-location practices. Conversation AI learns each practice and its individual patients (scheduling preferences, communication styles, appointment behaviors, motivators). After more than a year of live deployment across the United States and Puerto Rico, JAZA opened applications for an exclusive Partner Advisory Program (program start: June 1, 2026) offering selected operators 12 weeks of complete-platform access at no cost, white-glove onboarding, weekly strategic sessions with leadership, early access to new features, and direct input into product development. Differentiator: positions itself as the cross-location standardization layer, not an interchangeable receptionist; the Partner Advisory Program is a category-defining play to set front-office standards before category consolidation locks in.

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
- **Videa Voice Notes (launched October 2025; enterprise deployments expanding April 2026)** — First AI-powered ambient scribe purpose-built for dentistry. Smart Ambient Mode (multi-speaker) and Quick Dictation Mode (free-form speech), SOAP-structured output. Enterprise deployment at Emergency Dental of America (April 2026) demonstrates DSO-scale adoption. Vendor claims ~10 hrs/provider/week savings, up to ~$117K annual revenue per dentist, 95% first-pass completion. (See "VideaHealth → Videa rebrand" under Notable 2026 partnerships and enterprise deployments below.)
- **Overjet Voice (April 2026 global GA)** — Voice-powered clinical documentation; Overjet acquired DentalBee to accelerate this capability and brought it to general availability. Now paired in-product with Overjet's imaging AI.
- **Bola AI** — Voice Perio and Voice Restorative structured voice-command charting plus ambient AI Scribe. Vendor claims 10,000+ users and 3M+ charts completed. Verified Dentrix partnership, authorized Eaglesoft integration, bridge integration with Open Dental. Also announced Voice Perio integrations with Open Dental and Dentrix Enterprise.
- **Planet DDS Clinical Voice+ Suite (Denticon-native)** — Native voice-charting suite inside Denticon, the DSO-focused cloud PMS. **AI Voice Perio** launched February 2026 (positioned as "consistent, scalable perio charting" for DSOs), **AI Voice Restorative Charting** added April 2, 2026 (decay, existing restorations, fractures, and treatment-plan entries during the restorative exam). Both modes share the same voice-capture model and governance controls within Denticon, eliminating the dedicated chairside scribe role for single-clinician documentation. Roadmap: AI Voice Treatment Plan and AI Ambient Voice signaled for later in 2026 across the DentalOS platform. Differentiator vs. third-party voice-scribe vendors: native PMS write-back without a bridge or middleware layer.
- **Heidi (Heidi Health)** — Cross-vertical clinical AI scribe with a dental template library. April 2026: signed a two-year partnership with **PortmanDentex (UK + Ireland's second-largest dental group)** to deploy across the network, with approximately 60 clinicians going live each month. Specialty-specific notes and letters with clinician review-and-finalize workflow. Strong UK and Ireland presence; growing US dental footprint via DeepCura's "Best AI Scribe for Dentists" rankings.
- **Denti.AI Voice Perio** — Hands-free periodontal charting, speaker diarization in real time, writes to Dentrix, Eaglesoft, or Open Dental.
- **DentScribe** — Patent-pending AI Voice Perio Charting (PR Newswire April 2026).
- **Dentrix Ascend Voice** — Voice dictation integrated natively into Dentrix Ascend.
- **Alta AI** — Dental voice AI entrant.
- **SoapnotesAI** — General SOAP-note generator with dental-specific templates.
- **Archy Scribe (May 2026 launch)** — Native AI scribe operating directly inside the Archy all-in-one dental PMS (not a third-party add-on). Features at launch: AI-generated SOAP notes from recorded appointments, auto-population of custom clinical note templates, and hands-free voice perio charting. Key architectural differentiator: because Archy Scribe starts inside the PMS, it draws on the patient's existing chart, treatment history, planned procedures, and medical history without requiring the clinician to restate information the system already has — eliminating the manual-copy step required by standalone scribes. Part of the broader **Archy Intelligence** agent roadmap, which also includes Archy Revenue (insurance claims / billing automation), Archy Verify (insurance benefits verification), and Archy Connect (automated patient communications and scheduling). Founder background: Uber alumni; raised $20M Series B (Crunchbase). Beta clinicians report cutting note-writing time in half. Supported PMS: Archy only (not a third-party integration). HIPAA BAA: confirm directly with vendor.

These are in-operatory documentation tools and integrate with `clinical-note-assistant` rather than with the receptionist skills. They are upstream inputs to the PMS and downstream from the patient encounter. Evaluation criteria for the practice choosing among them: PMS write-back depth (one-click to chart vs. batch review), perio-specific vocabulary coverage (probe depths, bleeding, suppuration, mobility, furcation, recession all captured by voice), multi-speaker vs. single-speaker mode, provider template customization, BAA and PHI handling of the raw audio, and per-provider or per-location pricing.

## Notable 2026 partnerships and enterprise deployments

- **Imagen Dental Partners × Overjet (April 2026)** — Exclusive AI partnership across Imagen's 120+ locations in 17 states. Scale signal: DSOs are starting to standardize on a single imaging-AI vendor rather than piloting multiple.
- **Mortenson Dental Partners × Overjet IRIS (2026)** — 147 practices; ~1M patients annually.
- **Dental Care Alliance × Overjet (2026)** — Historic rollout of AI.
- **North American Dental Group × Overjet Voice (January 2026)** — 216 locations in 15 states.
- **TD Dental and Multi-Specialty Holdings × Overjet (2026)** — Complete AI suite across 34 locations.
- **Emergency Dental of America × Videa Voice Notes (April 2026)** — Network-wide voice-scribe deployment.
- **PortmanDentex × Heidi (April 2026)** — Two-year partnership; UK/Ireland's second-largest dental group; ~60 clinicians per month going live following a 2025 pilot. Largest dental ambient-scribe deployment in UK and Ireland to date.
- **Heartland Dental × DentalXChange (April 21, 2026)** — Eligibility AI plus PortalPass credential management across 1,900+ supported locations in 38 states and DC. Phased rollout starting April. Distinct adjacent category (eligibility AI / RCM, not ambient scribe), but a parallel DSO-scale deployment signal worth tracking.
- **VideaHealth → Videa rebrand and private-practice expansion (April 21, 2026)** — VideaHealth, the dental imaging AI vendor with deployments at 90,000 clinicians and 8 of the 10 largest DSOs, rebranded to "Videa" and announced expansion to independent and private-practice owners. Vendor-line capabilities now position as Clinical Assist (diagnostics, citing 20% case-acceptance lift), Auto-Documentation (cited 1–2 hrs/clinician/day), Insights (real-time performance), AutoVerify (insurance verification), and Clean Claims (cited 50% denial reduction). Practice-level implication: a previously enterprise-only AI suite is now a procurement option for single-location and small-group buyers, expanding the vendor shortlist for non-DSO practices evaluating an integrated AI stack.
- **Pearl Second Opinion 3D FDA clearance (April 22, 2026)** — Pearl became the first dental AI company with FDA clearance for both 2D and 3D image analysis. Adjacent imaging-AI signal — not an operational change for prompt-based skills, but expands the integrated-imaging-AI vendor shortlist relevant to BAA and 510(k) due diligence per `regulations/ada-ai-standards-2026.md`.
- **Dentsply Sirona Smart View – Detect FDA clearance and launch (May 4/12, 2026)** — World's first FDA-cleared AI-enabled diagnostic aid for detecting teeth with periapical radiolucencies (PARLs) in CBCT scans; also CE-marked in Europe. Available May 12, 2026 in the US and Europe on compatible Dentsply Sirona CBCT systems (Orthophos S, Orthophos SL, Axeos) with a DS Core Standard or Advanced subscription. Clinical study data: ~46% relative increase in tooth-level PARL detection sensitivity vs. unaided CBCT review, without meaningful increase in false positives. Patient-communication feature highlights areas of interest visually within the CBCT scan for chairside discussion. First FDA clearance for a CBCT-specific AI tool (prior clearances have been 2D-only). Relevant to `regulations/ada-ai-standards-2026.md` FDA 510(k) vendor-diligence section and Open Question #4 on ANSI/ADA 1110 CBCT coverage. Adjacent signal — no prompt-skill action, but marks the CBCT modality as now having a cleared AI diagnostic option for the first time.

These deployments matter for the repo because they signal the DSO category is moving past pilot into production AND that previously enterprise-only AI suites (Videa) are now opening to independent practices. Skills that depend on provider workflow (pre-visit intake summary, clinical-note assistant, morning huddle brief) should assume AI-scribe output is increasingly the default upstream input, not a future consideration. The eligibility-AI category (DentalXChange, HOOTL, Toothy AI, Curve Eligibility+, Ventus AI, DentalRobot, mConsent) is on track for a dedicated tools-ecosystem note in a future cycle as category consolidation settles.

## Adjacent category — patient intake automation

Patient intake automation (Weave, Solutionreach, NexHealth, Modento, Intake.Dental, RevenueWell, plus PMS-native form modules) often dispatches via the same SMS rails as the receptionist agent — the agent confirms the booking, the intake link arrives in the same thread minutes later. See `knowledge-base/tools-ecosystem/patient-intake-automation.md` for the vendor-landscape, design patterns, and implementation playbook for this adjacent category. The eligibility-AI handoff (insurance card photo capture in the intake form triggering an AutoVerify / Curve Eligibility+ / DentalXChange Eligibility AI / HOOTL workflow) is the clearest cross-category integration point.

---

*This file is maintained by the landscape-monitor scheduled task. Vendor names and capabilities change frequently; verify current state before recommending a specific vendor to a practice.*
