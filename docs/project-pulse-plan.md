# Project Pulse implementation plan

## Summary

Build a polished, accessible static Project Pulse dashboard that helps contributors scan multiple projects by name, owner, status, recent activity, and priority. The Orchestrator will use this plan to coordinate the Designer and Coder while keeping file ownership and dependencies explicit.

## Agent responsibilities

- **Planner:** Defines the implementation phases, file assignments, dependencies, edge cases, parallel work decisions, and validation expectations.
- **Designer:** Defines the information hierarchy, responsive card layout, status and priority treatments, accessibility guidance, and visual details such as spacing, typography, border radius, shadows, and contrast.
- **Coder:** Implements the approved design and data model, creates the runnable preview configuration, and validates the completed dashboard.
- **Orchestrator:** Delegates each phase, prevents conflicting file edits, confirms dependencies are satisfied, and verifies the integrated result.

## Implementation phases and file assignments

### Phase 1: Define the dashboard contract

1. The Designer specifies the dashboard structure, card content hierarchy, responsive behavior, accessible color and focus states, and deterministic CSS hooks.
1. The Coder defines the project data shape with a top-level `projects` array. Each project must contain `name`, `owner`, `status`, `recentActivity`, and `priority`.
1. The Orchestrator confirms that the design and data contract agree before implementation begins.

| File | Owner | Assignment |
| --- | --- | --- |
| `app/project-data.json` | Coder | Create valid JSON with a top-level `projects` key and multiple representative projects containing all required fields. |

### Phase 2: Implement the dashboard

| File | Owner | Assignment |
| --- | --- | --- |
| `app/index.html` | Coder | Create accessible semantic markup with the exact title `Project Pulse`, reference `styles.css` and `project-data.json`, load the project data, and render a visible `.project-card` for every project. Show each project's name, owner, status, `recentActivity`, and priority. Include an explicit user-visible error state if data loading fails. |
| `app/styles.css` | Designer, then Coder | The Designer owns the visual direction; the Coder implements it. Include `.dashboard` and `.project-card` selectors, responsive layout, readable spacing and typography, accessible contrast and focus states, status and priority treatments, `border-radius`, and `box-shadow`. |

### Phase 3: Add the runnable preview

| File | Owner | Assignment |
| --- | --- | --- |
| `.vscode/launch.json` | Coder | Create strict JSON with no comments. Add a launch configuration named `Run Project Pulse Dashboard` that runs `python3 -m http.server 5500` with `cwd` set to `${workspaceFolder}/app` and uses `serverReadyAction` to open `http://localhost:%s/index.html`. |

### Phase 4: Integrate and validate

1. The Coder checks that the HTML, CSS, JSON data, and launch configuration work together.
1. The Designer reviews the running dashboard for visual hierarchy, responsive behavior, and accessibility.
1. The Orchestrator verifies the required files and behavior, records any limitations, and approves the handoff.

## Dependencies

- `app/project-data.json` establishes the field names consumed by `app/index.html`; the data contract must be agreed before final rendering logic is completed.
- The Designer's structure and styling guidance must be available before the Coder finalizes `app/index.html` and `app/styles.css`.
- `app/index.html` depends on both `app/styles.css` and `app/project-data.json` being available at paths relative to the `app/` directory.
- `.vscode/launch.json` depends on the final app location and entry point so it can serve `${workspaceFolder}/app` and open `/index.html` rather than a directory listing.
- Integrated validation depends on all four assigned files being complete.

## Parallel and sequential work decisions

- **Parallel:** After the dashboard contract is agreed, the Coder can populate `app/project-data.json` while the Designer develops visual and accessibility guidance because those tasks have separate file scopes.
- **Parallel:** The Coder can create `.vscode/launch.json` while dashboard styling is underway because the launch configuration does not overlap the app files.
- **Sequential:** The Coder should finalize `app/index.html` only after the project data fields and Designer's content hierarchy are confirmed.
- **Sequential:** The Designer establishes the direction for `app/styles.css` before the Coder completes its implementation, preventing conflicting edits to the same file.
- **Sequential:** End-to-end validation starts only after `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` are integrated.

## Edge cases

- Handle an empty `projects` array with a clear empty state.
- Show an explicit error message if `project-data.json` is missing, malformed, or cannot be loaded.
- Keep long project names, owner names, and recent activity text readable without breaking the card layout.
- Ensure status and priority remain understandable without relying on color alone.
- Preserve keyboard-visible focus indicators and usable layouts on narrow screens.
- Use the HTTP preview rather than opening `index.html` directly so browser fetch requests for JSON succeed.

## Validation expectations

- Confirm `app/project-data.json` parses as JSON, has a top-level `projects` array, and every project contains `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Confirm `app/index.html` contains the exact title `Project Pulse`, references `styles.css` and `project-data.json`, renders multiple visible `.project-card` elements, and displays status, recent activity, and priority values.
- Confirm `app/styles.css` includes `.dashboard`, `.project-card`, responsive layout rules, `border-radius`, `box-shadow`, accessible contrast, and visible focus styling.
- Confirm `.vscode/launch.json` is strict JSON and contains `Run Project Pulse Dashboard`, `python3 -m http.server 5500`, `${workspaceFolder}/app`, and `http://localhost:%s/index.html`.
- Run `Run Project Pulse Dashboard` and verify that the browser opens the dashboard frontend at `index.html`, not a directory listing.
- Test the dashboard at desktop and narrow viewport widths and verify readable content, usable keyboard navigation, and clear loading, empty, and error states.

## Open questions

- The exercise does not prescribe specific project names or status and priority vocabularies; use a small, representative, deterministic data set.
- No framework is required, so use semantic HTML, CSS, and browser JavaScript without adding dependencies unless a later requirement demands them.
