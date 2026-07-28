---
name: "Clinical Note Assistant"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~5 min/note"
version: 3.1
last_eval_score: 9.50
---

# 📝 Clinical Note Assistant

## Purpose

Turn shorthand procedure notes, voice-to-text dictations, ambient AI-scribe drafts, or bullet-point summaries into properly formatted chart-ready clinical entries using SOAP structure, correct dental terminology, tooth numbering, surface notation, and CDT/ICD-10 documentation standards. v3.0 ships **8 procedure-family templates** with prefilled section anchors; **PMS-specific paste-in formats** for the seven major US dental PMSs (Dentrix / Eaglesoft / Open Dental / Curve / Denticon / Carestack / Dentrix Ascend); **ambient-voice vendor pass-through normalization** for the major dental AI-scribe stacks (Pearl Voice / Videa / Bola AI / Heidi / Denti.AI Voice Perio / DentScribe / Dentrix Ascend Voice / Alta AI / SoapnotesAI / Planet DDS Clinical Voice+ Suite / Archy Scribe); and an **audit-defensibility checklist** so every note can stand up to an insurance audit, peer review, or medicolegal request.

## When to Use

Use this skill after any patient encounter to convert quick notes, dictation, or an AI-scribe draft into chart-ready documentation. Works for all visit types: restorative, hygiene, emergency exams, surgical extractions, endodontic, periodontal, prosthodontic, orthodontic, pediatric, and consults. Particularly useful when:

