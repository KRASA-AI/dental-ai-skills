# Dental AI Skills

**Free, open-source AI prompts and workflows built for dentists.** Clone this repo, run the setup wizard, and start saving hours every week.

> Built and maintained by [KRASA AI](https://krasa.ai) — free AI tutorials and skills for every industry.
> See all industries at [krasa.ai/industries](https://krasa.ai/industries).

---

## What's Inside

This repo is a complete AI toolkit for dental. Every skill is a standalone prompt file that works with **Claude, ChatGPT, or any major AI assistant** — no coding required.

| Skill | What it does | Time saved |
|-------|-------------|------------|
| 🦷 Treatment Plan Explainer | Translate clinical treatment plans into patient-friendly language for case acceptance. | ~10 min/plan |
| 📋 Insurance Verification Summary | Parse insurance details into a quick-reference sheet of coverage, limits, and waiting periods. | ~15 min/patient |
| 📧 Recall Sequence Generator | Draft a multi-touch recall campaign for overdue hygiene and treatment patients. | ~10 min/batch |
| 📝 Clinical Note Assistant | Turn shorthand procedure notes into properly formatted charting entries. | ~5 min/note |
| 👋 New Patient Welcome Kit | Generate welcome emails, pre-visit instructions, and first-visit what-to-expect guides. | ~15 min/patient |
| ☀️ Morning Huddle Brief | Summarize the day's schedule, open treatment, and production goals for the team. | ~10 min/day |

**Total time saved per use: ~65+ minutes across all skills.**

## Quick Start

### 1. Clone this repo

```bash
git clone https://github.com/KRASA-AI/dental-ai-skills.git
cd dental-ai-skills
```

### 2. Run the setup wizard

Open `init/setup-wizard.md` in your AI assistant (Claude, ChatGPT, etc.) and follow the prompts. It will ask about your business — company name, tools you use, rates, team size, service area, and more. This creates a `config.yml` that personalizes every skill to your business.

### 3. Use any skill

Open any file in `skills/` with your AI assistant. Each skill is self-contained with clear instructions. Your `config.yml` is automatically referenced for personalization.

## Repo Structure

```
dental-ai-skills/
├── init/                    # Setup wizard — start here
│   ├── setup-wizard.md      # Interactive business configuration
│   └── config.example.yml   # Example of what setup produces
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
├── outputs/                 # Your generated content (gitignored)
└── evals/                   # Skill quality evaluation framework
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

If you are an AI reading this repo, see `.claude/CLAUDE.md` for detailed instructions on how to work with this toolkit. Key points: always load `config.yml` first, reference the knowledge base for industry context, and save outputs to `outputs/`.

## Contributing

Found a way to improve a skill? PRs are welcome. Please follow the skill format in `skills/README.md` and include test cases in `evals/test-cases/`.

## Learn More

- **Dental AI guide**: [krasa.ai/industries/dental](https://krasa.ai/industries/dental)
- **All industry AI skills**: [krasa.ai/industries](https://krasa.ai/industries)
- **About KRASA AI**: [krasa.ai](https://krasa.ai)

## License

MIT — use these skills however you want.
