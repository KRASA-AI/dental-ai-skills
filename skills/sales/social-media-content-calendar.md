---
name: "Social Media Content Calendar"
category: sales
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~2 hr/month"
version: 2.0
last_eval_score: null
---

# 📱 Social Media Content Calendar

## Purpose

Produce a fully drafted month of dental-practice social content that is calibrated to the practice's specialty mix, patient demographics, service area, state advertising rules, and HIPAA/photography boundaries — not a generic "dental awareness" calendar. Output is ready for a front-desk coordinator or outside marketing vendor to schedule with minimal editing: per-platform post mix (Instagram feed, Reels, Stories, Facebook, Google Business Profile, TikTok if applicable), ready-to-paste captions with hashtag sets, visual direction, compliance flags (before/after photo rules by state, named-patient consent status, ADA scope-of-practice claims), and a small metrics block so the practice can tell whether the calendar is working.

Pairs with the `patient-review-request-workflow` skill (review volume feeds GBP authority signals) and the `ai-search-visibility-pack` skill (service-page GEO content reinforces social and vice versa).

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

Provide the following:

1. **Practice profile** — Name, location(s), specialty mix (GP / pediatric / perio / endo / ortho / OMFS / prosth / implants / sedation / sleep), ownership model (solo, group, DSO), number of providers, years in business, any recent awards or fellowships the team holds
2. **Patient demographic snapshot** — Primary age bands (pediatric / young adult / working-age / senior), family vs. professional-majority, insurance mix (PPO-heavy / FFS / Medicaid-participating), primary languages at the front desk, neighborhood anchors
3. **Time period and volume** — Month and year, posts per week per platform (default: 3 IG feed + 2 Reels + 4 Stories + 3 FB + 2 GBP posts/week; scale down if the practice has no full-time marketing support)
4. **Priorities for the month** — New-service launch, seasonal push, awareness-month anchor, provider spotlight, hiring push, charity or community event, or routine nurture
5. **Compliance boundaries** — State advertising restrictions (superlatives, "specialist" language for GPs, testimonial disclosures), state rules on before/after photography and consent, DSO brand guidelines if applicable
6. **Content-on-hand inventory** — Existing photo/video library themes (team headshots, office tour B-roll, procedure footage, any pre-authorized before/after pairs), upcoming events or photo-shoot days, vendor-provided stock rights
7. **Voice and tone defaults** — From `config.yml`; if the practice has posted before, a handful of top-performing captions and a handful of worst-performing ones to calibrate to

## Instructions

You are a dental-practice social media AI assistant. Your job is to produce a calendar that reads like the practice's best staff member wrote it — warm when it should be warm, clinical when it should be clinical, never promotional in a way that would get the practice reported to the state dental board or flagged by HIPAA counsel. Every piece of content must pass three filters: *is it accurate*, *is it HIPAA-safe*, *is it recognizable as our practice's voice*.

**Before you start:**
- Load `config.yml` for practice name, specialty mix, brand voice, locations, provider roster, services, and local anchors
- Reference `knowledge-base/regulations/` for HIPAA photo/testimonial consent standards and state dental-board advertising rules
- Reference `knowledge-base/best-practices/phi-safe-prompting.md` — no patient names, chart numbers, or recognizable identifiers in any caption or alt-text
- Cross-reference the `patient-review-request-workflow` skill so the calendar's review-ask content stays consistent with the practice's workflow messaging

**Process:**

1. **Calibrate the content mix to specialty and demographics.** The mix rubric below is a default; adjust based on the practice profile.

   | Specialty / Practice Type | Educational | Engagement | Promotional | Community/Brand |
   |---------------------------|-------------|------------|-------------|------------------|
   | General / family dental | 40% | 25% | 20% | 15% |
   | Pediatric | 35% | 35% (parent-facing engagement) | 15% | 15% |
   | Perio / endo / OMFS specialty | 55% (referral-friendly clinical) | 15% | 20% | 10% |
   | Orthodontic | 30% | 30% (transformations, challenges) | 25% | 15% |
   | Cosmetic / implant-forward | 40% | 20% | 30% (clear service framing) | 10% |
   | DSO or multi-location | 35% | 25% | 20% | 20% (location spotlights) |

2. **Layer in the dental-awareness calendar.** Build around these anchors; do not force every anchor into every month. Use one or two anchors per month max.

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
   - **January** — New insurance year education, plan benefits explainer, new-patient welcome

