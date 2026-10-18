# Project Pulse dashboard implementation plan

## Goal and repository findings

Build a lightweight static dashboard that helps Mona's contributors see active projects, owners, status, recent activity, priority or risk, and a short summary. The project brief specifies `app/index.html`, `app/styles.css`, `app/project-data.json`, and a VS Code launch configuration that serves the `app/` directory and opens `index.html`.

Repository facts:

- `.github/project-pulse-brief.md` defines the dashboard content, data fields, static-app structure, and launch behavior.
- `.github/agents/` contains the Planner, Designer, Coder, and Orchestrator definitions. The Designer owns UI/UX and styling guidance; the Coder implements assigned files and can create the launch configuration.
- The repository already has `.vscode/tasks.json`, but no `.vscode/launch.json` or `app/` files.
- `scripts/validate-exercise.sh` checks generated learner files and several exact dashboard hooks/content requirements, including a `.dashboard` selector, project-card markup and styles, rounded/shadow styling, project data fields, launch configuration name, and valid launch JSON.

Assumptions to confirm during implementation: use a small representative set of fictional project records; keep the dashboard dependency-free and static; and use a locally available server command for the launch configuration rather than adding a package dependency.

## Ordered work and file ownership

| Phase | Owner | File assignments | Work and deliverables |
| --- | --- | --- | --- |
| 1. Agree on the interface contract | Designer, coordinated by Orchestrator | No file changes required | Define the dashboard hierarchy, responsive behavior, accessible status/priority treatments, and the HTML/CSS hooks needed for a polished view. Align on `.dashboard` and `.project-card` selectors before implementation. |
| 2. Build data and presentation in parallel | Designer and Coder, coordinated by Orchestrator | Designer: `app/styles.css`. Coder: `app/project-data.json`, `app/index.html`. | Designer creates responsive, readable styling with clear project cards, badges, priority/risk distinctions, spacing, contrast, rounded corners, and shadows. Coder creates a top-level `projects` array with `name`, `owner`, `status`, `recentActivity`, and `priority` for each record, and semantic HTML that links the stylesheet and displays those fields, including a short contributor-friendly summary. Both follow the agreed selectors and content contract. |
| 3. Add the runnable-app configuration | Coder | `.vscode/launch.json` | Add strict JSON for the **Run Project Pulse Dashboard** configuration. Set `cwd` to `${workspaceFolder}/app`, serve that directory, and open `/index.html` at a deterministic local URL. Do not add dependencies unless the environment lacks a suitable server. |
| 4. Integrate and validate | Orchestrator, with Designer and Coder for their respective fixes | All four requested files | Review the integrated UI, confirm the data, markup, styling, and launch target agree, run the checks below, and report any remaining limitation. Make fixes in the file owner's scope. |

## Dependencies and parallel-work decisions

- Phase 1 precedes parallel implementation: agreeing on selectors and the display contract prevents conflicts between the HTML and CSS.
- In Phase 2, the CSS, data JSON, and HTML can be created in parallel after the contract is agreed. The data shape and HTML hooks are fixed by the brief; the Coder should use the agreed hooks and expected fields rather than inventing incompatible ones.
- Phase 3 depends on the target path and launch behavior specified in the brief, but does not depend on the dashboard's visual styling. It can run in parallel with Phase 2 if the Orchestrator assigns it after confirming the launch contract; otherwise, do it immediately after Phase 2.
- Phase 4 depends on all deliverables and is sequential. Any changes prompted by integration belong to the Designer for `app/styles.css` or the Coder for the other assigned files, followed by another integration check.
- Keep file ownership non-overlapping: Designer owns only `app/styles.css`; Coder owns `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`. The Orchestrator coordinates and verifies rather than implementing these files.

## Validation expectations

1. Parse `app/project-data.json` and `.vscode/launch.json` as JSON. Confirm the data has a top-level `projects` array and every project contains `name`, `owner`, `status`, `recentActivity`, and `priority`.
2. Inspect `app/index.html` to confirm it loads `styles.css`, presents the Project Pulse dashboard and project summaries, and exposes the project name, owner, status, activity, and priority in understandable semantic markup.
3. Inspect `app/styles.css` for the `.dashboard` and `.project-card` hooks, visible status and priority treatments, readable spacing and contrast, rounded corners and shadows, and responsive behavior.
4. Confirm `.vscode/launch.json` is strict JSON and defines **Run Project Pulse Dashboard**, serves from `${workspaceFolder}/app`, and opens `index.html` rather than a directory listing. Start the configured launch target if the environment supports it and verify the dashboard page responds at its configured URL.
5. Run `bash scripts/validate-exercise.sh` and address any failures relevant to the new learner deliverables. Report environmental skips or launch limitations rather than treating them as successful runtime validation.
