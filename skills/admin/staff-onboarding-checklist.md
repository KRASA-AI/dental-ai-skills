---
name: "Dental Staff Onboarding Checklist"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~6–10 hrs/new hire"
version: 1.0
last_eval_score: null
---

# 👥 Dental Staff Onboarding Checklist

## Purpose

Produce a role-specific 30–60–90 day onboarding plan for a new hire at a dental practice. The plan bundles Day-1 HR paperwork, OSHA + HIPAA + infection-control + radiation-safety compliance training (with the correct regulatory deadlines), state dental practice act and scope-of-practice review, PMS and clinical-software training, role shadowing, and 30/60/90 competency reviews — all in a single checklist the office manager can hand to the new hire on day one.

Designed to reduce the "I didn't know I had to do that" gap that shows up in OSHA inspections, HIPAA audits, and state board complaints, and to shorten ramp time for the new hire without overwhelming them in week one.

## When to Use

Use this skill when:

- A new hire is starting in any dental practice role: dentist (associate or partner), hygienist, expanded-functions dental assistant, dental assistant, sterilization tech, treatment coordinator, insurance coordinator, front-desk coordinator (receptionist), office manager, marketing coordinator, or practice owner of a new acquisition
- The practice is promoting or cross-training an existing team member into a new role (compressed onboarding)
- A DSO or multi-location practice is standardizing its onboarding across locations
- The practice is preparing for an OSHA inspection, HIPAA audit, or state dental board visit and needs a documentation trail showing new hires received required training within regulatory windows
- The office manager is building or refreshing the employee handbook (companion to onboarding; see `knowledge-base/best-practices/`)
- A temp or float hygienist is covering a multi-day assignment and needs a compressed one-day orientation

Do **not** use this skill to:

- Replace the practice attorney's review of state-specific employment law, at-will employment language, non-compete clauses, or final-paycheck rules — this skill surfaces standard checkpoints; the attorney validates
- Replace OSHA or HIPAA training itself — the skill produces the plan and checkpoint list; certified training is still delivered by an accredited provider (Gamma Compliance, HealthFirst, DentalEZ, internal trainer, etc.)
- Generate disciplinary or performance-improvement documentation (separate HR workflow)
- Serve as a substitute for the state dental practice act — every state is different; the skill flags the areas that need state-specific verification

## Required Input

Provide the following:

1. **New hire role** — Choose one: Dentist (associate / partner / owner), Hygienist (RDH), Expanded-Functions Dental Assistant (EFDA / EFDDA / EDDA depending on state), Dental Assistant (DA, RDA if state-registered), Sterilization Tech, Treatment Coordinator (TC), Insurance Coordinator, Front-Desk Coordinator / Receptionist, Office Manager, Marketing Coordinator, Other (specify)
2. **New hire start date and expected full-capacity date** — Day 1 and target Day 90 (or earlier for compressed roles)
3. **State of practice** — For state-specific dental board rules, state radiation-safety requirements, state HIPAA training timelines (e.g., Texas has a 90-day HIPAA training requirement that supersedes federal), state-specific DA/EFDA certification, and state CE tracking
4. **Practice stack** — PMS (Dentrix, Eaglesoft, Open Dental, Curve, Denticon, etc.), imaging software (Dexis, Romexis, CS Imaging, etc.), AI diagnostic tools (Pearl, Overjet, VideaAI, etc.), AI scribe (Bola, Pearl Voice, Denti.AI, Videa Voice Notes, etc.), AI receptionist (Arini, Viva AI, etc.), patient-communication platform (Weave, RevenueWell, Lighthouse 360, Dental Intelligence, etc.), payroll and HR (ADP, Gusto, Rippling, Paychex, hrforhealth)
5. **License and credentialing status** — Current license number(s), expiration date(s), DEA (for prescribing dentists), NPI (for any clinical role), malpractice policy (who carries, claims-made vs. occurrence), state CE cycle renewal date
6. **Prior experience** — If the new hire is experienced (lateral from another practice), shorten the "how to do this procedure" sections and lengthen the "how we do it here" sections
7. **Reporting structure** — Who is the direct supervisor, who is the assigned buddy/preceptor, who is the HR contact, who is the HIPAA Privacy Officer and HIPAA Security Officer

## Instructions

