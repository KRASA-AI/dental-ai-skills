# Patient Intake Automation & Digital Forms — Dental Tools Ecosystem

A landscape note maintained by the landscape-monitor. Patient intake automation sits **upstream** of `pre-visit-intake-summary` (the AI summarization skill that consumes completed intake content) and `new-patient-welcome-kit` (the welcome-packet skill that wraps around the intake experience). It is **adjacent** to `ai-phone-receptionists.md` — the receptionist agent often dispatches the intake link via SMS at booking confirmation.

This note exists because the April 2026 cycle (with renewed coverage of the Weave-vs-Solutionreach digital-forms decision and the emergence of dental-specific intake-automation vendors like Intake.Dental) raised the topic from incidental to category-worthy.

## Category definition

A patient intake automation system is the digital pipeline that converts a paper or PDF medical-history packet into a structured, mobile-first form that the patient completes before arrival, with conditional logic, electronic signature, insurance-card photo capture, automated reminders, and direct write-back to the practice management system (PMS). The strongest implementations also trigger downstream actions: insurance eligibility verification, financial-counseling outreach for high-balance procedures, pre-visit medical-alert flags into the morning huddle, and language-routing for non-English-dominant patients.

Three operational sub-categories — practices typically choose one or combine two:

1. **All-in-one patient communication suites with embedded forms** — Forms are one module of a broader product (texting, recall, reviews, payments). Examples: Weave, Solutionreach, RevenueWell, NexHealth, Modento, Doctible, Lighthouse 360.
2. **Dental-specific intake-first products** — Built primarily around the intake/check-in experience with deeper PMS write-back and conditional logic. Examples: Intake.Dental, QuantumByte, Yapi, Adit (intake module).
3. **PMS-native intake modules** — Forms shipped inside the PMS itself (lower switching cost, narrower feature set). Examples: Dentrix Patient Engage forms, Open Dental built-in eForms, Curve forms, Eaglesoft forms.

## Active vendors (as of 2026-04-28)

Communication-suite vendors with a digital-forms module:

- **Weave** — Forms bundled with phone, texting, and payments. Pre-population from PMS history, predictive text, intelligent routing for complex cases. Pricing typically bundled around the broader platform tier; standalone forms not separately offered.
- **Solutionreach** — Mobile-first responsive forms, integrated co-pay/balance payments, response-driven follow-up sequences, segmentation by patient type, completion-rate and drop-off analytics. SR Intake specifically marketed for dental and offered through Patterson Dental.
- **RevenueWell** — Digital forms layered on a recall/marketing suite.
- **NexHealth** — Online booking and digital forms with API-first PMS integration.
- **Modento** — Communication suite with paperless office and intake forms; widely used in independent practices.
- **Doctible** — Patient experience platform with digital intake.
- **Lighthouse 360 (Yapi acquisition merger)** — Patient communication with intake.
- **Dental Intelligence (Engagement Suite)** — Communication-and-scheduling suite that bundles Digital Forms with Online Scheduling, Appointment Reminders, TouchPoints, Team Tasks, Team Chat, Online Reviews, Kiosks, and Payments. Practice-analytics roots (the original Dental Intelligence product line) mean the engagement layer ships alongside production-and-retention dashboards rather than as a standalone forms tool. **Supported PMS list expanded April 27, 2026 to add Denticon (Planet DDS)** alongside its existing PMS coverage — closing a gap that previously left DSO buyers on Denticon without a first-party Dental Intelligence engagement integration. The form module participates in the same channel-delivery, conditional-logic, e-signature, and PMS write-back patterns documented below; the differentiator is that completed-form data feeds directly into the same analytics surface a practice already uses for production-per-provider, hygiene-reappointment, and case-acceptance reporting.

Dental-specific intake-first vendors:

- **Intake.Dental** — Promoted as built by a practicing dentist; multilingual forms, AI clinical notes, automated morning huddle reports, treatment-plan management. AES-256-GCM cited for cloud storage.
- **QuantumByte** — Intake software positioned around 2026 best-of lists.
- **Yapi** — Long-standing dental front-office automation including paperless intake (now under the Lighthouse umbrella for some product lines).
- **Adit (intake module)** — Part of an all-in-one AI-powered platform; pairs intake with AI Front Desk and Call Intelligence.

PMS-native form modules (lower switching cost):

- **Dentrix Patient Engage forms**, **Open Dental eForms**, **Curve forms**, **Eaglesoft forms** — typically narrower conditional-logic depth and weaker analytics, but the cleanest write-back path because there is no integration layer.

## How this repo interacts with these tools

