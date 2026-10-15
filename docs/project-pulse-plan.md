# Project Pulse Dashboard Implementation Plan

## Summary

Build a lightweight static dashboard that helps Mona's contributors scan active projects, ownership, status, recent activity, priority or risk, and a short contributor-friendly summary. The page will use HTML, CSS, JSON, and the browser's built-in JavaScript APIs; no framework or package installation is needed.

The repository has no existing application code in `app/`. The product requirements come from `.github/project-pulse-brief.md` and `.github/steps/3-step.md`. Keep the implementation within the four requested app and launch files, and use the existing VS Code launch workflow to serve the page from `app/`.

## Assumptions and Scope

- Use plain HTML, CSS, JSON, and a small inline script in `app/index.html` to fetch and render project data. This avoids adding files or build tooling.
- Serve the dashboard over HTTP. Loading `project-data.json` from a page opened with `file://` is not a supported preview path.
- Each project must include `name`, `owner`, `status`, `recentActivity`, and `priority`. Include a `summary` field as well because the brief asks for a short contributor-friendly summary, although the current workflow does not check for it.
- No real project data or status and priority vocabulary is supplied. Use clearly identifiable example data until Mona's team confirms the real entries and labels.
- Keep the page focused on scanning project information. Search, filtering, editing, and persistence are not specified and are out of scope.
- The Codespace is Linux-based and the exercise specifies `python3 -m http.server 5500`. The launch configuration must use VS Code's supported launch schema and open the served `index.html`.

## Roles and File Ownership

- **Orchestrator:** Owns sequencing, scope boundaries, and integration review. Assigns each file to one specialist and confirms the final preview works.
- **Planner:** Produces this plan in `docs/project-pulse-plan.md`; identifies dependencies, edge cases, and validation expectations. Does not implement application code.
- **Designer:** Owns `app/styles.css`. Defines the visual hierarchy, responsive card layout, status and priority treatments, spacing, and accessible contrast. Agrees on the required CSS hooks with Coder before implementation.
- **Coder:** Owns `app/project-data.json`, `app/index.html`, and `.vscode/launch.json`. Defines consistent sample records, renders the data safely, integrates the agreed CSS hooks, and configures the runnable preview.

File ownership is exclusive: Designer edits only the stylesheet; Coder edits the HTML, data, and launch configuration. Orchestrator coordinates and reviews but does not take over implementation files.

## Ordered Implementation Phases

### Phase 1: Confirm the interface contract

**Owner:** Orchestrator, with Designer and Coder  
**Files:** No application files

Agree on the minimum shared contract before parallel implementation:

- JSON has a top-level `projects` array. Records contain the five required fields and the brief-driven `summary` field.
- HTML renders one `.project-card` per project within a `.dashboard` container.
- The page displays each project's name, owner, status, recent activity, priority, and summary.
- Designer and Coder agree on the class hooks needed for the page, cards, status badges, and priority treatment.

**Dependency:** This contract gates parallel work in Phase 2.

### Phase 2: Produce independent assets in parallel

**Designer assignment:** `app/styles.css`  
Create a polished, responsive dashboard stylesheet with `.dashboard` and `.project-card` selectors, clear visual hierarchy, readable spacing, and distinct status and priority treatments. Include `border-radius` and `box-shadow` as required by the exercise. Preserve visible keyboard focus and adequate text contrast.

**Coder assignments:** `app/project-data.json` and `.vscode/launch.json`

- Create a valid top-level `projects` array with multiple representative records. Use consistent values for status and priority; include the required fields and a short `summary`.
- Create strict JSON with no comments for the launch configuration. Name it **Run Project Pulse Dashboard**, serve from `${workspaceFolder}/app`, run `python3 -m http.server 5500`, and configure `serverReadyAction` to open `http://localhost:%s/index.html`.

**Parallel decision:** These assignments can run concurrently once Phase 1 fixes the data fields and CSS hooks. They touch separate files and have no direct data dependency on each other.

### Phase 3: Integrate the page

**Owner:** Coder  
**File:** `app/index.html`

Create an accessible HTML page with the exact title **Project Pulse**. Reference `styles.css` and `project-data.json`. Fetch the JSON, render a visible `.project-card` for each project, and display all required fields plus the contributor-friendly summary.

