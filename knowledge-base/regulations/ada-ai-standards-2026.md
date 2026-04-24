# ADA AI Standards, CE Requirements, and 2026 CDT Updates — Dental Regulatory Reference

A regulatory landscape note maintained by the landscape-monitor. Cross-references the `informed-consent-drafter`, `chart-audit-prep`, `pre-auth-narrative-writer`, and `clinical-note-assistant` skills, and the `phi-safe-prompting` best-practices note.

## Why This Matters

Through most of 2024 and 2025, AI in dentistry was a product-category story with no dedicated standards. In 2026 the picture has shifted: the ADA's first formal AI standard has been approved, state dental boards are actively reviewing mandatory AI-related continuing education, and the 2026 CDT updates reference the new ADA standards as part of the framework for point-of-care diagnostic tools and advanced 3D printing. Practices that adopt AI without tracking these developments face growing audit, compliance, and patient-communication risk.

## ANSI/ADA Standard No. 1110-1:2025

Title: *Dentistry — Validation Dataset Guidance for Image Analysis Systems Using Artificial Intelligence, Part 1: Image Annotation and Data Collection*. This is the first U.S. standard on AI in dentistry and is focused on the validation data used to train and evaluate image-analysis systems (for example, caries detection, bone-level measurement, and perio disease detection on 2D radiographs). Practices should understand that the standard governs the vendor's obligations for annotation quality and dataset composition — it is a supplier-side standard — but the downstream implication for practices is that asking vendors whether their AI tools conform to ANSI/ADA 1110-1 is now a reasonable purchasing-diligence question.

Expect follow-on parts of the 1110 series to address model performance reporting, post-market surveillance, and CBCT analysis. Track the ADA Standards Committee on Dental Informatics for updates.

## State-Level AI-Related CE Activity

As of early 2026, no state dental board has implemented a formal mandatory AI-assisted-diagnostics continuing education requirement. A small number of states — including California and Texas — have been described in industry coverage as reviewing possible mandates, and industry analysts expect the first state requirement to appear in late 2026 or early 2027. Practices should not wait for a mandate to train their team on AI-assisted workflows; the more common audit and malpractice exposures today come from undocumented AI use, not from missing CE hours.

In parallel, the ADA CERP (Continuing Education Recognition Program) standards are being streamlined effective June 1, 2026 — from 14 standards with 104 criteria down to 5 standards with 17 criteria. The streamlining is intended to allow more innovative CE programming, including AI-assisted diagnostics training. Practices relying on CERP-certified CE courses should expect more AI-focused offerings in the second half of 2026.

## 2026 CDT Updates and AI

The 2026 CDT reference cycle incorporates the new ANSI/ADA standards as part of the framework supporting interoperability, safety, and point-of-care diagnostic tools. Practice-level implications include:

- **Standardized documentation of AI-assisted findings.** When AI analysis supports a diagnostic code, the provider's review and concurrence must be documented in the clinical note. AI is a decision support, not a decision.
- **Billing integrity.** An AI-highlighted finding that has not been clinically confirmed by the provider is not billable as a diagnostic service. Auditors are increasingly aware of this distinction.
- **Documentation language.** Notes should reflect the provider's clinical judgment, using phrases such as "AI-assisted review confirmed by provider" rather than "AI diagnosed."

## FDA 510(k) and Vendor Diligence

AI diagnostic tools used for clinical decisions must be 510(k) cleared (or otherwise FDA-authorized) for the stated clinical intent. Practices evaluating an AI vendor should request:

- Current 510(k) clearance letter and the specific indications cleared
- Whether the tool is cleared for the imaging modality used in the practice (e.g., 2D bitewings vs. CBCT)
- Post-market surveillance and adverse-event reporting procedures
- HIPAA Business Associate Agreement, data residency, and training-data policy
- Whether patient data is used for continued model training (and the opt-out mechanism if so)

## Patient Consent and AI Disclosure