1. `pre-visit-intake-summary` consumes the completed intake — whichever vendor or PMS-native module the practice chose — and converts it into the one-page clinical summary the team reads before seating. Vendor-agnostic by design; the input is the form content.
2. `new-patient-welcome-kit` produces the welcome-packet content the intake link is delivered alongside (and the post-completion confirmation). The phrasing assumes the digital intake will arrive before the visit; if a practice still uses paper intake, the welcome packet shifts the "complete this online" instruction to "please arrive 15 minutes early to complete forms in office."
3. `morning-huddle-brief` consumes the medical-alert flags surfaced by the intake summary — high-acuity items (anticoagulants, MRONJ risk, sedation candidacy, pregnancy, premedication, controlled-substance history) move from the intake form to the huddle agenda automatically when the pipeline is configured.
4. `staff-onboarding-checklist` includes a Day-1 module on the practice's chosen intake vendor for front-desk and treatment-coordinator hires, because the intake pipeline is the first PMS surface a new front-desk staffer will touch.
5. `insurance-verification-summary` and `aging-ar-followup-playbook` benefit when the intake form captures insurance card photos and triggers an automated eligibility-verification workflow at booking; the verification summary is then ready before the visit rather than scrambled together at check-in.
6. `after-hours-emergency-triage` references the intake module when the AI receptionist triages an emergency caller and dispatches a same-day intake link by SMS.

## Design patterns that show up across vendors

When evaluating a vendor or designing a practice's intake pipeline, the same pattern surface area shows up regardless of brand. The repo's prompt-skills assume these are configurable upstream rather than baked into the LLM workflow:

- **Channel-based delivery** — SMS link, email, QR code at the front-desk kiosk. SMS conversion typically beats email for new-patient intake; QR is the fallback for walk-ins and emergencies.
- **Conditional logic / branching** — Anticoagulant question only fires if the patient indicates a heart or stroke history; pregnancy questions branch on biological-sex response and age range; recreational-substance follow-ups branch on a positive screen.
- **Pre-population from PMS history** — For recall patients, last-visit medical history is pulled forward and the patient confirms or edits, rather than re-typing.
- **Insurance card photo capture** — Front and back images attached to the patient record; ideally triggers an automated eligibility-verification workflow against the carrier portal or AI verification vendor.
- **Electronic signature** — HIPAA notice of privacy practices, financial-policy acknowledgment, photo/video release (state-by-state), telehealth consent, AI-tool disclosure (now a 2026 ADA consent-checklist item).
- **Multilingual variants** — Practices with ≥15% Spanish-dominant or other-language patient share should have parallel forms; never machine-translate without native-speaker review.
- **Automated reminder cadence** — Confirm appointment + intake-link reminder at booking, then 48-hour, 24-hour, and morning-of nudges if the form is incomplete.
- **PMS write-back validation** — On submit, structured data writes to PMS fields directly (medications, allergies, conditions, insurance), free-text notes attach to the chart, signatures store as time-stamped PDFs in the document tree.
- **Downtime fallback** — When the cloud forms vendor is offline (rare but it happens), the practice needs a printed paper packet ready for the day, plus a recovery protocol that re-enters the data when the vendor recovers.
- **Completion-rate and drop-off analytics** — Which question is the highest abandonment point; which channel converts best; which patient segment never completes (and triggers a phone call from the front desk).

## Evaluation criteria when selecting a vendor

- **PMS write-back depth** — Structured-field write vs. PDF attachment only; supported PMS list (Dentrix, Eaglesoft, Open Dental, Curve, Carestack, Denticon, Dentrix Ascend); read/write vs. read-only.
- **HIPAA BAA and data residency** — BAA must be in place before any PHI is captured; cloud storage encryption (AES-256 at rest is now the practical floor); 2026 HIPAA Security Rule update (see `regulations/hipaa-security-rule-2026.md`) is removing the "addressable" safety valve on encryption, which makes this a procurement requirement, not a nice-to-have.
- **Conditional-logic editor** — No-code editor depth; reusable question-block library; preview-before-publish.
- **Multilingual support** — Native-speaker-translated dental terminology vs. machine translation; supported language list; per-question language toggle vs. whole-form duplication.
- **Insurance card capture and downstream eligibility trigger** — Pure photo capture vs. integrated handoff to AutoVerify, Curve Eligibility+, DentalXChange Eligibility AI, Pearl Precheck, Overjet, Zuub, mConsent, HOOTL.
- **Electronic signature handling** — Time-stamped, audit-trail-backed, retained for the state's mandated retention period (typically 7–10 years post-last-visit; longer for minors).
- **Mobile experience** — Responsive design; submission completes on a 4-inch screen; image upload from camera roll without leaving the form.
- **Analytics and reporting** — Completion-rate dashboard, drop-off heatmap, per-question response distribution, segmentation by appointment type, exportable to BI.
- **Pricing model** — Per-location flat, per-form, per-active-patient, or bundled into a broader communication suite. Standalone forms in 2026 typically run $200/mo (Solutionreach SR Intake), bundled packages run $399/mo+ (Weave); dental-specific intake-first vendors typically price between those bookends.
- **Integration with downstream AI scribe / morning huddle / pre-visit summary** — Does the intake pipeline export the structured data the `pre-visit-intake-summary` skill expects, or does it dump a PDF that requires re-extraction.
- **Accessibility** — WCAG 2.1 AA compliance (or stronger); ADA-compliant in the practice-website sense (alt text, keyboard navigation, screen-reader labels, non-color-dependent error states).

