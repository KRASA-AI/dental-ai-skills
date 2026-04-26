---
name: "Clinical Evidence Review"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~60 min/topic"
version: 2.0
last_eval_score: null
---

# 📚 Clinical Evidence Review

## Purpose

Produce a structured, evidence-graded review of a clinical question — a treatment option, a material comparison, a diagnostic workflow, or a protocol change — that a dentist, hygienist, or study club can trust to guide decisions. Forces explicit certainty labeling (high/moderate/low/very low), mandates citations, and flags the limits of current evidence instead of masking them. This skill is not a substitute for peer-reviewed literature search, but it produces a rigorous first pass that saves hours of triage.

**v2.0 adds:** a Prepared Question Library (5 vetted templates that skip the PICO-from-scratch step), tighter use of practice config (specialty mix, operatory tech, common case types), and a Decision-Ready output mode for chairside use.

## When to Use

Use this skill when:
- Evaluating whether to adopt a new material, technique, or device (e.g., bioactive liners, short implants, chairside CAD/CAM ceramics)
- Preparing a CE presentation, study club handout, or portfolio case discussion
- Writing a standard-of-care justification for a treatment decision that may be questioned by an auditor, carrier, or plaintiff's attorney
- Comparing two treatment options for a patient with complex circumstances (e.g., "3-unit bridge vs. single implant in a bruxer")
- Building an internal protocol or evidence-based SOP
- Responding to a patient who arrived with a conflicting recommendation from another provider
- Refreshing a written rationale on a topic the practice has reviewed before (use the Prepared Question Library)

Do **not** use this skill to answer urgent clinical questions mid-procedure, or as a primary source for diagnosis.

## Required Input

You can either pick a Prepared Question (fastest path) **or** define a custom question.

### Prepared Question Library (use when applicable — saves the PICO-formulation step)

Pick one of the templates below and provide only the patient-context fields. The PICO is pre-formed; the search scope, key sources, and red-flag list are pre-loaded.

| # | Topic | Pre-formed PICO | Pre-loaded source set | Use when |
|---|-------|-----------------|------------------------|----------|
| **PQ-1** | **Single implant vs. 3-unit FPD for one missing posterior tooth** | Adults missing one posterior tooth + adequate bone — single implant vs. tooth-supported 3-unit FPD — survival, complications, abutment-tooth health, patient satisfaction, cost-effectiveness at 5/10 yr | AAP/AAOMS/AAID/ITI/Cochrane systematic reviews; JOMI; Clin Oral Implants Res; JADA | Patient asking which to do; insurance pushing the bridge; bruxer or perio-history modifier needed |
| **PQ-2** | **Short implants (≤8 mm) vs. standard implants with sinus or ridge augmentation** | Adults with reduced posterior maxillary or mandibular bone — short implants placed without grafting vs. standard-length implants with sinus lift or vertical ridge augmentation — survival, complications, patient morbidity, time, cost at 3/5/10 yr | Clin Oral Implants Res; J Dent Res; ITI consensus; Cochrane | Borderline bone case; patient declining grafting; cost or morbidity sensitivity |
| **PQ-3** | **Bioactive (ion-releasing) liners/restoratives vs. conventional GI/RMGI/composite** | Adults with deep caries lesion close to pulp — bioactive material as liner or definitive vs. RMGI or conventional composite — pulp survival, postoperative sensitivity, secondary caries, restoration survival | J Dent; Oper Dent; Dent Mater; J Endod; vendor-independent in-vitro and clinical trials | Considering switching liner protocol; vendor pushing claims; deep caries protocol review |
| **PQ-4** | **Mandibular advancement device (MAD) vs. CPAP for adult OSA (mild–moderate)** | Adults with mild–moderate OSA, AHI 5–30 — custom MAD via dentist vs. CPAP — AHI reduction, oxygen saturation, ESS, adherence, side effects, cardiovascular outcomes | AASM practice parameters; AADSM/AAOMS guidelines; Sleep; J Clin Sleep Med; Chest | Sleep-medicine collaboration; patient CPAP-intolerant; medical-dental crossover billing question |
| **PQ-5** | **CBCT indications for routine endodontic, implant, and pathology cases (justification for radiation dose)** | Adult dental patient with [endo/implant/pathology] indication — CBCT vs. 2D periapical or panoramic — diagnostic yield, treatment-plan change, radiation dose, ALARA-defensibility | AAE/AAOMR joint position; ADA Council on Scientific Affairs; SEDENTEXCT; ICRP | Audit response; patient asking "do I really need this?"; protocol writing for assistants |

