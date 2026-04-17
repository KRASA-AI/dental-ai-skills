# PHI-Safe Prompting in Dental AI Workflows

A best-practices reference for dental office managers and treatment coordinators who use AI assistants (Claude, ChatGPT, Gemini, Copilot) in HIPAA-regulated environments. Maintained by the landscape-monitor.

## Why This Matters

As of 2026, dental office managers are routinely expected to evaluate, configure, and audit AI tools for clinical and administrative workflows. DOMA and other dental management certification bodies now include "PHI-safe prompting" as a core competency. The 2026 HIPAA Security Rule updates (mandatory encryption, MFA, accelerated breach notification) raise the stakes for any AI workflow that touches patient data.

## Core Principles

### 1. Minimize PHI in Prompts

When using AI assistants for tasks like drafting narratives, creating patient communications, or analyzing data:

- **De-identify before prompting.** Replace patient names with initials or "Patient A." Remove DOB, SSN, insurance ID, and chart numbers. Use age ranges instead of exact ages when possible.
- **Use code-level references.** Refer to procedures by CDT code rather than embedding full treatment histories.
- **Never paste raw PMS exports** into a cloud-based AI tool. Aggregate or anonymize first.

### 2. Evaluate AI Tool Compliance

Before adopting any AI tool for practice use, verify:

- Does the vendor sign a HIPAA Business Associate Agreement (BAA)?
- Is data encrypted in transit and at rest?
- Does the vendor's AI model train on user inputs? (Most dental practices should use tools where inputs are NOT used for model training.)
- Where is data stored? (US-based data residency preferred for most practices.)
- What is the data retention policy?

### 3. Prompt Hygiene for Common Dental Tasks

| Task | PHI Risk | Mitigation |
|------|----------|------------|
| Insurance narrative drafting | High — requires clinical findings, tooth numbers, measurements | Use de-identified clinical data; add patient identifiers only in the final PMS-submitted version |
| Patient communication drafts | Medium — first name, appointment dates | First name is acceptable; avoid including DOB, insurance info, or diagnosis in the prompt |
| KPI / production reports | Low if aggregated — high if provider-level or patient-level | Use aggregated data; redact provider names if sharing outside leadership |
| Social media content | Low — should never contain PHI | Verify no identifiable patient photos or case details without signed consent |
| Clinical note drafting | High — full clinical context | Prefer on-premise or BAA-covered AI tools; never use consumer-grade chatbots for clinical notes |

### 4. Audit Trail

- Maintain a log of which AI tools are in use, what tasks they perform, and who authorized their use
- Include AI tools in the annual HIPAA Security Risk Analysis
- Document staff training on PHI-safe prompting (onboarding + annual refresher)

## Dental AI Standard Framework

DOMA's three-tier certification (AI Ready → AI Operator → AI Dental Driven Leader) provides a structured progression. Key takeaway: the office manager or compliance lead should be the gatekeeper for AI tool adoption, not individual providers experimenting independently.

## Cross-References

- `skills/admin/chart-audit-prep.md` — Chart-level audit readiness (includes documentation standards that AI-generated notes must meet)
- `skills/admin/pre-auth-narrative-writer.md` — Uses clinical data that must be de-identified during drafting
- `skills/operations/clinical-note-assistant.md` — Highest PHI risk; should only use BAA-covered tools
- `knowledge-base/regulations/` — HIPAA and state-specific requirements

---

*This file is maintained by the landscape-monitor scheduled task. Regulatory requirements change frequently; verify current HIPAA guidance before implementing any compliance workflow.*
