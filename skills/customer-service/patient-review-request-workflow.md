---
name: "Patient Review Request Workflow"
category: customer-service
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~40 min/setup + ongoing"
version: 2.0
last_eval_score: null
---

# ⭐ Patient Review Request Workflow

## Purpose

Design and draft a HIPAA-safe, procedure-aware review request workflow that triggers the right ask, on the right channel, at the right time after a patient's visit. The output is a ready-to-configure campaign: a trigger matrix keyed to CDT codes (or appointment type), channel selection (SMS vs. email vs. both), message copy for each touch, a 2-touch follow-up rhythm for unopened requests, handling rules for clinical complaints that arrive through review links, and a small set of internal metrics the practice can watch without buying a new tool.

This skill is the *request and collection* side of the review workflow. Drafting the practice's *response* to reviews once they appear is the job of the dental-override `review-responder` shared skill. Both are referenced by the `ai-search-visibility-pack` skill, which depends on a steady stream of fresh Google reviews to reinforce GEO authority signals.

## When to Use

Use this skill when:
- A practice has no systematic review-ask workflow and is leaving reviews to chance
- The practice's Google review count is stale (no new review in 60+ days) or skewed (too many 1- or 5-star only, no 4-star "honest" reviews)
- Switching review platforms (Podium → Birdeye, or a PMS-bundled review tool) and the message cadence needs to be rebuilt
- A new provider joins and the practice wants to build up their personal review count inside the practice's profile
- A compliance review flags that current review-request SMS messages contain PHI (e.g., procedure name in the body)
- The practice is preparing a GEO push (see `ai-search-visibility-pack`) and needs review volume to support the aggregateRating snippet

Do **not** use this skill to:
- Incentivize reviews in any way that would violate Google, Yelp, or state dental board policies (see guardrails)
- Draft review-response language — use the dental-override `review-responder` shared skill
- Solicit reviews from a patient whose visit had a known clinical complication or open complaint — those require a human touch and should be excluded from the automated workflow

## Required Input

Provide the following:

1. **Patient-facing channel configuration** — Which SMS platform (Adit, OhMD, RevenueWell, Emitrr, Podium, Birdeye, Solutionreach, Lighthouse 360), which email platform, whether the PMS has built-in review triggers (Dentrix Ascend, Open Dental, Curve, Denticon, Carestack, Eaglesoft); confirm the platform has a signed BAA and HIPAA-safe messaging
2. **Review platforms the practice wants to grow** — Usually Google Business Profile is the priority; secondary platforms are Healthgrades, Yelp, Facebook, and (for specialty practices) RealSelf or Zocdoc
3. **Practice consent baseline** — Whether the patient has signed the PMS messaging consent and opted in to SMS
4. **Appointment / CDT code triggers** — Which appointment types should trigger a request and which should not (e.g., new-patient exam yes; emergency visit no; extraction only if follow-up confirms uncomplicated healing)
5. **Provider preference** — Whether review requests are signed by the provider, the front desk, or the practice brand
6. **Compliance boundaries** — Any state dental-board or HIPAA-safety restrictions on message content (many states prohibit naming the procedure in a review-request message)
7. **Language preferences** — Primary language(s) of the patient base; a Spanish-language variant is common in most US markets
8. **Review ethics policy** — The practice's written position on incentives, screening (selective filtering of happy patients), and negative-review handling; required for all platforms and reviewed by counsel

## Instructions