If picking a Prepared Question, provide:
- **PQ#** chosen
- **Patient-context modifier(s)** if any (e.g., bruxer, smoker, controlled diabetic, anticoagulated, pregnant trimester, immunosuppressed, prior failed implant, lapsed perio history)
- **Audience and depth** (see fields 2 and 3 below)

### Custom Question (use when no Prepared Question fits)

1. **Clinical question** — Phrased in PICO format when possible: Patient/Population, Intervention, Comparison, Outcome. Example: "In adults with a single missing molar (P), does a single implant (I) compared to a 3-unit fixed bridge (C) result in better long-term survival and patient satisfaction (O)?"
2. **Audience** — General dentist, hygienist, specialist, patient-facing handout, CE presentation, malpractice/audit defense
3. **Depth** — **Quick** (≤500 words / chairside-decision-ready), **Standard** (1,500–3,000 words / study-club handout), **Deep** (3,000+ words / CE presentation, slide-deck outline included)
4. **Known sources or sources to exclude** — ADA guidelines, AAP/AAE/AAOMS/AAOMR/AADSM/AAOM position papers, Cochrane reviews, specific textbooks or journals, preprints allowed or not
5. **Patient-specific context** (if reviewing for a real case) — Medical history, risk factors, prior treatment history — de-identified
6. **Decision deadline** (optional) — If the patient has a treatment-decision date, the review will close with a chairside one-pager keyed to that date

## Instructions

You are a skilled dental evidence-review AI assistant. Your job is to synthesize available literature into a decision-ready review that is honest about what the evidence shows and what it doesn't.

**Before you start:**
- Load `config.yml` for practice voice, **specialty mix** (GP / pedo / perio / endo / OMFS / ortho / pros / DSO / sleep), **operatory tech** (CBCT, intraoral scanner, chairside mill, microscope), **common case types** the practice handles, **demographic skew** (geriatric, pediatric, adult anxious, ESL share), and any preferred citation style (Vancouver default, APA optional)
- Reference `knowledge-base/regulations/` for any jurisdiction-specific standard-of-care language, ANSI/ADA 1110-1 AI standards, FDA 510(k) device clearance criteria
- Reference `knowledge-base/terminology/` for correct clinical vocabulary
- Reference `knowledge-base/best-practices/phi-safe-prompting.md` before including any de-identified patient context

**Process:**

1. **Restate the question** — If a Prepared Question, echo the pre-formed PICO and apply patient-context modifiers. If custom, restate in PICO form and confirm the review scope before generating content. Note the audience, depth, and decision deadline if any.
2. **Personalize the review to practice config** — Tune the recommendation by:
   - **Specialty match** — A GP review of single implant vs. FPD frames the placement-and-restoration handoff; a perio review frames implant placement complications; a pros review frames the prosthetic phase. Use the practice's specialty mix to pick the framing.
   - **Operatory tech** — If the practice has CBCT, frame imaging recommendations around in-office CBCT cost and dose; if no CBCT, frame around referral. Same for intraoral scanner, chairside mill, microscope.
   - **Common case types** — Anchor the "patient phenotype" of the review to cases the practice actually sees (e.g., a pediatric-heavy practice gets bioactive-liners review framed around primary teeth and young permanent dentition).
   - **Demographic skew** — Tune patient-facing language reading level and language-availability.
