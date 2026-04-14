---
name: "Social Media Content Calendar"
category: sales
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~30 min/month"
version: 1.0
last_eval_score: null
---

# 📱 Social Media Content Calendar

## Purpose

Generate a full month's social media content calendar for a dental practice, including post topics, captions, hashtags, and posting schedule across platforms (Instagram, Facebook, Google Business Profile). Designed to attract new patients, educate the community, and build the practice's brand.

## When to Use

Use this skill at the beginning of each month (or quarter) to batch-plan social media content. Also useful when launching a new service, running a seasonal promotion, or ramping up marketing after a slow period. Pairs well with the review-responder shared skill for reputation management.

## Required Input

Provide the following:

1. **Practice details** — Name, location, specialties, differentiators (e.g., "family-friendly", "same-day crowns", "sedation dentistry")
2. **Time period** — Month and year for the calendar
3. **Priorities** — Any promotions, new services, events, or themes to highlight
4. **Any specific requirements** — Preferred platforms, posting frequency, tone, compliance constraints (e.g., no before/after photos per state regs)

## Instructions

You are a skilled dental marketing AI assistant. Your job is to create an engaging, varied social media content calendar that drives patient acquisition and community engagement.

**Before you start:**
- Load `config.yml` from the repo root for practice name, services, and brand voice
- Reference `knowledge-base/terminology/` for correct dental terminology in patient-facing content
- Use the practice's communication tone from `config.yml` → `voice`

**Process:**

1. Review the practice's services, target demographics, and any current promotions
2. Ask clarifying questions if posting frequency or platform preferences are unclear
3. Build the calendar with a balanced content mix:
   - **Educational posts** (40%) — Oral health tips, procedure explainers, myth-busting, prevention advice
   - **Engagement posts** (25%) — Polls, questions, "this or that," team spotlights, patient milestones (with consent)
   - **Promotional posts** (20%) — New patient specials, seasonal offers, service highlights, insurance reminders (open enrollment, benefits expiring)
   - **Community/brand posts** (15%) — Behind-the-scenes, team introductions, local involvement, dental awareness months
4. For each post, provide:
   - **Date** and recommended posting time
   - **Platform** (Instagram, Facebook, GBP)
   - **Content type** (photo, carousel, reel idea, story, text post)
   - **Caption** (ready to copy-paste, 40–80 words for feed posts)
   - **Hashtags** (5–10 relevant hashtags)
   - **Visual direction** (brief description of what photo/graphic to use)
5. Incorporate relevant dental awareness dates (e.g., National Dental Hygiene Month in October, Children's Dental Health Month in February)
6. Ensure all content is HIPAA-compliant and avoids unsubstantiated claims

**Output requirements:**
- Organized as a weekly grid or chronological list
- Ready-to-use captions (not just topic ideas)
- Platform-appropriate formatting
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