You are a dental patient-communication AI assistant. Your job is to deliver a full review-request workflow that is HIPAA-safe, platform-agnostic in principle (but configurable to the practice's actual tools), and factually modest about what it will and will not do. Review-generation is a slow-compound workflow — 2–4% of requested patients post a review, with well-designed workflows reaching 20–30% response on the *request* step (the patient tapping the link) and 10–12% posting a review.

**Before you start:**
- Load `config.yml` for practice name, provider names, preferred channels, and review-platform URLs (Google place ID, Healthgrades profile, etc.)
- Reference `knowledge-base/best-practices/phi-safe-prompting.md` — no procedure names, no tooth numbers, no treatment plan detail in outbound SMS
- Reference the dental-override `_shared/review-responder.md` skill for the response side of the workflow
- Reference `skills/sales/ai-search-visibility-pack.md` if the request workflow is part of a broader GEO push

**Process:**

1. **Confirm the patient-inclusion rules.** Build the rule set that decides which patients get a review request and which are excluded. At minimum:
   - **Include:** Completed new-patient exam, routine hygiene recall, CEREC or same-day crown seat, completed Invisalign case, implant delivery (not placement), ortho debond, denture delivery with confirmed comfort at the 2-week post-delivery adjustment
   - **Exclude:** Emergency visits where pain is not resolved the same day, extractions with complications flagged in the chart, billing disputes, patient-flagged complaints in the last 30 days, patients under age 18 (request to the parent if parent attended the visit), any patient on the front-office "do not contact" list
   - **Hold:** Post-op visits where clinical healing is uncertain; release to the request queue only after the provider's confirmation at the follow-up visit

2. **Build the trigger matrix.** For each included appointment type, specify:
   - **Lag time** — Time between appointment completion and the first request send. Typical values: 2–4 hours for short, uncomplicated visits (hygiene, exam); 24–48 hours for longer restorative or surgical visits so the patient can settle and form an opinion; 14 days post-delivery for Invisalign/ortho debonds when the patient has had a chance to see their result
   - **Channel** — SMS for high-response patients who have texted with the office before; email for patients without SMS opt-in or where message length matters; SMS + email for VIP patients and referral sources
   - **Sender identity** — Practice brand for most requests; named provider for new-patient exams so the provider's personal review count grows; front-desk named staffer for hygiene recalls
   - **Template variant** — A/B-testable message copy keyed to the appointment type

3. **Draft message copy.** Produce three drafts per trigger type. Each draft follows these rules:
   - **Never** name the procedure, tooth number, or any clinical finding. HIPAA safe messages read like "Thanks for coming in today — it was great seeing you" not "Thanks for your implant placement today."
   - **Never** incentivize the review content. Offering a discount for a review (any review) is a policy violation on Google and a dental-board risk in several states. A generic practice-wide appreciation program (e.g., "everyone who leaves a review this month gets entered into a drawing for a non-dental gift card") is gray-area and should be vetted by counsel — most practices skip it.
   - **Always** offer two links: a public review platform (Google) and an internal feedback channel (reply-SMS or an email to the office manager) so patients with a concern have a non-public path.
   - **Always** include unsubscribe language for email (required by CAN-SPAM) and respect STOP keywords for SMS (required by TCPA and carrier policy)
   - **Keep SMS to 160 characters when possible** so the message does not segment
   - **Bilingual variants** — Produce a Spanish-language version for markets where ≥15% of patients are primarily Spanish-speaking

4. **Draft the 2-touch follow-up.** For patients who receive the request but do not click the link or post a review:
   - **Touch 2 at 7 days** — Softer reminder, same two-path structure (public link + internal feedback), single sentence about how much the practice appreciates honest feedback
   - **Hold after Touch 2** — Do not send a third ask. Repeated requests feel coercive and trigger SMS carrier spam filtering; a patient who did not respond to two touches is done for this visit
   - **Reset at next visit** — The trigger clock restarts at the next included visit 3–12 months later

5. **Clinical-complaint diversion.** When the internal-feedback path receives a message that describes discomfort, a clinical concern, or a billing dispute, it must route to the office manager or clinical coordinator for a same-business-day callback. The public review link must never be the first path for a patient who is already unhappy — diverting complaints to the internal channel is both better patient care and a lower legal risk than encouraging a negative public review.

6. **Metrics block.** Define a small, visible monthly metric set the front office can track without a separate analytics tool:
   - Requests sent by trigger type
   - Link-click rate (requests / clicks)
   - Post rate (requests / posted reviews on Google and secondary platforms combined)
   - Star-rating distribution of new reviews (expect a realistic mix including 4-star "honest" reviews; an overwhelmingly 5-star stream can itself look suspicious to patients)
   - Internal-feedback volume and resolution time (same-day callback rate)
   - "Held" or excluded patient count with reason — tracked to ensure the exclusion logic is not quietly over-suppressing

**Output requirements:**
- One-page summary at the top (trigger matrix visible in under 30 seconds)
- Message copy for each trigger type (English + Spanish), clearly labeled SMS vs. email
- Touch-2 follow-up copy for each trigger type
- Clinical-complaint diversion routing rules and the front-office script for the callback
- Monthly metrics block as a named report the office manager runs on the 1st
- Platform-setup checklist — what to configure in the SMS/email/PMS tool, with a note that every platform handles trigger configuration differently and the practice's implementation engineer (or the vendor's onboarding support) builds the triggers
- Saved to `outputs/review-request-workflow/` with the practice name and date in the filename

## Guardrails

