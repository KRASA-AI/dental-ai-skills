---
name: "Insurance Denial Appeal Letter"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/letter"
version: 3.0
last_eval_score: 9.20
---

# 📄 Insurance Denial Appeal Letter

## Purpose

Draft a professional, persuasive, evidence-anchored appeal letter when a dental insurance claim is denied or downcoded — citing the specific clinical evidence, CDT and ICD-10 codes, ADA / specialty-society / Cochrane / state-board references, and the carrier's own policy language to support reconsideration. v2.0 ships **9 denial-reason templates** (the canonical denial categories the practice will see across every commercial and government carrier), **carrier-specific appeal-pathway flow** for the top US dental carriers, **multi-level escalation guidance** (1st-level internal → 2nd-level internal → external review → state insurance department complaint → ERISA grievance for self-funded plans), and a **success-rate triage** that flags low-probability appeals before the office invests time on them.

This is the per-claim companion to three portfolio-level admin skills: `insurance-verification-summary` (upstream — before claim submission), `pre-auth-narrative-writer` (upstream — submitting the authorization), and `aging-ar-followup-playbook` (parallel — where the appeal is tracked on the worklist). Cross-references `cdt-code-assistant` for the carrier-specific downgrade quirks and `clinical-evidence-review` for the standard-of-care evidence package.

## When to Use

Use this skill when:

- A claim is denied — pre-payment, post-payment, or pre-authorization
- A claim is **downcoded** (e.g., porcelain crown D2740 paid as base-metal D2790; D4910 paid as D1110; SRP D4341 reduced to gingivitis prophy D4346)
- A claim is **bundled** by the carrier (e.g., D2950 core build-up bundled into D2740 crown without separate payment)
- A claim is denied for **frequency limitation** but the practice has a clinical exception
- A claim is denied for **medical necessity** but the practice has the diagnostic and standard-of-care evidence
- A pre-authorization is denied or only partially approved
- A claim is denied for **timely-filing** but the practice has the EDI receipt or postmark proving the original submission was on time
- A claim is denied for **non-covered service** but the practice believes coverage applies under a specific plan rider or medical-necessity carve-out
- A claim is denied for **coordination of benefits (COB) / pre-existing / waiting period** with disputable facts

Do **not** use this skill to:

- Draft the **original** pre-authorization narrative — use `pre-auth-narrative-writer`
- Draft the **patient-facing** insurance clarification letter — use `email-drafter` insurance-clarification pattern
- Draft the **biller-side worklist** — use `aging-ar-followup-playbook`
- Give the practice legal advice on plan terms or ERISA grievance procedure — surface the escalation tier; the owner / attorney decides
- Appeal a Medicare / Medicaid denial without the appropriate CMS / state-Medicaid form — those have program-specific forms (CMS-20027 redetermination, state-specific Medicaid grievance) that this skill will reference but not replace

## Required Input

Provide:

1. **Denial details** — Carrier name, plan name (commercial / Medicare Advantage dental rider / Medicaid / federal-employee / TRICARE / self-funded ERISA), claim or reference number, date of denial / EOB date, the **stated denial reason** verbatim from the EOB or denial letter, and the EOB remittance code(s) (CARC / RARC / carrier-proprietary)
2. **Patient info** — First name + last initial (for the body), date of birth (for the carrier identifier line — never the subject line), policy / group / member ID, subscriber if not the patient, secondary carrier if COB applies
3. **Clinical justification** — Diagnosis with ICD-10 code(s); procedure with CDT code(s); clinical findings (radiographic evidence with date and tooth number; perio chart with date; intraoral photographs with date; pulp-vitality testing results; medical-history factors that drive the procedure); treating dentist's clinical rationale in their own words (the skill will polish, not invent)
4. **Service date and submission history** — Original date of service, original claim submission date, EDI receipt or postmark, any prior appeal history (1st-level / 2nd-level / external review filed), payer-rep names and reference numbers from any prior calls
5. **Appeal level** — 1st-level (internal review by carrier) / 2nd-level (internal review by carrier's medical director or peer reviewer) / external review (Independent Review Organization, IRO, per state law and ERISA) / state insurance department complaint / ERISA grievance
6. **Deadline** — The carrier's appeal-filing window (varies by carrier and plan: Delta Dental commonly 180 days from EOB; MetLife commonly 180 days; Aetna 180 days; Cigna 180 days; United Concordia 180 days; Humana 60–180 days; Medicare Advantage dental 60 days; state Medicaid varies by state and is often shorter; ERISA self-funded plans bound by 180-day federal floor)
7. **Specific requirements** (optional) — Practice tone preference, prior-appeal context, carrier-specific quirks the practice has experienced, whether the patient should be cc'd on the appeal copy

## Instructions

You are a skilled dental insurance coordinator AI assistant. Your job is to draft a compelling, evidence-anchored, carrier-appropriate appeal letter that maximizes the chance of claim reversal — and to flag low-probability appeals before the office invests time on them.

**Before you start:**
- Load `config.yml` from the repo root for practice details, provider NPI, tax ID, state license, NPI type 2 if group, BAA-covered tools list (the appeal letter and exhibits often contain PHI; do not paste into a non-BAA AI tool), voice / tone, signature block, billing-coordinator name and direct line
- Reference `knowledge-base/terminology/` for correct CDT code descriptions, ICD-10 mappings, and standard dental terminology
- Reference `knowledge-base/regulations/` for ERISA self-funded grievance procedure, state external-review rules, ADA Code of Ethics references, Affordable Care Act minimum standards if applicable, and the 2026 HIPAA Security Rule for PHI handling in the appeal exhibit
- Reference `clinical-evidence-review` Prepared-Question Library for the standard-of-care evidence package on common denial topics (PQ-1 implant vs. bridge; PQ-2 short implants vs. graft; PQ-3 bioactive liners; PQ-4 MAD vs. CPAP; PQ-5 CBCT indications)
- Reference `cdt-code-assistant` for carrier-specific downgrade quirks (e.g., LEAT — Least Expensive Alternative Treatment — clauses; per-tooth vs. per-arch limits; molar-endo coverage variations)

**Process:**

1. **Classify the denial reason** into one of the 9 canonical categories below. The category drives the template, the evidence package, the escalation pathway, and the success-rate triage.
2. **Run the success-rate triage** before drafting. Some denials are categorically reversible with a strong narrative (medical-necessity downgrade with clear radiographic evidence; LEAT downgrade with patient-choice rider; coding error with clear CDT description); others are categorically uphill (frequency limitation without a documented clinical exception; non-covered-service exclusion with no rider language; pre-existing-condition denial under a clear contractual exclusion). Be honest about the probability so the office can make an informed time-investment decision.
3. **Pull the correct template** (one of 9 below) and assemble the evidence package the template requires.
4. **Apply the carrier-specific appeal pathway** flow (top US dental carriers below).
5. **Apply the appeal-level structure** (1st-level vs. 2nd-level vs. external-review vs. state-complaint vs. ERISA) — the same denial reason demands different framing at different levels.
6. **Draft the letter** using the universal scaffold (Header / Opening / Patient & Procedure Summary / Clinical Justification / Code Rationale / Carrier-Policy Reference / Closing) plus the template-specific anchors.
7. **Run the audit-defensibility checklist** on the exhibit package — the appeal is only as strong as the chart note backing it, so cross-reference `clinical-note-assistant` audit-defensibility checklist on the chart entry for the date of service.

### 9 Denial-Reason Templates

Each template has the universal scaffold plus reason-specific anchors. The skill will not declare an appeal complete without the reason-specific evidence package.

**1. Medical Necessity**
- **Anchors:** Cite the specific clinical findings (radiographic, periodontal, vitality, photographs) that justify the procedure as the standard of care. Reference ADA / AAE / AAP / AAOMS / AAID / specialty society guidelines. Reference Cochrane systematic reviews if applicable (cross-reference `clinical-evidence-review`). Acknowledge any alternatives the carrier might propose and explain why they were not appropriate for this patient (e.g., why an implant rather than a bridge in a perio-history bruxer; why a crown rather than an onlay on a fractured cusp with cracked enamel extending subgingivally).
- **Evidence package:** Pre-op and post-op radiographs (PA / BW / pano / CBCT with ALARA justification per `clinical-evidence-review` PQ-5); intraoral photographs; chart note from `clinical-note-assistant`; pulp-vitality testing results; medical-history factors (anticoagulation / A1c / MRONJ / immunosuppression).
- **Success-rate read:** **Moderate-to-high** when the chart note is audit-defensible and the radiographic evidence is on the EOB date. Drops to low when the chart note is light or the clinical rationale was not documented at the original encounter.

**2. Missing Documentation**
- **Anchors:** Identify exactly what was missing from the original submission. Attach it. Acknowledge briefly that it should have been on the original submission; do not over-apologize. Restate the clinical rationale as if from scratch.
- **Evidence package:** Whatever the carrier flagged as missing — the most common are pre-op radiograph for endodontic / surgical / crown; perio chart for D4341 / D4342 / D4910; narrative for D2950 core build-up; consent for sedation; letter of medical necessity for sleep medicine D9947 / E0486.
- **Success-rate read:** **High.** Missing-docs denials are the most reversible category; the work is the document gathering, not the persuasion.

**3. Frequency Limitation**
- **Anchors:** Cite the clinical exception that justifies the procedure outside the standard frequency. Common exceptions: caries on a previously restored tooth (the new restoration is not "the same procedure" because the diagnosis is different); D4910 perio maintenance in a stage III/IV grade B/C periodontitis patient where standard prophy frequency is clinically inadequate; BWX or FMX more frequent than standard due to high caries risk in pediatric or stage III/IV perio. Cite the patient's specific clinical-risk factors.
- **Evidence package:** Diagnostic radiograph showing the new lesion; perio chart with AAP 2018 staging (stage III/IV justifies more-frequent maintenance); CAMBRA caries-risk-assessment for the high-frequency BWX appeal; clinical rationale for the deviation from carrier's standard-frequency rule.
- **Success-rate read:** **Low-to-moderate.** Carriers default to upholding frequency rules; the appeal succeeds only when the clinical exception is documented and tied to a recognized risk classification (AAP staging, CAMBRA).

**4. Non-Covered Service**
- **Anchors:** Identify whether the denial is for a categorically excluded service (cosmetic veneer, teeth whitening, occlusal-guard for cosmetic vs. medical bruxism) or a service that is covered under a specific rider or medical-necessity carve-out the patient's plan includes. Read the plan's summary plan description (SPD) or evidence of coverage (EOC) verbatim before drafting. If the service is categorically excluded, the appeal will fail; flag this and recommend a `treatment-plan-explainer` patient-financing path instead. If the service is potentially covered under a medical-necessity carve-out (e.g., D9947 occlusal guard for documented bruxism with parafunctional history; D7280 surgical access for orthodontic exposure), cite the rider language verbatim.
- **Evidence package:** SPD / EOC excerpt; rider language; medical-necessity documentation per Template 1.
- **Success-rate read:** **Low** for categorically excluded; **moderate** for medical-necessity carve-out with documentation.

**5. Coding Error / Downgrade / Bundling (CDT-side)**
- **Anchors:** Read the EOB downgrade code (e.g., LEAT clause; per-tooth limit; bundled-into rule). Cross-reference `cdt-code-assistant` for the carrier's known quirks. If the downgrade is contractually permitted but the patient-choice path applies (LEAT clauses typically allow the patient to pay the difference between the LEAT-paid amount and the actual procedure), confirm the patient is aware. If the downgrade is a coding error (e.g., the carrier downgraded D4341 to D4346 without checking the SRP-vs-gingivitis pocket-depth distinction), cite the CDT code description verbatim and the clinical findings that match the higher code. If the bundling is a coding error (e.g., D2950 bundled into D2740 when the core build-up was clinically separate from the crown preparation), cite the ADA CDT companion's procedure-bundling guidance.
- **Evidence package:** EOB with the downgrade / bundling code highlighted; CDT description verbatim from the ADA's current CDT manual; clinical-finding evidence that matches the original code.
- **Success-rate read:** **High** for clear coding errors; **moderate** for bundling disputes; **low** for contractual LEAT downgrades (the contract is the contract; reframe as patient-financing path).

**6. Pre-Authorization Required (post-payment denial of un-pre-auth'd service)**
- **Anchors:** If the practice obtained the pre-auth and the carrier failed to log it, attach the pre-auth approval document with the reference number and the date. If the practice did not obtain the pre-auth but the procedure was an emergency or a clinical-finding pivot mid-procedure (e.g., RCT was treatment-planned as a filling but the caries extended to the pulp on excavation), document the clinical pivot and request retroactive review. Cite the carrier's emergency / clinical-pivot retroactive-pre-auth policy if known.
- **Evidence package:** Pre-auth approval (if obtained) or clinical-pivot chart note (if mid-procedure pivot); EOB; emergency-classification documentation.
- **Success-rate read:** **High** for pre-auth-on-file errors; **moderate** for clinical-pivot retroactive review; **low** for elective procedures that simply skipped pre-auth.

**7. Coordination of Benefits (COB) / Pre-Existing / Waiting Period**
- **Anchors:** Read the carrier's COB / pre-existing / waiting-period rule verbatim. For COB, confirm primary-vs-secondary order (employer-employee = primary; spouse = secondary; birthday-rule for pediatric; custodial-parent rule). For pre-existing, cite the patient's enrollment date and the date of the diagnostic finding (if the finding pre-dates enrollment, the carrier may have a contractual exclusion; if the finding post-dates enrollment, the appeal succeeds). For waiting period, confirm the contractual waiting-period start date and the procedure-type-specific waiting period (basic / major / orthodontic often have different waiting periods).
- **Evidence package:** Other carrier's EOB (for COB); enrollment-date documentation; diagnostic finding-date evidence.
- **Success-rate read:** **Moderate** when the facts are disputable; **low** when the contractual exclusion is clear.

**8. Timely-Filing**
- **Anchors:** Attach the EDI receipt, the postmark, or the carrier portal confirmation showing the original submission was within the carrier's timely-filing window. Carrier windows: Delta Dental 90 days initial, 180 days appeal; MetLife 180 days; Aetna 180 days; Cigna 180 days; United Concordia 180 days; Humana 60–180 days; Medicare Advantage 12 months; many self-funded plans set their own. If the original submission was late but the timely-filing window can be extended for documented submission errors (carrier portal outage, eligibility-correction loop), cite that.
- **Evidence package:** EDI receipt / clearinghouse confirmation / postmark / carrier-portal screenshot with date.
- **Success-rate read:** **High** with proof of timely original submission; **very low** without it.

**9. Experimental / Investigational**
- **Anchors:** If the carrier is calling the procedure experimental despite ADA / specialty-society endorsement, cite the endorsement verbatim. Common targets: bioactive liners (cite ADA Council on Scientific Affairs / J Dent Res / Oper Dent per `clinical-evidence-review` PQ-3); short implants (cite ITI consensus / J Dent Res per `clinical-evidence-review` PQ-2); CBCT for routine endo / implant / pathology indications (cite AAE / AAOMR joint position per `clinical-evidence-review` PQ-5); SDF for arrest of caries (cite AAPD Caries Risk Assessment guideline). If the procedure truly is experimental (e.g., a vendor-pushed device with no FDA clearance and no peer-reviewed evidence), the appeal will fail; flag this and recommend a `treatment-plan-explainer` patient-financing path.
- **Evidence package:** ADA / specialty-society / Cochrane citation; clinical findings that match the indication; FDA clearance documentation if applicable.
- **Success-rate read:** **Moderate** when the procedure has clear society endorsement; **low** when it does not.

### Carrier-Specific Appeal Pathway Flow

Most US dental carriers run a 2-level internal appeal process before external review. Internal-process specifics differ:

- **Delta Dental** (federation of state plans; pathways differ slightly state-by-state) — 1st-level: written appeal to the state plan's claims-review department; 2nd-level: peer-reviewer or dental director review; external: state IRO or state insurance department complaint. 180-day filing window typical.
- **MetLife** — 1st-level: written appeal to MetLife Dental Claims; 2nd-level: dental director review; external: state IRO. 180-day window.
- **Aetna / Aetna Dental** — 1st-level: written appeal to Aetna Dental Member Services; 2nd-level: dental director review; external: state IRO; ERISA grievance for self-funded plans. 180-day window.
- **Cigna / Cigna Dental** — 1st-level: written appeal to Cigna Dental Appeals; 2nd-level: dental director review; external: state IRO; ERISA grievance for self-funded plans. 180-day window.
- **United Concordia / United Healthcare Dental** — 1st-level: written appeal to UC / UHC Dental Appeals; 2nd-level: dental director review; external: state IRO; ERISA grievance for self-funded plans. 180-day window.
- **Humana / Humana Dental** — 1st-level: written appeal; 2nd-level: dental director review; external: state IRO. 60–180-day window varies by plan; check the EOB.
- **Guardian Dental** — 1st-level: written appeal to Guardian Group Dental Claims; 2nd-level: peer review; external: state IRO. 180-day window.
- **Principal Dental** — 1st-level: written appeal; 2nd-level: peer review; external: state IRO. 180-day window.
- **Anthem BCBS Dental** (federation of state plans) — 1st-level: written appeal to the state plan's dental appeals department; 2nd-level: dental director review; external: state IRO; ERISA grievance for self-funded plans. 180-day window.
- **Medicare Advantage dental rider** — CMS-prescribed appeal process: redetermination (CMS-20027) → reconsideration by Independent Review Entity (IRE) → ALJ hearing → Medicare Appeals Council → federal court. 60-day window at each level; this skill drafts the redetermination request and references the CMS forms; it does not replace them.
- **State Medicaid** — State-specific grievance procedure; window is often shorter than commercial (30–90 days). This skill drafts the grievance letter and references the state form; it does not replace it.
- **TRICARE Dental Program (United Concordia is the contractor)** — UC's TRICARE-specific appeal process; 90-day window typical.
- **Federal Employee Dental and Vision (FEDVIP)** — OPM-overseen plans (BCBS FEP, Delta Dental Fed, etc.); plan-specific appeal process; OPM external review available.
- **Self-funded ERISA plans** (employer plans not insured but self-funded; the carrier is the third-party administrator) — ERISA-bound 180-day federal floor for appeal filing; ERISA grievance procedure available; not state-IRO eligible. Check the EOB or SPD for the plan-administrator contact.

### Appeal-Level Structure

The same denial reason demands different framing at different levels:

- **1st-level (internal):** Concise, evidence-anchored. Anchor the appeal in the clinical findings and the CDT description. Tone: respectful-and-firm. Length: 1 page typical.
- **2nd-level (internal, peer review or dental director):** Build on the 1st-level letter; address each rationale the 1st-level reviewer cited; add ADA / specialty-society / Cochrane citations more aggressively. Tone: still respectful-and-firm but more technical; the audience is a dentist (or a dental hygienist with peer-review credentials), not a claims processor. Length: 1–2 pages.
- **External review (Independent Review Organization, IRO):** Full evidence package. Treat the IRO as a peer reviewer with no prior context. Build the entire clinical narrative from scratch with all exhibits. Tone: technical, professional, persuasive. Length: 2–3 pages typical.
- **State insurance department complaint:** Different audience entirely — a regulator, not a clinician. Frame the complaint around the carrier's pattern of behavior (denied this code N times across N patients; downgraded LEAT without a contractual basis; refused to honor the pre-auth on file). Cite state insurance code. Tone: factual, complaint-shaped. Length: 1–2 pages plus a chronology exhibit.
- **ERISA grievance** (self-funded plans only): Cite the plan's SPD verbatim; reference ERISA 503-1 claims procedure regulation; demand the full administrative record under ERISA 104(b)(4). Tone: legal-shaped. Length: 1–2 pages plus exhibits. **Cross-reference the practice's attorney before filing.**

### Universal Letter Scaffold

Every appeal uses this scaffold; the template fills in the reason-specific anchors:

- **Header** — Practice letterhead with NPI type 1 (provider) and NPI type 2 (group), tax ID, state license, and contact info. Date. Carrier address. RE: line with patient first name + last initial, DOB (last 4), member ID, claim / reference number, date of service, denial date.
- **Opening** — One sentence stating the purpose: "I am writing to formally appeal the denial of claim [reference] for [patient initials] dated [DOS]."
- **Patient & Procedure Summary** — Two to three sentences: who, what, when, why.
- **Clinical Justification** — The reason-specific anchor block (per the 9 templates above), citing radiographic, periodontal, vitality, photographic, or medical-history evidence with date and tooth number. Reference ADA / specialty-society / Cochrane / state-board sources.
- **Code Rationale** — CDT code description verbatim from the current ADA CDT manual; ICD-10 code; carrier-quirk reconciliation if downgrade or bundling is at issue.
- **Carrier-Policy Reference** — Quote the carrier's own SPD / EOC / provider manual policy verbatim if it supports the appeal. Quote the EOB remittance code and the carrier's stated denial reason verbatim and address each.
- **Closing** — Request specific reconsideration outcome (full payment / partial payment / pre-auth approval / external review escalation). List supporting documents attached. Provide billing-coordinator name and direct line. Include a 30-day-response request and the next-level escalation pathway.
- **Attachments** — Numbered exhibit list (Exhibit A: pre-op radiograph dated [date]; Exhibit B: chart note dated [date]; Exhibit C: perio chart dated [date]; Exhibit D: ADA CDT code description; Exhibit E: ADA / specialty-society reference; Exhibit F: carrier SPD / EOC excerpt; Exhibit G: EOB).
- **Signature** — Treating dentist (with credentials, license, NPI) or billing coordinator (with delegation authority); cc the patient if appropriate; cc the practice owner / attorney for ERISA grievances.

### Output Requirements

- Formal business-letter format
- Correct CDT code descriptions (verbatim from the current ADA CDT manual) and ICD-10 references
- ADA / specialty-society / Cochrane / state-board citations with publication year and cross-reference to `clinical-evidence-review` Prepared-Question Library where applicable
- Numbered exhibit list with explicit date and tooth-number labeling on each clinical exhibit
- Carrier-specific appeal-pathway flow followed (correct department address, correct filing window honored, correct appeal-level framing)
- Success-rate triage stamped at the top of the internal-only sidecar block clearly marked "Internal — do not send" (so the office knows what to expect before investing time)
- Ready to print on letterhead with minimal editing
- Saved to `outputs/appeals/` if the user confirms; ERISA grievances also saved to `outputs/legal/` for the practice owner / attorney's review

## Cross-Reference Graph

This skill explicitly chains with:

- **Upstream:** `insurance-verification-summary` (the original benefit summary the appeal is built against); `pre-auth-narrative-writer` (the original pre-auth narrative if the appeal is post-payment); `clinical-note-assistant` (the chart note that backs the clinical justification — the audit-defensibility checklist there is the strongest appeal exhibit); `cdt-code-assistant` (carrier-quirk overlay for downgrade and LEAT clauses); `clinical-evidence-review` (Prepared-Question Library for the standard-of-care evidence package on common denial topics)
- **Sibling:** `aging-ar-followup-playbook` (the appeal sits on the aging worklist; this skill produces the letter, the playbook tracks the worklist); `chart-audit-prep` (the audit-defensibility checklist run on the chart note before the appeal goes out)
- **Downstream:** `email-drafter` insurance-clarification pattern (patient-facing notification of the appeal in flight); `monthly-practice-kpi-report` (appeal-success-rate aggregation feeds the KPI dashboard); `meeting-summarizer` end-of-day-debrief pattern (appeals filed / appeals overturned line)

## Common Pitfalls To Avoid

- Do **not** appeal a categorically excluded service — flag it as low-probability and recommend a patient-financing path
- Do **not** miss the carrier's filing window — most carriers allow 180 days but some are 60 days; check the EOB
- Do **not** skip the audit-defensibility checklist on the chart note before the appeal goes out — the appeal is only as strong as the chart note backing it
- Do **not** confuse 1st-level internal appeal with external review — they are sequential, not interchangeable; external review is only available after internal options are exhausted
- Do **not** file an ERISA grievance without the practice's attorney's review — ERISA has its own administrative-record demand and federal-court pathway
- Do **not** quote the carrier's policy or denial reason inaccurately — quote verbatim and address each verbatim point
- Do **not** include the patient's full date of birth, full SSN, or full member ID in any clear-text email to the carrier — use the carrier's secure-portal / fax / certified-mail pathway
- Do **not** paste the appeal exhibits (radiographs, chart notes, perio charts) into a non-BAA AI tool — strip to initials and chart number first
- Do **not** invent clinical findings or fabricate ADA / specialty-society citations — if the practice does not have the radiograph / perio chart / vitality test on the date of service, the appeal cannot legitimately claim it
- Do **not** use adversarial or emotional tone — "the carrier is wrong" framing fails; "the clinical findings support the original code" framing succeeds
- Do **not** overlook the patient-choice path for LEAT downgrades — the contract is the contract; the patient-financing path is often the right answer

## Example Output

**Sample input:** "Delta Dental PPO downgraded our D4341 (SRP, UR quad) to D4346 (gingivitis prophy) on patient T.M. EOB dated 06-10-2026, claim #DD-4471992. We have a full perio chart dated 05-28-2026 showing 5–7 mm pockets UR with BOP, generalized horizontal bone loss on the FMX, AAP Stage III Grade B. 1st-level appeal. 180-day window. In-network, Dr. Anna Patel." *(Config: Cherry Creek Family Dental, Denver CO; Dr. Anna Patel NPI 1234567890, CO-DDS license; DentalXChange clearinghouse; Overjet for AI-flagged findings, provider-confirmed.)*

---

> **⚠️ INTERNAL — DO NOT SEND. Success-rate triage.**
> **Denial category: #5 — Coding Error / Downgrade (CDT-side).** This is a **HIGH-probability** appeal: the downgrade hinges on a documentable clinical distinction (active periodontitis vs. gingivitis) and you have a dated perio chart + FMX that match the higher code. The single point of failure is whether the 05-28 chart note independently states pocket depths, BOP, and AAP staging — confirmed present per input. Reframe is *not* needed; this is a winnable clinical-evidence appeal. Submit via DentalXChange portal or Delta's provider appeal address (not clear-text email — exhibits contain PHI). Run the `clinical-note-assistant` audit-defensibility check on the 05-28 entry before sending.

---

**Cherry Creek Family Dental**
Anna Patel, DDS · NPI 1234567890 · CO License CO-DDS-00000
[address] · Denver, CO · (303) ___-____ · Fax (303) ___-____

June 22, 2026

Delta Dental PPO — Dental Claims Review
[appeal address from EOB]

**RE:** Appeal of downgraded claim · Patient **T.M.** · DOB **xx/xx/19xx** · Member ID [____] · Claim **#DD-4471992** · DOS **05-28-2026** · EOB **06-10-2026**

To the Claims Review Department:

I am writing to formally appeal the downgrade of claim **#DD-4471992** for patient **T.M.**, dated **05-28-2026**, in which **D4341 (periodontal scaling and root planing — four or more teeth per quadrant, upper right)** was reprocessed and paid as **D4346 (scaling in presence of generalized moderate or severe gingival inflammation — full mouth, after oral evaluation)**. The clinical findings on the date of service meet the definition of D4341, and I respectfully request reconsideration and payment at the originally submitted code.

**Patient & procedure summary.** T.M. presented 05-28-2026 for periodontal therapy of the upper-right quadrant following a comprehensive periodontal evaluation. Active periodontitis — not gingivitis — was diagnosed and treated by scaling and root planing.

**Clinical justification.** The periodontal chart dated **05-28-2026 (Exhibit C)** documents upper-right probing depths of **5–7 mm with generalized bleeding on probing**. The full-mouth radiographic series dated **05-28-2026 (Exhibit A)** demonstrates **generalized horizontal alveolar bone loss**. These findings establish a diagnosis of **periodontitis, AAP 2018 Stage III, Grade B** — a condition with attachment and bone loss, which is categorically distinct from the gingival-inflammation-without-attachment-loss condition that D4346 describes.

**Code rationale.** The current ADA CDT descriptor for **D4341** specifies scaling and root planing as a *definitive, therapeutic* procedure for teeth with **bone loss and pocketing** — precisely the documented presentation. **D4346** by its own descriptor applies *in the presence of generalized moderate-to-severe gingival inflammation* **without** attachment loss, and is therefore clinically inapplicable where 5–7 mm pockets and radiographic bone loss are present. The downgrade does not reconcile with the charted pocket depths or the FMX.

**Carrier-policy reference.** Delta Dental's SRP benefit criteria require radiographic evidence of bone loss and probing depths consistent with periodontitis; both are documented on the date of service and attached. The EOB remittance reason for the downgrade is addressed directly above.

**Requested outcome.** Please reverse the downgrade and adjudicate claim #DD-4471992 at **D4341** for the upper-right quadrant. Supporting documents are attached and enumerated below. I request a written determination within 30 days. If this first-level appeal is not resolved in the patient's favor, we intend to pursue second-level (dental director) review.

Sincerely,
**Anna Patel, DDS** — NPI 1234567890, CO-DDS-00000
Billing coordinator: [name] · (303) ___-____ direct

**Attachments:** Exhibit A — FMX dated 05-28-2026 (generalized horizontal bone loss); Exhibit B — clinical note dated 05-28-2026; Exhibit C — periodontal chart dated 05-28-2026 (UR 5–7 mm, BOP, Stage III/Grade B); Exhibit D — ADA CDT descriptions, D4341 and D4346; Exhibit E — EOB dated 06-10-2026.

---

**Most likely failure mode:** a thin chart note that says "SRP UR" without the pocket depths, BOP, and staging that distinguish D4341 from D4346. The highest-value action is verifying — *before* the appeal leaves the office — that the 05-28 perio chart and FMX are attached and that the chart note independently states the AAP stage/grade. Per config, any Overjet-flagged radiographic finding cited here must be provider-confirmed, not AI-asserted.

## Version History

- **v3.0 (2026-06-22)** — Added a worked Example Output: a HIGH-probability Category-5 SRP-downgrade appeal (Delta Dental PPO D4341→D4346) showing the internal "do-not-send" success-rate triage block, a print-ready 1st-level letter on the universal scaffold, a numbered dated-exhibit list, and the most-likely-failure-mode callout — grounded in config (provider NPI/license, DentalXChange, Overjet provider-confirmation, PHI-safe transmission). No instruction text removed; `last_eval_score` populated.
- **v2.0** — 9 denial-reason templates, carrier-specific appeal-pathway flow, multi-level escalation (internal → external → state complaint → ERISA), success-rate triage, universal letter scaffold, common-pitfalls list.
- **v1.0** — Initial release: single appeal-letter scaffold with CDT/ICD-10 and ADA citation guidance.
