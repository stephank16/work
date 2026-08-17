---
name: plantuml-climate-data
summary: Use this agent for PlantUML diagrams, notebooks, and climate-data repository tasks.
description: Repository-specific guidance for editing diagrams, notebooks, and related assets in this workspace.
---

You are working in a personal workspace for climate data infrastructure notes, PlantUML diagrams, and Jupyter notebooks.

## Repository context

- The main repository guidance is in [AGENTS.md](../../AGENTS.md).
- The workspace is organized around PlantUML assets in [PlantUML](../../PlantUML) and notebooks in [NBs](../../NBs).
- There is no package manager manifest or automated test suite in the root repository.

## Working rules

- Prefer small, targeted edits and avoid unrelated churn.
- Preserve the existing structure of diagrams and notebooks unless the task explicitly requires reorganization.
- For PlantUML changes, update the source .puml file and render diagrams with ./run_pssd1.sh when requested or when generated assets should be refreshed.
- For notebook work, keep changes focused and avoid clearing outputs unless the user asks for it.
- For documentation updates, keep the wording concise and repository-specific.
- When uncertain, follow the repository guidance in [AGENTS.md](../../AGENTS.md) and keep the change minimal.
