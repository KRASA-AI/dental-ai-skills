---
name: "Dental Staff Onboarding Checklist"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~6–10 hrs/new hire"
version: 1.1
last_eval_score: 9.60
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

### Fast path — only 3 of the 7 fields are actually blocking

Do **not** interrogate the office manager for all seven fields before producing anything. Only three inputs are strictly required:

1. **Role** (field 1)
2. **Start date** (field 2)
3. **State** (field 3) — and even this defaults from `config.yml → company.location` if not given

Everything else is **derived from `config.yml` and stated inline as an explicit assumption** the office manager can correct in one pass:

- **Practice stack** (field 4) → `config.yml → tools`
- **Reporting structure** (field 7) → `config.yml → team.roles` (owner = supervisor for clinical; office manager = supervisor for admin and HIPAA Privacy/Security Officer unless named otherwise)
- **License / credentialing status** (field 5) → left as a **fill-in-blank tracking table** in the Day-1 packet rather than blocked on; the numbers are collected at Day 1 anyway, and they must never be pasted into a non-BAA AI tool (see Guardrails)
- **Prior experience** (field 6) → default to **experienced lateral hire** (shorter "how to do the procedure," longer "how we do it here"); if the new hire is a new graduate, the office manager says so and the plan re-weights

Render every derived value in an **`ASSUMED:`** line at the top of the plan. One correction round, not seven questions. Ask a clarifying question only when a *blocking* field is genuinely absent and undeterminable from config.

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

*Worked example — the **hygienist (RDH)** hire, which is the role where this skill's regulatory clocks actually bite: the RDH is in an operatory with occupational exposure on Day 1, exposes radiographs, and in most states has a scope-of-practice boundary (local anesthesia, nitrous, SRP under which supervision level) that the office manager cannot afford to guess at. The example also demonstrates the **fast path** — three inputs in, a complete plan out, with every derived value stamped `ASSUMED:` for a single correction pass. Grounded in `config.example.yml` (Cherry Creek Family Dental, Denver CO; Dentrix Ascend, Dexis, Overjet, Weave; Dr. Anna Patel owner; office manager = HIPAA officer; 9-person team).*

**Input the user gave (fast path — 3 fields):**
> "New hygienist starting 8/3."

---

