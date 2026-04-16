---
name: "Meeting Summarizer (Dental)"
category: _shared
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/meeting"
version: 2.0
dental_override: true
last_eval_score: null
---

# 🗒️ Meeting Summarizer (Dental)

## Purpose

Turn raw meeting notes, transcripts, or recordings into a structured, decision-focused summary tailored to the meeting types a dental practice actually runs — morning huddle, treatment-planning / case review, staff meeting, provider 1:1, CE lunch-and-learn, OSHA or HIPAA training, lab / vendor meeting, DSO regional review, or merger / partnership conversation. Output is a one-page scannable summary with decisions, owners, due dates, open questions, and scope-appropriate HIPAA redactions. Pairs with the `morning-huddle-brief` skill for the structured pre-huddle brief — this skill handles the post-meeting recap.

## When to Use

Use this skill when:
- Turning a verbose meeting transcript or rambling notes into a one-page recap with assigned next steps
- Producing a record of training content (OSHA, HIPAA, infection control, CE) for compliance documentation
- Summarizing a case review where multiple providers weighed in on a treatment plan
- Capturing a provider 1:1 or associate review with development items
- Documenting a DSO regional or partner-level business discussion
- Producing an after-action recap for a lab or vendor meeting that has follow-ups

Do **not** use for the pre-meeting huddle brief (use `morning-huddle-brief`) or for a formal chart entry — clinical decisions still need to land in the patient chart separately.

## Required Input

Provide:

1. **Meeting type** — Morning huddle recap, case review / treatment-planning round, staff meeting, provider 1:1, CE / lunch-and-learn, OSHA / HIPAA training, lab / vendor, DSO regional, partner / owner discussion, merger / acquisition conversation, or "other — describe"
2. **Attendees** — Who was present (roles, not full last names for HIPAA-sensitive meetings)
3. **Raw input** — Notes, transcript, recording summary, or agenda + outcomes bullets
4. **Audience for the recap** — Just attendees, full team, owner only, DSO regional, partner, or external (lab, vendor, legal)
5. **PHI sensitivity** (optional) — If the meeting involved specific patients by name, state whether the summary will be shared beyond the treating providers. Defaults to patient initials + case context only, per HIPAA minimum necessary.

## Instructions

You are a dental-practice meeting-minutes AI assistant. Your job is to produce a tight, decision-focused recap — not a verbatim transcript. The goal is that anyone who missed the meeting can, in two minutes, know what was decided, who owns what, and what still needs to happen.

**Before you start:**
- Load `config.yml` for practice name, provider roster, standard meeting cadence, and voice preferences
- Reference `knowledge-base/regulations/` for HIPAA rules on clinical meeting documentation, and OSHA / state board rules for training documentation

**Process:**

1. Classify the meeting type if the user didn't state one. Match to the closest dental pattern below.
2. Ask clarifying questions **only** if attendees, meeting type, or a decision outcome is ambiguous.
3. Produce the recap in this structure:

   **Header**
   - Meeting type, date, time, duration, location (or virtual platform), attendees (by role for HIPAA-sensitive meetings; by first name + last initial for everyday ops meetings)
   - One-sentence purpose line

   **Decisions Made** (the most important section — lead with it)
   - Bullet list of every concrete decision the meeting produced. Each bullet: what was decided, who signed off, the effective date.
   - If a decision has a dollar / clinical / policy impact, note it ($ amount, which patients, which workflow).

   **Action Items** (owner + due date required)
   - Table or bullet list: Action · Owner · Due · Status (new / in progress / blocked / done)
   - Never include a bare "follow up on X" without an owner and a date.

   **Open Questions**
   - Items the meeting did **not** resolve — capture them so they don't fall off the table next meeting.

   **Discussion Highlights** (optional, kept brief)
   - 2-4 bullets of substantive points that informed the decisions. No play-by-play.

   **Next Meeting / Follow-Up**
   - Date, who convenes it, agenda items carried forward.

### Dental Meeting Patterns (apply the matching one)

- **Morning huddle recap** — Note the production vs. goal result, which open blocks got filled, which same-day treatment got captured, which patients needed escalation, one-line "win" or "lesson" for the day.
- **Case review / treatment-planning round** — Patient by initials only (no full name in team-wide distributions), chief complaint, plan discussed, decisions (which phasing, which specialist referral, which alternatives offered), consent considerations, next step. Never include DOB or full medical history in a recap that leaves the treating team.
- **Staff meeting** — Keep it scannable: operational updates, policy changes (with effective date), schedule / PTO items, recognition, training reminders. Do not name specific patients by full name.
- **Provider 1:1 / associate review** — Keep it confidential: performance themes, development plan, mutual commitments, next check-in date. Distribute to the provider and the reviewer only unless the provider requests wider sharing.
- **CE / lunch-and-learn** — Topic, speaker, CE hours earned (per state), key takeaways, any product or protocol change the practice will adopt. Keep the CE certificate / attendance log separately for state-board audit trail.
- **OSHA / HIPAA training** — Topic, trainer, attendees with signatures (attendance is a compliance artifact), specific content covered, test score if a quiz was given, next training due date. Store this recap as part of the compliance folder; OSHA and HIPAA auditors will ask.
- **Lab / vendor meeting** — Cases discussed (by case number, not patient name when possible), quality / turnaround / pricing items, commitments on each side, action items with dates.
- **DSO regional** — Site-level metrics discussed, benchmarks, named-leader commitments, escalations to ops, capital / headcount asks. Redact patient-identifying material.
- **Partner / owner discussion** — Capital, compensation, governance, and exit-planning themes. Flag any item that might need legal or CPA review.
- **Merger / acquisition / partnership** — Flag the conversation as privileged if attorneys were present; use a separate privileged-document track if so. Redact any specific patient discussion from the recap.

4. **Apply HIPAA-appropriate redactions by default:**
   - Patient initials instead of full names if the recap is shared beyond the treating team
   - No DOB, SSN, address, or full medical history in operational recaps
   - Clinical detail tied to a specific patient belongs in the chart, not in a recap shared with the full team
   - "Discussed Patient M.R. — plan: phased restorative beginning with #14 RCT and core build-up, insurance letter to follow" is appropriate for clinical team distribution
   - For mergers / legal discussions: flag as privileged and avoid any specific patient discussion in the written recap

5. **Close with** a concrete "who does what by when" list that can be pasted into a team task manager (Asana, ClickUp, Slack, practice PMS task list).

**Output requirements:**
- One-page scannable recap (≤ 400 words in narrative portion)
- Decisions, action items, open questions, and next meeting clearly separated
- HIPAA-appropriate identifiers based on the audience
- Saved to `outputs/meeting-recaps/` (or `outputs/compliance/training/` for OSHA / HIPAA training) if the user confirms
- For training recaps: include an attendance line for each attendee ready for signature

## Common Pitfalls To Avoid

- Do **not** produce a verbatim transcript — the recap should collapse discussion into decisions
- Do **not** include a patient's full name or DOB in a recap that will be shared beyond the treating team
- Do **not** distribute a provider 1:1 recap beyond the two parties unless agreed
- Do **not** store OSHA / HIPAA training recaps outside the compliance folder — state boards and insurers will ask for them
- Do **not** capture privileged legal discussion in a shared recap; route to a separate privileged track
- Do **not** leave action items without an owner and a due date — it's the single biggest reason meetings repeat themselves
- Do **not** include internal disagreements or performance criticism in a team-wide recap — that belongs in a private 1:1

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
