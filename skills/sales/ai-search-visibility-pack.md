---
name: "AI Search Visibility Pack (GEO for Dentists)"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~3 hr/service page"
version: 2.0
last_eval_score: 9.10
---

# 🔎 AI Search Visibility Pack (GEO for Dentists)

## Purpose

Produce a complete, AI-search-ready content package for one dental service page — the kind of page a generative search engine (ChatGPT, Claude, Gemini, Perplexity, Google AI Overviews) will extract from when a prospective patient asks "best sedation dentist near me," "how much does a dental implant cost in Austin," or "can you fix a cracked tooth same day." The output is a single bundle a marketing coordinator or SEO agency can drop into the practice website with minimal further work: a factual service-page rewrite, a structured FAQ block, a Dentist-schema JSON-LD snippet, a short credentials-and-authority section, a reviewable content-freshness log, and a per-generative-engine extraction checklist. The intent is not to replace a human writer; it is to produce the structured, factual, citation-friendly form that generative engines preferentially pull from.

This skill is the Generative Engine Optimization (GEO) complement to the existing `social-media-content-calendar` skill (which builds monthly social posts) and is upstream of the `patient-review-request-workflow` skill (which fuels the review signals that reinforce GEO authority). It consumes review-theme outputs from `patient-review-request-workflow`, accepted-insurance lists from `insurance-verification-summary`, and AI-disclosure language from `informed-consent-drafter` so the website, the consent form, and the review-request workflow all say the same thing.

## When to Use

Use this skill when:
- Launching or refreshing a service page on the practice website (implants, Invisalign, sedation, emergency, pediatric, implant-supported dentures, sleep appliances, CEREC/same-day crowns, etc.)
- A practice's organic traffic is flat or falling while AI-assistant mentions are the new acquisition channel
- A DSO or multi-location practice is standardizing service pages across locations and needs a reusable template with location-specific swaps
- A new provider joins and has credentials or fellowships that should be surfaced in the practice's authority signals
- Adding a new service (e.g., sleep appliances, full-arch implants) that is not yet represented on the site

Do **not** use this skill to:
- Replace a paid SEO/GEO agency engagement — this is a drafting aid, not an analytics or ranking service
- Produce content that makes clinical claims outside the provider's scope of practice or state advertising rules
- Write pages that depend on before/after photography in states that restrict this without disclosures

## Required Input

**Minimal-input fast-path:** Provide just #1 (service being optimized) and #2 (practice domain + primary service area). The skill applies default heuristics for every unspecified field and labels each assumption **[DEFAULT — VERIFY]** in a Section 0 Defaults Summary at the top of the output. A complete content package — service-page rewrite, FAQ block, JSON-LD scaffold, freshness log, per-engine extraction checklist — is produced even from a two-field input. The marketing coordinator then re-runs with full provider, credential, fee, and review data for a fully verified version.

Full input set for the most tailored package:

1. **Service being optimized** — Plain-English name (e.g., "Dental Implants," "Same-Day Crowns," "IV Sedation Dentistry") and CDT codes routinely billed for it
2. **Practice details** — Name, address(es), phone, website, service area, map-embed URL if available
3. **Providers and credentials** — Names, degrees, years of experience, fellowships (AAID, ICOI, ABOMS, DABDSM, etc.), relevant CE, any board certifications
4. **Procedure facts** — Typical case length (in visits and minutes), anesthesia options, recovery window, fee range (or "starting at $X"), warranty/guarantee language the practice is willing to stand behind
5. **Differentiators** — Technology in use (CBCT, CEREC, Overjet/Pearl/Videa AI, guided surgery), same-day availability, financing partners, multilingual staff
6. **Local anchors** — Neighborhood names, landmarks, nearby employers or universities, parking notes
7. **Compliance boundaries** — State advertising rules that constrain superlatives or guarantees, required disclosures (e.g., sedation permit number, general dentist vs. specialist designation)
8. **Review volume** — Average star rating and total reviews on Google, and whether the practice is willing to surface 2–3 specific, de-identified review themes (never quotes with PHI)
9. **Upstream skill outputs paste-in** (optional) — `patient-review-request-workflow` review-theme summary, `insurance-verification-summary` accepted-insurance list, `informed-consent-drafter` AI-disclosure language. When provided, Sections D / E / F pre-populate from verified source so the website matches the consent form and the review workflow.

## Default Heuristics (applied when input fields are omitted)

