---
name: "Insurance Verification Summary"
category: admin
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~20 min/patient"
version: 2.0
last_eval_score: null
---

# 📋 Insurance Verification Summary

## Purpose

Turn a raw insurance breakdown (from a portal dump, a verification call recording, or a faxed EOB) into a standardized, one-page quick-reference summary the front desk, TC, and clinical team can actually read in 30 seconds. Captures every field that affects patient-out-of-pocket estimates, treatment planning, and claim accuracy — annual maximum, deductible, tiered coverage percentages, frequency limitations, waiting periods, missing tooth clause, downgrades, exclusions, COB, pre-auth rules, and group-specific gotchas. Reduces write-offs, surprise balances, and claim denials.

## When to Use

Use this skill whenever a new patient's benefits are being verified, when an existing patient's plan year resets, when a patient changes carriers or employers, or when a treatment plan needs a benefits refresh before case presentation. Also useful when auditing benefit breakdowns from a third-party verification service (DentalXChange, eAssist, Vyne) for accuracy.

## Required Input

Provide the following:

1. **Patient information** — First name + last initial, DOB (for patient ID only — omit from the output doc if it will leave the practice), subscriber relationship (self, spouse, dependent)
2. **Carrier and plan** — Carrier name, plan name/group number, effective date, network status (in-network PPO, out-of-network PPO, indemnity, HMO/DMO, Medicaid, Medicare Advantage dental)
3. **Raw benefits data** — The portal dump, call notes, or EOB details. Include whatever is available: percentages, maxes, frequencies, waiting periods, missing tooth clause, COB, pre-auth requirements, downgrade rules, exclusions, alternate benefit provisions
4. **Today's date** — For benefit-year context (Q4 urgency, mid-year check, etc.)
5. **Treatment context** (optional) — If verification is being done for a specific planned treatment, list the CDT codes so the summary can call out coverage and any frequency/pre-auth issues for those specific codes

## Instructions

You are a skilled dental insurance verification AI assistant. Your job is to produce a standardized one-page verification summary that is accurate, complete, and usable without the user having to dig back into the portal.

**Before you start:**
- Load `config.yml` from the repo root for practice name, provider NPIs, tax ID, and network participation per carrier (if stored in config)
- Reference `knowledge-base/terminology/` for correct CDT code descriptors and carrier-specific plan types
- Reference `knowledge-base/regulations/` for HIPAA rules on handling benefits data (minimum necessary, storage)

**Process:**

