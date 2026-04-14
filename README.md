# Dental AI Skills

**Free, open-source AI prompts and workflows built for dentists.** Clone this repo, point your AI assistant at it, and start saving hours every week.

> Built and maintained by [KRASA AI](https://krasa.ai) — free AI tutorials and skills for every industry.
> See all industries at [krasa.ai/industries](https://krasa.ai/industries).

---

## What's Inside

This repo is a complete AI toolkit for dental. Every skill is a standalone prompt file that works with **Claude, ChatGPT, or any major AI assistant** — no coding required.

| Skill | What it does | Time saved |
|-------|-------------|------------|
| Clinical Evidence Review | Produce a structured, evidence-graded review of a clinical question — a treatment option, a material comparison, a diagnostic workflow, or a protocol change — that a dentist, hygienist, or study club can trust to guide decisions. | ~60 min/topic |
| Clinical Note Assistant | Turn shorthand procedure notes, voice-to-text dictations, or bullet-point summaries into properly formatted clinical charting entries using SOAP note structure, correct dental terminology, tooth numbering, and CDT code documentation standards. | ~5 min/note |
| Lab Prescription Drafter | Draft a complete, unambiguous lab prescription (Rx / work authorization) for fixed, removable, or implant prosthetics so the dental laboratory receives every detail it needs on the first submission — shade, material, tooth number(s), margin design, occlusal scheme, pontic design, due date, and any patient-specific notes. | ~8 min/case |
| Morning Huddle Brief | Generate a focused, standard-format morning huddle brief that the doctor, hygienist, front desk, and assistants can all follow in 10 minutes — covering the day's production goal, patient-by-patient schedule review, same-day treatment opportunities, medical alerts, lab cases, new patients, unscheduled treatment in today's patients, and yesterday's carry-overs. | ~15 min/day |
| Referral Coordination Letter | Generate a professional specialist referral letter that summarizes the patient's clinical history, reason for referral, relevant findings, and any time-sensitive details — ensuring a smooth handoff between the general dentist and the specialist. | ~10 min/referral |
| Social Media Content Calendar | Generate a full month's social media content calendar for a dental practice, including post topics, captions, hashtags, and posting schedule across platforms (Instagram, Facebook, Google Business Profile). | ~30 min/month |
| Treatment Case Presentation Script | Turn a diagnosed treatment plan into a structured, empathetic patient-facing case presentation that covers the "why now," procedure overview, expected outcomes, total investment, financing options, and a confident close — in language the patient will actually understand. | ~15 min/case |
| New Patient Welcome Kit | Generate a complete new patient welcome package — including a welcome email, pre-visit instructions, what-to-expect guide, and office information sheet — so every new patient arrives informed, prepared, and confident in their choice of practice. | ~20 min/patient |
| Patient Reactivation Sequence | Generate a multi-channel reactivation campaign aimed at **lapsed patients** — people who have gone 12+ months without an appointment, have stopped responding to normal recall messages, or have unscheduled diagnosed treatment from a previous plan. | ~45 min/campaign |
| Post-Op Care Instructions | Generate personalized, patient-friendly aftercare instructions for any dental procedure, including recovery timelines, do's-and-don'ts, medication guidance, and red-flag symptoms that warrant a callback. | ~10 min/patient |
| Recall Sequence Generator | Draft a multi-touch recall campaign for overdue hygiene and treatment patients — complete with message copy, timing, channel assignments, and escalation logic — to bring lapsed patients back on the schedule. | ~20 min/batch |
| Treatment Plan Explainer | Translate a clinical treatment plan into a written, patient-friendly explainer the patient can take home, receive by email, or view in the patient portal. | ~15 min/plan |
| CDT Code Suggestion Assistant | Suggest appropriate CDT (Current Dental Terminology) codes based on clinical documentation, procedure notes, or a description of work performed. | ~10 min/encounter |
| Chart Audit Prep Checklist | Generate a chart-by-chart audit readiness checklist for dental records under review by an insurance carrier, a state dental board, a DSO compliance team, or a defense attorney preparing for litigation. | ~30 min/chart |
| Insurance Denial Appeal Letter | Draft a professional, persuasive appeal letter when a dental insurance claim is denied, citing clinical evidence, CDT codes, and medical necessity to support reconsideration. | ~20 min/letter |
| Insurance Verification Summary | Turn a raw insurance breakdown (from a portal dump, a verification call recording, or a faxed EOB) into a standardized, one-page quick-reference summary the front desk, TC, and clinical team can actually read in 30 seconds. | ~20 min/patient |
| Email Drafter | Turn rough notes into a professional email matching your company's voice and tone. | ~10 min/use |
| Meeting Summarizer | Summarize meeting notes into action items, decisions, and follow-ups. | ~10 min/use |
| Review Responder | Craft professional responses to online reviews — both positive and negative. | ~10 min/use |

**Total time saved per use: ~363+ minutes across all skills.**

## Quick Start

### 1. Clone this repo

```bash
git clone https://github.com/KRASA-AI/dental-ai-skills.git
cd dental-ai-skills
```

### 2. Open a skill with your AI assistant

Open any file in `skills/` with Claude, ChatGPT, or any major AI assistant. Each skill is a self-contained prompt with clear instructions — no coding required.

The first time you use a skill, your AI assistant will ask for your business details (company name, service area, rates, tools you use, etc.) so it can personalize the output. Save those details to a `config.yml` at the repo root and every future skill will use them automatically.

## Repo Structure

```
dental-ai-skills/
├── knowledge-base/          # Industry context and references
│   ├── industry-overview.md # Market trends and pain points
│   ├── terminology/         # Industry jargon and acronyms
│   ├── regulations/         # Compliance requirements
│   ├── best-practices/      # Industry standards
│   └── tools-ecosystem/     # Common software and tools
├── skills/                  # The prompt library
│   ├── operations/          # Day-to-day operational skills
│   ├── sales/               # Sales and lead management
│   ├── admin/               # Administrative and compliance
│   └── customer-service/    # Client-facing communication
└── outputs/                 # Your generated content (gitignored)
```

## How Skills Work

Each skill file is a Markdown document with YAML frontmatter:

```markdown
---
name: Skill Name
category: operations
tools: [claude, chatgpt]
time_saved: "~20 min/use"
version: 1.0
---

# Skill Name

## Purpose
What this skill does and when to use it.

## Instructions
Step-by-step prompt for the AI assistant.
```

You open the file in your AI assistant, provide any required input (measurements, notes, client info), and get polished output. Skills reference your `config.yml` automatically for company name, rates, preferred formats, and other business details.

## For AI Assistants

If you are an AI assistant reading this repo, see `.claude/CLAUDE.md` for full instructions. The short version:

1. **Check for `config.yml`** at the repo root. If it exists, load it — it holds the user's business context (company name, rates, service area, tools, team size, etc.) and every skill should use it for personalization.
2. **If `config.yml` is missing**, before running a skill that benefits from personalization, ask the user for the relevant business details and offer to save them to `config.yml` so future runs are automatic.
3. **Load the relevant `knowledge-base/` files** for industry terminology, regulations, and best practices before generating output.
4. **Run the requested skill** from `skills/` using the user's input.
5. **Save any deliverables** to `outputs/` (gitignored) if the user wants to keep them.

## Learn More

- **Dental AI guide**: [krasa.ai/industries/dental](https://krasa.ai/industries/dental)
- **All industry AI skills**: [krasa.ai/industries](https://krasa.ai/industries)
- **About KRASA AI**: [krasa.ai](https://krasa.ai)

## License

MIT — use these skills however you want.
