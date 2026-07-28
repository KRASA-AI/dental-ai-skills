---
name: "Cybersecurity Incident Response Plan"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~12–20 hrs/incident (and avoids ~$250K+ downside in a worst-case ransomware event)"
version: 1.1
last_eval_score: 9.60
---

# 🛡️ Cybersecurity Incident Response Plan

## Purpose

Produce a practice-ready cybersecurity incident response plan (IRP) plus the role-specific playbooks, communication templates, and evidence-preservation checklists a dental office actually executes during the first 72 hours of a suspected ransomware, business-email-compromise, AI-powered phishing, or unauthorized-PHI-access event. The output covers detect, contain, eradicate, recover, and notify — and produces the artifacts the practice needs to satisfy the HIPAA Breach Notification Rule's 60-day patient notice deadline, the proposed 2026 HIPAA Security Rule's 72-hour incident reporting requirement, OCR notification (immediate for breaches affecting 500+, end-of-year roll-up for under 500), state attorney-general notice where required, and cyber-insurance carrier first-notice-of-loss timing.

This is the operational complement to the regulatory `knowledge-base/regulations/hipaa-security-rule-2026.md` note. The KB note tells the practice what is required; this skill tells the practice what to do at 7:43 a.m. when the front-desk computer shows a ransom screen and the phones are still ringing.

## When to Use

Use this skill when:

- The practice is building or refreshing its written incident response plan as part of HIPAA Security Rule compliance (required, not optional, regardless of practice size)
- A specific suspected incident is unfolding now: ransomware, suspicious encryption of files, locked-out admin accounts, unfamiliar wire-transfer requests from a "vendor," patient calls saying their card was charged after an appointment, the IT vendor reporting unusual outbound traffic from the network, or an employee clicking a phishing link
- The practice is preparing for a HIPAA risk-assessment engagement or a cyber-insurance underwriting review and needs a current IRP, a tested tabletop exercise, and a documented chain of custody for any prior incidents
- An associate dentist or new office manager is being trained on incident response as part of the `staff-onboarding-checklist` security module
- A DSO or multi-location practice is standardizing incident response across sites and needs a single template that local managers customize per location
- The practice is selecting or renewing a cyber-insurance policy and needs the IRP elements the carrier scores during underwriting (24/7 monitoring vendor, MFA coverage percentage, backup architecture, tabletop frequency, BAA inventory)

Do **not** use this skill to:

- Replace a managed security service provider (MSSP), endpoint detection and response (EDR) platform, or 24/7 SOC — the plan tells the practice when to call them, not how to detect intrusions in real time
- Replace forensic investigation services from a qualified incident-response firm — this skill produces the activation runbook that brings the firm in within the first 60 minutes
- Replace HIPAA legal counsel or breach counsel — the skill produces the draft notice; counsel reviews and authorizes
- Pay or negotiate ransoms — that is a decision for the practice owner, breach counsel, cyber-insurance carrier, and the FBI, not for an AI-generated playbook
- Replace the cyber-insurance carrier's incident-response panel — most policies require the carrier's pre-approved counsel and forensic firm be used, or the claim is denied

## Required Input

Provide the following:

- **Practice profile.** Number of locations, total seats, PMS in use (Dentrix, Eaglesoft, Open Dental, Curve, Denticon, Dentrix Ascend), imaging system, EHR if separate, payment-processor (in-office terminal, integrated), patient-portal vendor, AI vendors with PHI access (imaging AI, ambient scribe, AI receptionist, eligibility AI), backup architecture (on-prem, cloud, immutable copies, frequency), IT support model (in-house, MSP, MSSP).
- **Current incident-response posture.** Existing IRP date and version (or "none"), MFA coverage (% of accounts), endpoint protection (vendor + EDR vs. legacy AV), 24/7 monitoring (yes/no/MSSP name), prior incidents in the last 24 months, last tabletop exercise date, cyber-insurance carrier and limits, breach counsel on retainer (yes/no/firm name).
- **State of operation.** Each U.S. state has its own breach-notification statute that may be stricter than HIPAA (notice timing, AG notification threshold, content requirements). Multi-state practices should list every state where any patient resides.
- **Specific incident details (only if responding to a live event).** Time of detection, who detected it, what is visible (ransom note, locked accounts, file extensions changed, unusual emails), systems affected, whether systems are still on the network, whether backups are intact, whether anyone has communicated with the attacker, whether any patient data is confirmed exfiltrated.

