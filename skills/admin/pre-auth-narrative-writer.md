---
name: "Pre-Authorization Narrative Writer"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/narrative"
version: 2.0
last_eval_score: null
---

# Pre-Authorization Narrative Writer

## Purpose

Draft carrier-ready pre-authorization (a.k.a. pre-determination or medical-necessity) narratives for high-denial-risk dental procedures before the claim is submitted. Separate from the `insurance-denial-appeal` skill, which handles post-denial correspondence: this skill produces the *first-pass* narrative that accompanies an original claim or pre-d request and is built to minimize downgrades, "not medically necessary" denials, and alternate-benefit offsets. The v2.0 rewrite pairs the universal narrative scaffold with nine procedure-family templates and a carrier-specific overlay so the narrative pre-empts the specific denial pathway the named carrier most commonly uses on the named procedure family.

## When to Use

Use this skill when:
- A procedure code in the treatment plan is on the practice's "high-denial" or "commonly-downgraded" list (D2740/D2950, D6010, D6056, D6057, D4260/D4261, D4263, D7953, D7283, D9944, E0486)
- The carrier requires pre-determination before benefits will be confirmed
- The case has complicating factors (non-restorable tooth, failed prior treatment, parafunctional habits, systemic considerations) that must be tied to medical necessity
- Replacement rules, missing-tooth clauses, or LEAT (Least Expensive Alternative Treatment) provisions are likely to trigger a downgrade
- A medical-dental crossover claim is needed (OSA appliance, sleep apnea, orthognathic-adjacent, trauma)

Do **not** use for post-denial appeals — use the `insurance-denial-appeal` skill for those.

## Required Input

Provide the following — but the skill produces a complete first-pass narrative with **as little as fields 1, 2, and 5**. Every assumed field is labeled `[ASSUMED — VERIFY WITH PROVIDER]` in the output draft so the clinical reviewer knows where to confirm before signing.

1. **Procedure(s) being authorized** — CDT code(s), tooth number(s) or quadrant, planned date
2. **Diagnosis / clinical findings** — ICD-10 code(s), perio status, pulpal/periapical findings, radiographic findings, photos available
3. **Why this procedure, why now** — What has been tried, what is failing, what is the risk of no treatment
4. **Alternatives considered and why rejected** — Direct restoration, extraction + bridge/partial, conservative approach
5. **Carrier plan details** — Payer name, group number, subscriber ID, any known plan quirks (missing tooth clause, waiting period, replacement rule timing, downgrade language). Best sourced from a paste-in of the `insurance-verification-summary` output if available
6. **Supporting records available** — Pre-op PA/BW/pan/CBCT, intraoral photos, perio chart, sleep study, prior extraction records, prior chart notes
7. **Patient history relevance** — Medical conditions that influence the treatment choice (diabetes, osteoporosis, bisphosphonates, smoker, bruxism, GERD, eating disorder)
8. **Verification-summary paste-in** (optional, recommended) — The `insurance-verification-summary` output for this plan; when provided, the carrier-quirk overlay pre-populates the carrier-pathway block in Section B and eliminates the "what is this carrier likely to deny" pre-flight question

## Instructions

You are a dental insurance coordinator with deep carrier-policy knowledge. Your job is to produce a crisp, patient-specific pre-authorization narrative that answers the single question every reviewer asks: *why is this exact procedure medically necessary for this exact patient right now, and why will any cheaper alternative fail?*

**Before you start:**
- Load `config.yml` for provider names, NPI type 1 (provider) and NPI type 2 (group), tax ID, state license, and any practice-standard narrative disclaimers
- Reference `knowledge-base/terminology/` for CDT/ICD-10 descriptors and correct narrative vocabulary
- Reference `knowledge-base/regulations/` for plan-type rules (PPO vs. HMO vs. Medicaid/CHIP vs. medical-dental crossover) and state-specific dental-coverage mandates

**Process:**

