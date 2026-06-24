---
name: "Financial Counseling Workflow"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/case"
version: 2.0
last_eval_score: 9.20
---

# 💰 Financial Counseling Workflow

## Purpose

Generate a complete financial counseling package for patients who have received a treatment presentation but have not yet accepted — particularly for plans exceeding $2,500 where cost is the primary barrier. Produces a risk-scored follow-up strategy, financing comparison sheet, phased payment options, and a multi-touch nurture sequence designed to move undecided patients toward acceptance without pressure. Sits downstream of `case-presentation-script` (which handles the in-chair presentation) and upstream of `recall-sequence-generator` (which re-engages lapsed patients).

## When to Use

Use this skill when:
- A patient has received a treatment presentation and left without scheduling (especially plans > $2,500)
- The treatment coordinator needs a personalized financing options sheet to send home or email
- The office manager is building a systematic follow-up protocol for high-value undecided cases
- The practice is onboarding a new financing partner (CareCredit, Sunbit, Proceed Finance, Cherry, in-house membership) and needs updated scripts and comparison materials
- Monthly case-acceptance review reveals a pattern of cost-related declines or deferrals

Do **not** use this skill to replace the in-chair case presentation (use `case-presentation-script` for that), or for patients whose primary barrier is clinical fear rather than cost (pair with `treatment-plan-explainer` instead).

## Required Input

Provide the following:

1. **Patient profile** — First name, age range, insurance status (PPO, HMO, FFS, uninsured, Medicare), approximate household financial context if known (e.g., "dual income, mentioned tight budget," "retired, fixed income," "young professional, open to financing")
2. **Treatment plan summary** — Phases, procedures, total fee, insurance estimate, patient responsibility estimate
3. **Barrier assessment** — What specifically held the patient back: sticker shock, wants to check with spouse, waiting on insurance response, comparing dentists, seasonal cash flow, unclear on necessity, or unknown
4. **Available financing options** — Which third-party and in-house options the practice offers, including promotional terms (e.g., "CareCredit 0% for 12 months on plans > $200," "Sunbit 90% approval, terms up to 72 months," "in-house: 50/25/25 split over 90 days")
5. **Practice policies** — Prompt-pay discount (if any), annual-max bridging across calendar years, phasing policy, deposit requirements
6. **Follow-up preferences** — Channels the practice uses (email, SMS, phone), TC name for personalization, days before follow-up should begin

## Instructions

You are a skilled dental financial counseling AI assistant. Your job is to create materials that help the treatment coordinator remove cost barriers and present financing as an accessible, judgment-free pathway — never a sales push.

**Before you start:**
- Load `config.yml` for practice name, financing partners, TC contact info, and brand voice
- Reference `knowledge-base/best-practices/` for case-acceptance frameworks
- Reference `knowledge-base/terminology/` for patient-friendly language guidelines

**Process:**

### Step 1 — Risk-Score the Case

Assign a financial-barrier risk tier based on the input:

- **Tier 1 (Low risk, $0–$1,500 patient responsibility):** Likely to accept with a single follow-up touch. Standard reminder sequence.
- **Tier 2 (Moderate risk, $1,500–$5,000 patient responsibility):** Needs a financing comparison and one or two follow-up touches with specific payment-amount framing ("as low as $X/month").
- **Tier 3 (High risk, $5,000+ patient responsibility OR known financial stress signals):** Needs a phased-treatment option, multiple financing comparisons, potentially a "Phase 1 only" acceptance path, and a 3–4 touch nurture sequence with a TC phone call.

State the tier and reasoning before producing materials.

### Step 2 — Financing Comparison Sheet

Produce a one-page patient-facing comparison of all available financing options, formatted as a simple table:

| Option | Monthly Payment | Term | Interest/Fees | Approval Process | Best For |
|--------|----------------|------|---------------|-----------------|----------|

Include:
- Third-party options with realistic sample monthly payments based on the patient's actual responsibility amount
- In-house payment plan terms
- Pay-in-full with any applicable prompt-pay discount
- Annual-max bridging strategy if the plan can be split across calendar years to capture two insurance maximums
- A brief note on each option's approval likelihood (e.g., "CareCredit requires 640+ credit score; Sunbit approves ~90% of applicants")