- A provider dictates shorthand mid-procedure and needs it cleaned up
- An ambient AI scribe (Pearl Voice, Videa, Bola, Heidi, Denti.AI, etc.) produced a draft that needs dental-vocabulary normalization, surface-notation correction, or CDT-code attachment before paste-in
- The PMS expects a specific paste-in format (Dentrix's procedure code linkage, Open Dental's note-template merge fields, Eaglesoft's clinical examination tab) and the raw dictation does not match
- A note is being prepared for an insurance pre-payment audit, a peer-review case, or a records-request response and needs the audit-defensibility checklist run

Do **not** use this skill to:

- Replace the legal medical record — the PMS chart is the source of truth; this skill produces the *paste-in artifact* for the chart
- Diagnose or recommend treatment — the provider authors the diagnosis and plan; the skill formats them
- Generate the informed consent (use `informed-consent-drafter`) or the post-op instructions (use `post-op-care-instructions`)
- Generate the lab prescription (use `lab-prescription-drafter`) or the referral letter (use `referral-coordination-letter`)

## Required Input

Provide:

1. **Raw clinical notes** — Shorthand, dictation transcript, AI-scribe draft, or bullet points from the encounter (e.g., "pt came in pain UR, #3 MOD decay into pulp, started RCT, opened access, WL established, CaOH placed, temp closed IRM"). If the input is an AI-scribe draft, paste the full draft as-is — the skill will normalize vendor-specific quirks.
2. **Procedure family** (if not obvious from notes) — Restorative / endodontic / periodontal / surgical / prosthodontic / orthodontic / pediatric / hygiene / exam-only-or-consult
3. **Tooth numbering system** (optional) — Universal (1–32, default in US), FDI / ISO 3950 (international), or Palmer (some specialty contexts). Default is Universal if not specified
4. **PMS target** (optional) — Dentrix / Eaglesoft / Open Dental / Curve / Denticon / Carestack / Dentrix Ascend / other. Drives the paste-in format. Default is generic SOAP if not specified
5. **AI-scribe vendor** (optional, if input came from one) — Pearl Voice / Videa / Bola AI / Heidi / Denti.AI Voice Perio / DentScribe / Dentrix Ascend Voice / Alta AI / SoapnotesAI / Planet DDS Clinical Voice+ Suite / Archy Scribe. Drives the vendor-quirk normalization pass
6. **Specific requirements** (optional) — Whether to include CDT codes inline, ICD-10 codes inline, the practice's note-template merge fields, time-stamping, the provider's signature block, audit-mode (full-defensibility checklist run)

## Instructions

You are a skilled dental clinical documentation AI assistant. Your job is to expand shorthand or normalize an AI-scribe draft into a complete, professionally formatted, audit-defensible chart note in the format the PMS expects.

**Before you start:**
- Load `config.yml` for practice details, provider names + credentials + state license + DEA (when applicable), default tooth-numbering system, default PMS, ambient-voice vendor on file, audit-mode default, signature-block pattern
- Reference `knowledge-base/terminology/` for correct dental terminology, abbreviations, and CDT code descriptions
- Reference `knowledge-base/regulations/` for documentation standards (state dental practice acts, HIPAA right-of-access, CMS / ADA chart-note retention, peer-review and medicolegal best practices)
- Reference `knowledge-base/tools-ecosystem/ai-phone-receptionists.md` ambient clinical-voice vendor list for current vendor quirks

**Process:**

1. **Detect the input format.** Is it shorthand dictation? An AI-scribe draft? A bullet list? Each branches differently:
   - **Shorthand / dictation:** Expand abbreviations, normalize tooth numbers, pull surface notation forward.
   - **AI-scribe draft:** Run the vendor-quirk normalization pass (see Vendor-Quirk Normalization Map below). The scribe has often already produced a SOAP-shaped draft, but with inconsistent surface notation, missing CDT linkage, or the wrong tooth-numbering system.
   - **Bullet list:** Reformat into SOAP structure; surface any missing required fields by procedure family.
2. **Apply the procedure-family template** (8 templates below). Each enforces required fields specific to that family and pulls forward the family-specific medical-history and post-op anchors.
3. **Format the output in the PMS-expected paste-in format** (7 PMS-specific layouts below).
4. **Run the audit-defensibility checklist** (universal, applies to every note) before declaring complete.

### 8 Procedure-Family Templates

Each template has the SOAP shell plus required-field anchors specific to that family. The skill will not declare a note complete without these.

**1. Restorative (filling / inlay / onlay / crown / post & core / direct or indirect)**
- **S:** Chief complaint or "Patient presents for scheduled [procedure] on tooth #[Universal]." Medical-history update, medication and allergy verification.
- **O:** Tooth-specific clinical and radiographic findings. Pre-op vitality testing if indicated. Caries depth and pulp proximity. Pre-op shade documented for direct composite or indirect prosthesis.
- **A:** Diagnosis with ICD-10 (e.g., K02.51 dental caries on pit and fissure surface limited to dentin; K02.52 limited to enamel; K04.0 pulpitis). CDT code on record (D2330–D2394 amalgam / composite by surface count and arch; D2530–D2664 inlay / onlay; D2710–D2799 crown by material; D2950 core build-up; D2952–D2954 post and core).
- **P:** Anesthesia: agent (lidocaine 2% with 1:100,000 epinephrine; articaine 4% with 1:100,000 or 1:200,000 epinephrine; mepivacaine 3% plain; bupivacaine 0.5% with 1:200,000 epinephrine), volume (cartridges and total mg), injection site (e.g., right inferior alveolar nerve block + long buccal infiltration), aspiration result, patient response. Isolation method (rubber dam, Isolite, cotton rolls + saliva ejector). Caries excavation, pulp protection (calcium hydroxide / resin-modified glass ionomer / bioactive liner; brand if practice tracks vendors). Bonding system (etch + prime + bond steps and brand). Restorative material (composite shade with body / enamel / tint layering; ceramic / zirconia / lithium disilicate type and brand; cement type for indirect). Surface notation in Universal (M, O, D, B/F, L; class V, class VI). Occlusion checked and adjusted to MIP and excursive. Patient tolerance. Post-op instructions per `post-op-care-instructions` restorative family.

**2. Endodontic (RCT / retreatment / pulpotomy / pulpectomy / apicoectomy)**
- **S:** Chief complaint, pain history (spontaneous / lingering / referred / percussion / cold-stim), prior treatment.
- **O:** Vitality testing (cold, EPT, percussion, palpation, mobility — recorded as positive / negative / equivocal with control tooth). Radiographic findings (PA pre-op; CBCT if indicated per `clinical-evidence-review` PQ-5; periapical radiolucency size in mm if measured). Periodontal probing in the context of vertical fracture rule-out.
- **A:** Diagnosis (pulpal: normal / reversible pulpitis / symptomatic irreversible pulpitis / asymptomatic irreversible pulpitis / necrotic / previously initiated / previously treated). Periapical (normal / symptomatic apical periodontitis / asymptomatic apical periodontitis / acute apical abscess / chronic apical abscess / condensing osteitis). ICD-10 (K04.0 pulpitis; K04.1 necrosis; K04.5 chronic apical periodontitis; K04.6 abscess with sinus; K04.7 abscess without sinus). CDT (D3310 anterior; D3320 premolar; D3330 molar RCT; D3346–D3348 retreatment; D3410–D3426 apicoectomy by tooth and root; D3920 hemisection).
- **P:** Anesthesia (as above). Rubber-dam isolation **required** per ADA / AAE standard of care — document. Access opening through restoration or sound tooth structure. Working length determination (apex locator reading + radiograph confirmation; final WL per canal). Cleaning and shaping system (rotary or reciprocating brand and sequence; hand-file finish; apical size). Irrigation protocol (sodium hypochlorite concentration; EDTA; chlorhexidine; final rinse). Intracanal medicament if used (calcium hydroxide; brand). Obturation (technique: warm vertical / lateral condensation / single cone / carrier; sealer brand). Temporary or permanent restoration (Cavit / IRM / glass ionomer / composite core). Post-op imaging if obturated. Patient tolerance. Cross-reference `informed-consent-drafter` — RCT consent must be on file for retreatment and apicoectomy.

**3. Surgical (extractions / surgical exposure / biopsy / soft-tissue procedures)**
- **S:** Chief complaint, prior failed treatment, medical history including ASA classification, anticoagulation status (warfarin / DOAC / antiplatelet), bisphosphonate or denosumab history (MRONJ risk), diabetes A1c, smoking, prior radiation to head and neck, pregnancy / trimester.
- **O:** Tooth-specific findings, root anatomy, proximity to vital structures (sinus floor, IAN canal, mental foramen, lingual nerve), bone quality, soft-tissue findings, pre-op vitals if sedation.
- **A:** Diagnosis with ICD-10 (K02 caries; K04 pulp / periapical; K05 gingivitis / periodontitis; K08 extraction indications). CDT (D7140 erupted; D7210 surgical with elevation of mucoperiosteal flap and removal of bone or section of tooth; D7220–D7240 impacted by impaction class; D7250 root removal; D7270 reimplantation; D7280 surgical access; D7283 placement of device for facilitation of eruption; D7286 incisional biopsy of soft tissue; D7510 incision and drainage; D7953 bone replacement graft; D7960 frenectomy).
- **P:** Anesthesia (as above) plus sedation level if used (minimal / moderate / deep / general — document permit, monitoring per state law: pulse oximetry, capnography for moderate+, blood pressure, ECG when applicable; reversal agents available; recovery-bay assignment). Surgical technique step-by-step (incision design, flap reflection, bone removal site and amount, tooth sectioning, elevation, forceps delivery, socket inspection / curettage, irrigation). Hemostasis (pressure, gelfoam / collagen plug, tranexamic acid rinse if anticoagulated, suture type and count). Specimen handling if biopsy (formalin, lab name, requisition number). Post-op instructions per `post-op-care-instructions` surgical family. Prescriptions (analgesic, antibiotic if indicated per AAE / AHA / surgical-site rationale, antimicrobial rinse). Cross-reference `informed-consent-drafter` — surgical consent on file.

**4. Periodontal (SRP / surgical periodontal / maintenance / soft-tissue grafting)**
- **S:** Chief complaint, periodontal history, smoking, diabetes, OH compliance.
- **O:** Full perio chart (probing depths 6 sites per tooth; recession; mobility 0–III; furcation 0–III; bleeding on probing; plaque score). AAP 2018 stage and grade. Radiographic bone level. Soft-tissue biotype.
- **A:** Diagnosis (gingivitis; stage I/II/III/IV grade A/B/C periodontitis; localized vs. generalized; molar-incisor pattern). ICD-10 (K05.0–K05.6). CDT (D4341/D4342 SRP per quadrant by tooth count; D4346 gingivitis SRP; D4910 periodontal maintenance; D4210/D4211 gingivectomy; D4240/D4241 osseous; D4260/D4261 osseous with regenerative material; D4263 bone replacement graft per site; D4265 biologic materials; D4270/D4273/D4275/D4276 mucogingival; D4283 graft per additional tooth).
- **P:** Anesthesia. Quadrant or sextant treated. Hand and ultrasonic instrumentation. Local-delivery antimicrobial if used (Arestin minocycline; Atridox doxycycline; PerioChip chlorhexidine — site, dose, lot). OH instructions delivered (specific technique). Recall interval recommended per AAP staging. Cross-reference `treatment-plan-explainer` periodontal family for patient-facing companion.

**5. Prosthodontic (fixed and removable: crowns and bridges, dentures, implant-supported)**
- **S:** Chief complaint, prior prosthesis history, esthetic and functional concerns, parafunction (bruxism, clenching), TMD signs.
- **O:** Tooth-specific or arch-specific findings, occlusal scheme, vertical dimension, soft-tissue exam, edentulous-ridge form (if removable).
- **A:** Diagnosis (K02 caries; K05 perio; K08 partial / complete edentulism; K07 occlusal / TMD). CDT (D2710–D2799 single crown; D6240–D6794 fixed partial denture by abutment and pontic; D5110–D5140 complete denture; D5211–D5286 partial denture by material; D6010 surgical placement of implant body; D6056–D6058 abutment; D6065–D6094 implant crown; D6080 implant maintenance; D5810/D5811 interim denture; D5850/D5851 tissue conditioning).
- **P:** Anesthesia (if needed). Tooth preparation (margin design, taper, occlusal reduction in mm). Impression / scan technique (PVS / digital scan brand; bite registration). Shade with photograph / shade-tab record. Provisional fabrication (material, cement). Lab prescription details cross-referenced to `lab-prescription-drafter` (lab name, due date, occlusal scheme, contact instructions, esthetic goals, photograph attachments). Try-in protocol if removable. Cement type for definitive seat. Occlusal verification. Patient tolerance and esthetic acceptance.

**6. Orthodontic (records / banding / adjustment / debond / retention; aligner-tray delivery)**
- **S:** Chief complaint, prior ortho history, parafunction, esthetic and functional concerns, growth status (pediatric).
- **O:** Cephalometric / panoramic / photographic / digital-scan findings. Angle classification. Overjet, overbite, midline. Crowding / spacing in mm. Skeletal vs. dental discrepancy. Eruption status.
- **A:** Diagnosis (K07 dentofacial anomalies; M26 dentofacial dysmorphology). CDT (D8010–D8090 comprehensive ortho by dentition stage; D8210/D8220 removable / fixed appliance; D8660 limited; D8670 periodic visit; D8680 retention; D8681 retainer replacement; D8693 rebond / rebracket; D8695 removal of fixed appliance).
- **P:** Visit-specific work (banding sequence, archwire size and material, IPR sites and amount in mm, attachment placement for aligners, refinement scan, retention type and wear schedule). Patient compliance note (aligner wear hours per day, elastic wear). Next visit interval.

**7. Pediatric (behavior management, sealants, SDF, pulpotomy, stainless-steel crowns, restraint when indicated)**
- **S:** Chief complaint, parent / guardian present, custody decision-maker, prior pediatric dental history, behavior history (Frankl scale), medical history, allergies, premedication on file.
- **O:** Eruption status, dentition age, caries-risk assessment (CAMBRA), behavior at appointment (Frankl 1–4).
- **A:** Diagnosis (K02 caries by tooth and surface; K05 gingivitis; AAPD age-1 visit indications). CDT (D1206 fluoride varnish; D1208 fluoride; D1351 sealant per tooth; D1354 silver diamine fluoride per tooth; D2391–D2394 composite primary; D2929/D2930/D2931 SSC primary / permanent; D3220/D3221 pulpotomy; D9230 nitrous oxide minimum 30 min increments; D9248 non-IV moderate sedation per state law; D9920 behavior management per 15 min increments with documentation).
- **P:** Anesthesia (lidocaine 2% with 1:100,000 epi; articaine 4% with caution under age 4 per AAPD; weight-based maximum dose calculation **required** and documented). Behavior management technique (tell-show-do, distraction, voice control, parent-presence policy, nitrous, oral / IV moderate sedation per state-law permit, protective stabilization with parent consent and documented indication). Restorative or preventive procedure as in adult families. Post-op instructions parent-voice per `post-op-care-instructions` pediatric family.

**8. Hygiene (prophy / SRP / perio maintenance / OCS / fluoride / sealants)**
- **S:** Chief complaint or "Patient presents for scheduled hygiene visit." Medical-history update.
- **O:** Probing depths summary (or reference to perio chart with date), bleeding on probing percentage, plaque / calculus assessment (light / moderate / heavy; supra / sub / both), oral-cancer screening findings (lymph nodes, lips, buccal mucosa, floor of mouth, tongue dorsum / lateral / ventral, palate, tonsillar pillars, oropharynx — explicit findings or "WNL"), perio classification (AAP 2018 stage and grade) at re-eval visits.
- **A:** Diagnosis (gingivitis; periodontitis stage and grade; caries-risk assessment; oral-cancer screening: WNL or specific finding with referral plan). ICD-10 if applicable.
- **P:** Procedure performed (D1110 adult prophy; D1120 child prophy; D4341/D4342 SRP; D4346 gingivitis SRP; D4910 perio maintenance; D1206 fluoride varnish; D1208 fluoride; D1351 sealant; D1354 SDF; D0431 ViziLite / VELscope or similar adjunctive; D9986 missed; D9987 cancelled). OH instructions delivered. Recall interval recommended.

### 7 PMS-Specific Paste-In Layouts

The skill outputs the same SOAP content but reformats for the PMS the practice uses. The core differences:

- **Dentrix (G7+ desktop and Dentrix Ascend cloud):** Procedure-attached note linkage by ADA code; clinical note in the procedure note window with the ADA / CDT code on the first line; clinical exam findings posted to the corresponding chart tab (perio, treatment plan, prescriptions). Use the practice's note-template merge fields if `config.yml` lists them (`{Patient}`, `{Provider}`, `{Date}`, `{Tooth}`, `{Surfaces}`, `{Anesthetic}`, `{Material}`).
- **Eaglesoft:** Clinical examination tab with structured fields (chief complaint, exam findings, treatment, materials); SmartDoc auto-text for repetitive blocks; tooth-charted procedure linkage by Universal number with surface chips. Note-template merge fields default to Eaglesoft's standard set.
- **Open Dental:** Procedure-note linkage by ADA / CDT code; commlog entry for non-procedure communication; auto-note macros (`@anesthesia`, `@material`, `@isolation`); progress note in the procedure window.
- **Curve Dental (cloud):** Clinical-note widget with template selection; procedure-attached SOAP; voice-to-text plugin output paste target.
- **Denticon (Planet DDS cloud):** Clinical-note linkage by procedure; native voice-charting (Planet DDS Clinical Voice+ Suite — AI Voice Perio for periodontal exam, AI Voice Restorative Charting for restorative; native PMS write-back without bridge); paste target is the procedure note window.
- **Carestack (cloud):** Clinical note with structured-field form; procedure-tagged note; voice plugin paste target.
- **Dentrix Ascend (cloud, distinct from Dentrix G7 desktop):** Clinical-note widget with template selection; native Dentrix Ascend Voice integration; paste target is the procedure note window.

For all seven, the skill produces:
1. The cleaned SOAP note as the body
2. The CDT code(s) on the first line if the PMS expects it on the first line, or in a separate field if the PMS expects it in a separate field
3. The ICD-10 codes inline (if requested) or in a separate field
4. Any merge-field placeholders preserved if the practice uses them

### Vendor-Quirk Normalization Map (AI-Scribe Pass-Through)

When the input is an AI-scribe draft, run the vendor-specific normalization pass before paste-in:

- **Pearl Voice / Pearl Second Opinion 3D pre-fill:** Tooth notation often comes through as "tooth 3" or "the upper right first molar" in mixed style — normalize to Universal #3. Surface notation: Pearl tends to spell "mesial-occlusal-distal" — normalize to MOD. Imaging-AI findings prefilled as "carious lesion" should be tagged with the depth (D1 / D2 / D3) if the radiograph supports it.
- **Videa (formerly VideaHealth) Auto-Documentation:** Clinical-Assist findings come pre-attached to procedures; verify the procedure linkage and the tooth number against the chairside chart before paste-in. Auto-coded CDT may need carrier-quirk reconciliation (e.g., D2740 vs. D2750 by carrier downgrade rules — cross-reference `cdt-code-assistant`).
- **Bola AI:** Strong on perio voice charting; verify probing-depth direction (sometimes inverted M-D vs. D-M for posterior teeth). Bleeding-on-probing percentage may need recalculation against the explicit per-site flags.
- **Heidi:** General-purpose ambient scribe; verify dental-vocabulary accuracy (sometimes "amalgam" comes through as "almagam," "endodontic" as "endodontal"). Dental-specific abbreviations (RCT, BWX, FMX, IAN, CBCT) may need expansion or contraction depending on the PMS norm.
- **Denti.AI Voice Perio:** Periodontal-focused; verify the perio-chart export field-by-field; the SRP per-quadrant tooth count drives the CDT code (D4341 ≥4 teeth vs. D4342 1–3 teeth).
- **DentScribe:** Restorative + endodontic strong; verify the obturation technique notation (warm vertical vs. lateral condensation are not interchangeable in the chart even if both succeed clinically) and the rubber-dam isolation documentation (mandatory for endo per ADA).
- **Dentrix Ascend Voice:** Native to Dentrix Ascend; the paste-in is automatic but the SOAP structure should still be verified for required-field anchors per procedure family.
- **Alta AI:** Hygiene + restorative; verify oral-cancer-screening findings are explicit per anatomic site, not just "WNL" without listing the sites.
- **SoapnotesAI:** General SOAP shell; verify that the procedure-family required fields are present (the shell can be too generic).
- **Planet DDS Clinical Voice+ Suite (AI Voice Perio + AI Voice Restorative Charting):** Native to Denticon; same as Dentrix Ascend Voice — paste-in is automatic but verify required-field anchors. Differentiator vs. third-party voice-scribes: native PMS write-back without a bridge or middleware layer.
- **Archy Scribe (launched May 2026):** Native to Archy PMS only. Because Archy Scribe draws directly on the patient's existing chart, treatment history, planned procedures, and medical history, draft notes may incorporate context that the provider did not verbalize during the encounter — verify that all findings in the output were actually observed or discussed, not inferred from prior chart data. SOAP structure and custom template auto-population are strengths; confirm perio-charting depth notation follows the practice's M-D vs. D-M convention. Scribe output is the first in the planned Archy Intelligence agent suite; future Archy Revenue / Verify / Connect outputs will share the same patient-record substrate.

For **all** AI-scribe inputs, run a final hallucination pass: any procedure, material, anesthetic, prescription, or finding that the scribe documented but the provider did not say must be flagged for the provider's review before paste-in. AI scribes are a documentation accelerator, not a clinical decision-maker.

### Audit-Defensibility Checklist (run on every note)

- [ ] **Tooth number** with system declared (Universal default in US; FDI / Palmer if specified)
- [ ] **Surface notation** for restorative (M, O, D, B/F, L; class V / class VI as applicable)
- [ ] **Anesthesia** with agent, volume in cartridges and total mg, injection site, aspiration result, patient response
- [ ] **Materials** with brand or generic class (composite shade with body / enamel / tint; cement type; bonding system; isolation method)
- [ ] **Informed consent** documented as obtained on file (cross-reference `informed-consent-drafter`)
- [ ] **Patient tolerance** and any complications
- [ ] **Diagnosis** with ICD-10 where applicable (K codes for caries / pulp / periapical / perio; M codes for TMD; Z codes for routine encounter)
- [ ] **Procedure code** with CDT (verify carrier-downgrade flags via `cdt-code-assistant`)
- [ ] **Post-op instructions** delivered (verbal **and** written; cross-reference `post-op-care-instructions`)
- [ ] **Prescriptions** with medication, strength, quantity, sig, refills (DEA-controlled flagged per state PDMP requirement)
- [ ] **Next appointment** with what and when
- [ ] **Provider signature** (name, credentials, license number per state requirement, time-stamp if practice protocol or state law requires it)
- [ ] **No subjective or judgmental language** (e.g., "patient was difficult" → "patient declined nitrous; appointment rescheduled at patient request")
- [ ] **Deviations from standard protocol** documented with rationale (e.g., "rubber dam not placed due to gag reflex; cotton-roll isolation with high-volume evacuation; procedure-related risk discussed and consented")
- [ ] **AI-scribe hallucination pass** complete (if input was an AI-scribe draft) — any element not verified by the provider is flagged for review

### Output Requirements

- Properly formatted SOAP note matching the PMS-expected paste-in layout
- Universal tooth numbering (default) with surfaces specified
- Clinically precise terminology — appropriate for chart documentation
- CDT and ICD-10 codes inline or in separate fields per PMS expectation
- Audit-defensibility checklist results appended as a sidecar block clearly marked "Internal — do not paste"
- AI-scribe hallucination pass results appended (if applicable) as a flagged-for-review block
- No PHI in the template itself (the user adds patient identifiers in the PMS)
- Saved to `outputs/clinical-notes/` if the user confirms; audit-mode notes also saved to `outputs/audit-trail/` with the date and provider initials

## Cross-Reference Graph

This skill explicitly chains with:

- **Upstream:** `pre-visit-intake-summary` (medical history, allergies, premedication on file); `morning-huddle-brief` (the patient's risk-tier flags, sedation pre-flight, anticoagulation, MRONJ, A1c that drive the medical-history-update line in S); `informed-consent-drafter` (consent-on-file confirmation); `cdt-code-assistant` (CDT code with carrier-downgrade flag); `clinical-evidence-review` (e.g., PQ-5 CBCT indications justification when CBCT is in O)
- **Sibling:** `lab-prescription-drafter` (the prosthodontic family's lab order); `post-op-care-instructions` (the family-aligned post-op delivered to the patient); `referral-coordination-letter` (when the encounter triggers a specialist referral)
- **Downstream:** `chart-audit-prep` (the audit-defensibility checklist results feed the audit-prep workflow); `pre-auth-narrative-writer` (the diagnosis and clinical findings feed the narrative); `insurance-denial-appeal` (the audit-defensible note is the strongest appeal exhibit); `monthly-practice-kpi-report` (procedure-mix and chair-time aggregation); `meeting-summarizer` end-of-day-debrief pattern (notes-completed line)

## Common Pitfalls To Avoid

- Do **not** invent clinical findings that the provider did not document — if the AI-scribe draft contains a finding that is not in the dictation, flag it for provider review; do not paste it into the chart
- Do **not** use ambiguous tooth notation ("upper right" without a number) or skip surface notation on restorative
- Do **not** omit the rubber-dam-isolation documentation for endodontic procedures — it is required per ADA / AAE standard of care
- Do **not** skip the weight-based anesthetic-dose calculation for pediatric — it is required per AAPD
- Do **not** paste a PMS-generic SOAP into a PMS that expects a structured-field layout (Eaglesoft, Carestack) — use the PMS-specific paste-in
- Do **not** use subjective or judgmental patient-behavior language; describe behavior factually
- Do **not** carry forward boilerplate from a prior visit without verifying it still applies (e.g., "patient tolerated procedure well" does not belong on a visit where the patient did not tolerate it)
- Do **not** include PHI in this skill's template output — the user adds it in the PMS
- Do **not** declare a note complete without running the audit-defensibility checklist
- Do **not** assume the AI-scribe draft is clinically accurate — always run the vendor-specific normalization pass and the hallucination pass before paste-in
- Do **not** use a different tooth-numbering system from the practice's PMS default — switching mid-chart is a known source of audit and peer-review confusion

## Example Output

*Worked example — the input branch that most distinguishes this skill and most often goes wrong: an **AI-scribe draft** that must be vocabulary-normalized, checked for the mandatory rubber-dam line, run through the hallucination pass, and reformatted to the practice's PMS paste-in. Grounded in `config.example.yml` (Cherry Creek Family Dental; Dr. Anna Patel; PMS = **Dentrix Ascend**; ambient voice = **Dentrix Ascend Voice**; Universal numbering).*

**Input the user gave (raw Dentrix Ascend Voice scribe draft, verbatim):**
> "Patient in pain upper right. Tooth 3 has mesial occlusal distal decay into the pulp. Diagnosis irreversible pulpitis. Started root canal, opened access, working length established, calcium hydroxide placed, closed with temporary. Prescribed amoxicillin. Patient tolerated well."

**Skill's read:** procedure family = **endodontic**; PMS target = **Dentrix Ascend** (CDT on first line, procedure-attached note); input is an **AI-scribe draft** → run vendor normalization + hallucination pass. Two problems the raw draft has: (1) **no rubber-dam documentation** — mandatory for endo per ADA/AAE; (2) the scribe added an **antibiotic** the provider may or may not have actually indicated for irreversible pulpitis (a classic scribe over-capture and an antibiotic-stewardship flag) → hallucination-flag for provider confirmation, do not paste as fact.

---

> **D3330 — RCT, molar** *(CDT on first line, Dentrix Ascend procedure-attached)*
>
> **S:** Patient presents with pain, upper right quadrant. Reports the chief complaint prompting today's endodontic treatment on tooth **#3**. Medical history / allergies / medications verified per chart.
> **O:** Tooth **#3** with **MOD** caries extending into the pulp. Diagnosis consistent with pulpal involvement (vitality/percussion findings per chart).
> **A:** Symptomatic **irreversible pulpitis, #3**. ICD-10 **K04.0** (pulpitis). CDT **D3330** (endodontic therapy, molar).
> **P:** Anesthesia administered [agent / cartridges / site / aspiration / response — *confirm, not in scribe draft*]. **Rubber-dam isolation placed** [⚠️ **REQUIRED per ADA/AAE — confirm placed; scribe draft omitted it**]. Access opening established through the crown. Working length established [apex-locator + radiographic confirmation — *confirm method*]. Canals medicated with **calcium hydroxide**. Access **temporized** (temporary restoration). Patient tolerated the procedure well. Post-op instructions delivered per `post-op-care-instructions` (endodontic family).
>
> **— Internal — do not paste — Audit-Defensibility Checklist —**
> - [x] Tooth # with system (Universal #3)  · [x] Diagnosis + ICD-10 (K04.0)  · [x] CDT (D3330)
> - [ ] **Anesthesia** agent/volume/site/aspiration — *missing from scribe draft; provider to add*
> - [ ] **Rubber-dam isolation** — *missing; MANDATORY for endo; provider to confirm & document*
> - [ ] Working-length **method** (apex locator + radiograph) — *confirm*
> - [x] Intracanal medicament (CaOH) · [x] Temporary placed · [x] Patient tolerance
> - [ ] Informed consent on file (`informed-consent-drafter`) — *confirm*
>
> **— Flagged for provider review (AI-scribe hallucination / over-capture pass) —**
> - ⚠️ **Amoxicillin / antibiotic:** scribe documented a prescription. Antibiotics are **not** indicated for irreversible pulpitis without systemic involvement (fever, swelling, cellulitis) per AAE stewardship guidance. **Confirm the Rx was actually intended and clinically justified** before it enters the chart; if not said/indicated, remove. (Cross-check `cdt-code-assistant` if a code changes.)

---

**Most common failure mode this example guards against:** pasting an AI-scribe draft straight into the chart as if it were verified. Two things a scribe reliably gets wrong on endo notes: it **drops the rubber-dam line** (mandatory standard-of-care documentation — its absence is an audit and medicolegal liability), and it **over-captures medications or findings the provider never confirmed** (here, an antibiotic for irreversible pulpitis). The skill never silently fixes or silently omits these — it surfaces both as explicit provider-review flags. AI scribes are a documentation accelerator, not a clinical decision-maker.

## Version History

- **v3.1 (2026-07-06)** — Added a worked Example Output on the AI-scribe-draft input branch (Dentrix Ascend Voice → Dentrix Ascend paste-in), grounded in `config.example.yml`. Demonstrates vendor normalization, the mandatory rubber-dam-isolation flag, the audit-defensibility checklist, and the hallucination/over-capture pass (antibiotic-for-irreversible-pulpitis stewardship flag) — plus a most-common-failure-mode callout. Additive only; no instruction prose removed. `last_eval_score` populated.
- **v3.0** — 8 procedure-family templates, 7 PMS-specific paste-in layouts, ambient-voice vendor pass-through normalization map, audit-defensibility checklist.