If the input is incomplete, the output should still produce a usable plan and explicitly mark the assumptions made (for example, "Assuming MFA on PMS only — extend to email and remote access").

### Fast path — two modes, and standing-IRP mode needs zero blocking input

This skill has two invocation modes, and conflating them is what makes it feel heavy:

**Standing-IRP mode (the common case — building or refreshing the written plan).** No field is blocking. The entire practice profile is derived from `config.yml` and stamped as an explicit `ASSUMED:` block the practice corrects in one pass:

- **Practice profile** → `config.yml → company.size` + `tools` (for Cherry Creek: single location, 4 operatories; **Dentrix Ascend** PMS, **Dexis** imaging, **Overjet** AI diagnostic — a PHI-touching AI vendor, so it gets a BAA line and an "AI-tool prompt-injection PHI leak" tabletop scenario; **Weave** patient comms + payments, BAA on file; **DentalXChange** clearinghouse). Google Drive is stamped **non-PHI only** per config.
- **Roles** → `config.yml → team.roles` + `compliance`. Incident Commander = owner (Dr. Patel); Privacy/Security Officer = **office manager** (config names them HIPAA officer); Comms Lead = office manager; Clinical Continuity = lead hygienist. Cross-name an alternate for the office manager, since that role is triple-loaded.
- **State** → `config.yml → company.location` (**Colorado**) + `compliance.state_board` (Colorado Dental Board). Expand Colorado's breach-notification statute specifically (Colo. Rev. Stat. §6-1-716: notice "in the most expedient time possible and without unreasonable delay," **not to exceed 30 days** — stricter than HIPAA's 60 — and AG notice if **500+ Colorado residents** are affected). Never emit a generic "consult your state law."
- **Posture fields** (MFA %, EDR vendor, 24/7 monitoring, prior incidents, last tabletop, cyber-insurance carrier, breach counsel) → these are genuinely unknown from config. Do **not** block on them: render each as a **fill-in-blank line in the standing plan** and add any gap (e.g., "no last-tested-recovery date on file") to the next-step list. The plan is more valuable delivered with named gaps than withheld pending a security questionnaire.