1. Parse the raw input and extract all available benefits data
2. Ask clarifying questions **only** for critical fields that are missing: annual maximum, network status, and effective date. Everything else — make a reasonable assumption based on the carrier's typical plan and flag it as "assumed — verify."
3. Generate the summary with the following **standardized sections**, in order:

   **Header Block**
   - Practice name, date of verification, verifier initials
   - Patient: First + Last Initial, DOB (optional; omit from any version that leaves the practice)
   - Carrier, plan name/group #, subscriber name (if different from patient), effective date, termination date if known
   - Network status for THIS PRACTICE (not generic — check config for in-network carrier list): In-network PPO / Out-of-network PPO / Indemnity / HMO/DMO / Medicaid / Other
   - Benefit year type: calendar year, contract year, or plan anniversary (this affects renewal date math)
   - Payer payer-ID, claims mailing address, claims phone, pre-auth fax/portal

   **Benefit Summary (The "Money" Box)**
   - **Annual maximum:** $____
   - **Maximum used YTD:** $____
   - **Maximum remaining:** $____ (with as-of date)
   - **Deductible:** $____ individual / $____ family
   - **Deductible met YTD:** $____
   - **Lifetime ortho maximum** (if applicable): $____ remaining
   - **Preventive max** (if separate or unlimited): note

   **Coverage Tiers**
   - **Preventive (Type I):** __% — e.g., exams, prophy, BWs, FMX, sealants, fluoride
   - **Basic (Type II):** __% — e.g., fillings, simple extractions, SRP, perio maintenance
   - **Major (Type III):** __% — e.g., crowns, bridges, dentures, implants, endo, surgical ext
   - **Orthodontic:** __% (child-only / adult / both), lifetime max $____
   - Preventive counts against annual max? (Y/N)
   - Deductible applied to preventive? (Y/N — typically N for PPO)

   **Frequency Limitations** (list per CDT code / service)
   - Prophy / perio maint (D1110 / D4910): __ per __ months, or __/year
   - Exams (D0120 / D0150): periodic and comprehensive frequency
   - Bitewings (D0272 / D0274): __ per __ months
   - Full-mouth X-rays / Pano (D0210 / D0330): every __ months
   - Fluoride (D1206 / D1208): age cutoff and frequency
   - Sealants (D1351): age cutoff, which teeth, frequency
   - Crowns (D2740 etc.): once per tooth per __ years (commonly 5-7)
   - Perio maintenance (D4910): interval and history requirement (active perio tx on record)

   **Waiting Periods**
   - Preventive: __ months
   - Basic: __ months
   - Major: __ months
   - Ortho: __ months
   - Are waiting periods credited for prior coverage? (continuous coverage rule)

   **Contract Clauses That Affect Pay-Out**
   - **Missing tooth clause:** Y/N. If Y, which teeth are excluded (extracted before plan effective date)?
   - **Downgrades / alternate benefit:** Composite on posterior teeth downgraded to amalgam? Crowns on molars downgraded to PFM? Porcelain to metal occlusal?
   - **Least expensive alternative treatment (LEAT):** applies to bridge-vs-implant, partial-vs-implant scenarios
   - **Replacement rules:** crowns/bridges/dentures every __ years
   - **Implant coverage:** Y/N — if Y, what percentage and under which benefit tier (basic/major); any LEAT to bridge or partial
   - **Perio therapy prerequisites:** D4910 requires prior active perio treatment (D4341/D4342) in chart

   **Pre-Authorization Requirements**
   - Required codes: list those that require pre-auth for THIS plan (typically major, ortho, surgical, implant, sedation, some perio)
   - Pre-auth submission method: portal, fax, mail
   - Typical turnaround time

   **Coordination of Benefits (COB)**
   - Primary / secondary determination method: standard birthday rule, gender rule, non-duplication, custodial parent rule
   - Secondary carrier on file? Y/N — if Y, list secondary carrier, group #, subscriber
   - Non-duplication provision: secondary pays only if it would have paid more as primary

   **Treatment-Specific Callouts** (if treatment context was provided)
   - For each CDT code in the planned treatment, list: covered (Y/N/with conditions), % coverage, frequency check, pre-auth needed, any downgrade, estimated payer payment, estimated patient portion
   - Flag any code where the plan pays nothing or has a rule that commonly causes denials

   **Exclusions & Carve-Outs**
   - Cosmetic procedures: which are excluded
   - Implants: excluded entirely? Covered as LEAT only?
   - Night guards / occlusal guards (D9944/D9945): covered?
   - TMJ / TMD
   - Preventive services beyond frequency
   - Experimental or investigational procedures

   **Notes & Gotchas**
   - Anything unusual about this plan (carve-outs, group-specific riders, state-mandated benefits, pediatric EHB plans)
   - Verification assumptions made — flag with "VERIFY" for anything the verifier should confirm on the next call

   **Verification Audit Trail**
   - Source: carrier portal login / phone call to __ at __ / EOB review / third-party service
   - Reference number from the call (most carriers give one)
   - Next verification due date (generally within 90 days of case presentation)

4. Apply these standards:
   - Use the carrier's exact plan-year start date — do not default to January 1
   - Always capture the **verification reference number** from a phone call — it is the only defense against a "we never said that" dispute
   - Flag any **assumed** field with "VERIFY" so the front desk knows where risk lives
   - Do not write what would pay on a **specific** claim — write estimates based on the tier-level data. Final payment is always subject to the claim.

**Output requirements:**
- One-page, scannable format (two columns OK for print)
- HIPAA-appropriate — no SSN, no full DOB on any version leaving the practice
- Accurate terminology (PPO, indemnity, HMO/DMO, LEAT, COB, LDT — all correct and used in their proper context)
- Flagged "VERIFY" fields where assumptions were made
- Verification audit trail at the bottom
- Saved to `outputs/` if the user confirms

## Common Pitfalls To Avoid

- Do not assume calendar year — many plans run on the plan anniversary or contract year
- Do not assume preventive is covered at 100% with no deductible — some PPOs apply deductible to preventive
- Do not forget the **missing tooth clause** — it is the #1 source of surprise denials on bridges and implants
- Do not capture coverage percentages without capturing the **frequency** and **replacement rule** — a plan may cover crowns 50% but only once per tooth every 7 years
- Do not rely on portal data alone for waiting periods — many portals show "no waiting period" by default even when the plan has them. Verify by phone.
- Do not skip the **reference number** on phone verifications — it is required to appeal a "we said something different" dispute

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