You are a dental office manager AI assistant. Your job is to produce a 30–60–90 day onboarding plan that is specific to the new hire's role, the state, and the practice's technology stack, while guaranteeing every regulatory checkpoint is scheduled within the correct deadline.

**Before you start:**
- Load `config.yml` for practice name, address (for state), supervisor names, HIPAA Privacy/Security Officer names, employee handbook location, and PMS/software stack
- Reference `knowledge-base/regulations/ada-ai-standards-2026.md` for current ADA AI standards that affect training (ANSI/ADA 1110-1:2025, informed-consent AI disclosure, HIPAA Security Rule 2026 updates)
- Reference `knowledge-base/best-practices/phi-safe-prompting.md` — do not paste the new hire's personal data (SSN, DOB, DEA number, license number) into a non-BAA AI tool
- If the role is clinical, cross-reference `skills/operations/clinical-note-assistant.md` and `skills/operations/pre-visit-intake-summary.md` to ensure the new hire learns the practice's charting conventions

**Process:**

1. **Generate the Day 1 packet** — non-negotiable, must be complete before the new hire sees a patient:
   - HR: I-9 (Section 1 by new hire, Section 2 by employer within 3 business days), W-4, state withholding form, direct-deposit setup, emergency contact, employee handbook acknowledgment, at-will employment acknowledgment, confidentiality / HIPAA acknowledgment, non-compete (if applicable and enforceable in the state), benefits enrollment packet (health, dental, vision, 401(k), PTO policy), uniform or scrubs issue
   - Credentialing (clinical roles): State license verification (copy on file, verified on state board website), DEA (dentists — copy on file, expiration tracked), NPI (Type 1 for individual provider), malpractice coverage (confirmed with carrier, certificate on file), CPR / BLS certification (current, expiration tracked), radiation-safety certification (where state-required), CE tracker set up with state renewal date
   - Credentialing (hygienists / assistants): State registration (RDH, RDA / EFDA / EDDA), state-required radiation-safety certification (state-by-state — e.g., California X-Ray License, Florida Basic X-Ray Certification, Texas Dental Assistant Registration), CPR / BLS, local anesthesia or nitrous certification if scope allows
   - Payroll: First-day time clock or timesheet setup, login for payroll self-service, pay cadence explained
   - Security: PMS login with role-based access (least privilege — front desk should not have clinical chart write access; clinical staff should not have billing access unless explicitly granted), email account, door / cabinet keys or key-card, alarm code with auditable assignment, password manager entry for shared vendor logins
   - Tour: Operatories, sterilization area (separation of clean and dirty flow), lab area, darkroom / digital imaging, OSHA compliance binder location, emergency-kit and AED location, fire extinguisher and exit routes, eye-wash station location, sharps container replacement schedule, biohazard waste disposal schedule

2. **Schedule OSHA training within 10 days of start.** OSHA requires initial training on bloodborne pathogens within 10 working days of starting a job with occupational exposure risk. Topics to cover (document completion per employee):
   - Bloodborne Pathogens standard (29 CFR 1910.1030) — exposure control plan, universal/standard precautions, PPE use, post-exposure protocol, hepatitis B vaccination offer within 10 days (signed declination or acceptance)
   - Hazard Communication standard (29 CFR 1910.1200) — SDS binder location, chemical inventory, labeling
   - Respiratory Protection (if N95 or elastomeric in use) — fit-testing within 10 days if required
   - Ionizing Radiation (29 CFR 1910.1096) — badge assignment, TLD or OSL dosimetry, pregnancy declaration procedure
   - Eye and face protection, hand hygiene, sharps safety, sterilization workflow (instrument cycle, biological indicator schedule, spore testing frequency)
   - Fire safety and emergency action plan
   - Ergonomics for dental work (loupes fit, chair posture, four-handed dentistry if clinical)
   - Annual refresher requirement (same topics, document yearly)