## Implementation playbook (the operational pattern across vendor brands)

This is the pattern that shows up consistently in 2026 implementation guidance regardless of which vendor is chosen — it is the same playbook the `staff-onboarding-checklist` and `morning-huddle-brief` skills assume the practice has worked through.

1. **Workflow audit and baseline metrics** — Measure today's check-in time per patient, staff data-entry minutes per intake, paper-cost run rate, completion-before-arrival rate (typically near 0% for paper, target 70–85% for digital), patient-satisfaction baseline.
2. **Tech-readiness check** — Practice WiFi at minimum 25 Mbps for kiosk + concurrent submissions; iPad or tablet inventory if walk-in capture is needed; PMS API availability and integration vendor compatibility.
3. **Vendor selection** — Apply the criteria list above; require a live PMS write-back demo against the practice's actual PMS, not a generic environment.
4. **Form design** — Branch logic, multilingual variants at the ≥15% threshold, AI-tool disclosure language (per 2026 ADA guidance), photo capture, signature points; test with three internal staff acting as patients on three appointment types (new adult, new pediatric, recall).
5. **Phased rollout with parallel paper system** — Two weeks of side-by-side digital + paper, then four weeks of digital-default with paper as fallback only. Designate 1–2 staff as "digital intake champions" who own the rollout.
6. **Downtime fallback** — Printed packet for every appointment type, downtime workflow document, recovery protocol when the vendor restores.
7. **Post-launch optimization** — At 30 days, review completion-rate dashboard, drop-off heatmap, and the specific questions patients are skipping or quitting at. Iterate the form. At 90 days, measure check-in time, data-entry minutes, satisfaction, and the AR/eligibility-verification cycle-time delta against the baseline.
8. **Annual review** — Re-evaluate vendor against the criteria list (BAA still current, encryption posture, accessibility, PMS write-back depth, multilingual coverage). Re-confirm AI-tool disclosure language is current with the most recent ADA consent-checklist guidance.

## Compliance notes

- **HIPAA encryption posture** — As of the proposed 2026 HIPAA Security Rule update (see `regulations/hipaa-security-rule-2026.md`), encryption of ePHI at rest and in transit is moving from "addressable" to required. Vendor BAA must specify encryption posture explicitly.
- **AI-tool disclosure** — When an intake vendor uses AI for any patient-facing decision (auto-population, recommendation engine, language translation, eligibility prediction), 2026 ADA guidance recommends disclosure on the consent block. The `staff-onboarding-checklist` Day-1 HIPAA module covers this.
- **State retention rules** — Signed forms retained per state dental practice act (typically 7–10 years post-last-visit; longer for minors, often to age of majority + statute of limitations).
- **TCPA / SMS consent** — Intake-link SMS delivery requires prior express consent to text; the appointment-booking confirmation typically establishes that consent.
- **Photo / video release** — State-by-state; do not assume a generic photo release covers social-media use; the `social-media-content-calendar` skill cites the same release framework.

---

*This file is maintained by the landscape-monitor scheduled task. Vendor names, pricing, and capabilities change frequently; verify current state before recommending a specific vendor to a practice. First created 2026-04-26 in response to the April 26 ai.dentist Weave-vs-Solutionreach implementation guide and the renewed 2026 vendor-landscape coverage of dental-specific intake-first products (Intake.Dental, QuantumByte) that prompted moving this category from incidental to ecosystem-worthy. Vendor list updated 2026-04-28 to add Dental Intelligence (Engagement Suite) following the April 27, 2026 announcement that the Engagement Suite is now fully integrated with Denticon (Planet DDS).*
