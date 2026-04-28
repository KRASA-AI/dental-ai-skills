---
name: "Morning Huddle Brief"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~20 min/day"
version: 3.0
last_eval_score: null
---

# ☀️ Morning Huddle Brief

## Purpose

Generate a focused, standard-format morning huddle brief that the doctor, hygienist, front desk, assistants, TC, and (when present) office manager can all follow in 10 minutes — covering the day's production goal, patient-by-patient schedule review, same-day treatment opportunities, medical alerts, sedation-day rules, lab cases, new patients, unscheduled treatment in today's patients, balance-collect-at-checkin flags, Q4 benefits-remaining patients, and yesterday's carry-overs. The goal of the huddle is not just awareness — it is to leave the meeting with concrete decisions on who fills open chair time, which patients get offered same-day treatment, which patients need extra preparation, and which scheduled patients need a verification phone call before they walk in.

The v3.0 brief adapts to **practice specialty mix** (general / perio / endo / OMFS / pediatric / ortho / prostho / sleep / DSO multi-doctor) and overlays a **risk-tier flag layer** (sedation NPO, MRONJ, anticoagulant, A1c, pregnancy, anxiety, pediatric, in-office emergency-history) that drives the day's prep tasks rather than just sitting as awareness.

## When to Use

Use this skill every practice morning (or at the end of the prior day for the next morning). Works for solo GP, multi-doctor group practice, specialty-only practice, and DSO offices. Also useful for virtual huddles when providers are traveling, and for training new office managers, TCs, or surgical coordinators on huddle structure.

**Skip the huddle when:** a solo provider half-day with ≤4 patients and no surgical/sedation/new-patient cases, or any day where the entire schedule is hygiene-only with no doctor on site (a 2-minute hygiene huddle replaces the full brief). Document the skip — auditors and coaches notice the cadence break.

## Required Input

Provide the following:

1. **Today's schedule** — Appointment list with patient names, appointment types (NP exam, hygiene, crown seat, surgical extraction, implant placement, RCT, etc.), provider, operatory, start and end times. PMS-export equivalents: Dentrix Day Sheet / Eaglesoft Daily Schedule / Open Dental Appointments View / Curve Daily / Denticon Schedule View / Carestack Schedule / Dentrix Ascend Day View.
2. **Daily production goal** — Doctor and hygiene dollar targets for the day (from `config.yml → production_goals.daily_doctor` and `daily_hygiene` if not specified).
3. **Yesterday's outcomes** (optional) — Production achieved, any cancellations or no-shows, treatment presented but not accepted, any escalations from after-hours triage.
4. **Known medical alerts** — Patients on anticoagulants (warfarin INR check / DOAC last-dose timing / antiplatelet), patients requiring AHA-guideline antibiotic premedication (prosthetic joint per ADA / AAOS, valve, prior IE), significant allergies, recent hospitalizations, pregnancy by trimester, current/prior IV bisphosphonate / denosumab / anti-angiogenic (MRONJ risk), uncontrolled diabetes (HbA1c >8.0), recent cardiac event within 30 days.
5. **Sedation cases** — Any in-office IV / oral / nitrous-only / pediatric sedation: confirm escort, confirm fasting (NPO ≥6 hr solids / ≥2 hr clears), confirm post-op driver, confirm pre-procedure consent on file (`informed-consent-drafter`).
6. **Lab cases** — Cases arriving today (need to be seated), cases leaving today (need to ship), overdue cases.
7. **Open chair time / holes** — Unfilled blocks that need backfill.
8. **Unscheduled treatment** (optional) — If available, list of today's patients who have diagnosed-but-unscheduled treatment.
9. **Balance-collect flags** (optional) — From `aging-ar-followup-playbook` PT-BAL queue: which patients with appointments today have a PT-BAL action coded for collect-at-checkin.
10. **Q4 / benefits-remaining patients** (optional, October-December) — From `insurance-verification-summary` Q4 push: today's patients with significant unused annual maximum.
11. **Bilingual flag** — Whether the practice serves a ≥15% Spanish-speaking patient population (from `config.yml → demographics.spanish_speaking_pct`); flips the brief to bilingual notation for patient-facing language samples and inserts `[ES]` flag against patients whose preferred language is Spanish.

## Instructions

You are a skilled dental practice management AI assistant. Your job is to produce a tight, scannable morning huddle brief that drives action — not a narrative summary. Every section should end with a specific decision or assignment, with an owner and a timestamp.