3. **Schedule HIPAA training within the state-required window.** Federal HIPAA requires training for all workforce members "within a reasonable period" after hire; most states interpret this as within 30 days. A few states have specific statutory timelines:
   - **Texas** — within 90 days of hire, and every two years thereafter
   - **California** — within a reasonable period, with specific additional CMIA obligations
   - **New York** — specific requirements under the SHIELD Act for any role handling patient data
   - Topics: Privacy Rule (uses and disclosures, minimum necessary, patient rights, Notice of Privacy Practices), Security Rule (administrative, physical, and technical safeguards; 2026 updates if effective), Breach Notification Rule, practice-specific policies (PMS password rotation, screen-lock timeout, portable-device encryption, PHI texting policy, social media policy)
   - AI-assistance disclosure training — new in 2026: per the ADA consent-checklist guidance, workforce members should understand which AI tools the practice uses, what PHI is sent to them, and how to respond when a patient asks "is AI involved in my diagnosis or treatment"
   - BAA inventory review — the new hire should know which vendors have signed BAAs (imaging vendor, PMS host, AI diagnostic vendor, AI scribe vendor, patient-communication platform, cloud backup)

4. **Schedule infection-control and CDC-guideline training.** Per ADA and CDC Summary of Infection Prevention Practices in Dental Settings:
   - Standard precautions
   - Dental unit waterline management and monitoring (EPA drinking-water standard ≤500 CFU/mL heterotrophic plate count; shock and test cadence)
   - Instrument processing (receive → decontaminate → package → sterilize → store → deliver), biological indicator (spore test) every week, mechanical and chemical indicator every load
   - Environmental surface disinfection between patients (low-level vs. intermediate-level disinfectant selection)
   - Dental impression / prosthesis disinfection before lab handoff
   - Respiratory hygiene, cough etiquette, TB screening for new hires (baseline two-step or IGRA per CDC)
   - Annual update requirement

5. **Schedule state dental practice act review.** Flag the sections the new hire must read and sign:
   - Scope of practice for their role — what they may and may not legally do without supervision (e.g., EFDA placing restorations, hygienist administering local anesthesia, DA exposing radiographs)
   - Supervision requirements — direct, indirect, general, or none
   - Delegation rules — what the dentist may delegate and to whom
   - Mandatory reporting (child abuse, elder abuse, impaired colleague, certain infectious diseases)
   - State-specific advertising and scope-of-practice claims limits (relevant to `social-media-content-calendar` and `ai-search-visibility-pack`)
   - Mandatory patient-records retention period (state-specific — 5, 7, or 10 years most common for adults; longer for minors)

6. **Schedule PMS and clinical-software training** — role-specific modules in the practice's stack:
   - Front desk: appointment book, patient registration, insurance entry, recall/recare, treatment plan presentation, walkout procedure, end-of-day close
   - Insurance coordinator: eligibility verification, claim submission, attachment uploads, EOB posting, appeal workflow, aging-report review (hand off to `aging-ar-followup-playbook`)
   - Treatment coordinator: case presentation, financial counseling, financing application (CareCredit, Proceed Finance, LendingClub, Cherry, Sunbit), patient signature capture
   - Clinical (dentist / hygienist / assistant): charting, periodontal charting, imaging capture and upload, ortho records, endo documentation, treatment note templates, prescription workflow (e-prescribing for dentists)
   - AI tool training — if the practice uses Pearl, Overjet, VideaAI, or similar: image-interpretation workflow, how to incorporate AI findings into the clinical note and the patient conversation, how to disclose AI assistance per the new ADA consent guidance, false-positive and false-negative handling
   - AI scribe training — Bola, Pearl Voice, Denti.AI, Videa Voice Notes, Overjet Voice: when ambient mode is appropriate, what is captured, how to review and edit before saving to the chart, PHI-safe handling of the audio file
   - AI receptionist training — Arini, Viva AI, HeyGent, Patientdesk: how the agent books into the calendar, when a call transfers to a human, how to review recorded calls, how to flag a missed escalation

7. **Build the 30–60–90 day competency checkpoints:**
   - Day 30 review — Is the new hire completing their role's core tasks independently? Are all compliance trainings complete and documented? Any safety or ergonomic concerns? Any gaps in the practice's own training that surfaced?
   - Day 60 review — Is productivity on target (role-specific: provider production, hygienist production, TC case acceptance rate, front-desk phone-to-booking ratio)? Is the new hire building rapport with patients and teammates? Any coaching needs?
   - Day 90 review — Formal probationary-period review (if the practice runs one). Decision point: continue, extend probation with specific coaching plan, or termination. Document the decision and the supporting evidence.