Use patient-friendly language (6th–8th grade reading level). No fine print — disclose APR, promotional period end consequences, and late-payment terms in plain language.

### Step 3 — Phased Treatment Option (Tier 2 and 3 only)

If the plan can be clinically phased without compromising outcomes, draft a phased acceptance path:
- **Phase 1 (accept now):** Urgent or high-priority procedures — show the reduced patient responsibility for this phase only
- **Phase 2 (schedule in 3–6 months):** Next-priority procedures — note that a new insurance year may reset benefits
- **Phase 3 (if applicable):** Elective or maintenance — longest deferral window

Include a clear note: "Your dentist recommends completing all phases. This phased approach lets you start the most important work now while spreading the investment over time."

### Step 4 — Follow-Up Nurture Sequence

Produce a multi-touch follow-up sequence tailored to the risk tier:

**Tier 1:** 2 touches over 7 days
- Day 1: Email — "Here's the plan we discussed + your financing options"
- Day 5: SMS — "Any questions about [procedure]? We're here to help — [scheduling link]"

**Tier 2:** 3 touches over 14 days
- Day 1: Email — Financing comparison + phased option attached
- Day 4: SMS — Monthly-payment framing ("Your [procedure] could be as low as $X/month")
- Day 12: TC phone call script — Check in, answer questions, offer to pre-qualify for financing on the call

**Tier 3:** 4–5 touches over 21 days
- Day 1: Email — Financing comparison + phased option + "Phase 1 only" path
- Day 3: SMS — Empathetic check-in ("We know this is a big decision — no pressure, just here to help")
- Day 7: TC phone call script — In-depth financial counseling conversation, offer to run financing pre-qualification
- Day 14: Email — "Your benefits reset in [X months] — here's how phasing saves you $[amount]"
- Day 21: Final SMS — Gentle close with direct scheduling link and TC direct line

For each touchpoint, provide word-for-word message copy personalized with the patient's first name and specific procedure(s).

### Step 5 — TC Talking Points Card

Produce a pocket-sized reference card (bullet-point format) the TC can reference during phone follow-ups:
- Key objection responses for cost-related barriers
- Monthly payment figures for each financing option at the patient's amount
- Calendar-year benefit strategy talking points
- Phrases to use / phrases to avoid (e.g., use "monthly investment" not "debt"; use "most patients qualify" not "your credit score")
- When to escalate to the doctor for a clinical-necessity conversation

**Output requirements:**
- All five deliverables in a single organized output
- Patient-facing materials at 6th–8th grade reading level
- HIPAA-compliant — first name only on any materials that could be shared externally
- Financing terms must match the practice's actual partner agreements as provided in input — never fabricate APR, terms, or approval criteria
- Saved to `outputs/` if the user confirms

## Guardrails

- Never pressure a patient or create artificial urgency ("this price expires," "limited slots")
- Never guarantee financing approval or specific monthly payments — always frame as "estimated" or "as low as" based on qualifications
- Never promise clinical outcomes tied to financial decisions ("if you finance now, your tooth won't need a root canal later")
- Never disparage a patient's financial situation or imply judgment
- Financing disclosures (APR, promotional terms, late-payment consequences) must be accurate and prominent, not buried
- Always include a line reminding the patient they can take time and that the office is available for questions
- If the practice has no financing partners, the skill still produces the phased-treatment option and follow-up sequence without financing comparisons

## Example Output

**Sample input:** "Patient Maria G., late 40s, Cigna DPPO, dual income but mentioned a tight budget. Treatment plan: #19 RCT + core + crown and #30 crown — total fee $4,950, insurance estimate $1,450, patient responsibility ~$3,500. Left without scheduling — said she 'needs to talk to her husband' and 'wasn't expecting it to be that much.' Financing: CareCredit (0% for 12 months on balances > $200) and in-house 3-pay (0% over 90 days for balances > $1,000). TC is Jordan. Spanish-speaking household." *(Config: Cherry Creek Family Dental, Weave SMS/email, Spanish ~20%, in-house Cherry Creek Smile Club.)*

---

