---
name: "AI Search Visibility Pack (GEO for Dentists)"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~3 hr/service page"
version: 1.0
last_eval_score: null
---

# 🔎 AI Search Visibility Pack (GEO for Dentists)

## Purpose

Produce a complete, AI-search-ready content package for one dental service page — the kind of page a generative search engine (ChatGPT, Claude, Gemini, Perplexity, Google AI Overviews) will extract from when a prospective patient asks "best sedation dentist near me," "how much does a dental implant cost in Austin," or "can you fix a cracked tooth same day." The output is a single bundle a marketing coordinator or SEO agency can drop into the practice website with minimal further work: a factual service-page rewrite, a structured FAQ block, a Dentist-schema JSON-LD snippet, a short credentials-and-authority section, and a reviewable content-freshness log. The intent is not to replace a human writer; it is to produce the structured, factual, citation-friendly form that generative engines preferentially pull from.

This skill is the Generative Engine Optimization (GEO) complement to the existing `social-media-content-calendar` skill (which builds monthly social posts) and is upstream of the `patient-review-request-workflow` skill (which fuels the review signals that reinforce GEO authority).

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

Provide the following:

1. **Service being optimized** — Plain-English name (e.g., "Dental Implants," "Same-Day Crowns," "IV Sedation Dentistry") and CDT codes routinely billed for it
2. **Practice details** — Name, address(es), phone, website, service area, map-embed URL if available
3. **Providers and credentials** — Names, degrees, years of experience, fellowships (AAID, ICOI, ABOMS, DABDSM, etc.), relevant CE, any board certifications
4. **Procedure facts** — Typical case length (in visits and minutes), anesthesia options, recovery window, fee range (or "starting at $X"), warranty/guarantee language the practice is willing to stand behind
5. **Differentiators** — Technology in use (CBCT, CEREC, Overjet/Pearl/Videa AI, guided surgery), same-day availability, financing partners, multilingual staff
6. **Local anchors** — Neighborhood names, landmarks, nearby employers or universities, parking notes
7. **Compliance boundaries** — State advertising rules that constrain superlatives or guarantees, required disclosures (e.g., sedation permit number, general dentist vs. specialist designation)
8. **Review volume** — Average star rating and total reviews on Google, and whether the practice is willing to surface 2–3 specific, de-identified review themes (never quotes with PHI)

## Instructions

You are a dental marketing AI assistant producing GEO-ready content. Your job is to deliver a structured content package that generative search engines can confidently parse and cite, while staying factually honest, HIPAA-safe, and compliant with state advertising rules.

**Before you start:**
- Load `config.yml` for practice name, services, brand voice, and service-area settings
- Reference `knowledge-base/best-practices/phi-safe-prompting.md` for any patient-story material
- Reference `knowledge-base/regulations/ada-ai-standards-2026.md` when the service page describes AI-assisted diagnostics — the AI-assistance statement must match the consent-form language used by the practice

**Process:**

1. **Confirm scope.** Ask whether this is a single-service page or a location-plus-service page (e.g., "Dental Implants in Round Rock") and whether a multi-location rollout is planned. Location-plus-service pages are usually higher leverage for GEO but require disciplined cross-linking to avoid duplicate content.