**Live-incident mode (responding now).** The *only* additional block that matters is the **specific-incident details** field (time/what's visible/systems affected/backups intact/attacker contacted). Take those five facts and route straight to the severity tier and hour-zero runbook — do not re-collect the config-derivable profile mid-incident.

Render the derived profile as an `ASSUMED:` block at the top of the plan. Standing-IRP mode is effectively one-shot from config; live-incident mode is five facts to a runbook.

## Instructions

The output is a four-part bundle. Produce all four parts unless the user explicitly requests only a subset.

### 1. Written Incident Response Plan (the standing document)

A practice-customized IRP that follows the NIST SP 800-61 lifecycle (Preparation, Detection & Analysis, Containment / Eradication / Recovery, Post-Incident Activity) and the HIPAA Security Rule's incident-response standard. It must include:

- **Roles and responsibilities.** Incident Commander (typically the owner-dentist or office manager), IT Lead (in-house tech or MSP point-of-contact), Privacy Officer (HIPAA-required), Communications Lead, Clinical Continuity Lead. For each role: name, primary phone, secondary phone, alternate (cross-named in case of vacation or the role-holder being the affected user). The plan is useless if the only copy is on the encrypted server; it must exist as a printed binder at every location AND in a cloud location reachable from a phone.
- **External contact list.** MSP/MSSP after-hours number, EDR vendor support, cyber-insurance carrier first-notice-of-loss number, breach counsel direct line, forensic firm (if pre-engaged), FBI Internet Crime Complaint Center (IC3) and the local FBI field office, state dental board (if reportable), state attorney general data-breach intake (state-specific), HHS Office for Civil Rights breach-portal URL, primary PMS vendor support, primary imaging vendor support, payment processor fraud line.
- **Severity classification.** Tier 1 (full IT outage / confirmed ransomware / confirmed PHI exfiltration), Tier 2 (suspected compromise of one workstation, isolated phishing click, suspected card-skimming), Tier 3 (anomalous behavior, vendor advisory, false positive being investigated). Every tier has a different escalation path and a different hour-zero checklist.
- **First 60 minutes runbook.** Disconnect (not power off — preserve volatile memory for forensics), photograph the screen, do not interact with the attacker, isolate from network (unplug Ethernet, disable Wi-Fi at the access-point level, unplug any backup drives still attached), preserve the logs, call the MSP, call the cyber-insurance carrier, lock down email (the attacker often has email access already), hold all patient-data communications until counsel approves.
- **Patient-care continuity protocol.** What does the practice do for the patients in the chairs and the patients arriving in the next 4 hours? Paper appointment list (printed every morning as a standing control), paper medical-history forms, paper-prescription protocol if e-prescribing is down, after-hours emergency-call routing, a same-day-cancellation script that does not disclose a security incident, when and how to communicate with patients who are physically in the practice when the attack is detected.
- **Backup and recovery posture.** Last verified clean backup date, last recovery test date, immutable copy presence, recovery time objective (RTO), recovery point objective (RPO), and a hard rule that recovery only proceeds after the forensic firm clears the environment. Restoring from backup into an unremediated environment is the single most common reason a recovered practice gets re-encrypted within 30 days.
- **Reporting decision tree.** Was PHI accessed, acquired, used, or disclosed in a manner not permitted by HIPAA? If unable to demonstrate "low probability of compromise" via the four-factor risk assessment, presume breach and notify. The plan should walk the user through the four-factor analysis and produce the conclusion in writing for the file.
- **Annual exercise schedule.** Tabletop scenario per quarter (ransomware, phishing → BEC, lost laptop, vendor compromise, AI-tool prompt-injection PHI leak — the AI scenario is new and increasingly featured in 2026 exercises). One full live drill per year. Documented after-action notes are required for the next risk assessment.

### 2. Hour-Zero Activation Runbook (the wall poster)

A single-page laminated runbook for every workstation. Bullet-list, no narrative. The exact order to call, the exact words to say to the front-desk team, what NOT to do (do not pay the ransom, do not communicate with the attacker, do not delete anything). Include the cyber-insurance carrier's claim number formatting and a placeholder for the policy number. Explicit reminder that opening a ticket with the cyber-insurance carrier preserves coverage; engaging a forensic firm or breach counsel without carrier authorization may not.

### 3. Communication Templates (drafts only — counsel must approve before sending)

Produce drafts of:

- **Patient breach notification letter.** Plain-language, 7th–8th grade reading level. Required HIPAA elements: brief description, types of unsecured PHI involved, steps the practice has taken, what the patient should do to protect themselves, contact information. State-specific addenda where applicable (for example, California adds substitute notice rules; Massachusetts requires specific content; Texas adds 60-day to AG language).
- **Substitute notice and media notice.** Required when more than 500 individuals affected; the practice must notify prominent media outlets in the state and post a substitute notice on the practice website for 90 days.
- **Internal staff briefing.** What the team can and cannot say if a patient calls. The single biggest source of HIPAA secondary disclosure during an incident is well-meaning team members improvising on the phone.
- **Vendor notice.** When a business associate is implicated, the BAA-required notice template within the BAA's contractual deadline (typically 24 to 72 hours).
- **OCR breach portal entry draft.** The narrative the Privacy Officer pastes into the HHS portal, sized to the portal's character limits.
- **Cyber-insurance first-notice-of-loss script.** The exact information the carrier intake will ask for.

Every template ends with a **REVIEW REQUIRED** block that flags counsel review, carrier approval, and final sign-off by the Incident Commander. Mark all of these as drafts. Patient notice that goes out without counsel review is a significant malpractice exposure.

### 4. Post-Incident Documentation Bundle

After the incident is closed, the practice must produce and retain:

- **Incident timeline reconstruction** in chronological order with timestamps and owner per action
- **Four-factor risk assessment** written analysis (nature of PHI, unauthorized person, was PHI actually acquired or viewed, mitigation extent)
- **Evidence chain of custody** for any forensic images, log captures, or hardware that left the premises
- **After-action review** noting what worked, what failed, and what changes go into the next IRP version (assigned owners and target dates)
- **Updated risk analysis** reflecting the lessons learned — required by HIPAA before the next annual review

## Output Requirements

- **Format:** All four parts as separate sections in a single document, with anchor links at the top so the practice can jump to the relevant part during a live incident. The Hour-Zero Activation Runbook is the single most important artifact and must be reproducible as a one-page printable PDF without further editing.
- **Customization:** Every state-specific reference must be expanded for the practice's state. Do not output a generic "consult your state law" without identifying the controlling statute and timing.
- **Severity routing:** Tier 1 vs. Tier 2 vs. Tier 3 must be visually distinct so a stressed user finds the right path in 30 seconds.
- **Plain language:** Communication templates target 7th–8th grade reading. Internal-use portions can use HIPAA / NIST terminology where it is the precise term.
- **Mandatory disclaimers.** Every output ends with: (1) "This plan does not replace counsel. Engage breach counsel immediately when PHI is implicated." (2) "Cyber-insurance carriers typically require pre-approved counsel and forensic firms — engaging non-panel vendors may void coverage." (3) "The 2026 HIPAA Security Rule final text and effective date control over any draft language in this plan; verify against the current rule."

## Guardrails

- **Never recommend ransom payment.** The decision to pay is for the practice owner with breach counsel, the cyber-insurance carrier, and law enforcement. The skill produces the decision framework and lists the considerations (encryption-only vs. exfiltration, OFAC sanctions risk, restoration confidence, business survival), but does not advocate.
- **Never recommend deleting or formatting affected systems before forensic capture.** Volatile memory is evidence; once gone, it cannot be reconstructed.
- **Never advise communicating with the attacker outside of carrier-coordinated negotiation.** Direct DM with attacker risks escalation and is rarely covered by insurance.
- **Never produce a final patient notice ready to send.** The output is always marked DRAFT pending counsel review. State law variance and content-specific requirements make any AI-only notice high-risk.
- **State-law specificity is required.** Generic "consult your state" output is not acceptable. The plan must call out the specific state statute (e.g., California Civil Code §1798.82, Texas Business and Commerce Code Chapter 521, NY SHIELD Act, Massachusetts 201 CMR 17), the AG-notification threshold, the timing window, and any content-specific requirements.
- **PHI handling in prompt usage.** Never paste actual patient data into a non-BAA AI tool while drafting the incident response plan. De-identify any examples used to instantiate templates. The `knowledge-base/best-practices/phi-safe-prompting.md` note covers the prompt-hygiene rules.
- **Breach versus security incident.** Not every security incident is a breach. The four-factor risk assessment exists precisely to distinguish them. The plan must walk the user through the analysis rather than presuming a breach has occurred.
- **HIPAA does not preempt stricter state law.** Where state law is stricter (timing, content, AG notification), the stricter rule controls.
- **Cyber-insurance panel rule.** Many policies require pre-approved breach counsel and forensic firm. The plan must remind the user to call the carrier before retaining outside firms.
- **Documentation retention.** Six years minimum for HIPAA-related incident records (NPP exception is six years from later of creation or last effective date). State law and the cyber-insurance contract may require longer.

## Cross-references

- `knowledge-base/regulations/hipaa-security-rule-2026.md` — Standing regulatory reference for the 2026 Security Rule update (mandatory encryption, MFA, accelerated incident reporting, annual penetration testing) and the Breach Notification Rule
- `knowledge-base/regulations/ada-ai-standards-2026.md` — ADA 1110-1, FDA 510(k) vendor diligence, AI-disclosure consent posture
- `knowledge-base/best-practices/phi-safe-prompting.md` — Prompt hygiene rules for any incident drafting that touches PHI
- `knowledge-base/tools-ecosystem/ai-phone-receptionists.md` — BAA and data-residency posture for AI vendors that hold PHI; vendor notice template ties into this
- `skills/admin/staff-onboarding-checklist.md` — The Day-1 security training and least-privilege PMS access provisions are the single biggest preventive control; the IRP and onboarding are paired skills
- `skills/admin/informed-consent-drafter.md` — AI Disclosure language; patient breach notice may need to acknowledge AI-tool involvement when a vendor is implicated
- `skills/admin/chart-audit-prep.md` — Documentation standards used during post-incident review
- `skills/customer-service/after-hours-emergency-triage.md` — Patient-care continuity protocol when the AI receptionist or PMS is offline
- `skills/operations/morning-huddle-brief.md` — Daily printed appointment list as a continuity control referenced in the IRP

## Example Output

Practice profile: Single-location general dentistry, 6 operatories, 14 staff, Open Dental on-prem, Pearl Second Opinion imaging AI, Heidi ambient scribe, Arini AI receptionist, on-prem backup + Datto cloud immutable copy, MSP support (no MSSP), Texas operating state.

Generated bundle includes:

1. **IRP version 2026.04.** Roles assigned: Incident Commander = Dr. Owner; IT Lead = MSP main number plus after-hours; Privacy Officer = office manager; Comms Lead = office manager; Clinical Continuity = lead hygienist. External contact list pre-populated with the cyber-insurance carrier's first-notice-of-loss line, breach counsel firm, FBI San Antonio field office, Texas AG identity-theft division, HHS OCR breach portal URL, Open Dental support, Pearl support, Heidi support (BAA-bound vendor), Arini support (BAA-bound vendor). Severity tiers customized for a single-location general practice. First-60-minutes runbook with tear-off cards. Patient continuity protocol assumes printed schedule control. Backup posture flagged: no last-tested-recovery date — added to next-step list. Annual tabletop schedule populated with four scenarios including the "AI-tool prompt-injection PHI leak" scenario for the Heidi and Pearl integrations.

2. **Hour-Zero Activation Runbook.** One-page printable. Top of page: Incident Commander cell, MSP after-hours, cyber-insurance FNOL number with policy number placeholder. Middle: 12-step containment runbook (disconnect, photograph, do-not-interact, isolate, preserve, call MSP, call carrier, hold communications). Bottom: explicit "DO NOT" list and the cyber-insurance pre-approval rule.

3. **Communication template drafts.** Patient breach notification letter at 7th-grade reading level with Texas-specific 60-day patient notice + Texas AG notice for 250+ Texas residents (Texas threshold is stricter than HIPAA's 500). Substitute notice and media-notice template if breach exceeds 500 individuals. Internal staff briefing script. Vendor BAA notice template. OCR breach portal narrative. Cyber-insurance FNOL script. All marked DRAFT — REVIEW REQUIRED.

4. **Post-incident bundle templates.** Timeline reconstruction template, four-factor risk-assessment worksheet, evidence chain-of-custody form, after-action review template, updated risk-analysis cover memo.

The practice now has a working IRP, the wall-poster runbook is laminated at every workstation, the communication drafts are sitting with breach counsel for review and shelving, and the next quarter's tabletop is on the calendar.

### Fast-path (standing-IRP mode) example — zero blocking input, everything from `config.yml`

**Sample input:** *"We need our incident response plan refreshed for the HIPAA risk assessment. What do you need from me?"* — nothing else provided.

> **`ASSUMED:` (derived from `config.yml` — correct any line and I'll re-render)**
> • **Profile** — single location, 4 operatories; **Dentrix Ascend** (PMS/EHR), **Dexis** (imaging), **Overjet** (AI diagnostic — PHI-touching, BAA required), **Weave** (comms + payments, BAA on file), **DentalXChange** (clearinghouse). Google Drive = **non-PHI only** per config.
> • **Roles** — Incident Commander = Dr. Anna Patel (owner); Privacy/Security Officer = office manager (config's named HIPAA officer); Comms Lead = office manager; Clinical Continuity = lead hygienist. **Alternate needed** — office manager is triple-loaded; cross-name a backup.
> • **State** — Colorado. Breach notice **≤ 30 days** (Colo. Rev. Stat. §6-1-716, stricter than HIPAA's 60); Colorado AG notice if **500+ CO residents** affected. Colorado Dental Board for any licensure-reportable angle.
> • **Overjet** → gets a BAA-inventory line and the **"AI-tool prompt-injection PHI leak"** quarterly tabletop scenario.
> • **Fill-in-blank (not blocking, added to next-step list):** MFA coverage %, EDR vendor, 24/7 monitoring, cyber-insurance carrier + limits, breach counsel on retainer, last tested-recovery date.

The four-part bundle then renders exactly as in the Texas example above, but Colorado-specific and Cherry-Creek-specific — one correction pass on the ASSUMED block, no security questionnaire before the first draft.

## Version History

- **v1.1 (2026-07-20)** — Added a **fast-path rule** to Required Input distinguishing two modes: **standing-IRP mode** (building/refreshing the plan) now has **zero blocking input** — the practice profile, roles, and state are derived from `config.yml` (Dentrix Ascend / Dexis / Overjet / Weave / DentalXChange; office manager = HIPAA officer; Colorado §6-1-716 30-day notice) and stamped as a correctable `ASSUMED:` block, with unknown posture fields rendered as fill-in-blank next-steps rather than blockers; **live-incident mode** routes the five incident facts straight to the severity tier and hour-zero runbook without re-collecting the config-derivable profile. Added a config-grounded standing-IRP fast-path example (Cherry Creek, Colorado) alongside the existing generic Texas example. Additive only; no instruction prose or guardrails removed. `last_eval_score` populated.
