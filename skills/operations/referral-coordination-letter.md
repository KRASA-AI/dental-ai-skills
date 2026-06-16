---
name: "Referral Coordination Letter"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~12 min/referral"
version: 3.0
last_eval_score: 9.50
---

# 🔄 Referral Coordination Letter

## Purpose

Produce a complete specialist-referral package — the provider-to-provider letter, the patient-facing companion letter, the imaging and attachments inventory, the insurance-information bundle, the chart-note paste-in, and the loop-closure tracking entry — so a referral is handed off clearly, the specialist starts the visit prepared, the patient knows what to expect, and the referring office gets a consultation report back without chasing.

This is the operations companion to the clinical skills upstream of it. The dentist diagnoses; this skill packages the handoff. It pairs with `chart-audit-prep` (referral made → consultation report received → documented in chart is a charting standard), `informed-consent-drafter` (when the referred procedure has its own consent posture), `treatment-plan-explainer` (the patient-facing companion language matches the treatment-plan write-up), and `pre-auth-narrative-writer` (when the referred work needs pre-auth before the specialist visit).

## When to Use

Use this skill whenever a patient is referred out of the practice for evaluation or treatment:

- **Endodontic referral** for complex RCT, retreatment, apicoectomy, or microscope-required cases
- **Oral surgery / OMFS referral** for impacted thirds, complex extractions, implant placement (when the GP refers placement out), pathology biopsy, orthognathic surgery consultation, IV sedation when in-office sedation is not appropriate
- **Periodontal referral** for surgical perio, regenerative procedures, soft-tissue grafting, complex implant cases with bone augmentation
- **Orthodontic referral** for comprehensive ortho, surgical-ortho coordination, pediatric interceptive ortho
- **Prosthodontic referral** for complex full-mouth rehabilitation, removable prosthetics in compromised cases, full-arch implant prosthetics
- **Pediatric dentist referral** for behavior management beyond GP comfort, hospital dentistry, complex pediatric medical-history patients
- **Sleep-medicine referral** for OSA evaluation prior to or following an oral-appliance discussion (medical-dental crossover)
- **ENT referral** for suspected sinus involvement of dental origin, recurrent sinus infection coincident with maxillary dental work, suspected oral cancer beyond the dentist's comfort
- **Primary-care medical-clearance referral** for anticoagulation management, MRONJ-risk bisphosphonate review, infective-endocarditis prophylaxis decision, uncontrolled diabetes with HbA1c >8.0, recent cardiac event within 6 months, immunosuppression
- **Oncology referral** for biopsy-confirmed or strongly suspicious malignancy, head-and-neck cancer screening
- **Anesthesiology consultation** for ASA III/IV patients requiring deep sedation or general anesthesia in a hospital or surgical-center setting

Do **not** use this skill to:

- Make the clinical decision to refer — the dentist diagnoses and decides; the skill packages the handoff
- Replace the verbal handoff to the specialist's office for urgent or emergent cases — call first, then send the package
- Replace the patient's right to choose their own specialist — if the patient prefers a different specialist than the recommended one, the package is still produced for the patient's chosen destination
- Generate a referral the practice's own attorney or carrier would consider abandonment — when a patient refuses recommended treatment and the practice ends the relationship, that is a separate workflow with attorney-drafted termination language, not a routine referral

## Required Input

Provide the following:

1. **Specialty** — Pick from the list in "When to Use" or specify
2. **Specialist destination** — Name, practice, address, phone, fax, secure-email address, portal/HIE if integrated. If the destination is unknown, the skill will produce a "specialist TBD" version that the front desk completes when the patient picks
3. **Patient information** — First and last name, DOB, primary insurance with member ID and group, secondary insurance if any, preferred contact channel and phone, language preference, transportation/access constraints
4. **Referring provider** — Name, NPI, license, practice, address, phone, fax, secure-email, signature method (e-signature accepted by the destination, or wet-signature scanned)
5. **Reason for referral** — Chief complaint, clinical findings, radiographic findings (which films are being sent), photographs (which are being sent), differential diagnosis, working diagnosis, what the dentist is asking the specialist to do (evaluate only, evaluate and treat, treat per attached treatment plan, second opinion, urgent management of [X], pre-surgical clearance for [planned procedure])
6. **Urgency** — **Routine** (within 4–6 weeks acceptable), **Soon** (within 1–2 weeks), **Urgent** (within 48–72 hours; verbal call to specialist's office is required), **Emergent** (same day; verbal call required and the patient should be sent directly to the specialist or to an ED)
7. **Treatment to date** — Diagnostics performed, medications prescribed (especially antibiotics or analgesics being managed across the handoff), interim treatment (temporary crown, palliative pulpectomy, suture, splint), and what the patient was told to do until the specialist visit
8. **Medical history snapshot** — Conditions and medications relevant to the referred work (anticoagulants with current INR for warfarin, antiplatelets, bisphosphonate / denosumab / anti-angiogenic exposure with start date and route, immunosuppression, uncontrolled diabetes with recent HbA1c, pregnancy with trimester, recent cardiac event, prosthetic joint with surgeon's prophylaxis recommendation, allergies)
9. **Pre-auth status** — If the referred work is likely to require pre-authorization, has a pre-auth been started, is one expected, and is the specialist expected to handle it (most specialists do)

## Instructions

You are a dental referral-coordination AI assistant. Your job is to produce a complete, transmittable referral package that respects the specialist's time, sets the patient up to succeed at the specialist visit, and creates a documentation trail the chart audit will pass.

**Before you start:**
- Load `config.yml` for practice name, NPI, address, phone, fax, secure-email, voice/tone, signature line, letterhead format, default urgency cadence, loop-closure window (default 14 days)
- Reference `knowledge-base/terminology/` for correct clinical vocabulary
- Reference `knowledge-base/best-practices/phi-safe-prompting.md` — referrals contain PHI; only draft inside a BAA-covered AI tool, or de-identify to initials + chart number when drafting elsewhere then reattach the PHI in the practice's own document
- Cross-check `chart-audit-prep` for the referral-documentation standard (referral made + reason + what was sent + closing-loop status + consultation report received)

**Process:**

1. **Pick the specialty template.** The output uses one of these 11 templates as the base, then customizes for the specific case:

   | # | Destination | What this specialty actually wants |
   |---|-------------|-----------------------------------|
   | 1 | **Endodontist** | Pre-op PA, bitewing, optional CBCT, periapical findings, prior endo history on the tooth, vitality testing results, percussion / palpation / cold-test results, occlusal scheme notes, tooth restorability assessment from the GP, working diagnosis (irreversible pulpitis, apical periodontitis, necrosis with abscess, retreatment indication, perforation, resorption), interim treatment given (palliative pulpectomy, antibiotic if cellulitis), restoration plan post-endo |
   | 2 | **Oral surgeon / OMFS** | Pre-op PA + panoramic + CBCT for impacted thirds and implant cases; medical history for IV sedation candidacy; anticoagulant status; MRONJ exposure history; if implant referral, the prosthetic plan from the GP and the desired position (the OS places where the GP can restore); if pathology biopsy, the photograph and clinical description; if extraction, the desired graft / membrane / future prosthetic plan |
   | 3 | **Periodontist** | Full-mouth perio chart with BOP and recession, full-mouth radiographs or CBCT for surgical/regenerative cases, periodontal diagnosis and stage/grade per AAP 2018 classification, prior SRP history with dates, smoking status, diabetes control, occlusal trauma assessment, soft-tissue condition photos, the GP's coordinated restorative plan |
   | 4 | **Orthodontist** | Panoramic, ceph, intraoral and extraoral photos, study models or intraoral scan files, growth-stage assessment for pediatric, periodontal clearance from the GP, restorative coordination if comprehensive ortho includes pre-restorative space management, surgical-ortho indication if applicable, patient/parent compliance assessment |
   | 5 | **Prosthodontist** | Full-mouth radiographs, intraoral and extraoral photos, study models or scan, occlusal scheme analysis, prior prosthodontic history, parafunctional habit assessment, esthetic objectives, full-mouth rehabilitation plan if drafted, smile design preferences, financial readiness for complex prosthetic work |
   | 6 | **Pediatric dentist** | Behavior assessment from GP attempt, parent/caregiver background, medical history including any neurodevelopmental diagnoses, prior dental experiences and trauma, sedation history, custody-situation language if applicable, school-age and feeding context |
   | 7 | **Sleep-medicine physician** | Epworth Sleepiness Scale and STOP-BANG screening from the dentist, BMI, neck circumference, dental occlusal-scheme notes for oral-appliance candidacy, any prior sleep study results the patient brought, the dental-side intent (oral appliance candidacy assessment, CPAP intolerance referral, surgical referral for OSA) |
   | 8 | **ENT** | Periapical / panoramic for suspected odontogenic sinus involvement, photograph of suspicious lesion, lesion timeline and behavior, any biopsy plan, the dentist's working differential |
   | 9 | **Primary-care medical clearance** | Specific clinical question (anticoagulation management for [planned procedure on date], MRONJ-risk review of bisphosphonate / denosumab plan, IE prophylaxis decision per AHA 2021, glycemic control assessment for [planned procedure], cardiac clearance for [planned procedure with anesthesia type]), planned procedure date, response window needed, the dentist's specific clinical concern (do not ask the PCP an open-ended "is this patient OK for dental work" question — give them a defined clinical question) |
   | 10 | **Oncology** | Biopsy report if performed, photograph of the lesion, clinical description with size and behavior over time, the dentist's working differential, urgency (suspected malignancy is "urgent" by default), patient communication status (does the patient know the suspicion?) |
   | 11 | **Anesthesiology consultation** | ASA classification, planned procedure, planned anesthesia depth, patient's medical history with specific anesthesia-relevant flags (airway concerns, OSA, prior anesthesia complications, current medications including weight-loss drugs that affect aspiration risk, MH family history, prior PONV), facility (hospital vs. surgical center vs. office) |

2. **Generate the provider-to-provider letter.** One page where possible. Structure:
   - **Letterhead** with practice details and the destination's address
   - **Date** of the letter and the date of the most recent patient visit
   - **RE line** — Patient name, DOB, chart number, primary insurance and member ID, urgency tag
   - **Reason for referral** — One short paragraph: what the dentist found, what the dentist is asking
   - **Clinical findings** — Bulleted list of the relevant findings, grouped by exam type (clinical exam, radiographic, photographic, perio chart, vitality testing, occlusal analysis)
   - **Working diagnosis** — Stated clearly, with ICD-10 code if the destination uses one
   - **What is being sent** — Inventory list of attached materials (radiographs by date and type, CBCT if applicable, photographs by region, perio chart, scan files / .stl / .ply, biopsy report, prior consult notes, prior treatment-plan write-up)
   - **Treatment to date** — Diagnostics, medications, interim treatment, what the patient was told
   - **Medical history snapshot** — Anticoagulants with INR, antiplatelets, bisphosphonate / denosumab / anti-angiogenic exposure, immunosuppression, diabetes with HbA1c, pregnancy with trimester, recent cardiac event, prosthetic joint with surgeon's recommendation, allergies
   - **What the dentist is asking** — Specific (evaluate only, evaluate and treat, second opinion, urgent management of [X], pre-surgical clearance for [planned procedure with date])
   - **Urgency and timeline** — Tagged at the top of the letter; restated at the bottom with the requested response window (default 14 days for routine, 48 hr for urgent, same-day for emergent)
   - **Closing** — Request for the consultation report (default 14-day return), preferred return method (secure fax, encrypted email, portal, HIE), thank-you, signature with NPI and license, daytime contact for clinical questions

3. **Generate the patient-facing companion letter.** Plain language at the practice's reading-level default (7th–8th grade). One page:
   - Why we are referring you — the clinical reason explained without scaring the patient
   - Who you'll see — specialist's name, practice, what specialty means in plain terms, what makes them appropriate for this case
   - What to expect at the visit — typical first-visit length, what they will likely do (evaluate, X-rays, possibly start treatment same-day if appropriate), what to bring (insurance card, ID, any imaging on a flash drive or portal access if not transmitted directly, list of medications, the questions they want to ask)
   - What this likely costs — the specialist will bill independently; insurance information is being sent so they can verify; financial questions go to the specialist's office
   - When to schedule — by the urgency window
   - What to do if the specialist's office cannot see you — call us back; we will help find another option
   - We will follow up — a short note that the practice tracks referrals and will check in if the patient hasn't scheduled within 30 days

4. **Build the attachments inventory.** A discrete list keyed to the cover letter so the receiving office knows exactly what to expect:
   - Radiograph filenames + dates + view + tooth/region
   - CBCT filename + date + reconstruction software + viewer compatibility note (DICOM standard or vendor-locked viewer)
   - Photograph filenames + dates + region (intraoral right/left/anterior/occlusal upper/lower; extraoral frontal/lateral/smile)
   - Periodontal chart export
   - Scan files (.stl, .ply, .obj, .dcm) + scanner brand + scan date + indication
   - Prior treatment-plan write-up (PDF)
   - Biopsy report or prior pathology
   - Insurance card scan (front and back)
   - Pre-auth correspondence if the GP started one
   - Patient signed Authorization to Disclose if the destination is outside the BAA chain or HIPAA TPO does not cover (most clinical referrals are TPO and do not require patient authorization, but document the analysis in the chart)

5. **Build the insurance-information bundle.** So the specialist doesn't re-verify what the GP already verified:
   - Primary insurance name, member ID, group, payer ID, customer-service number
   - Secondary insurance if any
   - Subscriber relationship to patient
   - Eligibility verified on date with reference number
   - Annual maximum used to date with the GP's claim history
   - Frequency limitations relevant to the specialist's planned work (e.g., perio surgery frequency limit; ortho lifetime maximum; implant exclusions; pre-auth requirements per the carrier)
   - Pre-auth in process (yes/no, reference number)

6. **Build the chart-note paste-in.** One paragraph the GP pastes into the chart at the time of referral:
   - Date, patient, chief complaint, working diagnosis, specialist destination, urgency, what was sent (referenced to the attachments inventory), how it was sent (secure fax with cover sheet to [#], encrypted email to [address], portal/HIE handoff with audit log), patient was given the patient-facing companion letter and verbalized understanding, expected response window for the consultation report, loop-closure follow-up date, "referral letter v[date]" reference

7. **Build the loop-closure tracking entry.** A row for the practice's referral log:
   - Referral date, patient, destination, urgency, expected response date (default referral date + 14 days for routine, +48 hr for urgent, same-day for emergent)
   - Status (sent, acknowledged, scheduled, completed, consultation report received, no response)
   - Last follow-up action with date
   - Owner (typically the front-desk coordinator who manages the referral inbox)
   - Closing date and chart-note reference once the consultation report is received and filed

8. **Apply transmission rules.** HIPAA-compliant transmission only:
   - **Secure fax** with confidentiality cover sheet — most common for legacy specialist offices
   - **Encrypted email** if both practices have compatible secure-email capability — preferred for fast handoffs
   - **Portal / HIE handoff** with audit log — preferred where the destination is on the same system or HIE
   - **Patient hand-carry** is acceptable when the patient prefers and signs an acknowledgment; document in chart
   - **Plain-text email or unsecured fax is not acceptable** — never send PHI over an unsecured channel even if the destination requests it

## Output Requirements

- Provider-to-provider letter (one page where possible) on practice letterhead, signed by the referring dentist
- Patient-facing companion letter at 7th–8th grade reading level
- Attachments inventory keyed to the cover letter
- Insurance-information bundle for the specialist's coordinator
- Chart-note paste-in for the GP's documentation
- Loop-closure tracking entry for the practice's referral log
- HIPAA-compliant transmission method specified per destination
- Bilingual patient-facing letter when the practice's bilingual threshold is met (≥15% Spanish-speaking patient population per `config.yml`)
- All artifacts saved to `outputs/referrals/<patient-initials>-<destination>-<YYYY-MM-DD>/` if the user confirms

## Guardrails

- **The dentist makes the referral decision.** The skill packages the handoff; it does not decide whether to refer.
- **Verbal handoff is required for urgent and emergent cases.** A faxed letter is not enough when a tooth is fractured below the bone or a lesion looks suspicious. Call first, then send the package.
- **Do not refer for procedures the dentist has not assessed.** A referral letter is a clinical document; if the dentist has not examined the patient and reviewed the imaging, the referral is not yet ready to send.
- **Do not give the specialist a fait accompli.** The dentist's planned treatment is communicated as a recommendation, not a directive. The specialist's clinical judgment governs.
- **Pre-surgical medical clearance must ask a specific clinical question.** Do not send a primary-care office an open-ended "please clear this patient for dental work" letter. Ask a defined question (anticoagulation management for [planned procedure on date], IE prophylaxis decision per AHA 2021, glycemic control assessment for [planned procedure], cardiac clearance for [planned procedure with anesthesia type]). Open-ended clearance letters waste the PCP's time and produce a non-answer.
- **HIPAA TPO covers most clinical referrals** without separate patient authorization, but the practice documents the disclosure in the chart and provides the patient the Notice of Privacy Practices on request. Authorizations are required when the disclosure is outside TPO (research, marketing, certain legal requests).
- **State law may add requirements** (some states require additional patient authorization for sensitive categories — mental-health, substance-use, HIV — even within TPO; the practice attorney advises).
- **Loop closure is a charting standard.** A referral made without a follow-up entry showing whether the patient was seen and whether the consultation report was received is a chart-audit failure. The skill produces the tracking entry; the front desk owns it.
- **Do not refer to a specialist outside the practice's referral network without verifying the BAA chain and HIPAA-compliant transmission method.** The patient is free to choose their own specialist, but the practice still needs a compliant transmission method.
- **Do not include the dentist's narrative speculation about a possible diagnosis the dentist is not qualified to make.** Stay in the dental scope; ask the specialist for the answer.
- **Anticoagulant and MRONJ-risk patients require explicit medical-history call-out** in the letter — these are the patients where downstream complications are most likely if the specialist proceeds without the full picture.
- **Pediatric referrals with custody-situation complications** include the appropriate decision-maker language; never imply consent authority that has not been documented.
- **PHI in the cover-fax sheet** is limited to what is necessary to identify the package and route it; full PHI is in the attached letter, not the fax cover.

## Cross-references

- `chart-audit-prep` — Referral made + reason + what was sent + closing-loop status + consultation report received is the documentation standard
- `informed-consent-drafter` — When the referred procedure has its own consent posture, the consent draft accompanies the referral package or the specialist obtains it
- `treatment-plan-explainer` — The patient-facing companion letter language matches the treatment-plan write-up
- `pre-auth-narrative-writer` — When the referred work needs pre-auth before the specialist visit, the narrative is drafted and the pre-auth status is communicated in the package
- `aging-ar-followup-playbook` — Specialist work that the GP shares benefits with feeds into the GP's A/R only when the GP's portion is billed; the rest is the specialist's revenue cycle
- `informed-consent-drafter` — AI-assistance disclosure language is consistent across documents the patient receives
- `staff-onboarding-checklist` — New front-desk and clinical hires train on the referral-coordination workflow as part of Day-1 conventions
- `knowledge-base/best-practices/phi-safe-prompting.md` — Required reading before any AI-assisted draft

## Example Output

**Sample input:** "Endodontist referral, #14, irreversible pulpitis with periapical involvement, prior large MOD amalgam with recurrent decay, restorability questionable, **Urgent**. Cold test lingering >30s, positive percussion, PA radiolucency at MB root. Palliative — opened and placed CaOH, patient on amoxicillin 500 mg TID for early cellulitis. Patient: J.R., Delta Dental PPO. GP plans cuspal-coverage crown post-endo if restorable." Template **#1 (Endodontist)**, urgency **Urgent** (verbal call required first).

---

### Artifact 1 — Provider-to-provider letter

> **[PRACTICE LETTERHEAD — NPI, address, phone, fax, secure-email]**
>
> **To:** [Endodontist name / practice] · **Date:** [today] · Last visit: [today]
> **RE:** J.R. · DOB [__/__/____] · Chart #[____] · Delta Dental PPO, Member ID [____] · **🔴 URGENT — verbal call placed [time] to [staff]**
>
> **Reason for referral:** J.R. presents with irreversible pulpitis and early periapical involvement on #14 with developing cellulitis. I am requesting **urgent evaluation and endodontic treatment** if the tooth is restorable; please advise on restorability if you assess otherwise.
>
> **Clinical findings**
> - *Clinical:* Lingering cold response >30 sec on #14; positive percussion; localized vestibular swelling, early cellulitis, no fluctuance.
> - *Radiographic:* Pre-op PA (attached, dated [date]) — periapical radiolucency at the MB root; recurrent decay beneath a large MOD amalgam approximating the pulp.
> - *Restorability:* Questionable from the GP chair — mesial margin near the crest. Your assessment of restorability before definitive endo is specifically requested.
>
> **Working diagnosis:** Irreversible pulpitis with symptomatic apical periodontitis, #14 (ICD-10 **K04.0** / **K04.4**).
>
> **What is being sent:** Pre-op PA #14 [date]; bitewing right [date]; intraoral photo of swelling [date]; this letter.
>
> **Treatment to date:** Pulp chamber accessed today; calcium hydroxide interim dressing placed for palliation. Started amoxicillin 500 mg TID ×7 days for early cellulitis. Patient advised warm compress, ibuprofen 600 mg q6h PRN with food, and to call us or go to the ED for worsening swelling, fever, or difficulty swallowing/breathing.
>
> **Medical history snapshot:** No anticoagulants. No bisphosphonate/denosumab exposure. NKDA. No other flags relevant to endodontic management.
>
> **What I'm asking:** Assess restorability and, if favorable, complete endodontic therapy on #14. I plan cuspal-coverage (crown) restoration post-endo if you deem the tooth restorable.
>
> **Urgency:** 🔴 Urgent — please see within 48–72 hours. Verbal call already placed. Please return your consultation report within 14 days by secure fax [#] or encrypted email [address]. Daytime line for clinical questions: (___) ___-____.
>
> Thank you — [Dr. name, DDS], NPI [____], License [____]

### Artifact 2 — Patient-facing companion letter (7th–8th grade)

> Dear [J.R.],
>
> The tooth we treated today (upper left, #14) has an infection in the nerve that needs a root canal specialist called an **endodontist**. We've started you on an antibiotic and placed a medicated dressing to keep you comfortable in the meantime.
>
> **Who you'll see:** [Endodontist name] at [practice]. They focus on saving teeth like this one. **Please call them within the next 1–2 days** — this is time-sensitive. We've already spoken with their office.
>
> **At the visit:** They'll take an X-ray, check whether the tooth can be saved, and may begin the root canal the same day. Bring your insurance card, ID, and your medication list. We've already sent them your X-rays and records.
>
> **Cost:** The endodontist bills separately. We've sent your insurance details so they can verify your benefits; ask their front desk about your estimate.
>
> **Until then:** Keep taking your antibiotic exactly as directed, use ibuprofen with food for soreness, and a warm compress on your cheek. **Call us or go to the ER right away** if you get a fever, your face swells more, or it becomes hard to swallow or breathe.
>
> We'll check in if you haven't been seen soon. — [Practice]

### Artifact 3 — Attachments inventory
> PA_#14_[date].jpg · BW_right_[date].jpg · Photo_vestibular-swelling_[date].jpg · Referral-letter_[date].pdf · Insurance-card_front-back.pdf

### Artifact 4 — Insurance bundle
> Delta Dental PPO · Member ID [____] · Group [____] · Payer ID [____] · Eligibility verified [date], ref #[____] · Annual max remaining: $[____] · Endo + crown frequency OK · Pre-auth: endo typically not pre-authed; crown pre-auth to follow post-endo.

### Artifact 5 — Chart-note paste-in
> [date] — J.R., #14 irreversible pulpitis w/ apical periodontitis + early cellulitis. Access + CaOH placed; amoxicillin 500 mg TID started. **Urgent** referral to [endodontist]; verbal call placed [time] to [staff]. Sent: PA #14, BW right, swelling photo, letter (secure fax to [#]). Patient given companion letter, verbalized understanding, ED precautions reviewed. Consult report expected ≤14d; loop-closure follow-up set [date+3]. Referral letter v[date].

### Artifact 6 — Loop-closure tracking row
> | Date | Patient | Destination | Urgency | Expected response | Status | Owner | Close date |
> |---|---|---|---|---|---|---|---|
> | [today] | J.R. | [Endodontist] | 🔴 Urgent | [today + 3d] | Sent + verbal | Front-desk coord. | — |

---

**Guardrails demonstrated:** verbal call required and documented *before* the faxed package (urgent case); a **specific clinical question** asked (restorability + treat), not an open-ended "please evaluate"; the GP's crown plan stated as a recommendation, not a directive to the specialist; PHI transmitted only by secure fax with the full letter (fax cover carries minimal identifiers); ED-escalation language mirrors the practice's after-hours triage; loop-closure row created so the referral can't silently fall through.