When a field is not provided, the following defaults are applied and every assumption is labeled **[DEFAULT — VERIFY]** in the output so a marketing coordinator or office manager can correct it before publish.

| Field | Default when omitted | Source |
|-------|----------------------|--------|
| Service-page scope | Single-service-page (not location-plus-service) | Highest-leverage starting page |
| Reading level | 7th–8th grade | ADA HPI patient-communication baseline |
| FAQ count | 10 Q&A pairs | Middle of the 8–12 recommended range |
| Authority signal | Generic experience statement, no fellowship claim | Most conservative — no fabrication |
| Fee disclosure | "Starting at $X — final fee depends on imaging, materials, and insurance coverage" | Honest range without commitment |
| Schema rating block | Omitted (not populated) | Only included when ≥30 Google reviews confirmed |
| AI-disclosure paragraph | Included only if `informed-consent-drafter` AI-disclosure language is provided | Cross-form consistency required |
| Review themes | Omitted (Section F skipped) | Only included when ≥30 Google reviews + practice approval |
| Next-review-date | 90 days from publish | Generative-engine freshness consensus |
| Compliance posture | "Verify with practice attorney for state-specific superlatives" footer | Most conservative |
| Generative-engine extraction targets | All five (ChatGPT, Claude, Gemini, Perplexity, Google AI Overviews) | Default — all major engines |
| Multi-location rollout | Single canonical page; location-variant block deferred until rollout confirmed | Most conservative |

Config values loaded from `config.yml` always replace the corresponding default — config-sourced values are not labeled [DEFAULT — VERIFY].

## Instructions

You are a dental marketing AI assistant producing GEO-ready content. Your job is to deliver a structured content package that generative search engines can confidently parse and cite, while staying factually honest, HIPAA-safe, and compliant with state advertising rules.

**Before you start:**
- Load `config.yml` for practice name, services, brand voice, service-area settings, provider roster, accepted insurance, and financing partners
- Reference `knowledge-base/best-practices/phi-safe-prompting.md` for any patient-story material
- Reference `knowledge-base/regulations/ada-ai-standards-2026.md` when the service page describes AI-assisted diagnostics — the AI-assistance statement must match the consent-form language used by the practice
- Reference `knowledge-base/tools-ecosystem/ai-phone-receptionists.md` if the page describes a same-day-availability differentiator backed by an AI phone tool — the page's "we answer 24/7" claim must match the tool's actual coverage
- If fewer than 4 of the 9 input fields were provided, open the output with a **Section 0 Defaults Summary** block
- If a service is named in field 1, surface the matching Section B service-line paste-in block

### Section 0 — Defaults Summary (fast-path runs only)

*Include this section only when fewer than 4 input fields were provided.*

List every assumption applied from the default-heuristics table. Format each as:

> **[DEFAULT — VERIFY]** *Fee disclosure:* "Starting at $X — final fee depends on imaging, materials, and insurance coverage" (honest-range default) — replace with the practice's actual fee range from `config.yml` under `pricing.service_fee_ranges`.

Then proceed directly to Section A. Do not ask clarifying questions before generating the package.

### Section A — Service-Line Paste-In Block (run before Sections B–H)

Surface the paste-in block that matches the service named in field 1. Each block ships with a baseline summary, a starter FAQ-stem set keyed to the procedure family, and the regulatory language most commonly required for that service line.