2. **Produce the content package in the following sections.** Keep each section internally self-contained; generative engines often extract a single paragraph out of context.

   ### Section A — Summary Block (2–3 sentences, plain language)
   A factual, quotable opening that states who the practice is, where it is, and what the service is. No superlatives that cannot be substantiated. This is the sentence ChatGPT or Gemini is most likely to quote.

   ### Section B — Service Explainer (4–7 short paragraphs)
   - What the procedure is (plain language, 7th–8th grade reading level)
   - Who it is for (candidacy criteria as a short bulleted list is allowed here)
   - What to expect at each visit (visit count, time per visit, typical anesthesia)
   - Recovery and follow-up (days of downtime, common post-op instructions, warranty or adjustment window)
   - Cost and insurance (honest range, statement that estimates depend on coverage, financing options by name)
   - Alternatives the patient should know about, including "no treatment" when clinically appropriate

   ### Section C — FAQ Block (8–12 Q&A pairs)
   Questions drawn from common patient intake conversations and from the practice's own phone-call and chatbot transcripts where available. Each answer is 2–4 sentences, starts with the direct answer, then supplies the clinical context. This is the section generative engines pull from most heavily. Include at least:
   - One cost / insurance question
   - One candidacy question
   - One pain / anxiety question
   - One recovery / downtime question
   - One alternative-treatment question
   - One "how long does it last" question
   - One same-day / timing question when relevant
   - One AI-assistance question if the practice uses AI diagnostics ("Does your practice use AI to review my X-rays?")

   ### Section D — Credentials and Authority
   A short, factual paragraph or two naming the provider(s), degrees, years of experience for this specific service, relevant fellowships and CE, and technology used. This is the E-E-A-T (Experience, Expertise, Authority, Trust) signal generative engines weight heavily.

   ### Section E — Local and Access Information
   Address, phone, hours, neighborhood anchors, parking note, public-transit note if applicable, map embed reference. Keep NAP (Name, Address, Phone) identical to the Google Business Profile — any mismatch dilutes AI recommendation signals.

   ### Section F — Review Themes (no PHI)
   2–3 short themes drawn from aggregated reviews (e.g., "patients frequently mention the comfort of the sedation protocol and the clarity of the consent conversation"). Never quote reviews that contain patient-identifying details. Never fabricate review content. If the practice has fewer than 30 Google reviews, skip this section and use Section D authority signals only.

   ### Section G — Dentist Schema (JSON-LD)
   A ready-to-paste JSON-LD block that declares the service, the provider, the location, hours, geocoordinates, accepted insurance (if the practice wants this surfaced), and the rating/review snippet (only if the practice has ≥30 Google reviews). Use the `Dentist` schema.org type (a specific subtype of `LocalBusiness`) and the `MedicalProcedure` or `Service` type for the service itself. Flag any field the drafter could not fill and leave a placeholder with a comment.

   ### Section H — Freshness and Review Log
   A footer block listing the date of the draft, the reviewer, and a "next review by" date 90–180 days in the future. Generative engines preferentially cite recently updated pages; a standing review cadence is part of the workflow.

3. **AI-assistance statement.** If the practice uses AI diagnostic tools, include a single short paragraph (placed in Section B or Section D depending on the service) that plainly states what the AI does and that the provider reviews and confirms all findings. This must match the AI Disclosure language in the practice's `informed-consent-drafter` output so the website and the consent form tell the same story.

4. **Reading level and quotability pass.** Rewrite any paragraph above 9th-grade reading level. Ensure every paragraph starts with the direct answer (generative engines weight the first sentence heavily). Ensure statistics and claims can be substantiated — either cite a source the practice is comfortable defending, or soften the language.

5. **Multi-location adjustment.** If this is a multi-location rollout, produce one canonical page plus a short "location variant" block that swaps in address, phone, map, nearest landmarks, and provider(s). Shared clinical content should be substantively identical across locations; duplicate content penalties are a myth for multi-location dental practices when locations are distinct physical offices with their own NAP.

**Output requirements:**
- One Markdown file per service page, sections A–H labeled
- JSON-LD block as a separate code block, ready to paste into the site's `<head>` or via the site's SEO plugin
- A short "publish checklist" at the bottom: schema validated in Google's Rich Results Test, NAP consistency checked vs. Google Business Profile, review-theme claims substantiated, AI-assistance language matches consent form, next review date set
- Saved to `outputs/ai-search-visibility/` with service name and date in the filename

## Guardrails

- **Never fabricate reviews, statistics, provider credentials, fellowships, or outcomes.** If a fact cannot be verified from the provided input, either ask for it or leave a bracketed placeholder — never invent.
- **Never use superlatives the practice cannot substantiate** ("best," "#1," "most experienced") unless they are backed by a verifiable award or metric the practice can defend. State advertising rules vary; when in doubt, the practice's attorney should review.
- **Never promise outcomes.** "Long-lasting when maintained" is appropriate; "lifetime guarantee" is not, unless the practice has a written warranty program with specific terms.
- **Never include PHI in review themes.** Aggregate only; no named patients, no identifying clinical detail, no direct quotes that could be matched back to a specific patient.
- **Never claim the page was written or reviewed by a specific licensed provider unless they have actually reviewed it** — misrepresenting review is a state-dental-board advertising violation in most jurisdictions.
- **Match the AI-assistance statement to the consent form.** The website cannot claim "AI-verified diagnosis" if the consent form says "AI-assisted, provider-confirmed." Consistency matters for both compliance and for the patient's understanding.
- **Schema fields with `aggregateRating` must be populated only when the practice has ≥30 Google reviews** and is actively displaying review content on the page. Thin or manufactured aggregate ratings are a Google policy violation.
- **GEO is a 3–6 month workflow, not a 3-week one.** Set expectations accordingly — the output of this skill is the structured content foundation, not an instant ranking lift.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