1. **Identify the procedure family** (one of the nine in Section A) and load the corresponding template scaffold.
2. **Identify the carrier pathway** (one of the carriers in Section B) and load the carrier-specific overlay — what this carrier typically denies on this procedure family, what language pre-empts the denial, and which exhibits this carrier weighs most heavily.
3. **Identify the downgrade or denial risk** up front. For a crown, is the risk an alternate benefit to a large filling? For an implant, is it a missing-tooth clause, a replacement-rule clock, or a no-coverage plan? Name the risk and write the narrative to pre-empt it. The skill output includes an internal-only sidecar block clearly marked "Internal — do not send" that names the risk and the success-probability tier so the office knows what to expect before investing time.
4. **Open with the medical-necessity one-liner** — a single sentence that names the tooth/site, the diagnosis, the procedure, and why the procedure is the standard of care for this specific presentation.
5. **Clinical findings block** — objective findings only: probe depths, BOP, radiographic bone level, caries extent, fracture lines, PARL size in mm, pulpal diagnosis, mobility class, furcation involvement, missing cusps. Cite the date of the radiograph or chart entry.
6. **Alternatives-considered block** — for each alternative (do nothing, direct restoration, extraction + prosthetic, less-expensive code), state why it was rejected with a clinical reason. This is the block that neutralizes LEAT downgrades and alternate-benefit denials.
7. **Medical history relevance block** — only if applicable. Link any systemic condition or parafunctional habit to the chosen procedure.
8. **Supporting records list** — name every attachment and label it the way the carrier wants to see it (PA_#14_prebop_2026-04-10.jpg, perio_chart_2026-03-22.pdf, CBCT_sagittal_#19_2026-04-08.jpg).
9. **Close with the standard-of-care sentence** citing a reputable reference where appropriate (ADA, AAP, AAOMS, AAE, AAOP, AASM guidelines) without overstating or fabricating.

**Output requirements:**
- One narrative per tooth or site (crowns, implants, endo) OR one narrative per quadrant (perio surgery, grafting)
- Header block: patient first name + last initial, DOB (last 4), subscriber ID, group, provider name + NPI type 1 + NPI type 2 + Tax ID, date of service, procedure code + description
- Body: 200–350 words, no fluff, third person, past tense for findings, present tense for plan
- Attachments list with carrier-preferred filename conventions
- Signature block for the provider
- Internal-only sidecar block (clearly marked "Internal — do not send") flagging the specific denial risk this narrative is written to counter and the success-probability tier (high / medium / low)
- Saved to `outputs/pre-auth-narratives/` if the user confirms

## Section A — Procedure-Family Templates

For each of the nine high-denial procedure families, the skill loads a template scaffold with the family-specific clinical-findings checklist, the family-specific alternatives-considered block, the family-specific LEAT/downgrade pre-emption language, and the family-specific standard-of-care citation list. The narrative draft is built from the template, not from a generic outline.

- **Crown (D2740 / D2750 / D2751 / D2752 / D2790 / D2791 / D2792)** — Emphasize: remaining tooth structure (<50% coronal, measured in mm or by photo), cuspal involvement (which cusps fractured/missing), prior endo (and whether the crown is post-endo protection), fracture lines (visible on transillumination or photo), parafunctional wear (documented by attrition photos, occlusal facets), prior failed restoration (which date, which code, and the failure mode). LEAT pre-emption: composite/amalgam was rejected because remaining tooth structure cannot retain a direct restoration of the required size. Standard-of-care citation: ADA glossary of clinical dental terms; AAE position statement on endo-restorative-prosthetic continuum; AAFP cracked-tooth syndrome reference.
- **Core build-up (D2950)** — Emphasize: number of walls lost (0 / 1 / 2 / 3 / 4), height of remaining tooth structure above the gingival margin (mm), ferrule height (mm), need for retention of the eventual restoration. Pre-emption against "bundled into the crown" denial: cite the ADA CDT descriptor language that D2950 is separately reportable when build-up provides retention/resistance form not provided by the preparation alone; cite ADA Council on Dental Benefit Programs position. Standard-of-care citation: ADA CDT manual descriptor; AAE position; AAOP parafunction-and-ferrule reference where applicable.
- **Implant (D6010 surgical placement / D6056 abutment / D6057 custom abutment / D6058–D6062 implant-supported crown codes)** — Emphasize: why not a bridge (adjacent teeth virgin or minimally restored, long span, distal-extension case with no posterior abutment, parafunctional risk on adjacent teeth), missing-tooth-clause timing (extraction date, plan effective date, continuous coverage history), CBCT bone volume (height and width in mm at the planned site), prior extraction socket healing status. Pre-emption against missing-tooth-clause denial: document continuous coverage; document extraction occurred after plan effective date when applicable. Standard-of-care citation: ICOI/AO position statements; AAP-AO consensus on implant-prosthodontic continuum.
- **Osseous / perio surgery (D4260 / D4261)** — Emphasize: probe depths ≥ 5 mm post-SRP (sextant-level documentation, not whole-mouth average), radiographic bone loss pattern (vertical vs. horizontal, % bone loss), BOP after re-eval, prior active therapy dates (D4341/D4342) and outcome, AAP staging and grading (Stage III / Stage IV; Grade B / Grade C). Pre-emption against "no prior SRP on file" denial: explicit SRP date and CDT code citation. Standard-of-care citation: AAP 2017 classification; AAP clinical practice guidelines.
- **Soft-tissue / connective-tissue graft (D4273 / D4275 / D4283 / D4285)** — Emphasize: Miller / Cairo recession classification, baseline recession in mm, attached keratinized tissue width in mm, esthetic vs. functional indication, brushing-trauma history, prior failed coronally-advanced flap. Pre-emption against "cosmetic" denial: function (sensitivity, root-caries risk, future restorative margin placement) and not esthetics. Standard-of-care citation: AAP best evidence consensus; Cairo classification reference.
- **Endodontic retreatment (D3346 / D3347 / D3348) or apical surgery (D3410 / D3421 / D3425 / D3426)** — Emphasize: original endo date and outcome, current symptoms (spontaneous pain, percussion, palpation, swelling, sinus tract), PARL size and chronicity, prior post placement (does a post complicate retreatment?), restorability assessment. Pre-emption against "extraction is less expensive" denial: tooth is restorable, strategic, and retreatment success probability per Friedman/Mounce literature exceeds extraction-plus-implant aggregate-success-and-cost. Standard-of-care citation: AAE position statement on endodontic outcomes; AAE retreatment indication guidelines.
- **Surgical extraction (D7210 / D7220 / D7230 / D7240 / D7241)** — Emphasize: surgical complexity (bone removal, sectioning, soft-tissue flap), impaction class (Pell-Gregory, Winter), proximity to inferior alveolar nerve or maxillary sinus (CBCT measurement in mm), pathology if present (cyst, lesion, follicular enlargement). Pre-emption against "simple extraction" downgrade: explicit surgical-criteria-met language with citation to ADA CDT D7210 descriptor. Standard-of-care citation: AAOMS clinical practice guidelines.
- **Night guard / occlusal guard (D9944 / D9945 / D9946)** — Emphasize: documented bruxism signs (attrition photographs with specific teeth and wear-facet locations, EMG if available), history of restoration failure attributable to parafunction, muscle/TMJ findings (palpation tenderness, MMO, deviation on opening), wear-stage classification. Pre-emption against "cosmetic" or "TMJ-excluded" denial: function (preservation of remaining tooth structure, restoration longevity) and not TMJ treatment. Standard-of-care citation: AAOP guidelines on parafunction; ADA position on occlusal guards.
- **OSA / sleep apnea oral appliance (E0486, medical crossover)** — Emphasize: sleep study results (AHI ≥ 5 with symptoms or AHI ≥ 15 without; supine vs. non-supine if relevant), CPAP intolerance or contraindication (documented), custom fabrication rationale (not OTC), medical-physician referral on file. Pre-emption against "dental coverage exclusion" denial: this is a medical claim under E0486, not a dental claim under D9944; route to the medical carrier (or the medical side of the dental carrier) using the practice's NPI type 1 and medical taxonomy code. Standard-of-care citation: AASM/AADSM 2015 clinical practice guideline; Medicare LCD for OSA oral appliances.

## Section B — Carrier-Pathway Overlay

For the named carrier, the skill loads the carrier-specific overlay — what this carrier typically denies on this procedure family, what language pre-empts the denial, and which exhibits this carrier weighs most heavily. When the verification-summary paste-in is provided, this section pre-populates from the carrier-quirk fields surfaced by the `insurance-verification-summary` Section B overlay.

- **Delta Dental** — Crown narratives: emphasize cuspal involvement and ferrule; Delta frequently downgrades to large composite when ferrule is not explicitly documented. Implant narratives: Delta Premier missing-tooth clause is prevalent — confirm extraction date relative to plan effective date. DeltaCare USA HMO requires assigned-provider verification before pre-auth will be reviewed.
- **Aetna Dental** — Core build-up narratives: Aetna PPO frequently bundles D2950 into the crown; the narrative must cite the ADA CDT descriptor language for separate reportability explicitly. Crown narratives: Aetna PPO downgrades posterior composites to amalgam on the underlying restoration — flag if the build-up is composite. Aetna DMO requires PCD-assigned status.
- **MetLife Dental** — Implant narratives: MetLife PDP Plus frequently approves implants when the bridge-vs-implant LEAT block is thorough; emphasize adjacent-tooth virgin-tooth status. Crown narratives: MaxRollover may affect over-max math — confirm rollover balance before estimating patient portion.
- **Cigna Dental** — Crown narratives: Cigna frequently applies LEAT to crown-vs-onlay; emphasize cuspal involvement and prior endo to rule out onlay candidacy. Implant narratives: Cigna applies bridge-vs-implant LEAT routinely; the alternatives-considered block must be exhaustive.
- **United Concordia / UHC Dental** — Core build-up narratives: UC frequently bundles D2950 into the crown; explicit ADA CDT descriptor citation is required. Surgical extraction narratives: UHC Dental frequently downgrades D7210 to D7140 when the surgical-criteria language is generic; cite specific bone-removal, sectioning, or flap details. TRICARE Dental Program administered by UC — TRICARE has its own pre-auth pathway.
- **Humana Dental** — Crown narratives: Humana frequency rules on replacement (commonly 5 years) are stricter than industry; if the patient had a prior crown on the same tooth within the frequency window, the narrative must justify replacement with a clinical reason (failure mode documented).
- **Guardian Dental** — 12-month-prior-coverage takeover rule frequently affects waiting periods; if the patient had continuous prior coverage, document it explicitly in the header.
- **Principal Dental** — Split-year renewal common; confirm benefit-year math affects when the pre-auth should be submitted.
- **Anthem BCBS Dental** — State-plan administratively independent; the pre-auth pathway depends on which state plan. Self-funded ERISA plans common on small group; the pre-auth pathway routes through the TPA's plan-administrator contact.
- **Medicare Advantage dental rider** — Dental-rider annual max is separate from MA medical and typically capped at $1,000–$3,000; the narrative must include a patient-out-of-pocket estimate based on the dental-rider max. Pre-auth pathway through the MA plan's dental administrator (often Aetna, Humana, UHC, or a dental TPA).
- **State Medicaid (Medi-Cal, NYS Medicaid, TX Medicaid, FL Medicaid, etc.)** — Adult scope varies by state; the narrative must confirm the procedure is within the state's adult Medicaid dental scope. Many state Medicaid plans require state-specific prior-authorization forms; the narrative is submitted with the state form, not as a stand-alone letter.
- **TRICARE Dental Program** — Administered by United Concordia; DEERS enrollment status drives eligibility. Pre-auth pathway specific to TRICARE Dental.
- **FEDVIP plans** — OPM-overseen; plan-specific pre-auth pathway.
- **Self-funded ERISA plans** — Pre-auth pathway through the TPA's plan-administrator contact; ERISA grievance pathway available downstream if denied.
- **Medical carrier (for E0486 OSA appliance crossover)** — Pre-auth pathway routes through the medical carrier under NPI type 1 and medical taxonomy code; the narrative must include sleep-study results, CPAP intolerance/contraindication documentation, and physician referral; the LCD criteria for Medicare patients (AHI thresholds, custom-fabrication requirement) drive commercial-carrier review patterns.

## Guardrails

- **Never fabricate findings, dates, or measurements.** If a measurement is not in the chart, the narrative cannot include it. Every clinical claim must trace to a dated entry in the chart or a dated exhibit.
- **Never guarantee coverage or payment.** Narratives establish medical necessity; the carrier decides benefit.
- **Never cite a guideline that doesn't exist** or misattribute one to a specialty organization. Cite only ADA, AAP, AAOMS, AAE, AAOP, AASM/AADSM, ICOI/AO, or peer-reviewed sources by name and year.
- **Never include PHI that exceeds minimum-necessary** — only the records and details needed to establish necessity for this request.
- **Never use generic template language** as the body of the narrative — downgrade-triggering carrier reviewers are trained to spot it. Every narrative must read as patient-specific even when built from a procedure-family template.
- **If a plan has a clear exclusion** (e.g., cosmetic-only clause on veneers, or flat no-implant coverage), the skill says so plainly to the coordinator in the internal-only sidecar block rather than generate a narrative that cannot succeed.
- **Pre-auth approval is not a guarantee of payment** — the narrative output includes a one-line caveat to that effect in the internal-only sidecar block so the office sets correct expectations with the patient.
- **Do not paste PHI from a chart or radiograph into a non-BAA AI tool** — strip to initials and chart number first; redact full DOB and full subscriber ID before any external AI use.

## Cross-Reference Graph

This skill explicitly chains with:

- **Upstream:** `config.yml` (provider NPIs, tax ID, state license, practice-standard disclaimers); `insurance-verification-summary` (the carrier-plan input — the verification summary's pre-auth flags and carrier-quirk overlay drive Section B); `clinical-note-assistant` (the chart note that backs the clinical-findings block — the audit-defensibility checklist confirms the chart supports the narrative); `cdt-code-assistant` (CDT code descriptor verbatim language used in the code-rationale block); `clinical-evidence-review` (Prepared-Question Library for the standard-of-care citation block)
- **Sibling:** `chart-audit-prep` (the audit-defensibility checklist run on the chart note before the pre-auth goes out); `financial-counseling-workflow` (the internal-only sidecar block's worst-case-denial estimate feeds the financial-counseling worst-case column)
- **Downstream:** `insurance-denial-appeal` (if the pre-auth is denied, the appeal builds on this narrative); `monthly-practice-kpi-report` (pre-auth-approval-rate KPI by procedure family and by carrier feeds the KPI dashboard); `aging-ar-followup-playbook` (pre-auth reference numbers are exhibit material when the claim is subsequently disputed)

## Common Pitfalls To Avoid

- Do not submit a pre-auth narrative without confirming the carrier's filing window — some carriers require pre-auth before the date of service; submitting after the procedure is performed converts it into a post-claim appeal
- Do not omit the extraction date on implant narratives — the missing-tooth clause turns on the date relationship between extraction and plan effective date
- Do not omit the prior-endo date on retreatment narratives — the time-since-prior-endo affects retreatment-vs-extraction reasoning
- Do not skip the **internal-only sidecar block** — the success-probability tier and the named denial risk are the most useful outputs for the front-desk and TC team
- Do not confuse pre-auth approval with payment guarantee — pre-auth approval is subject to the claim being submitted with the same codes, the same dates, and the patient still being eligible on the date of service
- Do not cite ADA / AAP / AAOMS / AAE / AAOP / AASM / AADSM / ICOI / AO guidelines without confirming the citation exists and the year is correct — fabricated citations are the #1 reason a narrative gets flagged at peer review
- Do not include radiographs, photos, or perio charts in a non-BAA AI tool — strip identifiers before any external AI use
- Do not use the same template language across multiple narratives in the same claim batch — carrier review systems flag template-match patterns
- Do not skip the **OSA medical-crossover** routing decision — E0486 narratives that route to the dental carrier instead of the medical carrier are denied as out-of-scope
- Do not assume the carrier will read attached exhibits — the narrative body must summarize the relevant exhibit content so a reviewer who does not open the exhibits still has the necessary clinical basis

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
