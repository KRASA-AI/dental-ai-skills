# PMS-Native AI Agents, MCP Layers & Platform AI — Dental Tools Ecosystem

A landscape note maintained by the landscape-monitor (created 2026-10-13). It tracks a 2026 shift: the practice-management and patient-engagement platforms themselves are shipping AI agents and, in one case, an MCP (Model Context Protocol) layer that lets AI assistants read and act on PMS data directly. This matters to every skill in this repo, because the sample practice in `init/config.example.yml` runs **Dentrix Ascend** and **Weave**.

## What shipped (concept-level summary; verify current availability with the vendor)

| Platform | 2026 move | What it means for a practice |
|---|---|---|
| **Dentrix Ascend (Henry Schein One)** — announced 2026-04-28 | Opened an **MCP layer** in three tiers: *Ask* (plain-language questions over clinical, operational, and financial data — preview), *Orchestrate* (pre-built agents for real-time eligibility, image/documentation-quality checks, claims reconciliation, recall-gap management, patient follow-up; cross-location orchestration for DSOs), and *Build* (practice- or DSO-configured workflows, dashboards, and agents — later phase). | An MCP-capable assistant can, in principle, pull live PMS data instead of relying on a pasted export. The announcement did not name supported AI clients or describe permissions, audit, or BAA terms — those are the diligence questions. |
| **Planet DDS (Denticon)** — Feb–Mar 2026 | "DentalOS" AI agents positioned as an operations workforce; separately, voice-driven periodontal charting inside Denticon. | Agent work starts inside the PMS; hygiene charting by voice changes how perio data reaches `clinical-note-assistant`-style workflows. |
| **Weave** — 2026-08-03 | AI Receptionist extended to voice **and** text with hours-based behavior, new-vs-existing caller handling, and staff-handoff rules; patient-submitted insurance capture that runs an eligibility check and writes verified details to the chart; desktop SSO. | Eligibility capture moves upstream of the front desk — overlaps `insurance-verification-summary` inputs. |

Related entries already tracked elsewhere: Dental Intelligence Explorer / AI Receptionist / Patient Score (see `ai-phone-receptionists.md`), Videa AutoVerify, Pearl RCM and Pearl Voice.

## How this repo should position against PMS-native agents

1. **Skills are the reasoning and narrative layer; the platform is the system of record.** Where a PMS agent already computes a number (eligibility status, recall gap list, claim reconciliation), skills should cite it and spend effort on judgment, wording, and next actions — the same rule already applied in `monthly-practice-kpi-report` and `morning-huddle-brief` for vendor analytics.
2. **Live data beats pasted exports, but only behind controls.** If a practice connects an AI assistant to a PMS MCP layer, every skill's "Required Input" can be satisfied by a tool call rather than a paste. Before doing so, confirm: a BAA that covers the AI assistant *and* the connector; least-privilege, read-only scopes for Ask-style use; per-user (not shared) credentials; an audit trail of what the agent read or changed; and a human approval step before any write-back (claims, chart notes, patient messages).
3. **Prompt injection becomes a PHI risk.** Once an assistant can read free-text fields (patient-submitted forms, call transcripts, referral letters) *and* call tools, text inside those fields can try to steer the agent. Treat patient- or third-party-authored text as data, never instructions — see `best-practices/phi-safe-prompting.md` and the AI-tool prompt-injection tabletop in `cybersecurity-incident-response-plan`.
4. **Vendor-concentration risk.** Agents with standing access to PMS, clearinghouse, and payer portals have the same blast radius as an outsourced billing vendor (see the standing-credential tier in `regulations/hipaa-security-rule-2026.md`). Inventory them in the BAA list.

## Evaluation questions for any PMS-native agent or MCP connection

- Which AI clients can connect, and is each covered by a BAA?
- What scopes exist (read vs. write; per-module), and can they be limited per user role?
- Is every agent action logged with user, time, and record touched? Can the practice export that log?
- Does any write-back (claim, ledger, chart, patient message) require explicit staff approval?
- Where is data processed and retained, and is it used for model training?
- What happens to agent access when a staff member or vendor is offboarded?

## Open items to watch

- General availability of the Dentrix Ascend Orchestrate and Build tiers, and whether named third-party AI clients are supported.
- Whether other PMS vendors (Eaglesoft, Open Dental, Curve, Carestream) publish MCP or comparable agent APIs.
- Any OCR or ADA guidance specific to agentic AI acting inside the dental record.
