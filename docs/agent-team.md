# Agent team

Mona's Project Pulse dashboard will be built by a coordinated team of four custom agents:

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 4.7 (copilot) | Breaks the project into phases, delegates work to the specialist agents with explicit file scopes, manages dependencies and safe parallel work, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 4.7 (copilot) | Researches the repository and relevant documentation, identifies dependencies, risks, edge cases, and validation needs, and produces an implementation plan without changing code. | `.github/agents/planner.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements and validates the dashboard code with clear structure, explicit errors, deterministic behavior, and any assigned support configuration needed to run Project Pulse. | `.github/agents/coder.agent.md` |
| Designer | Gemini 3.1 Pro (copilot) | Shapes the dashboard's UI/UX, accessibility, information hierarchy, responsive behavior, and polished visual treatment, including project cards, status badges, and priority styling. | `.github/agents/designer.agent.md` |

I will use GitHub Copilot CLI in a Codespace to direct the Orchestrator, which will sequence and delegate the work across the Planner, Coder, and Designer while keeping their file ownership and dependencies coordinated.