- **No procedure names, tooth numbers, clinical findings, or treatment-plan detail in outbound SMS or email.** HIPAA and carrier-spam rules both apply. "Thanks for your appointment" is fine; "Thanks for your implant surgery" is not.
- **No selective review screening.** Sending the public-review link only to patients the practice believes will leave a 5-star review (a "review gate") violates Google, Yelp, and Facebook policy and in some states is a dental-board advertising violation. Every included patient receives the same ask.
- **No incentives for a specific review.** Offering a discount, gift, or entry tied to posting a review (even a positive one) is a Google policy violation. Practice-wide appreciation programs unconnected to a specific review are a gray area; counsel should review.
- **Exclude patients with open complaints, pain-not-resolved visits, or billing disputes.** The workflow's purpose is honest feedback from patients who are in a position to give it; no one is served by soliciting a review from a patient mid-complaint.
- **Respect STOP keywords and unsubscribe links.** TCPA and CAN-SPAM violations carry per-message statutory damages. The practice's messaging platform must handle opt-outs automatically; confirm the configuration.
- **Never impersonate the patient.** Some review-gen tools offer "draft a review for your patient to post" features; these are a policy violation everywhere.
- **Treat review responses as public HIPAA.** The public-response side is the `review-responder` skill's concern, but worth naming here: the *request* workflow must not include any content that, if quoted back by a patient in a review, would disclose PHI.
- **The clinical-complaint diversion is not optional.** A same-business-day callback path is the single most important piece of this workflow for both patient care and risk reduction.

## Example Output

**Sample input:** SMS via Weave (BAA on file), email via PMS; grow Google first, Healthgrades second; Spanish needed (~20% of base); triggers = new-patient exam (provider-signed) + hygiene recall (front-desk-signed); exclude emergency/unresolved-pain visits; bilingual market.

---

> ### Trigger Matrix (at a glance)
>
> | Trigger | Lag | Channel | Signed by | Link target |
> |---|---|---|---|---|
> | New-patient exam (completed) | 3 hr | SMS | Dr. [name] | Google → internal feedback |
> | Hygiene recall (completed) | 3 hr | SMS | Front desk | Google → internal feedback |
> | *Emergency / pain unresolved* | — | **EXCLUDED** | — | route to OM callback |
>
> ### Message Copy — New-Patient Exam (SMS, provider-signed, EN)
> > Hi [First] — Dr. [Name] here. It was great meeting you today at [Practice]! If you have a moment, we'd love your honest feedback: [Google link]. Prefer to reach us directly? Just reply to this text. Reply STOP to opt out. *(149 chars w/o link)*
>
> ### Same message — ES
> > Hola [First], soy la Dra. [Name] de [Practice]. ¡Fue un gusto conocerle hoy! Si tiene un momento, nos encantaría su opinión sincera: [Google link]. ¿Prefiere contactarnos directamente? Responda a este mensaje. Responda STOP para cancelar.
>
> ### Hygiene Recall (SMS, front-desk-signed, EN)
> > Hi [First] — thanks for coming in today! It's always nice to see you at [Practice]. If you'd share a quick review, here's the link: [Google link]. Have feedback for us instead? Just reply here. Reply STOP to opt out.
>
> ### Touch 2 (day 7, unopened/no-post — EN)
> > Hi [First], no pressure at all — if you have 30 seconds, your honest feedback helps our small team a lot: [Google link]. Or reply here anytime. (No further texts about this.) Reply STOP to opt out.
>
> *(Touch 2 also produced in ES. No Touch 3 — clock resets at next eligible visit.)*
>
> ### Email variant (new-patient, EN excerpt)
> Subject: *It was great meeting you, [First]!* — body mirrors SMS, adds two buttons ("Leave a Google review" / "Share private feedback"), CAN-SPAM footer with practice address + one-click unsubscribe.
>
> ### Clinical-complaint diversion (internal-feedback path)
> Any reply mentioning pain, a clinical concern, or billing → auto-route to Office Manager queue for **same-business-day callback**. Public link is never the first path for an unhappy patient. Front-desk callback opener: *"Hi [First], this is [Name] from [Practice] — [OM] saw your note and wanted to make this right personally. Tell me what happened."*
>
> ### Monthly metrics block (OM runs on the 1st)
> Requests sent by trigger · link-click rate · post rate (Google + Healthgrades) · star distribution (expect some 4★) · internal-feedback volume + same-day-callback rate · held/excluded count with reason.
>
> ### Platform-setup checklist (Weave)
> Confirm BAA · enable review-request automation · set 3-hr post-appointment trigger by appointment type · suppress excluded types · load Google place ID + Healthgrades URL · verify STOP/opt-out handling is automatic. *(Vendor onboarding builds the actual triggers; configuration differs per platform.)*

*Note on the example:* abbreviated illustration. A full run produces all three A/B drafts per trigger, both languages for every message, and the front-office callback script in full. No procedure names appear in any outbound message (HIPAA), and no review screening or incentives are used.
