# Agent team

I will use this custom agent team to build Mona's Project Pulse dashboard:

- **Orchestrator** — Claude Opus 4.7 (copilot). Coordinates the specialists, assigns file scopes and phases, and verifies the integrated result. Definition: `.github/agents/orchestrator.agent.md`.
- **Planner** — Claude Opus 4.7 (copilot). Researches the repository and dependencies, then produces an implementation plan with tasks, dependencies, edge cases, and validation expectations. Definition: `.github/agents/planner.agent.md`.
- **Designer** — Gemini 3.1 Pro (copilot). Shapes the dashboard's user experience, accessibility, information hierarchy, interactions, responsive behavior, and visual design. Definition: `.github/agents/designer.agent.md`.
- **Coder** — GPT-5.5 (copilot). Implements assigned code and app support, handles errors clearly, and validates changes. Definition: `.github/agents/coder.agent.md`.

I will use GitHub Copilot CLI in a Codespace to orchestrate the work and delegate tasks to these agents.
