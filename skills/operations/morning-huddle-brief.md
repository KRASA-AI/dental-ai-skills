---
name: "Morning Huddle Brief"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/day"
version: 2.0
last_eval_score: null
---

# ☀️ Morning Huddle Brief

## Purpose

Generate a focused, standard-format morning huddle brief that the doctor, hygienist, front desk, and assistants can all follow in 10 minutes — covering the day's production goal, patient-by-patient schedule review, same-day treatment opportunities, medical alerts, lab cases, new patients, unscheduled treatment in today's patients, and yesterday's carry-overs. The goal of the huddle is not just awareness — it is to leave the meeting with concrete decisions on who fills open chair time, which patients get offered same-day treatment, and which patients need extra preparation.

## When to Use

Use this skill every practice morning (or at the end of the prior day for the next morning). Works for solo GP, multi-doctor group practice, and DSO offices. Also useful for virtual huddles when providers are traveling, and for training new office managers or TCs on huddle structure.

## Required Input

Provide the following:

1. **Today's schedule** — Appointment list with patient names, appointment types (NP exam, hygiene, crown seat, etc.), provider, operatory, start and end times
2. **Daily production goal** — Dollar target for the day (from config if not specified)
3. **Yesterday's outcomes** (optional) — Production achieved, any cancellations or no-shows, treatment presented but not accepted
4. **Known medical alerts** — Patients on anticoagulants, patients requiring premedication, significant allergies, recent hospitalizations, pregnancy
5. **Lab cases** — Cases arriving today (need to be seated), cases leaving today (need to ship), overdue cases
6. **Open chair time / holes** — Unfilled blocks that need backfill
7. **Unscheduled treatment** (optional) — If available, list of today's patients who have diagnosed-but-unscheduled treatment

## Instructions

You are a skilled dental practice management AI assistant. Your job is to produce a tight, scannable morning huddle brief that drives action — not a narrative summary. Every section should end with a specific decision or assignment.

**Before you start:**
- Load `config.yml` from the repo root for practice name, provider roster, daily production goal, hygiene production goal, and standard huddle format preferences
- Reference `knowledge-base/best-practices/` for huddle frameworks if present
- Reference `knowledge-base/terminology/` for correct procedure and code descriptors

**Process:**

1. Parse the schedule and group patients by provider, operatory, and visit type
2. Ask clarifying questions **only** if critical data is missing (no production goal, no schedule data, ambiguous provider names)
3. Generate the brief using the following standard sections, in order:

   **1. Today at a Glance** (top of page, 3-5 lines)
   - Date, day of week, weather if relevant to attendance
   - Doctor production goal vs. scheduled ($ and %)
   - Hygiene production goal vs. scheduled ($ and %)
   - Total patients: doctor column, hygiene column
   - Open chair time: list of blocks (operatory, time, length)

   **2. Yesterday's Scoreboard** (optional, 2-3 lines)
   - Production achieved vs. goal
   - Notable misses (no-shows, last-minute cancellations)
   - Treatment presented / accepted / pending

   **3. Patient-by-Patient Review** (bulk of the brief)
   For each patient, a one-line entry plus flags:
   - `[Time] • [Provider/Op] • [Patient First Name + Last Initial] • [Appt Type] • [Production $]`
   - 🚩 **Medical alert:** anticoagulant, premed required, allergy, pregnancy, recent cardiac event, diabetes A1c concerns
   - 🔄 **Unscheduled Tx:** list the top 1-2 diagnosed-but-unscheduled items for this patient, so the provider can offer same-day
   - ⚠️ **Account balance:** if there is an outstanding balance to collect at check-in
   - 📷 **Radiograph due:** if BWs/FMX/pano is due today
   - 💍 **New patient:** flag first-time patients and note the referral source
   - 👶 **Pediatric / parent present** or 😰 **Anxiety flag** when noted

   **4. Lab & Materials**
   - Cases seated today: patient name, provider, case (e.g., "Smith J • Dr. Patel • #14 zirconia crown") — verify arrival before appointment
   - Cases shipping today: what needs to leave and by when
   - Overdue or missing cases: escalation needed

   **5. Same-Day Treatment Opportunities**
   - For hygiene patients with doctor checks: list likely same-day treatment the doctor should offer (e.g., "#30 existing MOD amalgam — watch for recurrent decay, sealant-to-composite conversion")
   - For patients with cancelled next appointments: can today's block be extended?
   - List any open block the team can fill with these opportunities

   **6. New Patient Spotlight**
   - For each new patient: first name + last initial, appointment time, referral source, chief complaint or reason for visit, what the team should know to make a great first impression
   - Who will do the practice tour? Who will do the financial presentation?

   **7. Open Chair Time & Fill Plan**
   - List each open block with a targeted fill plan (e.g., "9:30 Op 2, 60 min — call waitlist for #19 crown seat; text recall-due patients within 5 miles")
   - Assign each fill task to a specific team member

   **8. Team Logistics**
   - Out-of-office: who's out, who covers
   - CE or admin blocks built into the schedule
   - Meeting reminders (staff meeting, vendor lunch, huddle tomorrow)
   - OSHA / HIPAA / compliance tasks due this week

   **9. Huddle Close: Decisions & Assignments**
   - Bullet list of concrete decisions coming out of the huddle: who is calling whom, what gets offered, what stays on the schedule
   - "One focus word" for the day (energy-setter) — e.g., "presence," "follow-through," "accuracy"

4. Apply these formatting rules:
   - Keep the entire brief to **1 page** (roughly 400-600 words) — it's a huddle, not a briefing book
   - Use bullets, not paragraphs
   - Prioritize scannability — the doctor reads it in 30 seconds before the huddle starts
   - Use patient first name + last initial only (HIPAA minimum-necessary for a document that may be posted in a clinical area)

**Output requirements:**
- One-page format ready to print or display on a shared screen
- HIPAA-appropriate (no SSN, no DOB, no full last name, no clinical diagnoses in detail)
- Decisions and assignments explicit — not implied
- Tone professional but energetic — this sets the day's mood
- Saved to `outputs/` if the user confirms

## Common Pitfalls To Avoid

- Do not produce a narrative summary — the huddle brief is a scannable action document
- Do not list every diagnostic finding for every patient — only those relevant to today's decisions
- Do not include detailed PHI beyond what's needed (first name + last initial, not full identifiers)
- Do not skip the "Decisions & Assignments" section — a huddle without assigned actions is just a status meeting

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