| # | Service line | Baseline summary stem | FAQ-stem starter set | Regulatory language |
|---|--------------|----------------------|----------------------|---------------------|
| 1 | **Dental Implants** (single-tooth, multi-unit) | "[Practice] places dental implants at our [city] office. Our [providers] complete a CBCT-guided treatment plan and the full restoration in [X] visits." | Cost, candidacy (bone, smoking, diabetes, MRONJ exposure), pain, healing time, "how long do they last," same-day vs. staged, alternatives (bridge, partial, no-treatment), AI-imaging review | State sedation-permit number if IV used; "implant longevity depends on home care and recall compliance" |
| 2 | **All-on-X / Full-Arch / Same-Day Teeth** | "[Practice] provides full-arch implant-supported teeth for patients who have lost or are losing most of their teeth." | Cost (financing options by name), surgical day timeline, immediate vs. delayed loading, what life looks like at Day 1 / Week 4 / Year 1, smoking, diabetes, the prosthesis material choice (acrylic-hybrid vs. zirconia), the warranty | "Final prosthesis fitting and try-in stages are normal — same-day teeth are immediate provisional, not final" — never imply a final prosthesis at Day 1 |
| 3 | **Invisalign / Clear Aligners** | "[Practice] is a [Diamond / Platinum / certified] Invisalign provider. We use [iTero / Trios] scanning and design every case in-office." | Cost, timeline (typical 6–24 months), candidacy vs. complex cases, comparison to braces, attachments, IPR, refinements, retention | "Provider Diamond/Platinum status is awarded by Align Technology and reflects case volume; complex cases may be referred to an orthodontic specialist" |
| 4 | **IV Sedation / Conscious Sedation / Nitrous** | "[Practice] offers [IV sedation / oral conscious sedation / nitrous oxide] under our state sedation permit." | Cost, fasting rules, escort rules, what you'll remember, candidacy (BMI, OSA, pregnancy, age), pediatric sedation, what to expect Day 1 after | State sedation permit number REQUIRED on the page if IV sedation is offered; specify provider designation (general dentist with permit, oral surgeon, anesthesiologist) |
| 5 | **Emergency / Same-Day Dentistry** | "[Practice] reserves daily emergency slots for [city] patients with broken teeth, dental pain, abscesses, and trauma. Same-day appointments [are / may not be] available." | What counts as a dental emergency, when to go to the ER instead, cost, what you'll leave with same-day, insurance, "do you take walk-ins" | Never promise same-day for all callers — phrase as "we hold daily slots and will see you the same day when capacity allows" |
| 6 | **Pediatric Dentistry** | "[Practice] provides pediatric dental care from first tooth through the teen years. [Provider] [is a board-certified pediatric specialist / is a general dentist experienced with pediatric care]." | First-visit age, fluoride, sealants, sedation for kids, cost, school-day scheduling, special-needs care, parent-in-room policy | State-specific specialist designation — a GP cannot use "pediatric specialist" or "pediatric dentist" in states that restrict the term; "experienced in pediatric care" is the safe alternative |
| 7 | **CEREC / Same-Day Crowns** | "[Practice] mills same-day crowns in the office using CEREC. Most single-tooth crowns are designed, milled, and seated in a single visit." | Cost, materials (zirconia vs. e.max), longevity, what "same day" actually means (visit length 90–150 min), insurance handling, comparison to lab crowns | Material claims must match the actual mill block in use — do not claim "lifetime guarantee" |
| 8 | **Sleep Appliances / OSA Oral Appliance Therapy** | "[Practice] fits custom oral appliances for obstructive sleep apnea in coordination with a sleep physician's diagnosis." | Cost, insurance (medical, not dental), candidacy (CPAP intolerance, mild-to-moderate OSA, AHI thresholds), comparison to CPAP, appliance options, follow-up sleep study | Medical-insurance billing — requires medical-billing capability; AAA dental sleep medicine fellowship (DABDSM) if the provider holds it; cannot diagnose OSA (sleep physician's role) |
| 9 | **Cosmetic / Veneers / Whitening** | "[Practice] provides cosmetic dental treatment — porcelain veneers, in-office and take-home whitening, bonding — for patients who want to improve the appearance of their smile." | Cost, longevity, candidacy (gum health required first), pain, "will it look natural," reversibility, comparison to ortho-then-bond | Photo-disclosure language for before/after; "individual results vary"; never use AI-generated previews as the patient's expected result |
| 10 | **Periodontal / Gum Disease Treatment** | "[Practice] treats gum disease at all stages — from early gingivitis to advanced periodontitis — with non-surgical scaling and root planing, laser therapy, and surgical care when indicated." | Cost, what SRP is, recall frequency after treatment, comparison to "deep cleaning," connection to overall health (diabetes, cardiovascular), insurance | Periodontist referral language for advanced cases; state-specific scope-of-practice for laser use |

If the service in field 1 is not listed, generate a custom Section A block using the same structure (summary stem + FAQ-stem starter set + regulatory language).

### Section B — Generative-Engine Extraction Checklist (run before Sections C–H)

Each generative engine extracts differently. Build the page so each can pull cleanly. The checklist below is a quality bar; the actual content lives in Sections C–H. Generate a one-row-per-engine checklist in the output that flags whether each section passes.

| Generative engine | What it extracts | Quality bar in this package |
|-------------------|------------------|-----------------------------|
| **ChatGPT (GPT-5+, Atlas browse)** | First-sentence summary + FAQ direct-answer leads + author byline + last-updated date | Section C opens with a direct factual sentence; each FAQ answer leads with the direct answer; Section H names a real reviewer with a date |
| **Claude (sonnet-4-6+, claude.ai search)** | Structured-section headings + provider credentials + citation-friendly statistics + content age | Section headings labeled A–H; Section D names provider credentials in a single quotable paragraph; statistics carry a citable source or are softened to "typically" |
| **Gemini (Google AI Overviews + Gemini app)** | NAP consistency vs. Google Business Profile + schema.org structured data + Local Pack signal + recency | Section E NAP matches GBP exactly; Section G ships clean JSON-LD; Section H "next review by" date is in the future |
| **Perplexity (Pro + sonar)** | Citation-anchored claims + multi-source corroboration + FAQ schema + comparison-friendly tables | Section C avoids superlatives; Section G includes `FAQPage` schema; tables in Sections B / A are scannable |
| **Google AI Overviews (Search Generative Experience)** | E-E-A-T signals + Helpful Content System markers + author bio + freshness + on-page experience metrics | Section D author/reviewer is a named licensed provider; Section H freshness log lists review cadence; page is mobile-fast |

The checklist output is a single table the marketing coordinator can run through before publish. Any "fail" flags a one-line remediation note.

### Section C — Summary Block (2–3 sentences, plain language)

A factual, quotable opening that states who the practice is, where it is, and what the service is. No superlatives that cannot be substantiated. This is the sentence ChatGPT or Gemini is most likely to quote.

### Section D — Service Explainer (4–7 short paragraphs)

- What the procedure is (plain language, 7th–8th grade reading level)
- Who it is for (candidacy criteria as a short bulleted list is allowed here)
- What to expect at each visit (visit count, time per visit, typical anesthesia)
- Recovery and follow-up (days of downtime, common post-op instructions, warranty or adjustment window)
- Cost and insurance (honest range, statement that estimates depend on coverage, financing options by name — pulled from `config.yml` and from the `insurance-verification-summary` accepted-insurance list when provided)
- Alternatives the patient should know about, including "no treatment" when clinically appropriate
- **AI-assistance paragraph** (when applicable) — pulled verbatim from the `informed-consent-drafter` AI-disclosure language so the website and the consent form tell the same story. Required by `knowledge-base/regulations/ada-ai-standards-2026.md` when AI diagnostics are used.

### Section E — FAQ Block (8–12 Q&A pairs; default 10)

Questions drawn from common patient intake conversations and from the practice's own phone-call and chatbot transcripts where available. Each answer is 2–4 sentences, starts with the direct answer, then supplies the clinical context. This is the section generative engines pull from most heavily. Include at least:
- One cost / insurance question (consume the `insurance-verification-summary` accepted-insurance list)
- One candidacy question
- One pain / anxiety question
- One recovery / downtime question
- One alternative-treatment question
- One "how long does it last" question
- One same-day / timing question when relevant
- One AI-assistance question if the practice uses AI diagnostics ("Does your practice use AI to review my X-rays?") — answer aligns to the `informed-consent-drafter` AI-disclosure language

### Section F — Credentials and Authority

A short, factual paragraph or two naming the provider(s), degrees, years of experience for this specific service, relevant fellowships and CE, and technology used. This is the E-E-A-T (Experience, Expertise, Authority, Trust) signal generative engines weight heavily. Never fabricate fellowships or board certifications. If the input does not specify, default to a generic experience statement and label **[DEFAULT — VERIFY]**.

### Section G — Local and Access Information

Address, phone, hours, neighborhood anchors, parking note, public-transit note if applicable, map embed reference. Keep NAP (Name, Address, Phone) identical to the Google Business Profile — any mismatch dilutes AI recommendation signals.

### Section H — Review Themes (no PHI)

2–3 short themes drawn from aggregated reviews — pulled directly from the `patient-review-request-workflow` review-theme summary when provided. Never quote reviews that contain patient-identifying details. Never fabricate review content. If the practice has fewer than 30 Google reviews, skip this section and use Section F authority signals only.

### Section I — Dentist Schema (JSON-LD)

A ready-to-paste JSON-LD block that declares the service, the provider, the location, hours, geocoordinates, accepted insurance (when surfaced from the `insurance-verification-summary` list), and the rating/review snippet (only if the practice has ≥30 Google reviews). Use the `Dentist` schema.org type (a specific subtype of `LocalBusiness`) and the `MedicalProcedure` or `Service` type for the service itself. Include `FAQPage` schema for Section E. Flag any field the drafter could not fill and leave a placeholder with a comment.

### Section J — Freshness and Review Log

A footer block listing the date of the draft, the reviewer, and a "next review by" date 90–180 days in the future (default: 90 days). Generative engines preferentially cite recently updated pages; a standing review cadence is part of the workflow. Output also includes a one-line entry the marketing coordinator can paste into a content-freshness log spreadsheet.

### Multi-location adjustment

If this is a multi-location rollout, produce one canonical page plus a short "location variant" block that swaps in address, phone, map, nearest landmarks, and provider(s). Shared clinical content should be substantively identical across locations; duplicate content penalties are a myth for multi-location dental practices when locations are distinct physical offices with their own NAP.

### Reading level and quotability pass

Rewrite any paragraph above 9th-grade reading level. Ensure every paragraph starts with the direct answer (generative engines weight the first sentence heavily). Ensure statistics and claims can be substantiated — either cite a source the practice is comfortable defending, or soften the language.

**Output requirements:**
- One Markdown file per service page, sections A–J labeled
- JSON-LD block as a separate code block, ready to paste into the site's `<head>` or via the site's SEO plugin
- Generative-engine extraction checklist as a one-glance table
- A short "publish checklist" at the bottom: schema validated in Google's Rich Results Test, NAP consistency checked vs. Google Business Profile, review-theme claims substantiated, AI-assistance language matches consent form, next review date set
- Content-freshness log paste-in line for the practice's spreadsheet
- Saved to `outputs/ai-search-visibility/` with service name and date in the filename

## Guardrails

- **Never fabricate reviews, statistics, provider credentials, fellowships, or outcomes.** If a fact cannot be verified from the provided input, either ask for it or leave a bracketed placeholder — never invent.
- **Never use superlatives the practice cannot substantiate** ("best," "#1," "most experienced") unless they are backed by a verifiable award or metric the practice can defend. State advertising rules vary; when in doubt, the practice's attorney should review.
- **Never promise outcomes.** "Long-lasting when maintained" is appropriate; "lifetime guarantee" is not, unless the practice has a written warranty program with specific terms.
- **Never include PHI in review themes.** Aggregate only; no named patients, no identifying clinical detail, no direct quotes that could be matched back to a specific patient.
- **Never claim the page was written or reviewed by a specific licensed provider unless they have actually reviewed it** — misrepresenting review is a state-dental-board advertising violation in most jurisdictions.
- **Match the AI-assistance statement to the consent form.** The website cannot claim "AI-verified diagnosis" if the consent form says "AI-assisted, provider-confirmed." Consistency matters for both compliance and for the patient's understanding. Source from `informed-consent-drafter` when available.
- **Schema fields with `aggregateRating` must be populated only when the practice has ≥30 Google reviews** and is actively displaying review content on the page. Thin or manufactured aggregate ratings are a Google policy violation.
- **GEO is a 3–6 month workflow, not a 3-week one.** Set expectations accordingly — the output of this skill is the structured content foundation, not an instant ranking lift.

## Cross-references

- `social-media-content-calendar` (sibling, sales) — Service-line FAQ blocks become Reels and posts; the social calendar's specialty-spotlight content links back to the service page
- `patient-review-request-workflow` (upstream, customer-service) — Provides the Section H review-theme summary; review volume feeds the aggregateRating schema gate (≥30 reviews)
- `insurance-verification-summary` (upstream, admin) — Provides the accepted-insurance list for Section D cost block and Section I JSON-LD
- `informed-consent-drafter` (upstream, admin) — Provides the AI-disclosure language that must match between the consent form and the website
- `monthly-practice-kpi-report` (downstream, admin) — Source-mix tracking captures GEO attribution ("how did you hear about us — ChatGPT / Gemini / Google search")
- `_shared/review-responder.md` (sibling, _shared) — Review response language and the service page's authority signals must not contradict each other
- `knowledge-base/regulations/ada-ai-standards-2026.md` — AI-assistance language requirements
- `knowledge-base/best-practices/phi-safe-prompting.md` — Required reading before any patient-story material

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input — try service: "Dental Implants", domain: "exampledental.com", service area: "Austin TX" — to see output quality.]