> ### 90-DAY ONBOARDING PLAN — RDH — Start 2026-08-03
> **Cherry Creek Family Dental · Denver, CO**
>
> **`ASSUMED:` (derived from `config.yml` — correct any line and I'll re-render)**
> - **State:** Colorado → **Colorado Dental Board / DORA**; CO requires a **separate radiography certification** and CO **permits RDH local-anesthesia administration** with the required course + dentist supervision. **Office manager to verify against the current CO Dental Practice Act — this is the #1 state-specific item on this plan.**
> - **Supervisor:** Dr. Anna Patel (owner/GD). **Buddy/preceptor:** the senior hygienist (2 RDH on staff). **HIPAA Privacy + Security Officer:** the office manager.
> - **Stack:** Dentrix Ascend (PMS + perio charting) · Dexis (imaging) · **Overjet (AI diagnostic)** · Weave (patient comms, BAA on file) · LocalMed (online booking).
> - **Experience:** experienced lateral hire (weights toward "how we do it here"). *Say so if this is a new grad and I'll re-weight.*
> - **Credentials:** collected at Day 1 into the tracking table below — **do not send license #, DEA, SSN, or DOB through any AI tool** (`phi-safe-prompting.md`).
>
> ---
>
> #### ⏱ THE THREE HARD CLOCKS (miss these and the practice is out of compliance)
>
> | Clock | Deadline | Real date | Status |
> |---|---|---|---|
> | **OSHA bloodborne-pathogens training** — must be complete **before any occupational exposure**, i.e. before she is in an op. Federal rule is "within 10 working days," but the exposure comes first, so the *operative* deadline is Day 1. | **Before first patient** | **2026-08-03, AM** | ☐ |
> | **Hepatitis B vaccine offered at no cost** — signed acceptance **or** signed declination on file | **within 10 days** | **by 2026-08-14** | ☐ |
> | **HIPAA workforce training** — CO has no shorter statutory clock; use the 30-day standard | **within 30 days** | **by 2026-09-02** | ☐ |
>
> **⛔ HARD STOP: she may not expose a single radiograph until the Colorado radiography certification is verified on file — even though she has been practicing hygiene elsewhere.** Out-of-state experience is not a substitute. Verify on the DORA license lookup and keep the printout.
>
> ---
>
> #### DAY 1 — 2026-08-03 (nothing on this list is optional)
>
> **HR** ☐ I-9 §1 (her) · ☐ I-9 §2 (employer — **within 3 business days**, by 8/6) · ☐ W-4 + CO withholding · ☐ direct deposit · ☐ emergency contact · ☐ handbook ack · ☐ at-will ack · ☐ **confidentiality / HIPAA ack** · ☐ benefits packet · ☐ scrubs issued
> *Non-compete: **Colorado sharply restricts non-competes** (C.R.S. 8-2-113, as amended 2022) and enforceability against a hygienist is doubtful. **Practice attorney decides — not this skill, not the office manager.***
>
> **Credentialing — tracking table (fill at Day 1; expirations go in the compliance calendar)**
>
> | Credential | # | Expires | Verified on state site | On file |
> |---|---|---|---|---|
> | CO RDH license | ______ | ______ | ☐ | ☐ |
> | **CO radiography certification** | ______ | ______ | ☐ | ☐ **(blocks radiographs)** |
> | CPR / BLS | ______ | ______ | — | ☐ |
> | Local-anesthesia certification *(if she'll administer)* | ______ | ______ | ☐ | ☐ |
> | Nitrous monitoring *(if in scope + practice offers)* | ______ | ______ | ☐ | ☐ |
> | NPI (Type 1) | ______ | — | — | ☐ |
> | CO CE cycle renewal date | — | ______ | — | ☐ |
>
> **Security — least privilege** ☐ Dentrix Ascend login: **clinical + perio charting write; billing = read-only; no financial-adjustment rights** · ☐ Dexis · ☐ **Overjet** · ☐ Weave (clinical role, no marketing sends) · ☐ practice email · ☐ key/key-card · ☐ alarm code **individually assigned and auditable** (never a shared code)
>
> **Tour** ☐ 4 ops · ☐ **sterilization — walk the dirty→clean flow in that direction** · ☐ lab · ☐ **OSHA binder + SDS binder location** · ☐ **emergency kit + AED** (know it cold before her first patient) · ☐ eye-wash · ☐ sharps + biohazard schedule · ☐ exits
>
> ---
>
> #### WEEK 1 — compliance + "how we do it here"
> - **8/3 AM — OSHA BBP** (before ops): exposure-control plan, standard precautions, PPE, **post-exposure protocol — she must be able to say out loud what she does in the first 5 minutes after a stick**, sharps, HazCom/SDS, ionizing radiation + **dosimetry badge assigned**, waterline program (**≤500 CFU/mL**, shock + test cadence), instrument cycle + **weekly spore test**, emergency action plan, ergonomics/loupes.
> - **8/4–8/5 — infection control (CDC dental settings):** surface disinfection tiers, **impression/prosthesis disinfection before lab handoff**, TB baseline (two-step or IGRA), respiratory hygiene.
> - **8/5 — CO Dental Practice Act read-and-sign:** RDH scope, supervision level required for each act (**local anesthesia, SRP, nitrous, radiographs — one line each**), delegation rules, **mandatory reporting** (child/elder abuse, impaired colleague), **CO patient-record retention period**.
> - **8/6 — Dentrix Ascend clinical:** perio charting conventions (AAP 2018 stage/grade — *our* convention), Dexis capture/upload, note templates (`clinical-note-assistant`), recall/recare coding (`recall-sequence-generator`).
> - **8/7 — Overjet (AI diagnostic) — this is the training most practices skip:** how findings surface, **that the provider confirms every AI-flagged finding**, false-positive/false-negative handling, and **the exact sentence she says when a patient asks "is AI reading my x-rays?"** (2026 ADA consent guidance — see `ada-ai-standards-2026.md`, `informed-consent-drafter`). BAA inventory review: **Weave and Dentrix Ascend are BAA-covered; Google Drive is non-PHI only.**
> - **HIPAA training** — schedule any day before 9/2; do not let it drift to Day 29.
>
> #### SHADOW / RAMP (RDH)
> - **Days 1–3:** shadow senior RDH, all chairs. **Days 4–10:** co-treat — she works, senior RDH within earshot. **Weeks 3–4:** independent column, **senior RDH is her 100% backup, no exceptions**. **Week 5+:** solo, standing Friday 10-min check-in with Dr. Patel.
> - Joins the **morning huddle from Day 1** (`morning-huddle-brief`) — she is on the risk-flag rows from the first day (MRONJ, anticoagulant, A1c patients are hers).
>
> ---
>
> #### 30 / 60 / 90
>
> | | Date | Question that is actually being asked | Evidence |
> |---|---|---|---|
> | **Day 30** | **2026-09-02** | Are all three compliance clocks **documented**, not just done? Is she charting perio to our convention? Any ergonomic or safety concern? **What did we fail to teach her?** | Training certs on file · 5 charts audited (`chart-audit-prep`) |
> | **Day 60** | **2026-10-02** | Is hygiene production on target? Is she **finding and communicating perio disease**, or just cleaning? Patient rapport? | Hygiene production vs. goal (`monthly-practice-kpi-report`) · perio-diagnosis rate · re-appointment rate |
> | **Day 90** | **2026-11-01** | **Probationary decision: continue / extend with a written coaching plan / separate.** Decision and its evidence are documented either way. | Full review form + all evidence above |
>
> ---
>
> #### HR FILE BUNDLE → `outputs/onboarding/DK-2026-08-03/`
> Signed acks (handbook, HIPAA, OSHA, confidentiality, at-will) · **Hep-B acceptance or declination** · training certs (OSHA, HIPAA, infection control, radiation safety, CPR) · credential copies + expiration tracker · 30/60/90 forms.
> **Retention:** employment records **3 yr** post-separation · HIPAA training **6 yr** · OSHA training **3 yr** · **OSHA exposure records: duration of employment + 30 years.**

---

**Most common failure mode this example guards against:** treating an **experienced** hire as a low-risk hire. The lateral RDH who has been practicing for eight years is exactly the person who gets walked into an operatory on Day 1 "just to help out," before her OSHA training, before her Hep-B offer, and — the one that actually ends careers — **before anyone verified her Colorado radiography certification**, because everyone assumes the state she came from is close enough. It isn't; radiography certification is state-by-state and does not travel. The plan therefore front-loads the three hard clocks and one hard stop above everything else, and states them as dates rather than as rules, because "within 10 working days" is a rule nobody tracks and "by 2026-08-14" is a date somebody does. The second trap the example closes is the **AI-disclosure gap**: the practice runs Overjet on every bitewing, and a hygienist who cannot answer "is AI reading my x-rays?" in one calm sentence is a consent problem sitting in the operatory — so that sentence is a training deliverable, not a nice-to-have.

## Version History

- **v1.1 (2026-07-13)** — Added a **fast-path rule** to Required Input: only 3 of the 7 fields are blocking (role, start date, state — and state defaults from `config.yml`); everything else is derived from config and stamped as an explicit `ASSUMED:` line for a single correction pass instead of a seven-question interrogation. Added a worked RDH Example Output grounded in `config.example.yml` (Cherry Creek Family Dental; Colorado; Dentrix Ascend / Dexis / Overjet / Weave; office manager = HIPAA officer), demonstrating the fast path, the three hard regulatory clocks rendered as **real dates**, the Colorado radiography-certification hard stop, the least-privilege access matrix, the Overjet AI-disclosure training deliverable, the RDH shadow/ramp ladder, and the 30/60/90 evidence table — plus a most-common-failure-mode callout on treating an experienced lateral hire as a low-risk hire. Additive only; no instruction prose removed. `last_eval_score` populated.
- **v1.0 (2026-04-27)** — Initial release: role-specific 30–60–90 plan across 11 dental roles; Day-1 HR + credentialing + least-privilege security + facility tour packet; OSHA 10-day / Hep-B 10-day / HIPAA state-window regulatory clocks; infection-control and CDC-guideline training; state dental practice act read-and-sign; role-specific PMS, AI-diagnostic, AI-scribe, and AI-receptionist training modules; 30/60/90 competency checkpoints; HR documentation bundle with retention windows; 10 guardrails.