Provide clear loading, empty, and data-load error states so a failed fetch or empty dataset does not leave the dashboard blank. Use safe text insertion rather than interpreting project data as HTML.

**Dependencies:** Requires the Phase 1 interface contract and the JSON shape from Phase 2. The stylesheet can be produced in parallel, provided its agreed hooks are used.

### Phase 4: Integrate and validate

**Owner:** Orchestrator, with Coder and Designer as needed  
**Files:** All four implementation files

Review the integrated page against the brief, run the focused checks below, and launch the preview from VS Code. Resolve any mismatches within the owning file scopes; repeat validation after changes.

**Dependency:** Requires Phases 2 and 3 to be complete.

## Dependencies and Parallel Work

- The JSON schema must be agreed before Coder builds the rendering logic.
- Designer and Coder must agree on CSS hooks before stylesheet and HTML integration.
- In Phase 2, Designer can build `app/styles.css` while Coder builds `app/project-data.json` and `.vscode/launch.json`.
- `app/index.html` follows the agreed data and styling contracts. The launch configuration is otherwise independent, but end-to-end preview validation requires all four files.
- Keep implementation sequential after integration begins: avoid simultaneous edits to `app/index.html` or `app/styles.css` by multiple agents.

## Edge Cases and Risks

- **Empty project list:** Render a clear empty state rather than an empty grid.
- **Fetch or JSON parsing failure:** Show an understandable error state; do not silently render a blank page.
- **Missing or unexpected field values:** Avoid displaying `undefined`; use a reasonable fallback or omit the unavailable value.
- **Untrusted project text:** Insert values as text, not HTML, to prevent markup injection.
- **Long names, summaries, and activity:** Ensure wrapping does not cause horizontal overflow or overlap at narrow widths.
- **Status and priority variation:** Use text labels as well as visual styling; do not rely on color alone. Keep unknown values readable.
- **Local file restrictions:** Fetching JSON may fail when opening the HTML directly with `file://`; validate through the configured HTTP server.
- **Port conflict:** Port `5500` may already be occupied. If the launch fails, report the conflict and use a free port consistently in the command and launch URL.
- **Launch configuration validity:** `.vscode/launch.json` must be strict JSON. A syntactically valid file may still use an unsupported launch schema, so test it through VS Code Run and Debug.
- **Unspecified real data:** Example projects and status or priority labels are placeholders until Mona's team provides approved content.

## Validation Expectations

### Plan validation

The Step 2 workflow checks for `docs/project-pulse-plan.md` and the required Project Pulse, Designer, Coder, file assignment, dependency, parallel-work, and validation references. Keep these terms explicit in the plan.

### File and syntax checks

- Confirm `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` exist.
- Parse the project data with `python3 -m json.tool app/project-data.json`.
- Parse the launch configuration with `python3 -m json.tool .vscode/launch.json`.
- Confirm the HTML references both `styles.css` and `project-data.json`, includes the exact title **Project Pulse**, and contains the `.project-card` rendering hook.
- Confirm the stylesheet includes `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.
- Confirm project records have the required fields and a contributor-friendly summary.

The Step 3 workflow checks file existence, required phrases, and JSON syntax. It does not start the application or verify its visual behavior.

### Runtime and visual checks

- Start **Run Project Pulse Dashboard** from VS Code Run and Debug.
- Confirm the server starts from `app/` and the browser opens `index.html`, not a directory listing.
- Confirm multiple project cards render from the JSON and display name, owner, status, recent activity, priority, and summary.
- Check loading, empty, and fetch-error behavior.
- Inspect the page at desktop and narrow mobile widths for readable text, usable spacing, no horizontal overflow, and visible focus indicators.
- Stop the preview server after testing.
- Run `bash scripts/validate-exercise.sh` only once the repository's other required learner files are present; this exercise-wide script checks more than the Project Pulse implementation.

## Open Questions

- Which real projects, owners, summaries, status labels, and priority or risk levels should replace the example data? This does not block implementation; use clearly labeled representative examples until confirmed.
- If the Codespace does not support the chosen VS Code launch adapter or port `5500` is unavailable, which alternative launch method or port should the exercise standardize on? First test the specified setup before changing it.