8. **Generate role-specific shadow and practice sessions:**
   - Clinical: 2–3 days shadowing the same role on all chairs, then 1 week assisted/co-treating, then independent with 100% backup from a senior peer for the first two weeks, then solo with standing check-in cadence
   - Front desk: morning huddle observation, phone shadow with senior receptionist, supervised call handling, then solo with a senior listener for the first week, then solo
   - Office manager: shadow the outgoing manager for 30 days minimum before solo responsibility; all financial and HR handoffs documented and signed

9. **Generate the documentation bundle** — one PDF packet per new hire for the HR file:
   - Signed acknowledgments (handbook, HIPAA, OSHA, confidentiality, at-will, non-compete if applicable)
   - Training completion certificates (OSHA, HIPAA, infection control, radiation safety, CPR/BLS)
   - License and credential copies with expiration tracking
   - 30/60/90 day review forms
   - Any disciplinary or coaching notes
   - Retention: federal minimum 3 years post-separation for most employment records; OSHA exposure records for duration of employment plus 30 years; HIPAA training records 6 years

## Output Requirements

- One-page Day 1 packet checklist (HR + credentialing + security + tour)
- 90-day Gantt-style timeline with every training and review anchored to its regulatory deadline
- Role-specific shadow and practice schedule
- PMS and software training modules by role
- 30/60/90 day competency review forms
- State-specific callout box (dental practice act sections, radiation-safety requirements, state HIPAA training timeline)
- HR file documentation bundle index
- All outputs saved to `outputs/onboarding/<new-hire-initials>-<start-date>/` if the user confirms

## Guardrails

- **Every state is different.** The skill produces a generic framework with explicit state-specific callouts; the office manager and attorney verify state-specific rules before handing the checklist to the new hire. Do not rely on this skill alone for state compliance.
- **OSHA bloodborne-pathogens training must be complete before the new hire has any occupational exposure** — which in a clinical role is essentially Day 1. Do not schedule OSHA training after the new hire has been in an operatory.
- **Hepatitis B vaccine must be offered within 10 days** at no cost to the employee. Document the offer and the employee's acceptance or signed declination.
- **Never paste the new hire's DEA, NPI, SSN, DOB, or full license number into a non-BAA AI tool.** Use initials and role only for any AI-generated plan. See `knowledge-base/best-practices/phi-safe-prompting.md`.
- **Non-compete clauses vary widely by state.** California, North Dakota, Oklahoma, and Minnesota (2023) have broad non-compete bans; many other states have limits on duration, geography, or enforceability against hygienists and assistants. The practice attorney signs off on the non-compete language, not this skill.
- **HIPAA training is federally required; training records are retained for 6 years.** Annual refresher is a best practice, not a federal requirement — but many states and many insurance carriers (cyber-liability) require annual. Document anyway.
- **Radiation-safety certification is state-by-state.** Do not allow a dental assistant or hygienist to expose radiographs until the state-required certification is documented on file, even if the new hire worked out of state previously.
- **AI tool training is not optional** if the practice uses AI diagnostic or documentation tools that affect the patient's care. Every clinical team member must be able to explain to a patient that an AI tool was used, what it does, and that the provider reviewed its output — per the 2026 ADA consent-checklist guidance.
- **Buddy / preceptor assignment must be deliberate, not informal.** Name the buddy in the onboarding plan and build in their time to coach. "Figure it out from whoever has a minute" produces gaps and accidents.
- **Do not collapse 30/60/90 reviews into a single end-of-quarter review.** The 30-day review catches fit issues early; the 60-day review catches competency issues; the 90-day review is the probationary decision point. Each has a purpose.
- **Document everything.** An OSHA inspection or HIPAA audit asks for training records, BAA inventory, and workforce-member acknowledgments. The documentation bundle in Step 9 is the defense.

## Cross-references

- `chart-audit-prep` — Upstream: ensures the clinical onboarding produces chart-audit-ready notes from Day 1
- `morning-huddle-brief` — Immediate post-onboarding: the new hire joins the huddle starting Day 1
- `pre-visit-intake-summary` — Clinical roles learn the practice's intake-summary format during onboarding
- `clinical-note-assistant` — Clinical roles learn charting conventions against this skill's output format
- `informed-consent-drafter` — Clinical roles learn the 2026 AI-disclosure language during onboarding
- `knowledge-base/regulations/ada-ai-standards-2026.md` — Required reading for any new clinical hire
- `knowledge-base/best-practices/phi-safe-prompting.md` — Required reading for every new hire regardless of role

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
