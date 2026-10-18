# Agent team

I will use this custom agent team to build Mona's Project Pulse dashboard, with GitHub Copilot CLI in a Codespace coordinating the work.

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 5.5 (copilot) | Breaks the request into phases, delegates scoped work to specialists, coordinates dependencies, and verifies the integrated result. It does not implement the dashboard itself. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 5.5 (copilot) | Researches the repository, documentation, dependencies, and edge cases, then provides an ordered implementation plan with file assignments and validation expectations. | `.github/agents/planner.agent.md` |
| **Designer** | Gemini 3.7 Flash (copilot) | Shapes the dashboard's user experience, visual hierarchy, accessibility, responsive behavior, and styling. For Project Pulse, it focuses on polished project cards, status badges, priority treatment, and clear layout. | `.github/agents/designer.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements dashboard code and assigned runnable-app support, following repository patterns and validating changes. For Project Pulse, this includes the launch configuration when assigned. | `.github/agents/coder.agent.md` |

The Orchestrator will have the Planner establish the approach, then assign non-overlapping implementation work to the Designer and Coder as appropriate. The agents will not stage, commit, or push changes; I retain control of Git operations through Copilot CLI.
