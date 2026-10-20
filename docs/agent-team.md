# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the agents building Mona's Project Pulse dashboard.

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 5.5 (copilot) | Breaks work into tasks, delegates to the specialist agents, coordinates phases, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 5.5 (copilot) | Researches the repository and relevant dependencies, then creates an implementation plan with assignments, dependencies, risks, and validation expectations. | `.github/agents/planner.agent.md` |
| Designer | Gemini 3.7 Flash (copilot) | Shapes the dashboard's usability, accessibility, information hierarchy, interaction flow, responsive behavior, and visual design. | `.github/agents/designer.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements assigned application logic and support files, following repository patterns and validating the changes. | `.github/agents/coder.agent.md` |