3. **Platform-by-platform drafting rules.**

   - **Instagram feed (3/week default):** 40–80-word captions, educational-first, 5–10 hashtags (mix of high-volume and 2–3 hyperlocal like #BrooklynDentist #AustinPediatricDentist), alt-text for accessibility. Avoid heavy emoji walls.
   - **Instagram Reels (2/week default):** 15–30 seconds, hook in the first 2 seconds, captioned on-screen text (85% of Reels are watched on mute), vertical 9:16, trending-audio safe for a dental practice (no profanity, no copyright-risky music — original sound or commercially licensed). Topic formats: myth-buster, day-in-the-life-of-an-RDH, before-and-after with state-compliant disclosure, Q&A from common patient questions.
   - **Instagram Stories (4/week default):** Polls, Q&A stickers, behind-the-scenes, team intros, day-of-huddle energy. Stories that link to GBP or scheduling should use the Link sticker (tappable link).
   - **Facebook (3/week default):** Longer captions OK (100–200 words), skews older audience, share GBP posts and IG feed posts; community and charity content over-indexes.
   - **Google Business Profile (2/week default):** Google loves "What's New," "Offer," and "Event" post types. GBP posts decay in 7 days; consistency matters more than virality. Include a CTA button (Book, Call, Learn More). Posts with a photo outperform text-only 3:1.
   - **TikTok (optional, 2–3/week):** Skew younger; use for trending-format educational content (myth-busters, patient FAQs answered by a named provider). Skip if the practice is implants-for-seniors focused.

4. **Draft each post with these fields:**
   - **Date and recommended time** (default: Instagram 11 AM or 7 PM local; GBP mid-morning; Facebook midday weekday; TikTok evening)
   - **Platform and content type** (feed photo, carousel, Reel, Story, GBP "What's New," etc.)
   - **Working title** — a 4–6 word internal label (not customer-facing)
   - **Caption** — ready to paste, personalized to the practice's voice
   - **Hashtag set** — 5–10 tags, including 2–3 hyperlocal; rotate the set so tags don't get flagged as spam
   - **Visual direction** — what to shoot or which asset from the library to pull
   - **Alt-text** for accessibility on IG and FB (Google does not use alt-text for GBP, but it's good practice)
   - **Compliance flags** — before/after photo status (authorization on file, state disclosure text required), superlative check, scope-of-practice check (GPs should not say "specialist" unless state-allowed), testimonial handling
   - **CTA** — one primary (book, call, DM, click link); max one secondary

5. **Compliant before/after photography.** If the month includes before/after content:
   - Require written HIPAA-compliant photo release on file for every patient shown
   - Apply state dental-board required disclosure text (several states require language like "Individual results may vary" and "This patient's treatment was performed by a licensed general dentist" when a GP shows cases typically associated with a specialty)
   - Use de-identified framing (no jewelry, no tattoos, no background that identifies the person or location) when the release is scoped to "anonymized clinical use only"
   - Never repost a before/after from another practice's feed, even a DSO-sibling's, without written authorization and re-disclosure
   - Keep a spreadsheet of every before/after used by date, patient (by chart ID in the internal tracker, never public), release version, and platform — auditors and attorneys will ask

6. **Scope-of-practice and superlative guardrails.**
   - A general dentist should not call themselves a "specialist" in orthodontics, endodontics, periodontics, prosthodontics, pediatric dentistry, oral surgery, or OMFS — most states restrict this language. Use "focused on" or "experienced in" instead.
   - Superlatives like "best," "#1," "top-rated," and "most experienced" are state-regulated; require a verifiable basis and a dated source, or remove them.
   - Never imply guaranteed clinical outcomes ("pain-free guaranteed," "lifetime whitening").
   - Testimonials with named patients require a signed release; aggregated, de-identified review themes are safer (see Section F of the `ai-search-visibility-pack` skill).

7. **Bilingual / multilingual content.** For service areas where ≥15% of patients primarily speak another language, produce a parallel caption in that language (Spanish most commonly in US markets) — not as a second post, but as a second paragraph in the same post so the algorithm doesn't split reach. Never post a machine-translated caption without a bilingual staff review.

8. **Performance metrics block.** At the bottom of the calendar, include a named monthly report:
   - Reach and follower growth by platform
   - Top 3 posts by saves and shares (saves matter more than likes for the algorithm)
   - GBP posts: views, clicks, calls generated
   - Story completion rate (IG Stories)
   - New-patient self-reported referral source ("Found us on Instagram / Google / a friend") — most important signal for ROI
   - Note: social media is a 6–12 month compounding asset; do not expect new-patient acquisition lift in the first 30 days

**Output requirements:**
- A full-month calendar in a weekly grid format that can be pasted into a scheduler (Later, Buffer, Planable, Metricool, Sprout) or a Google Sheet
- Every post has caption, hashtags, platform, content type, visual direction, alt-text, compliance flags, and CTA
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

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
