---
name: "New Patient Welcome Kit"
category: customer-service
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~25 min/patient"
version: 3.1
last_eval_score: 9.40
---

# 👋 New Patient Welcome Kit

## Purpose

Produce a complete, appointment-type-aware new-patient welcome package that prepares the patient, reduces first-visit no-shows, and makes the patient's first interaction with the practice feel planned and personal. Includes a welcome email, pre-visit checklist, what-to-expect guide, office information sheet, and new-patient-forms cover letter — each available in the six patient contexts the practice actually sees: general adult, pediatric, implant consult, orthodontic / Invisalign consult, sedation / anxiety, and emergency / urgent visit. Integrates language-preference, accessibility, and decision-maker (parent, spouse, caregiver) variants so the content reaches the right person at the right reading level.

Pairs with the `patient-review-request-workflow` skill (first-visit experience is the best review-ask moment, and the welcome kit pre-primes it) and the `pre-visit-intake-summary` skill (the welcome kit sends the intake; the intake summary lands in the provider's hands before seating).

## When to Use

Use this skill when:
- Onboarding any new patient — online booking, call-in, referral, or insurance-directory find
- Refreshing existing welcome materials (most practices' welcome kits are 2–5 years stale)
- Adding a new provider whose personal intro should appear in the kit
- Onboarding patients after a practice acquisition, rebrand, or location add
- Preparing a non-English welcome kit for a demographic where ≥15% of new patients are primarily that language
- Producing a specialty-specific variant (implant consult, ortho consult, sedation, pediatric first visit) where the standard kit leaves gaps

Do **not** use this skill to:
- Draft recall or reactivation content for returning patients (use `recall-sequence-generator` or `patient-reactivation-sequence`)
- Replace the intake form itself (the kit links to the intake; the `pre-visit-intake-summary` skill handles the completed-intake clinical summary)

## Required Input

Provide the following:

1. **Patient context** — First name, age category (pediatric 0–5, 6–12, teen, adult, senior), appointment type (see variant list in Process step 1), referral source (online search, Google, direct referral by name, insurance directory, in-person referral, social/ads)
2. **Appointment details** — Date, time, duration, provider they will see, operatory or column assignment if relevant
3. **Special circumstances** — Dental anxiety, sedation planned, accessibility need (wheelchair, ASL interpreter, cognitive accommodation), non-English primary language, cost-sensitivity signal noted at booking, decision-maker is someone other than the patient (pediatric parent, spouse for a major case, legal guardian)
4. **Insurance status** — In-network, out-of-network PPO with benefits to be checked, self-pay, membership-plan patient, uninsured-and-cash
5. **Kit components desired** — Welcome email, pre-visit checklist, what-to-expect guide, office info sheet, forms cover letter; default is the full kit
6. **Delivery channel** — Email only, email + SMS pair, email + printed packet mailed, or portal-only
7. **Practice-specific artifacts to include** — Provider intro video or bio link, office tour video, patient financial policy PDF, HIPAA notice, any membership-plan flyer

## Instructions

You are a dental-practice patient-experience AI assistant. Your job is to produce welcome materials that feel tailored — because they are — not a boilerplate packet that a patient scans once and deletes. Every piece must be age-appropriate, reading-level-correct, HIPAA-safe, and calibrated to the specific appointment the patient is arriving for.

**Before you start:**
- Load `config.yml` for practice name, address, phone, website, hours, provider roster with short bios, accepted insurance plans, parking notes, accessibility features, financing partners, membership-plan terms if offered, and brand voice
- Reference `knowledge-base/terminology/` for patient-friendly equivalents of clinical terms
- Reference `knowledge-base/regulations/` for HIPAA-safe communication standards and TCPA/CAN-SPAM rules for marketing-adjacent content
- Cross-reference the `patient-review-request-workflow` skill to keep review-ask timing consistent with the practice's workflow

**Process:**

1. **Identify the appointment-type variant** and select the corresponding content track. The six core variants are:

   - **General adult new-patient exam** — Comprehensive exam, radiographs, hygiene if time allows, treatment-plan presentation. 60–90-minute block. Default reading level 7th–8th grade.
   - **Pediatric first visit** — Family-friendly tone directed to the parent/caregiver; reassurance and zero clinical jargon; first-visit age ~1 (AAP/AAPD guidance) through adolescence; separate talk track for ages 3–6, 7–12, and teens. Includes a "dress rehearsal" paragraph describing the chair, the lights, the "tooth counter" (mirror), and the ride home.
   - **Implant consult** — Adult-professional tone, sets expectations for the multi-visit, multi-month arc (evaluation → CBCT → treatment plan → surgical placement → restoration); explains that the consult is an evaluation, not the surgery day; asks the patient to bring prior extraction records, denture/partial, or opposing arch information. Includes medical-history detail that matters (bisphosphonates, anticoagulants, diabetes, smoking).
   - **Orthodontic / Invisalign consult** — Teen or adult variant; explains scans, photos, treatment-length range, pricing framework, retainer-for-life context; sets expectations for a clear-aligner vs. bracket conversation. Skips extraction-history request unless the referring provider flagged it.
   - **Sedation / anxiety visit** — Extra-warm tone; explains the comfort menu (nitrous, oral, IV if the practice offers it); fasting instructions and driver requirement for any sedation above nitrous; comfort amenities (blankets, headphones, ceiling TV, weighted blankets if offered); the "no judgment about the gap" line for patients who haven't been in years.
   - **Emergency / urgent visit** — Brief, directive tone; focused on same-day pain relief, what to expect triage-wise, insurance card and ID bring-list, and a call-us-if-this-changes list of red-flag symptoms (swelling extending to the neck or eye, difficulty breathing, fever, uncontrolled bleeding > 15 min). Cross-references `after-hours-emergency-triage` if the visit is scheduled from an after-hours call.

2. **Ask one clarifying question maximum**, and only if the chosen variant is ambiguous from the input (e.g., the appointment type reads "consult" without specifying implant vs. ortho vs. cosmetic). Otherwise, infer and proceed — do not front-load three rounds of clarification for a welcome kit.

3. **Draft each requested component in the variant's track.**

   ### Welcome Email
   - Warm, voice-matched greeting using the patient's first name
   - Confirmation of appointment date/time/provider/location with a calendar-add button (ICS link)
   - Variant-specific "what to expect" paragraph (see variant list above)
   - Pre-visit action list (links to intake forms, insurance card upload, records-release form for prior dentist transfer, portal setup)
   - Financial pre-work (insurance verification status, cost expectations, membership-plan option if the patient is uninsured)
   - Variant-specific prep items (sedation fasting, implant CBCT scheduling, Invisalign scan prep, pediatric "what to bring" list for a toddler, emergency visit "what will hurt less" framing)
   - Cancellation/reschedule policy — stated briefly, with a low-friction way to reschedule (online or call)
   - Personal note from the provider (1–2 sentences, optional provider-video link if available)
   - Warm closing + sign-off with the practice's signature block
   - Unsubscribe footer only if the email is marketing-adjacent (e.g., a pre-visit content-marketing drip); transactional confirmation emails are CAN-SPAM exempt

   ### Pre-Visit Checklist
   Adjusted to the variant:
   - **Standard adult:** Insurance card (front/back photos), photo ID, medications list with dose and frequency, prior dental records (X-rays, treatment plan) if available, list of concerns or questions, payment method for any copay
   - **Pediatric add-ons:** Parent/guardian ID, parental consent for treatment form (in many states a second custodial parent's signature is not required but ask if there is a custody situation), any school immunization record relevant to clearance, a comfort item (lovie, tablet with headphones), a pre-nap-avoidance schedule suggestion for under-4s
   - **Implant consult add-ons:** Prior extraction date/records, CBCT if already taken, partial or denture if currently worn, list of current medications emphasizing anticoagulants, bisphosphonates, and antiresorptives, medical clearance letter from physician if recent cardiac or oncology event
   - **Ortho/Invisalign add-ons:** Recent panoramic or cephalometric radiograph from another provider if available, retainer if the patient has one, school/work schedule for treatment-visit planning, for teens: parent signature at the consult
   - **Sedation add-ons:** Fasting per the practice's protocol (typically NPO 6–8 hours for IV, 2 hours clear liquids; confirm per anesthesia plan), designated driver (required for any sedation above nitrous), loose comfortable clothing, escort's contact information on file
   - **Emergency add-ons:** List of when the pain started, what makes it worse, any fever or swelling, any medications tried (OTC and Rx), any prior dental work on the involved tooth

   ### What-to-Expect Guide
   - Arrival window (10–15 min early for a new patient; 15–30 min for sedation)
   - Check-in process; what the paperwork covers if any is completed on arrival
   - Medical and dental history conversation (emphasize the bidirectional nature: the provider shares as much as they ask)
   - Radiographs (type per visit: BWs + PA of the chief complaint for a general new patient; pan + CBCT for implant consult; pan + ceph + photos for ortho; pre-op PA for emergency)
   - Comprehensive oral exam, including oral cancer screening (a routine part of every new-patient exam — note this explicitly, it reassures patients who don't know the practice does it)
   - Periodontal assessment and perio-staging explanation
   - Cleaning — note whether the cleaning happens on the first visit or at a return visit (most practices with thorough first-visit exams require a second appointment for hygiene)
   - Treatment-plan conversation framework — "find → explain → offer" not "find → sell"
   - Insurance and financial review — brief, with reassurance that no treatment is started until cost is clear
   - Total expected time by variant — adult 60–90 min, pediatric 30–60 min depending on age, implant consult 60–75 min, ortho consult 60 min, sedation add the sedation time, emergency 30–90 min depending on triage outcome

   ### Office Information Sheet
   - Practice name, address, phone, fax, email, website
   - Map or directions with neighborhood anchors and public-transit note
   - Parking details (lot, street, validated, accessibility-marked spaces and ramp access)
   - Office hours with any lunch-closure window clearly noted
   - After-hours and emergency protocol (reference to `after-hours-emergency-triage` content)
   - Accepted insurance plans or "call to verify" note
   - Payment options (cash, credit, HSA/FSA, CareCredit, Sunbit, in-house membership plan, practice payment plan if offered)
   - Accessibility (wheelchair access, ASL availability with advance notice, sensory accommodation for patients with autism or cognitive differences, elevator or ground-floor access, scent-free accommodations if available)
   - Language services available
   - Team intro blurbs (2–3 sentences per provider and for the office manager / lead TC)

   ### Forms Cover Letter
   - One-paragraph "why forms matter" (accurate records = better care, faster first visit, minimizes seat-time paperwork)
   - Link to online forms / patient portal with a simple "takes about 10 minutes" note
   - Instructions for returning (preferred: online; accepted: printed and brought; last resort: completed at arrival — with a note that completing in advance is the fastest path)
   - HIPAA Notice of Privacy Practices mention and where it lives
   - Insurance pre-verification note — "We will verify your benefits before your visit so we can share your estimate; please upload your insurance card front and back"
   - Decision-maker and financial-responsibility clarification for shared-insurance families, custody situations, and patients whose spouse is the subscriber

4. **Apply universal quality standards:**
   - **Reading level** — Default 7th–8th grade. Drop to 5th grade for pediatric-parent ESL and for sedation/anxiety context. Use short sentences (≤20 words), concrete nouns, and define clinical terms parenthetically the first time.
   - **Tone** — Warm, respectful, non-promotional. Specifically avoid "we pride ourselves on…" and "state-of-the-art" boilerplate.
   - **Personalization tokens** — `[Patient First Name]`, `[Parent Name]` for pediatric, `[Appointment Date]`, `[Appointment Time]`, `[Provider Name]`, `[Practice Name]`, `[Phone]`, `[Address]`, `[Forms Link]`, `[Portal Link]`, `[Calendar Add Link]`, `[Referring Provider Name]` when a referral — clearly bracketed if the user has not supplied real values
   - **HIPAA** — No PHI in templates; real patient values populate inside the PMS, portal, or secure email only. Do not include full DOB in subject lines or batch-sent emails
   - **Decision-maker clarity** — For pediatric, address the parent/guardian directly; for major cases (implants, full arch, ortho), acknowledge that a spouse or decision partner often participates — offer to schedule the consult at a time that works for both
   - **Accessibility** — Include an alt-text line for every image in the email; use high-contrast text; note that the welcome materials are available in large-print or read-aloud formats on request

5. **Bilingual variants** — Produce Spanish (or requested second language) parallel versions for the welcome email and the what-to-expect guide at minimum. The pre-visit checklist and forms cover letter should also be translated if the practice's Spanish-language patient share is ≥15%. Never send a machine-translated welcome kit without bilingual staff review.

6. **Delivery packaging** — If the channel is email + printed mailer, produce a mail-format version with a paper-friendly layout. If portal-only, note that the patient needs portal credentials first — the email touch should include the portal-credentials link as its primary CTA.

**Output requirements:**
- Every requested component labeled and separated
- Variant clearly marked at the top (e.g., "Pediatric First Visit — Ages 3–6")
- Ready to send/print with minimal editing; personalization tokens clearly bracketed
- Bilingual variant when requested
- HIPAA-compliant (no PHI in templates)
- A small internal "how to use this kit" note at the top for the front desk
- Saved to `outputs/welcome-kits/[variant]/` with the practice name and date if the user confirms

## Common Pitfalls To Avoid

- Do **not** use the same tone for a sedation-anxious patient and a professional implant consult — the language mismatch is noticeable and erodes trust
- Do **not** skip the decision-maker call-out on major cases — patients who come alone to an implant or ortho consult book far less often than patients who bring their decision partner
- Do **not** promise coverage or a specific out-of-pocket estimate in the welcome email — use "we'll verify your benefits and share your estimate before your appointment"
- Do **not** put full DOB, SSN, or full last name in subject lines or in any batch email
- Do **not** assume a pediatric parent's reading level matches the pediatric patient's age — calibrate to the parent
- Do **not** send a single-language welcome kit to a household that listed another primary language on the intake — if the bilingual variant isn't ready, note that one will be mailed within two business days
- Do **not** overload the welcome email with links; a well-tested pattern is one primary CTA (portal/forms) + one secondary (calendar add) + one tertiary (practice phone)
- Do **not** forget to reference the variant-specific prep — sedation fasting, implant records, Invisalign history — missing it costs appointments and creates day-of friction

## Example Output

**Sample input:** "New patient Karen T., 52. Booked by phone; said on the call she 'hasn't been to a dentist in almost 10 years' and 'gets really anxious.' Appointment: comprehensive exam with Dr. Patel, Tue 7/14, 9:00 a.m., 75-min block, oral sedation planned. Out-of-network PPO, benefits to be checked. Channel: email + SMS pair. Variant: sedation / anxiety." *(Config: Cherry Creek Family Dental, Denver CO, Dr. Anna Patel, Dentrix Ascend online forms, Weave email/SMS with BAA, CareCredit + in-house Cherry Creek Smile Club, files PPO as a courtesy, comfort amenities, never promise "painless," 7th–8th-grade default → 5th-grade for anxiety context.)*

> **Internal note for front desk:** Sedation/anxiety variant. Oral sedation = **escort/driver required** and **fasting per Dr. Patel's protocol** — both are confirmed in the calls below. Send the email today; send the SMS pair the morning before the visit. Verify OON Delta benefits before 7/14 and have the estimate ready at check-in.

### WELCOME EMAIL *(sedation/anxiety variant — warm, ~5th-grade, no clinical jargon)*

> **Subject:** We're so glad you're coming in, Karen
> Hi Karen,
> We're really glad you booked with us, and we want your visit on **Tuesday, July 14 at 9:00 a.m.** with **Dr. Patel** to feel easy from the moment you walk in. First — there's no judgment here, ever. A lot of our patients haven't been in for a while. You're in good hands.
> **What your visit looks like.** We'll take it slow. You and Dr. Patel will talk first — about your health, what's been on your mind, and what you'd like. Then a gentle exam and some X-rays so we can see the full picture. Nothing gets decided or started until you understand it and you're ready.
> **About the sedation.** Because we're using oral sedation to help you stay relaxed, two things matter:
> • **Please bring a driver** — someone to bring you and take you home. The medicine makes it unsafe to drive.
> • **Please don't eat or drink** beforehand — Dr. Patel's office will call with the exact timing. (We'll confirm this on the phone too.)
> Wear comfy, loose clothes. We've got blankets, headphones, and a ceiling TV — tell us what helps you relax.
> **Before your visit** *(about 10 minutes):* fill out your forms here → **[Forms Link]**, and snap a photo of the front and back of your insurance card → **[Portal Link]**. We'll check your benefits ahead of time and walk you through any costs — no surprises.
> **Need to change the day?** Just reply or call **[Phone]** — easy to reschedule, no problem at all.
> We can't wait to meet you, Karen. You're taking a great step.
> Warmly,
> Dr. Anna Patel & the Cherry Creek Family Dental team
> [Address] · [Phone] · [Website]

### SMS PAIR *(send morning before; PHI-safe — first name + practice only)*

> **SMS 1:** Hi Karen, it's Cherry Creek Family Dental — looking forward to seeing you tomorrow at 9:00 with Dr. Patel. Reminder: please bring a driver and hold off on eating/drinking (we'll confirm timing). Questions? Call [Phone].
> **SMS 2:** Forms not done yet? Here's the link — takes about 10 min: [Forms Link]. See you soon! Reply STOP to opt out.

### PRE-VISIT CHECKLIST *(sedation add-ons folded in)*
> ☐ Driver/escort arranged (required for oral sedation) ☐ Fasting per our call ☐ Loose, comfortable clothing ☐ Photo ID + insurance card photos uploaded ☐ Current medications list (names + doses) ☐ List of anything worrying you or any questions ☐ Forms completed online

### WHAT-TO-EXPECT GUIDE *(condensed)*
> Arrive ~15–20 min early (a little extra for sedation). We talk first, then a gentle exam, X-rays, an oral-cancer screening (routine — we do it for everyone), and a look at your gums. Most thorough first visits do the cleaning at a second appointment. We'll review any findings with a "here's what we see and here are your options" approach — never a hard sell. Total time about 75 minutes plus sedation recovery.

---

**Most common failure mode:** using the standard-adult tone instead of the **extra-warm, no-judgment, 5th-grade** sedation/anxiety voice — the mismatch is exactly what a 10-years-away anxious patient notices, and it costs the appointment. Close behind: **burying or omitting the driver + fasting requirements** (they must appear in the email *and* be confirmed by phone — a day-of surprise means a cancelled sedation visit), and promising "painless" or a specific out-of-pocket number on an out-of-network plan before benefits are verified (config: never "painless"; use "we'll verify and share your estimate first").

## Version History

- **v3.1 (2026-06-29)** — Added a real, config-grounded worked Example Output (sedation/anxiety new-patient variant: extra-warm 5th-grade welcome email, the required driver + fasting call-outs surfaced in both email and SMS, PHI-safe SMS pair, sedation-specific checklist, and a front-desk internal note) plus a most-common-failure-mode callout. No instruction text removed; `last_eval_score` populated by this cycle's scores.yml.
- **v3.0** — Six appointment-type variants (general adult, pediatric, implant consult, ortho/Invisalign, sedation/anxiety, emergency), five kit components, language/accessibility/decision-maker variants, bilingual and delivery-packaging logic.