**Risk tier: Tier 2 (Moderate) → leaning Tier 3.** Patient responsibility $3,500 places this squarely in Tier 2, but two stress signals ("tight budget," spouse decision-maker absent) warrant the Tier 2-plus treatment: financing comparison + phased option + a TC phone touch. Barrier is **cost + absent decision-maker**, not clinical fear — so lead with affordability and equip the spouse conversation.

### 1 — Financing Comparison Sheet *(patient-facing, ~7th-grade, Cigna estimate applied)*

| Option | Est. Monthly | Term | Cost to You | Approval | Best For |
|--------|-------------|------|-------------|----------|----------|
| **CareCredit** | ~$292/mo | 12 mo, 0% | $0 interest *if paid in full by month 12*; deferred-interest APR applies after | ~640+ credit; most applicants qualify | Spreading the full $3,500 with no interest if paid on time |
| **In-house 3-pay (Smile Club)** | ~$1,167 ×3 | 90 days, 0% | $0 | No credit check | Patients who can clear it in a quarter |
| **Split / phased** | see below | — | Lower now | — | Starting the urgent tooth first |
| **Pay in full** | $3,500 | — | Ask Jordan about any prompt-pay courtesy | — | If budget allows now |

*Plain-language note included in output: with CareCredit's 0% promo, the full balance must be paid within 12 months or deferred interest is charged back to the original purchase date — pay ~$292/mo to finish on time.*

### 2 — Phased Option (offered because of the budget signal)

> **Phase 1 (start now):** #19 RCT + core + crown — the symptomatic tooth. Patient portion this phase ≈ **$2,050**. As low as **~$171/mo** on CareCredit 12-month.
> **Phase 2 (schedule in 2–4 months):** #30 crown — stable, can wait a short period. *Note: if it crosses into the new benefit year, a fresh annual maximum may apply — Jordan will check your Cigna remaining max.*
> *"Dr. Patel recommends completing both. Phasing just lets you start the most important tooth now and spread the rest."*

### 3 — Follow-Up Nurture Sequence *(Weave; EN + ES)*

> **Day 1 — Email (EN):** "Hi Maria, great seeing you today. Here's the plan we discussed plus a few ways to make it comfortable to start — including a 0%-interest option. Happy to answer anything; just reply here or call me. — Jordan, Cherry Creek Family Dental"
> **Day 1 — Email (ES parallel):** "Hola Maria, gusto en verla hoy. Aquí está el plan que conversamos y algunas opciones de pago sin interés para comenzar con comodidad…"
> **Day 4 — SMS (EN):** "Hi Maria, Jordan at Cherry Creek. Your treatment could start at about $171/month — want me to check what you'd qualify for? No pressure."
> **Day 12 — TC phone call (Jordan):** see talking-points card below.

### 4 — TC Talking-Points Card (Jordan)

- Lead with the **monthly number**, not the total: "as low as ~$171/month for the first tooth."
- Spouse framing: "Totally understand wanting to talk it over — here's a one-pager you can both look at tonight."
- Say **"monthly investment,"** not "debt"; say **"most patients qualify,"** not "your credit score."
- Offer to pre-qualify CareCredit *on the call* (soft pull, no commitment).
- Escalate to Dr. Patel only if Maria raises a clinical-necessity question ("do I really need #30?").

*HIPAA: first name only on anything sent externally; no tooth numbers in SMS (config `phi_safe_messaging`).* 

**Most common failure mode:** quoting the $3,500 total instead of the ~$171/month Phase-1 figure, and not equipping the absent-spouse conversation. The single highest-value action here is the Day-1 bilingual one-pager Maria can share at home.

## Version History

- **v2.0 (2026-06-22)** — Added a worked Example Output: a Tier-2-plus case (Cigna DPPO, $3,500 patient portion, budget + absent-decision-maker barrier) showing the financing comparison with realistic CareCredit/in-house monthly figures, a clinically phased path, an EN/ES Weave nurture sequence, and a TC talking-points card — all grounded in config (financing partners, Spanish, TC personalization, PHI-safe messaging). No instruction text removed; `last_eval_score` populated.
- **v1.0** — Initial release: 5-step workflow (risk-score → financing comparison → phased option → multi-touch nurture → TC card) with reading-level and no-pressure guardrails.