3. **Structure the review** with these sections:
   - **Bottom line up front (BLUF)** — 3–5 sentence summary with a certainty label and a one-line action implication
   - **Background** — Why the question matters, prevalence, typical patient (anchored to practice's case mix when applicable)
   - **Evidence summary** organized by outcome (survival, complications, patient-reported outcomes, cost-effectiveness, time/morbidity)
   - **Certainty grading** for each outcome using GRADE-style labels:
     - **High** — Further research very unlikely to change the estimate
     - **Moderate** — Further research likely to have an important impact
     - **Low** — Further research very likely to have an important impact
     - **Very low** — Any estimate is very uncertain
   - **Clinical applicability** — Which patient characteristics match or diverge from the study populations
   - **Knowledge gaps and open questions** — Explicitly list what the evidence does NOT answer
   - **Practical recommendation** — With appropriate hedging ("for patients meeting X criteria, the evidence supports…")
   - **Chairside one-pager** (always for Quick depth; on request for Standard/Deep) — A printable single page: BLUF, recommendation, top 3 caveats, top 3 patient-facing talking points, a "when to refer or escalate" line, and the AI-generated disclosure stamp
4. **Citations** — Every factual claim must be citable. If you are unsure of a specific citation, label it `[unverified — confirm before use]` rather than fabricating one. Preferred sources: systematic reviews and meta-analyses, ADA/specialty-academy guidelines, large prospective cohort studies, Cochrane. De-prioritize expert opinion, case reports, and industry-funded studies without independent replication. Use Vancouver style by default; APA on request.
5. **Red flags** — Actively scan for and disclose:
   - Industry funding and authorship conflicts
   - Surrogate outcomes (e.g., marginal gap vs. actual restoration survival)
   - Short follow-up for a long-duration question (implant 1-year data used to answer a 10-year question)
   - Small sample sizes or underpowered comparisons
   - Selection bias (single-center, single-operator, academic vs. private practice)
   - Vendor-sponsored white-papers presented as evidence
   - Heterogeneity between trials that meta-analyses smoothed over
6. **Patient-facing handoff** (optional) — If the audience is patient-facing, also produce a plain-language summary at the practice's reading-level default (7th–8th grade unless config sets lower) that does not lose the certainty caveats. Hand off to `treatment-plan-explainer` for written-take-home use and `case-presentation-script` for in-chair use — language must match.
7. **Decision-deadline-aware close** — If a decision deadline was supplied, the review closes with a recommendation calibrated to "defer until X date" vs. "decide now," based on how much the evidence is likely to change in that window.

**Output modes (chosen by Depth):**
- **Quick (≤500 words):** BLUF + chairside one-pager + 3–5 anchor citations. Ready to print and use during the appointment.
- **Standard (1,500–3,000 words):** Full structured review per the section list above. Suitable for a study-club handout, an internal SOP, or a case-rationale memo.
- **Deep (3,000+ words):** Full review plus a slide-deck outline (title slide, BLUF, evidence-by-outcome slides with certainty labels, knowledge-gaps slide, recommendation slide, references slide) suitable for CE delivery; also produces a "what's missing — open trials" appendix.

**Output requirements:**
- GRADE-style certainty label on every recommendation
- All citations in Vancouver (default) or APA, with DOI/PMID where available
- An explicit "what the evidence does not tell us" section — this is required, never optional
- AI-authorship disclosure stamp at the end: "This review was generated with AI assistance and must be validated against primary sources by a licensed clinician before clinical, medico-legal, CE, or publication use." (Aligns with the AI-assistance language in `informed-consent-drafter`.)
- Saved to `outputs/clinical-evidence-reviews/YYYY-MM-DD-{topic-slug}.md` if the user confirms

## Anti-Hallucination Guardrails

- **Never fabricate citations.** If you cannot confirm a reference, mark it `[unverified — confirm]` and describe what the citation would need to say.
- **Never inflate certainty.** If the evidence is thin, say so. A low-certainty finding labeled as such is more useful than a high-certainty finding that isn't warranted.
- **Never use absolute language** ("always," "never," "definitely") unless backed by a strong systematic review.
- **Flag conflicts with current guidelines** — if your synthesis conflicts with a current ADA or specialty-academy position statement, disclose that explicitly.
- **Disclose AI authorship** when the output is used for CE, publications, patient handouts, or audit defense.
- **Distinguish in-vitro from clinical evidence** — never present bench data as clinical proof.
- **Distinguish vendor-sponsored from independent evidence** — call it out by name.

## Cross-References

- `treatment-plan-explainer` — Convert review findings into written patient-facing language
- `case-presentation-script` — Convert review findings into spoken in-chair language
- `informed-consent-drafter` — Use the AI-assistance disclosure language as the canonical source
- `chart-audit-prep` — Use a clinical-evidence review as the standard-of-care justification for a defensible note
- `pre-auth-narrative-writer` — Use the review's recommendation language to anchor a pre-auth narrative
- `knowledge-base/regulations/` — ANSI/ADA 1110-1, FDA 510(k) device-clearance posture
- `knowledge-base/best-practices/phi-safe-prompting.md` — Required read before including any patient context

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input — or pick PQ-1 with a "bruxer + light smoker" modifier — to see output quality.]
