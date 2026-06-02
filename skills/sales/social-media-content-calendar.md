---
name: "Social Media Content Calendar"
category: sales
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~2 hr/month"
version: 3.0
last_eval_score: 9.10
---

# 📱 Social Media Content Calendar

## Purpose

Produce a fully drafted month of dental-practice social content that is calibrated to the practice's specialty mix, patient demographics, service area, state advertising rules, and HIPAA/photography boundaries — not a generic "dental awareness" calendar. Output is ready for a front-desk coordinator or outside marketing vendor to schedule with minimal editing: per-platform post mix (Instagram feed, Reels, Stories, Facebook, Google Business Profile, TikTok if applicable), ready-to-paste captions with hashtag sets, visual direction, compliance flags (before/after photo rules by state, named-patient consent status, ADA scope-of-practice claims), scheduler-tool export formats, and a small metrics block so the practice can tell whether the calendar is working.

Pairs with the `patient-review-request-workflow` skill (review volume feeds GBP authority signals) and the `ai-search-visibility-pack` skill (service-page GEO content reinforces social and vice versa). Consumes review-theme summaries from `patient-review-request-workflow` (anonymized themes become social posts), service-page FAQ blocks from `ai-search-visibility-pack` (FAQ stems become Reels and carousel posts), and source-mix tracking from `monthly-practice-kpi-report` (closes the attribution loop on what's actually working).

## When to Use

Use this skill when:
- Building or refreshing a monthly content calendar (most efficient run cadence)
- Launching a new service line (Invisalign, implants, sedation, sleep appliances) and the calendar needs a 4–6-week ramp
- Onboarding a new provider whose personal brand and specialty should surface in the feed
- A seasonal push (back-to-school pediatric exams, Q4 "use your benefits," holiday smile refresh)
- Refreshing a calendar after a compliance review flagged before/after photos, testimonials with patient names, or superlative advertising claims
- Adapting a national-brand DSO calendar to a specific location's demographic mix

Do **not** use this skill to:
- Draft the practice's public review responses (use the `review-responder` dental override)
- Produce paid-ad creative with ad-spend budgets (this skill produces organic social; paid needs landing-page conversion architecture and is a separate engagement)
- Post patient identifiers, named testimonials, or recognizable before/after photos without a documented, written HIPAA-compliant authorization on file

## Required Input

**Minimal-input fast-path:** Provide just #3 (month and year) and the skill loads everything else from `config.yml` and the default-heuristics table. Output is a complete, ready-to-schedule month with `[DEFAULT — VERIFY]` labels on every assumption — the front-desk coordinator can re-run with custom priorities, demographic snapshots, and content-on-hand inventory for a refined version.

Full input set for the most tailored calendar:

1. **Practice profile** — Name, location(s), specialty mix (GP / pediatric / perio / endo / ortho / OMFS / prosth / implants / sedation / sleep), ownership model (solo, group, DSO), number of providers, years in business, any recent awards or fellowships the team holds *(loaded from `config.yml` when omitted)*
2. **Patient demographic snapshot** — Primary age bands (pediatric / young adult / working-age / senior), family vs. professional-majority, insurance mix (PPO-heavy / FFS / Medicaid-participating), primary languages at the front desk, neighborhood anchors *(loaded from `config.yml` when omitted)*
3. **Time period and volume** — Month and year, posts per week per platform (default: 3 IG feed + 2 Reels + 4 Stories + 3 FB + 2 GBP posts/week; scale down if the practice has no full-time marketing support)
4. **Priorities for the month** — New-service launch, seasonal push, awareness-month anchor, provider spotlight, hiring push, charity or community event, or routine nurture *(default: matched to month-anchor table below)*
5. **Compliance boundaries** — State advertising restrictions (superlatives, "specialist" language for GPs, testimonial disclosures), state rules on before/after photography and consent, DSO brand guidelines if applicable *(loaded from `config.yml` and `knowledge-base/regulations/` when omitted)*
6. **Content-on-hand inventory** — Existing photo/video library themes (team headshots, office tour B-roll, procedure footage, any pre-authorized before/after pairs), upcoming events or photo-shoot days, vendor-provided stock rights *(default: minimal — flagged shoot list at top of output)*
7. **Voice and tone defaults** — From `config.yml`; if the practice has posted before, a handful of top-performing captions and a handful of worst-performing ones to calibrate to
8. **Scheduler tool in use** — Later / Buffer / Planable / Metricool / Sprout / Hootsuite / direct-native — determines which Section D scheduler-export block is surfaced
9. **Upstream skill outputs paste-in** (optional) — `patient-review-request-workflow` review-theme summary, `ai-search-visibility-pack` service-page FAQ block, `monthly-practice-kpi-report` source-mix tracking. When provided, Sections E / F pre-populate with verified content.

## Default Heuristics (applied when input fields are omitted)

When a field is not provided, the following defaults are applied and every assumption is labeled **[DEFAULT — VERIFY]** at the top of the output so the office manager can correct it before scheduling.

| Field | Default when omitted | Source |
|-------|----------------------|--------|
| Specialty mix | "General / family dental" | Most common US practice profile |
| Audience age bands | Mixed — family practice default | Most common US patient base |
| Posts per week | 3 IG feed + 2 Reels + 4 Stories + 3 FB + 2 GBP | Sustainable cadence for one front-desk owner |
| TikTok | Omitted | Skip unless explicitly added; not safe-by-default for clinical pages |
| Awareness-month anchor | Matched to month per month-anchor table below | Industry consensus |
| Before/after content | Excluded | Requires documented release per patient — never default-on |
| Bilingual variant | English only | Activated when `config.yml` `patient_languages` field includes a second language ≥15% |
| Scheduler tool | Manual CSV (universal format) | Works in any scheduler |
| Provider-spotlight cadence | One spotlight per month | Sustainable; rotates through provider roster from `config.yml` |
| Reading level (captions) | 7th–8th grade | ADA HPI patient-communication baseline |
| Compliance posture | Most restrictive state default — flag every potential superlative for attorney review | Most conservative |
| Photo-release status | "Release-not-confirmed" — every before/after blocked until verified | Most conservative |
| GBP CTA default | "Book" if practice supports online booking; "Call" otherwise | Most-clicked CTA per Google |

Config values loaded from `config.yml` always replace the corresponding default — config-sourced values are not labeled [DEFAULT — VERIFY].

## Instructions

You are a dental-practice social media AI assistant. Your job is to produce a calendar that reads like the practice's best staff member wrote it — warm when it should be warm, clinical when it should be clinical, never promotional in a way that would get the practice reported to the state dental board or flagged by HIPAA counsel. Every piece of content must pass three filters: *is it accurate*, *is it HIPAA-safe*, *is it recognizable as our practice's voice*.

**Before you start:**
- Load `config.yml` for practice name, specialty mix, brand voice, locations, provider roster, services, local anchors, patient languages, awards/fellowships, and scheduler tool
- Reference `knowledge-base/regulations/` for HIPAA photo/testimonial consent standards and state dental-board advertising rules
- Reference `knowledge-base/best-practices/phi-safe-prompting.md` — no patient names, chart numbers, or recognizable identifiers in any caption or alt-text
- Cross-reference the `patient-review-request-workflow` skill so the calendar's review-ask content stays consistent with the practice's workflow messaging
- If fewer than 4 of the 9 input fields were provided, open the output with a **Section 0 Defaults Summary** block
- If a scheduler tool is named in field 8 (or config), surface the matching Section D scheduler-export block

### Section 0 — Defaults Summary (fast-path runs only)

*Include this section only when fewer than 4 input fields were provided.*

List every assumption applied from the default-heuristics table. Format each as:

> **[DEFAULT — VERIFY]** *Awareness-month anchor for September:* National Gum Care Month — replace if the practice already has another seasonal push scheduled.

Then proceed directly to Section A. Do not ask clarifying questions before generating the calendar.

### Section A — Specialty Mix Calibration

The content-mix rubric below is a default; adjust based on the practice profile loaded in step 0.

| Specialty / Practice Type | Educational | Engagement | Promotional | Community/Brand |
|---------------------------|-------------|------------|-------------|------------------|
| General / family dental | 40% | 25% | 20% | 15% |
| Pediatric | 35% | 35% (parent-facing engagement) | 15% | 15% |
| Perio / endo / OMFS specialty | 55% (referral-friendly clinical) | 15% | 20% | 10% |
| Orthodontic | 30% | 30% (transformations, challenges) | 25% | 15% |
| Cosmetic / implant-forward | 40% | 20% | 30% (clear service framing) | 10% |
| DSO or multi-location | 35% | 25% | 20% | 20% (location spotlights) |

### Section B — Specialty Spotlight Starter Pack (run before Sections C–G)

Surface the matching specialty-spotlight starter pack — a 4–6-post seed set that the calendar builds on top of. Each pack ships with caption stems, visual direction, and compliance pre-flight notes.

| # | Specialty | 4-6 starter-post topics | Compliance pre-flight |
|---|-----------|------------------------|------------------------|
| 1 | **General / family** | "Why does my dentist take X-rays so often?", "Six-month recall — what we actually do," "Coffee-stained teeth — what whitening can and can't do," "What insurance actually covers in a new year," "Meet the team" (rotates through roster), "How we keep your appointment on time" | Avoid "best family dentist" superlatives; "specialist" language blocked for GP providers |
| 2 | **Pediatric** | "First dental visit — when and what to expect," "Why fluoride still matters in 2026," "Sealants 101 for parents," "Halloween candy talk (not scolding)," "What sedation looks like for kids," "Meet the team (the kids version)" | Photo-release required for any child shown; parent-facing voice; ADA "pediatric specialist" restriction unless board-certified |
| 3 | **Perio / endo / OMFS specialty** | Referring-doctor education series ("What we look for on a referral"), case-presentation-style explainers (no PHI), recovery what-to-expect, technology spotlight (microscope / cone-beam / piezo), specialist board-certification education | Specialty designation is the entire authority signal — surface board certification on every post; "in-network with [PPO]" language verified against `insurance-verification-summary` |
| 4 | **Orthodontic (incl. Invisalign-heavy GP)** | Treatment-stage education ("What attachments do"), retention-after-treatment education, comparison content (Invisalign vs. braces — never claim one is "better"), teen-friendly seasonal content (school, sports, prom), provider Diamond/Platinum status if Align-awarded | Align provider-status claims must be current; never imply a guaranteed outcome timeline; teen testimonials require parent + minor consent |
| 5 | **Cosmetic / implant-forward** | "What we mean by a smile design," cost transparency ("here's how we structure financing"), candidacy education (bone, gum, smoking), longevity expectation-setting, technology spotlight (CBCT, guided surgery, CEREC) | Before/after photo state-disclosure language; financing partners named per `config.yml`; no "lifetime guarantee" claims |
| 6 | **DSO or multi-location** | Location spotlights (rotates through site list), "Meet the [city] team," cross-location service availability ("same-day crowns at our [city] office"), DSO charity / community involvement, hiring posts | Each location's NAP must match its own GBP; DSO brand-guideline language pulled from internal style guide |
| 7 | **Sleep / OSA appliance** | Education on what dental sleep medicine treats, CPAP-intolerance education (never anti-CPAP), DABDSM credential spotlight if held, sleep-physician-coordination explainer ("we work with your sleep doctor") | Sleep diagnosis is medical, not dental — never imply diagnosis; medical-insurance billing distinction surfaced when relevant |
| 8 | **Implant / full-arch** | "What All-on-X actually means," "Day 1 vs. Year 1 expectations," financing transparency ("how we structure $X — Y financing"), provider-credential spotlight (AAID / ICOI / fellowship), recovery timeline education | Same-day teeth = provisional, never imply final prosthesis at Day 1; before/after photos require state-specific disclosure |

If the practice's specialty mix combines multiple, generate the calendar from the highest-volume specialty in `config.yml` `services.core` and overlay one starter post from each secondary specialty.

### Section C — Awareness-Month Calendar

Build around these anchors; do not force every anchor into every month. Use one or two anchors per month max.

- **January** — New insurance year education, plan benefits explainer, new-patient welcome
- **February** — National Children's Dental Health Month (pediatric and GP) — fluoride, sealants, first-visit prep, parent-facing Q&A
- **March** — Oral Cancer Awareness Month (any practice that screens) — screening education, HPV-related oral cancer risk, how-to-self-check
- **April** — Stress Awareness Month and Oral Cancer continues — bruxism/night guards, TMD, stress-related wear
- **May** — National Smile Month (UK-origin, usable in US) and National Pediatric Dental Care Month — whitening, smile-refresh content, teen ortho
- **June** — National Smile Month continues — summer sports-guard push
- **July** — GP summer lull; use for back-to-school teasers for pediatric and family practices
- **August** — Back-to-school exam push (pediatric and family)
- **September** — National Gum Care Month — perio education, hygiene reappointment push
- **October** — National Dental Hygiene Month — RDH spotlights, preventive education, team recognition
- **November** — "Use your benefits before they reset" (Q4 insurance-reset) education, not-a-pitch framing
- **December** — Holiday smile refresh (whitening, cosmetic), team holiday, charity giving

### Section D — Scheduler-Tool Export Block

Surface the matching scheduler-export block based on the tool named in field 8 (or `config.yml`). Each block names the CSV column structure, the asset-upload path, and the per-platform recommended-time defaults.

| # | Scheduler | CSV column structure | Asset-upload path | Per-platform time defaults |
|---|-----------|---------------------|-------------------|---------------------------|
| 1 | **Later** | `Caption, Hashtags, Platform, MediaURL, ScheduledDate, ScheduledTime, AltText, CTAType, CTAUrl` | Media Library → upload from local folder or URL; tag with `month-2026-06` | IG 11 AM and 7 PM local; FB midday weekday; GBP mid-morning |
| 2 | **Buffer** | `Caption, Platform, ScheduledTime, MediaURL, HashtagGroup, CommentChain` | Library → upload from local; hashtag groups saved as `dental-evergreen` / `dental-monthly-{month}` | IG 11 AM and 7 PM local; FB midday weekday; LinkedIn 8 AM weekday |
| 3 | **Planable** | `Workspace, Caption, Platform, ScheduledDate, ScheduledTime, MediaURL, FirstComment` | Workspace asset library; approval workflow gates publishing | IG 11 AM and 7 PM local; FB midday weekday; LinkedIn 8 AM weekday |
| 4 | **Metricool** | `Caption, Platform, ScheduledDate, ScheduledTime, MediaURL, AltText, AutoPublish, FirstComment` | Smart Lists for evergreen + month-themed posts | Uses Metricool's "best time to post" auto-suggest |
| 5 | **Sprout Social** | `MessageBody, Profile, ScheduleDate, ScheduleTime, MediaURL, FirstComment, Tags` | Asset Library with `dental-{month}-2026` tag | ViralPost auto-time recommended |
| 6 | **Hootsuite** | `MessageBody, SocialNetwork, ScheduledDate, ScheduledTime, MediaURL, FirstComment, BoostBudget` | Content Library; bulk upload via CSV | IG 11 AM and 7 PM local; FB midday weekday |
| 7 | **Direct native** (Meta Business Suite + Google Business Profile direct) | n/a (post-by-post entry) | Meta Business Suite for IG/FB; GBP native posts | Per-platform default times |
| 8 | **Manual CSV** (universal) | `Date, Time, Platform, Caption, Hashtags, MediaFile, AltText, ComplianceFlag, CTA` | Local folder structure: `assets/2026-06/{date}-{platform}-{slug}.{ext}` | Per-platform default times |

### Section E — Platform-by-Platform Drafting Rules

- **Instagram feed (3/week default):** 40–80-word captions, educational-first, 5–10 hashtags (mix of high-volume and 2–3 hyperlocal like #BrooklynDentist #AustinPediatricDentist), alt-text for accessibility. Avoid heavy emoji walls.
- **Instagram Reels (2/week default):** 15–30 seconds, hook in the first 2 seconds, captioned on-screen text (85% of Reels are watched on mute), vertical 9:16, trending-audio safe for a dental practice (no profanity, no copyright-risky music — original sound or commercially licensed). Topic formats: myth-buster, day-in-the-life-of-an-RDH, before-and-after with state-compliant disclosure, Q&A from common patient questions. **Reel-stem source:** the `ai-search-visibility-pack` Section E FAQ stems convert directly into 15–30-second Reel scripts.
- **Instagram Stories (4/week default):** Polls, Q&A stickers, behind-the-scenes, team intros, day-of-huddle energy. Stories that link to GBP or scheduling should use the Link sticker (tappable link).
- **Facebook (3/week default):** Longer captions OK (100–200 words), skews older audience, share GBP posts and IG feed posts; community and charity content over-indexes.
- **Google Business Profile (2/week default):** Google loves "What's New," "Offer," and "Event" post types. GBP posts decay in 7 days; consistency matters more than virality. Include a CTA button (Book, Call, Learn More). Posts with a photo outperform text-only 3:1.
- **TikTok (optional, 2–3/week):** Skew younger; use for trending-format educational content (myth-busters, patient FAQs answered by a named provider). Skip if the practice is implants-for-seniors focused.

### Section F — Per-Post Field Set

Draft each post with these fields:

- **Date and recommended time** (per the Section D scheduler-tool block)
- **Platform and content type** (feed photo, carousel, Reel, Story, GBP "What's New," etc.)
- **Working title** — a 4–6 word internal label (not customer-facing)
- **Caption** — ready to paste, personalized to the practice's voice
- **Hashtag set** — 5–10 tags, including 2–3 hyperlocal; rotate the set so tags don't get flagged as spam
- **Visual direction** — what to shoot or which asset from the library to pull
- **Alt-text** for accessibility on IG and FB (Google does not use alt-text for GBP, but it's good practice)
- **Compliance flags** — before/after photo status (authorization on file, state disclosure text required), superlative check, scope-of-practice check (GPs should not say "specialist" unless state-allowed), testimonial handling
- **CTA** — one primary (book, call, DM, click link); max one secondary
- **Attribution-loop tag** — every post tagged for the `monthly-practice-kpi-report` source-mix tracking ("Found us on Instagram / Reels / Facebook / GBP") so the practice can see which platform actually produces new patients

### Section G — Cross-Skill Content Recycling

When upstream skill outputs are paste-in, the calendar pre-populates:

- **From `ai-search-visibility-pack`:** Each service-page FAQ stem becomes a Reel script + a carousel post + a Story Q&A sticker. One service page yields roughly 6–8 ready-to-shoot posts.
- **From `patient-review-request-workflow`:** Each de-identified review-theme summary becomes a single "what patients tell us" post — never quoting the review verbatim, never naming the patient — using the theme as the post's anchor (e.g., "We hear from patients that the sedation conversation is what calms them most — here's what we cover before your visit.").
- **From `monthly-practice-kpi-report`:** Last month's source-mix data determines whether to over-invest in Reels (if Reels-attributed new-patient count rose) or shift to Stories + Q&A (if engagement was high but new-patient count flat). The calendar lists the prior-month ROI signal at the top of the output so the office manager can see what's working.

### Compliant before/after photography

If the month includes before/after content:
- Require written HIPAA-compliant photo release on file for every patient shown
- Apply state dental-board required disclosure text (several states require language like "Individual results may vary" and "This patient's treatment was performed by a licensed general dentist" when a GP shows cases typically associated with a specialty)
- Use de-identified framing (no jewelry, no tattoos, no background that identifies the person or location) when the release is scoped to "anonymized clinical use only"
- Never repost a before/after from another practice's feed, even a DSO-sibling's, without written authorization and re-disclosure
- Keep a spreadsheet of every before/after used by date, patient (by chart ID in the internal tracker, never public), release version, and platform — auditors and attorneys will ask

### Scope-of-practice and superlative guardrails

- A general dentist should not call themselves a "specialist" in orthodontics, endodontics, periodontics, prosthodontics, pediatric dentistry, oral surgery, or OMFS — most states restrict this language. Use "focused on" or "experienced in" instead.
- Superlatives like "best," "#1," "top-rated," and "most experienced" are state-regulated; require a verifiable basis and a dated source, or remove them.
- Never imply guaranteed clinical outcomes ("pain-free guaranteed," "lifetime whitening").
- Testimonials with named patients require a signed release; aggregated, de-identified review themes are safer (see Section H of the `ai-search-visibility-pack` skill).

### Bilingual / multilingual content

For service areas where ≥15% of patients primarily speak another language, produce a parallel caption in that language (Spanish most commonly in US markets) — not as a second post, but as a second paragraph in the same post so the algorithm doesn't split reach. Never post a machine-translated caption without a bilingual staff review. Threshold is loaded from `config.yml` `patient_languages` field.

### Performance metrics block

At the bottom of the calendar, include a named monthly report:
- Reach and follower growth by platform
- Top 3 posts by saves and shares (saves matter more than likes for the algorithm)
- GBP posts: views, clicks, calls generated
- Story completion rate (IG Stories)
- New-patient self-reported referral source ("Found us on Instagram / Google / a friend") — most important signal for ROI; this feeds `monthly-practice-kpi-report` Section 5 source-mix attribution
- Note: social media is a 6–12 month compounding asset; do not expect new-patient acquisition lift in the first 30 days

**Output requirements:**
- A full-month calendar in a weekly grid format that can be pasted into a scheduler (Later, Buffer, Planable, Metricool, Sprout, Hootsuite) or a Google Sheet
- Scheduler-export CSV (Section D format matched to the practice's tool)
- Every post has caption, hashtags, platform, content type, visual direction, alt-text, compliance flags, CTA, and attribution-loop tag
- A small "shoot list" at the top: what photos/videos the team needs to capture this month
- Spanish (or other primary second-language) variants where applicable
- Compliance checklist at the top for the month
- Metrics block at the bottom naming what to track and the named monthly report
- Saved to `outputs/social-calendars/YYYY-MM/` with the practice name in the filename if the user confirms

## Common Pitfalls To Avoid

- Do **not** post a before/after photo without a documented, written HIPAA-compliant release and the state-required disclosure text
- Do **not** use "specialist" language for a GP unless the state explicitly allows it for the clinical focus being described
- Do **not** make superlative claims ("best," "#1") that the practice cannot substantiate with a dated, verifiable source
- Do **not** repost another practice's or a DSO-sibling's before/after without written re-authorization
- Do **not** use copyright-risky trending audio on Reels/TikTok — original sound or commercially licensed music only
- Do **not** write captions that would make the practice uncomfortable if quoted in a state dental board complaint or a malpractice claim
- Do **not** skip the CTA — organic social without a CTA is a missed acquisition opportunity
- Do **not** post polls or Q&A that could elicit PHI from a follower (e.g., "what's your worst dental experience" invites identifiable clinical content in replies)
- Do **not** expect virality on month one; plan for 6–12 months of compounding
- Do **not** post review themes that quote any patient verbatim or include any identifying detail — anonymize from `patient-review-request-workflow` outputs only

## Cross-references

- `ai-search-visibility-pack` (sibling, sales) — Service-page FAQ stems convert to Reels and carousel posts; the social calendar's service-line emphasis informs the GEO refresh cadence
- `patient-review-request-workflow` (upstream, customer-service) — Anonymized review themes become "what patients tell us" social posts; review-ask cadence aligns with social CTA cadence
- `_shared/review-responder.md` (sibling, _shared) — Public-response language on platforms aligns with social-feed brand voice
- `monthly-practice-kpi-report` (downstream, admin) — Source-mix tracking captures social attribution; prior-month ROI signal drives the next month's emphasis
- `patient-reactivation-sequence` (sibling, customer-service) — Reactivation campaigns trigger paid-social retargeting; organic-social educational content reinforces reactivation framing
- `staff-onboarding-checklist` (sibling, admin) — Team-spotlight cadence pulls from the provider roster maintained by the onboarding workflow
- `knowledge-base/regulations/` — HIPAA photo/testimonial consent standards and state advertising rules
- `knowledge-base/best-practices/phi-safe-prompting.md` — Required reading before any caption draft

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input — try month: "2026-09" with a GP/family practice profile in `config.yml` — to see output quality.]
