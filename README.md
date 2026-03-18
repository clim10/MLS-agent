# AI Agents — Manulife Singapore Data Platform

## Overview

This repository contains markdown-based agent definitions, skill documents, and operating instructions for a multi-role AI copilot serving **Manulife Singapore**, a life insurance company. The framework supports three practitioner roles — **Data Scientist**, **Data Engineer**, and **Data Strategy Lead** — plus an independent **Data Scientist Reviewer** for quality assurance and governance.

All agents and skills are tailored to the Manulife Singapore context:
- **Technology stack**: Databricks (Unity Catalog, Delta Lake, MLflow) on Microsoft Azure
- **Regulatory environment**: MAS (Monetary Authority of Singapore), PDPA, MAS FEAT Principles for AI/ML
- **Insurance domain**: life insurance, wealth management, protection, group insurance

## Repository Layout

```
agents/                              — Agent definitions and routing logic
  data-scientist-agent.md            — Primary agent: routes to all 4 skills
  data-scientist-reviewer-agent.md   — Reviewer agent: audits DS deliverables
instructions/                        — Agent operating context
  copilot-instructions.md            — Company context, tech stack, routing rules, guardrails
skills/                              — Skill documents organized by domain
  data-scientist/SKILL.md            — Advanced analytics, ML, insurance modeling (Databricks/Azure)
  data-engineer/SKILL.md             — Pipelines, Delta Lake, Unity Catalog, medallion architecture
  data-strategy/SKILL.md             — Governance, roadmaps, MAS/PDPA compliance, stakeholder comms
  data-scientist-reviewer/SKILL.md   — Review checklists, scoring, regulatory compliance audit
```

## Skills Quick Reference

| Skill | Use When |
|---|---|
| `data-scientist` | EDA, ML modeling, forecasting, segmentation, A/B testing, insurance analytics (lapse, claims, CLV, propensity) |
| `data-engineer` | Databricks pipelines, Delta Lake, Unity Catalog, medallion architecture, data quality, SCD, streaming |
| `data-strategy` | Data governance, strategy roadmaps, data maturity assessments, MAS FEAT / PDPA frameworks, executive comms |
| `data-scientist-reviewer` | Review/audit completed DS outputs — methodology, fairness, PDPA compliance, production-readiness |

> For production-bound or customer-facing models, always chain `data-scientist` → `data-scientist-reviewer`.

## Quick Start

1. Open this folder in Visual Studio Code.
2. Read [instructions/copilot-instructions.md](instructions/copilot-instructions.md) — it defines the agent's operating context: company background, technology stack, skill routing rules, regulatory guardrails, and output standards. Start here to understand how the copilot behaves.
3. Edit or add agent definitions in the `agents/` folder and skill documents in the `skills/` folders.
4. If you use a local agent runtime or extension, follow that runtime's steps to reload or publish agents (runtime not included here).
5. To use a markdown agent as a generic local agent, copy the agent file into your home `.copilot/agents/` folder:

- Windows: `%USERPROFILE%\.copilot\agents\`
- macOS/Linux: `~/.copilot/agents/`

Create the `.copilot/agents/` folder if it doesn't exist.

## Skills and Placement

- If your agent references files in a `skills/` folder, keep those `skills/` files alongside the agent files you copy into `.copilot/agents/` so relative paths continue to work.
- Alternatively, you can maintain a global `~/.copilot/skills/` folder for shared skills across agents, but be careful of name collisions and versioning.

## Recommendation

- **Root-level `skills/` (recommended for shared skills):** Store `skills/` at the repository root next to `agents/` when multiple agents reuse the same skill files. This reduces duplication and makes updates easier across agents.
- **Agent-local `skills/` (recommended for portability):** Keep `skills/` inside an agent folder when you want that agent to be self-contained and easily portable.
- **If centralizing:** namespace skill folders (for example, `skills/data-scientist/`) and use simple versioning (for example, `skills/data-scientist/v1/`) to avoid conflicts and manage changes.

## Copy Examples

Option A — Shared root-level skills (recommended when multiple agents reuse skills)

PowerShell (Windows)
```powershell
# Create global agents and skills folders, then copy repo-level agents and skills
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.copilot\agents"
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.copilot\skills"
Copy-Item -Path ".\agents\*" -Destination "$env:USERPROFILE\.copilot\agents\" -Recurse -Force
Copy-Item -Path ".\skills\*" -Destination "$env:USERPROFILE\.copilot\skills\" -Recurse -Force
```

Bash (macOS/Linux)
```bash
# Create global agents and skills folders, then copy repo-level agents and skills
mkdir -p ~/.copilot/agents
mkdir -p ~/.copilot/skills
cp -r agents/* ~/.copilot/agents/
cp -r skills/* ~/.copilot/skills/
```

Option B — Single agent with its skills (useful when copying one agent at a time)

PowerShell (Windows)
```powershell
# Example: copy the data-scientist agent and its skill to the global agents folder
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.copilot\agents"
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.copilot\skills\data-scientist"
Copy-Item -Path ".\agents\data-scientist-agent.md" -Destination "$env:USERPROFILE\.copilot\agents\" -Force
Copy-Item -Path ".\skills\data-scientist\SKILL.md" -Destination "$env:USERPROFILE\.copilot\skills\data-scientist\" -Force
```

Bash (macOS/Linux)
```bash
# Example: copy the data-scientist agent and its skill to the global agents folder
mkdir -p ~/.copilot/agents
mkdir -p ~/.copilot/skills/data-scientist
cp agents/data-scientist-agent.md ~/.copilot/agents/
cp skills/data-scientist/SKILL.md ~/.copilot/skills/data-scientist/
```