Emerging 2026 guidance — from the British Dental Journal, the Journal of Medical Internet Research, and a dentistry-specific AI consent checklist published in a recent peer-reviewed article — converges on a requirement to disclose AI involvement to patients in a plain-language form, confirm provider oversight, and document the patient's opportunity to ask questions or decline. The `informed-consent-drafter` skill includes an AI Disclosure section that implements this guidance.

## HIPAA Security Rule 2026 Updates

The 2026 HIPAA Security Rule updates (mandatory encryption, MFA, accelerated breach notification) apply to any practice using AI tools that touch patient data. See `knowledge-base/best-practices/phi-safe-prompting.md` for the prompt-hygiene implications.

## ADA Position on AI in Claims Adjudication and Prior Authorization (April 2026)

In its March 27, 2026 comments on the CMS Comprehensive Regulations To Uncover Suspicious Healthcare (CRUSH) initiative — published on the ADA news site in early April 2026 — the ADA took an explicit position that AI should not be used as the sole basis for claim denials or prior-authorization decisions affecting dental providers. The ADA framed this around two concerns specific to dentistry: first, dental claims data are structured for benefit administration and are not well-suited for medical-necessity determination or fraud detection; second, dental data systems are fragmented and lack standardized formats, which degrades the training data that any AI adjudication model would rely on. The ADA also urged that CMS require independent validation of AI tools used in the claims pipeline.

**Practice-level implications for the repo:**

- When a denied claim shows signals of AI-only adjudication (identical boilerplate language across unrelated claims, denials that do not reference any specific clinical fact from the submitted documentation, suspiciously fast turnaround), the appeal letter can cite the ADA's April 2026 position on AI adjudication as part of the case for human review of the denial. The `insurance-denial-appeal` skill should be aware of this option, though citation is optional and case-specific.
- The `pre-auth-narrative-writer` skill should continue to prioritize narratives that are specific, clinically anchored, and radiograph-referenced — the harder the narrative is to dismiss on auto-review, the more likely it is to reach a human adjudicator in an AI-heavy pipeline.
- Practices with DSO or corporate affiliations should check whether their carrier contracts include (or lack) an AI-escalation clause that guarantees human review of denials above a dollar threshold; this is becoming a negotiated contract term.

This position is advocacy, not law — CMS has not adopted it in a final rule as of April 2026 — but it is the clearest official ADA statement to date on AI in claims adjudication and is useful context in appeal correspondence.

## Cross-References

- `skills/admin/informed-consent-drafter.md` — AI Disclosure section implements the consent guidance described above
- `skills/admin/chart-audit-prep.md` — Documentation standards that AI-assisted findings must meet
- `skills/admin/pre-auth-narrative-writer.md` — Carrier narrative standards for codes supported by AI analysis
- `skills/operations/clinical-note-assistant.md` — Note language conventions for AI-assisted findings
- `knowledge-base/best-practices/phi-safe-prompting.md` — Prompt hygiene for PHI-adjacent workflows
- `knowledge-base/tools-ecosystem/ai-phone-receptionists.md` — Adjacent vendor ecosystem; BAA and data residency apply identically

## Open Questions to Track in Subsequent Runs

1. Which state will be first to mandate AI-specific CE hours, and what will the hour count and content look like?
2. Will the ADA or a state board publish model consent language specifically for AI-assisted diagnostics, or will this remain a practice-level drafting exercise (as the `informed-consent-drafter` skill currently assumes)?
3. How will payers adapt pre-authorization and audit workflows when AI analysis is part of the supporting documentation?
4. Will subsequent parts of the ANSI/ADA 1110 series cover CBCT and 3D printing workflows?
5. Will CMS incorporate the ADA's "no AI-only denials" position into the final CRUSH rule, and if so will that framework propagate to private dental carriers?

---

*This file is maintained by the landscape-monitor scheduled task. Regulatory requirements change frequently; verify current ADA, state dental board, and FDA guidance before implementing any compliance workflow.*