**Before you start:**
- Load `config.yml` from the repo root for practice name, specialty mix, provider roster (NPI, license #, DEA where relevant), daily doctor and hygiene production goals, standard huddle format preferences, demographic skew (Spanish-speaking %, pediatric %, geriatric %), sedation modalities offered, and the active vendor stack (PMS, AI imaging, AI voice, intake automation).
- Reference `knowledge-base/best-practices/` for huddle frameworks if present.
- Reference `knowledge-base/terminology/` for correct procedure and code descriptors.
- Cross-reference `knowledge-base/regulations/` for state-specific patient-identifier rules if the brief is posted in a clinical area.

**Process:**

1. **Detect specialty mix** from `config.yml → specialty_mix` and pick the matching profile (the brief structure is the same; the section weights and the typical risk overlays differ):
   - **General practice (GP)** — Default. Mix of hygiene, restorative, endo, single-tooth surgical. Same-day-treatment section is the highest-leverage section.
   - **Periodontal practice** — Lead with SRP / quadrant-perio / surgical / implant cases. Risk overlay leans MRONJ + anticoagulant + diabetes A1c. AAP 2018 stage/grade tracked in patient-by-patient row.
   - **Endodontic practice** — Lead with RCT and retreatment cases by tooth. Same-day-treatment is rare; lab-case section is usually empty. Sedation cases are oral/nitrous, rarely IV.
   - **Oral surgery / OMFS** — Lead with surgical extractions, third molars, implant placement, biopsy. Sedation rules dominate the brief — escort confirmation, NPO compliance, recovery-bay assignment, anesthesiology coverage if applicable. Pre-op imaging confirmation (CBCT / pano).
   - **Pediatric practice** — Behavior-management notes per child, parent-present-required appointments, nitrous candidacy, sedation by weight, custody-situation notes. AAPD age-1 first-visit flag for new patients ≤12 months.
   - **Orthodontic practice** — Bonding, IPR, Invisalign attachments, retainer checks. Lab section becomes "appliance / aligner case" section. Post-op section becomes "post-bonding / post-IPR comfort message" section.
   - **Prosthodontic practice** — Full-arch, full-mouth rehab, complex prosthetic planning. Lab cases are the dominant section; case-by-case verify-before-seat.
   - **Sleep medicine / DSM** — Mandibular advancement device (MAD) consults, titration visits, follow-up Epworth scoring, CPAP-intolerant referrals. Q4 medical-billing crossover flag (medical insurance, not dental).
   - **DSO / multi-doctor** — Split the patient-by-patient view into a column per provider; add an inter-op coverage matrix; flag any provider running CE / PTO / off-site.

2. **Parse the schedule** and group patients by provider, operatory, and visit type. For multi-doctor offices, produce the brief as a master 1-page view plus a per-provider column detail (artifact `doctor-column-detail.md`).

3. **Ask clarifying questions only** if critical data is missing (no production goal, no schedule data, ambiguous provider names, no medical-alert source).

4. **Generate the brief** using the following standard sections, in order:

   **1. Today at a Glance** (top of page, 4-6 lines)
   - Date, day of week, weather if relevant to attendance (e.g., snowstorm warning)
   - Doctor production goal vs. scheduled ($ and %), per provider in multi-doctor offices
   - Hygiene production goal vs. scheduled ($ and %)
   - Total patients: doctor column, hygiene column, surgical / sedation column if applicable
   - Open chair time: list of blocks (operatory, time, length)
   - Q4-benefits-remaining patient count if October-December (links to `insurance-verification-summary` Q4 push)

   **2. Yesterday's Scoreboard** (optional, 3-4 lines)
   - Production achieved vs. goal (doctor and hygiene)
   - Notable misses (no-shows, last-minute cancellations, broken appointments by provider)
   - Treatment presented / accepted / pending — feeds `monthly-practice-kpi-report` weekly
   - One-line callout of any after-hours-triage call from last night that needs morning follow-up

   **3. Patient-by-Patient Review** (bulk of the brief, ordered by chair time)

   For each patient, a one-line entry plus the relevant flags:
   - `[Time] • [Provider/Op] • [First Name + Last Initial] • [Appt Type] • [Production $] • [Risk Flags] • [Action]`

   **Risk-flag overlay (apply only those that apply):**
   - 🚩 **Med alert** — anticoagulant (specify warfarin/DOAC/antiplatelet), premed required (specify regimen), allergy (latex / NSAID / penicillin / amide-anesthetic / sulfite), pregnancy (specify trimester), recent cardiac event (<30 days post-MI/CVA/stent), diabetes A1c >8.0, current/prior IV bisphosphonate or denosumab (MRONJ — confirm dosing window), seizure disorder, asthma rescue inhaler chairside
   - 💤 **Sedation** — confirm fasting status (NPO ≥6 hr solids / ≥2 hr clears), confirm escort (name + phone), confirm consent on file (`informed-consent-drafter`), confirm pulse-ox / capnography / O₂ / reversal agents available, recovery bay assignment
   - 🔄 **Unscheduled Tx** — top 1-2 diagnosed-but-unscheduled items so the provider can offer same-day (e.g., "#30 cracked, RCT/crown unscheduled — offer if time"); link to `case-presentation-script` for objection-handling
   - ⚠️ **Balance** — outstanding amount + PT-BAL action code from `aging-ar-followup-playbook` (e.g., "$214.30 PT-BAL — collect at checkin")
   - 💰 **Q4 benefits** (Oct-Dec only) — unused annual max + days until plan resets (e.g., "$840 unused, expires 12/31")
   - 📷 **Imaging due** — BWs / FMX / pano / CBCT due today
   - 💍 **New patient** — flag and note referral source, who handles the practice tour and financial presentation
   - 👶 **Pediatric** — parent must be present, nitrous candidacy, behavior-management note from prior visit
   - 😰 **Anxiety** — known dental-anxiety patient; assign comfort-room / extra time / HALO-style review-request opt-out
   - 🧒 **Custody** — pediatric custody-situation note (which parent has medical decision authority)
   - 🌎 **[ES]** — Spanish preferred (flips post-op delivery, treatment-plan-explainer, and consent forms to bilingual)
   - 📝 **Consent missing** — flag any procedure today that does not have a current signed consent on file (route to `informed-consent-drafter`)

   **4. Lab & Materials**
   - Cases seated today: patient name, provider, case (e.g., "Smith J • Dr. Patel • #14 zirconia crown") — verify arrival before appointment, verify shade match, verify tray and abutment if implant
   - Cases shipping today: what needs to leave and by when (lab pickup window)
   - Overdue or missing cases: escalation needed (call lab by [time], reschedule patient if not arrived by [time])
   - Implant cases: verify the surgical guide, the abutment driver, and the torque-wrench calibration

   **5. Same-Day Treatment Opportunities**
   - For hygiene patients with doctor checks: list likely same-day treatment the doctor should offer (e.g., "#30 existing MOD amalgam — watch for recurrent decay, sealant-to-composite conversion if margins fail bitewing scan")
   - For patients with cancelled next appointments: can today's block be extended to capture the cancelled work?
   - For unscheduled-treatment patients (from PMS): list the top procedure and the production opportunity
   - List any open block the team can fill with these opportunities

   **6. New Patient Spotlight**
   - For each new patient: first name + last initial, appointment time, referral source (existing-patient referral name → personal thank-you note assignment), chief complaint or reason for visit, what the team should know to make a great first impression
   - Insurance verification status (per `insurance-verification-summary`) — verified / pending / out-of-network / self-pay
   - Who will do the practice tour? Who will do the financial presentation? Has the welcome kit (`new-patient-welcome-kit`) gone out?

   **7. Open Chair Time & Fill Plan**
   - List each open block with a targeted fill plan (e.g., "9:30 Op 2, 60 min — call waitlist for #19 crown seat; text recall-due patients within 5 miles")
   - Assign each fill task to a specific team member with a clock-in deadline (e.g., "Maria — texting waitlist by 7:45")
   - Reactivation-eligible? Cross to `patient-reactivation-sequence`

   **8. Team Logistics**
   - Out-of-office: who's out, who covers, who answers their phone
   - CE or admin blocks built into the schedule
   - Meeting reminders (staff meeting, vendor lunch, huddle tomorrow)
   - OSHA / HIPAA / compliance tasks due this week
   - Sterilization spore-test result (weekly) and waterline-test status

   **9. Huddle Close: Decisions & Assignments** (the most important section — never skip)
   - Bullet list of concrete decisions coming out of the huddle: who is calling whom, what gets offered, what stays on the schedule, what gets shifted
   - Each decision: **Owner** + **Action** + **By when**
   - "One focus word" for the day (energy-setter) — e.g., "presence," "follow-through," "accuracy," "warmth"
   - End-of-day recap stub: which 2-3 metrics will be reviewed at end-of-day (production %, same-day-treatment captured, no-show count, new-patient experience score)

5. **Apply formatting rules:**
   - Keep the entire brief to **1 page** (roughly 500-700 words for a typical 12-18 patient day) — it's a huddle, not a briefing book
   - Use bullets, not paragraphs
   - Prioritize scannability — the doctor reads it in 30 seconds before the huddle starts
   - Use patient first name + last initial only (HIPAA minimum-necessary for a document that may be posted in a clinical area)
   - Multi-doctor offices: master view 1 page; per-provider view 1 page each (artifact)
   - Risk flags use the emoji shortcodes above for at-a-glance scan; do not narrate them ("🚩 anticoagulant" not "patient is on a blood thinner")

**Output requirements:**
- Master 1-page brief saved to `outputs/huddles/[YYYY-MM-DD]-huddle.md`
- Per-doctor column detail saved to `outputs/huddles/[YYYY-MM-DD]-doctor-[lastname].md` (multi-doctor only)
- End-of-day recap stub saved to `outputs/huddles/[YYYY-MM-DD]-eod-stub.md` (auto-populated at huddle close, completed at end of day, feeds `meeting-summarizer` end-of-day pattern)
- HIPAA-appropriate (no SSN, no DOB, no full last name in any version intended for clinical-area posting; full last names allowed only in the per-doctor encrypted variant if the practice's HIPAA risk analysis permits)
- Decisions and assignments explicit — owner + verb + deadline
- Tone professional but energetic — this sets the day's mood
- Saved to `outputs/` if the user confirms

## Cross-References

- **Upstream:** `pre-visit-intake-summary` (intake completion status feeds patient-by-patient row), `insurance-verification-summary` (verification status + Q4 benefits feed), `aging-ar-followup-playbook` (PT-BAL collect-at-checkin flag), `informed-consent-drafter` (sedation / surgical consent verification), `referral-coordination-letter` (incoming specialist patients with pre-op packet)
- **Downstream:** `clinical-note-assistant` (carry-over notes from prior day surface in patient-by-patient row), `case-presentation-script` (same-day treatment offer language), `meeting-summarizer` (end-of-day recap pattern), `monthly-practice-kpi-report` (yesterday's scoreboard feed weekly)
- **Sibling:** `scheduling-optimizer` (when open chair time fill is recurring rather than one-off), `after-hours-emergency-triage` (last-night call follow-up surfaces in Yesterday's Scoreboard)

## Common Pitfalls To Avoid

- Do not produce a narrative summary — the huddle brief is a scannable action document
- Do not list every diagnostic finding for every patient — only those relevant to today's decisions
- Do not include detailed PHI beyond what's needed (first name + last initial, not full identifiers)
- Do not skip the "Decisions & Assignments" section — a huddle without assigned actions is just a status meeting
- Do not let the risk-flag overlay become wallpaper — flags surface only when they change behavior; a flag everyone sees every day stops being a signal
- Do not allow same-day-treatment opportunities to become a sales-pressure ritual — frame as "patient may benefit from being offered this if time and clinical fit allow"
- Do not skip the sedation pre-flight (NPO + escort + consent + reversal agents) on any sedation case — auditors and morbidity reviews trace these failures back to a missed huddle item
- Do not run a brief without a one-line yesterday-scoreboard feedback loop — practices that skip the loop drift on production by 5-15% within a quarter
- Do not include a multi-doctor brief without a per-provider column — generic averages obscure the provider whose schedule actually has problems
- Do not produce a Spanish-flag patient row without a Spanish-language post-op / consent / treatment-plan companion ready to hand them at checkout

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]

## Version History

- **v3.0 (2026-04-27)** — Added specialty-mix profiles (GP / perio / endo / OMFS / pedo / ortho / prostho / sleep / DSO multi-doctor), risk-tier overlay (sedation NPO, MRONJ, anticoagulant, A1c, pregnancy, anxiety, pediatric, custody, [ES] bilingual), Q4 benefits-remaining flag, balance-collect-at-checkin flag from aging-ar-followup-playbook, three output artifacts (master + per-doctor column + end-of-day recap stub), PMS export field guidance, sedation pre-flight checklist, skip-condition rule, expanded cross-reference graph (8 upstream/downstream/sibling skills).
- **v2.0 (2026-04-13)** — Standard 9-section structure, HIPAA-appropriate identifiers, scannable formatting rules.
- **v1.0** — Initial release.
